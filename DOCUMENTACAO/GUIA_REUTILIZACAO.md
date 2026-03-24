# 🔄 Guia de Reutilização & Padrões

## Introdução

Este guia documenta os **padrões de design** utilizados na solução Programme Pathway Portal para facilitar:
- Manutenção futura
- Reutilização em novos projetos
- Escalabilidade
- Consistência

---

## 🎯 Padrões Identificados

### Padrão 1: Trigger + Verificação Periódica

**Onde é usado:**
- Convites de disponibilidade (WF 103a, 201a, 301a)
- Verificações de confirmação (WF 104, 203, 303)
- Processamento de contratos

**Estrutura:**
```
1. EVENT TRIGGER: Algo acontece no SharePoint
   ↓
2. CONDITION: Condicional simples
   ├─ SIM: Executa ação
   └─ NÃO: Aguarda próxima iteração
   ↓
3. RECURRENCE: Verifica novamente em X horas
   ↓
4. LOOP: Continua até condição ser atendida
```

**Benefício:** Permite ações assíncronas sem bloquear o fluxo

**Exemplo na solução:**
- WF 104 verifica durante madrugadas se todos trainers confirmaram
- Se não, continua verificando
- Se sim (todos confirmados), progride para próxima fase

---

### Padrão 2: Rejection Handler

**Onde é usado:**
- Convites rejeitados (WF 102b, 201b, 301b)

**Estrutura:**
```
QUANDO: Convite é marcado como "Rejeitado" no SharePoint
   ↓
ENTÃO: 
   ├─ Adiciona contador de tentativas
   ├─ Aguarda tempo antes de re-enviar
   └─ Re-envia convite com mensagem atualizada
```

**Benefício:** Automaticamente "dá segunda chance"

**Implementação:**
- Campo no SharePoint: "Rejection_Count"
- Campo no SharePoint: "Last_Rejection_Date"
- Lógica: Se contagem < 3, re-envia; Se ≥ 3, escalaciona

---

### Padrão 3: Adobe Sign Integration

**Onde é usado:**
- Assinatura de contratos (WF 106, 205, 305)
- Verificação de assinatura (WF 107, 206, 306)

**Estrutura:**
```
1. GERAÇÃO: Contrato é gerado (Word → PDF via Content Conversion)
2. ENVIO: Adobe Sign connector envia para assinatura
3. WEBHOOK: Adobe Sign notifica quando assinado
4. VERIFICAÇÃO: Sistema valida assinatura recebida
5. ARMAZENAMENTO: Contrato assinado vai para SharePoint
```

**Parametrização:**
- EMAIL: Do ator (trainer/coach/marker)
- DOCUMENT: Template Word com dados do cohort
- RECIPIENTS: Sempre 1 (o ator específico)
- REMINDER: Adobe envia lembretes automáticos

**Benefício:** Rastreabilidade total + validade legal

---

### Padrão 4: Paralelização Controlada

**Onde é usado:**
- Coaches e Markers rodam simultaneamente (WF 201-207 paralelo com WF 301-307)

**Estrutura:**
```
FASE 2 (TRAINERS) COMPLETA
   ↓
MOMENTO X: Dispara WF 201a E WF 301a simultaneamente
   ↓
PARALELO:
├─ Coaches confirmam
└─ Markers confirmam
   ↓
AMBOS precisam completar antes de próxima fase
```

**Implementação:**
- Não há `Wait` entre WF 110 e WF 201a/301a
- Ambos workflows verificam se "Fase 1 completa"
- Status field controla sincronização

**Benefício:** Reduz tempo total em ~33%

---

### Padrão 5: Status Machine (Estados Sequenciais)

**Onde é usado:**
- Campo "Status" no Cohort no SharePoint

**Estrutura:**
```
Estados são SEQUENCIAIS e IMUTÁVEIS:
0 → 1 → 2 → 3 → 4 → 5 → 6 → 8 → 10 → (20-23) → (30-33)

Cada workflow:
├─ LEIA status atual
├─ VALIDE se pode executar (ex: não pode ser 5 se status ainda é 3)
├─ EXECUTE lógica
└─ ATUALIZE para próximo status
```

**Benefício:** 
- Previne execução fora de ordem
- Rastreabilidade de onde está no processo
- Fácil entender state do cohort em qualquer momento

**Estados:**
- **0**: Novo cohort
- **1-2**: Setup inicial
- **3-6**: Fase Trainers
- **8**: Eventos criados
- **10**: Units criadas
- **20-23**: Fase Coaches
- **30-33**: Fase Markers

---

## 🛠️ Pattern Recipes (Receitas)

### Recipe 1: Criar um Novo Workflow "Envio"

**Caso de uso:** Você precisa enviar algo (email, convite, etc.)

**Passos:**
1. **Trigger**: Recurrence Daily + Condition "Status = X"
2. **Action**: Send Email (Office 365) ou Send Message (Teams)
3. **Update**: Mude status para "Enviado - Aguardando"
4. **Schedule**: Configure para rodar durante off-peak (madrugada)

**Template:**
```json
{
  "trigger": "Recurrence (Daily, XX:XX)",
  "condition": {
    "field": "Status",
    "equals": "PENDING_SEND"
  },
  "actions": [
    { "send": "email|teams" },
    { "updateStatus": "SENT_WAITING_CONFIRMATION" }
  ],
  "retries": 3,
  "scheduleOffPeak": true
}
```

---

### Recipe 2: Criar um Novo Workflow "Verificação"

**Caso de uso:** Você precisa validar se algo foi confirmado

**Passos:**
1. **Trigger**: Recurrence Daily
2. **Query**: Verificar se TODOS os items têm status "Confirmado"
3. **Condition**: 
   - Se todos: Progride
   - Se não: Continua verificando dia que vem
4. **Update**: Mude status do Cohort para próxima fase

**Template:**
```json
{
  "trigger": "Recurrence (Daily, XX:XX)",
  "query": "SELECT * FROM Confirmations WHERE cohort_id = X AND status != 'Confirmed'",
  "condition": {
    "if": "query.count == 0",
    "then": ["updateCohortStatus", "notifyStakeholders"],
    "else": "requeue_next_iteration"
  }
}
```

---

### Recipe 3: Adicionar Tratamento de Rejeição

**Caso de uso:** Alguém rejeitou um convite, precisa re-enviar

**Passos:**
1. **Trigger**: Quando status = "Rejected"
2. **Action**: Incrementar "rejection_count"
3. **Condition**:
   - Se count < 3: Re-enviar + aguardar
   - Se count ≥ 3: Escalonar para manual
4. **Update**: Mude timestamp de última tentativa

**Template:**
```json
{
  "trigger": "When item modified AND status == 'Rejected'",
  "actions": [
    { "increment": "rejection_count" },
    { "condition": {
        "if": "rejection_count < 3",
        "then": ["resend_invitation", "set_next_check_date"],
        "else": ["escalate_to_manual", "notify_admin"]
      }
    }
  ]
}
```

---

## 📋 Checklist para Novo Workflow

Quando adicionar um novo workflow, valide:

- [ ] **Nome claro**: Segue padrão "XXX-Descrição" (ex: "401-When...")
- [ ] **Trigger definido**: Event-based ou Recurrence?
- [ ] **Status control**: Campo atualizado após execução?
- [ ] **Tratamento de erros**: Retry logic implementada?
- [ ] **Rejeições**: Existem casos onde ator pode rejeitar?
- [ ] **Agendamento**: Off-peak se for recurrence?
- [ ] **Notificações**: Alguém precisa saber do resultado?
- [ ] **Documentação**: README em Workflows/FASE_X/_README.md
- [ ] **Teste**: Validado em ambiente de staging?

---

## 📊 Métodos de Extensão

### Adicionar Nova Fase (ex: Attendance)

**Estrutura esperada:**
```
Workflows/FASE_5_ATTENDANCE/
├── _README.md (explicar esta fase)
├── 401-When-attendance-marked.md
├── 402-Check-attendance-completion.md
├── ... (demais workflows)
```

**Atualizações necessárias:**
1. Criar pasta FASE_5
2. Atualizar `00_WORKFLOW_REFERENCE.md` com novos workflows
3. Atualizar `ARQUITETURA.md` com novo fluxo
4. Atualizar `GUIA_REUTILIZACAO.md` (este arquivo) se novos padrões
5. Atualizar `README.md` com status do projeto

**Padrão esperado em Attendance:**
- Provavelmente triggers quando presença é marcada
- Verificações se quórum foi atingido
- Relatórios de presença por trainer/cohort

---

### Integrar Novo Conector (ex: Power BI, SAP)

**Considerações:**
1. **Autenticação**: Como o connector se autentica?
2. **Rate Limits**: Há limites de chamadas?
3. **Custo**: Afeta orçamento?
4. **Redundância**: O que fazer se connector falhar?
5. **Localização no fluxo**: Qual workflow usará?

**Padrão:**
```
NOVO_CONECTOR_ACTION:
├─ Try: Executa chamada ao conector
├─ Catch: Se falhar, log + retry com backoff
└─ Finally: Notifica se excedeu max retries
```

---

## 🐛 Troubleshooting Comum

### Problema: Workflow "Preso" em Status X

**Causa Provável:**
- Verificação (WF 104, 203, 303) está esperando X confirmações mas nunca chegam
- Um trainer/coach não confirmou

**Solução:**
1. Verifique SharePoint - quem falta confirmar?
2. Verifique se email foi entregue (verificar logs do Office 365)
3. Manual: Marque como confirmado para teste, ou
4. Manual: Re-envie convite para pessoa

---

### Problema: Duplicação de Emails

**Causa Provável:**
- Recurrence está disparando múltiplas vezes
- Não há lock/semáforo impedindo ação dupla

**Solução:**
1. Adicione campo "last_sent_date"
2. Condition: Só enviar se diferença > 23 horas
3. Prevenção: Use "Recurrence" e não "Do until"

---

### Problema: Adobe Sign não retorna assinatura

**Causa Provável:**
- Campo de email errado no contrato
- Adobe Sign aguardando assinador que não recebeu email
- Webhook não está configurado

**Solução:**
1. Azure Portal → Verificar Webhook do Adobe Sign
2. Testar manualmente no Adobe Sign admin console
3. Verificar email está correto no contrato (template Word)

---

## 📚 Referências Adicionais

- [ARQUITETURA.md](ARQUITETURA.md) - Fluxos detalhados
- [GLOSSARIO.md](GLOSSARIO.md) - Definição de termos
- [Workflows/00_WORKFLOW_REFERENCE.md](Workflows/00_WORKFLOW_REFERENCE.md) - Lista de todos os WF
- Power Automate Best Practices: [Microsoft Docs](https://docs.microsoft.com)
- Adobe Sign Integration: [Adobe Documentation](https://www.adobe.io)

---

## ✅ Conclusão

A solução está construída sobre **5 padrões principais**:
1. ✅ Trigger + Verificação Periódica
2. ✅ Rejection Handler
3. ✅ Adobe Sign Integration
4. ✅ Paralelização Controlada
5. ✅ Status Machine (Estados)

Compreender esses padrões facilita **manutenção, debug e extensão** da solução.

**Quando em dúvida, consulte este documento!**
