# FASE 2: Trainers - Detalhes (WF 103-110)

## 📋 Visão Geral

Tecnicamente, **Trainers não são uma "fase" separada** - são processados como parte da **Fase 1 (Setup do Cohort)**. Este documento separa apenas para **facilitar a leitura** dos workflows relacionados a trainers.

## 🎯 Objetivo

- ✅ Convidar trainers para o programa
- ✅ Confirmar disponibilidade deles para as datas propostas
- ✅ Montar a Schedule (cronograma) baseado nas disponibilidades
- ✅ Enviar e garantir assinatura de contratos
- ✅ Criar eventos Teams para as aulas

## 📊 Workflows da Fase 2 (Trainers)

| ID | Nome | Trigger | Função |
|----|------|---------|--------|
| 103a | Send Invitation to Trainer | Recurrence (1:00 AM) | Envia convite inicial |
| 103b | Calculate Dates for Deadline | Recurrence | Calcula datas de lembretes |
| 104 | Check Trainers Dates Confirmed | Recurrence (1:00 AM) | Verifica se TODOS confirmaram |
| 105 | Send Contracts to Trainers | Recurrence (1:30 AM) | Envia para assinatura |
| 106 | Send via Adobe Sign | Auto | Integra com Adobe Sign |
| 107 | Check Adobe Agreement | Recurrence Daily | Verifica assinatura |
| 108 | Check Contracts Confirmed | Recurrence Daily | Confirma conclusão |
| 109 | Create Online Events | Recurrence (3:00 AM) | Cria reuniões Teams |
| 110 | Create Cohorts Units | Auto | Cria estrutura de units |

**E também:**
- WF 102a: Quando Schedule é criada (trigger para esta fase)
- WF 102b: Trata rejeção de convites

## ⏰ Timeline Típica

```
DIA 1 (Quando Cohort é criado):
  - WF 101 dispara
  - WF 102a cria Schedule automática
  - WF 103a envia convites para trainers

DIAS 2-3:
  - Trainers recebem email e confirmam disponibilidade
  - Se disponível: marca como "Sim"
  - Se não: marca como "Não" (WF 102b intervém)
  - WF 104 verifica status

DIAS 3-4:
  - WF 105 envia contratos quando TODOS confirmaram
  - WF 106 integra com Adobe Sign
  - Trainers assinam digitalmente

DIAS 4-5:
  - WF 107-108 verifica assinaturas
  - WF 109 cria teams meetings
  - WF 110 cria unidades

RESULTADO: Fase 1 completa, pronta para Coaches & Markers
```

## 🔄 Diferença: Confirmação de "Availability" vs "Capacity"

| Aspecto | Trainers | Coaches | Markers |
|--------|----------|---------|---------|
| **Confirmam** | Disponibilidade de DATAS | Capacidade GERAL | Capacidade de AVALIAÇÃO |
| **Pergunta** | "Você pode nos dias X, Y, Z?" | "Você pode mentorizar este cohort?" | "Você pode avaliar este cohort?" |
| **Decision** | Sim/Não por data | Sim/Não geral | Sim/Não geral |
| **Impacto se Não** | Reschedule necessária | Procurar outro coach | Procurar outro marker |

## 🎓 Pontos-Chave

### WF 103a - Convite Inicial
- Busca todos trainers que não foram convidados
- Envia email com datas PROPOSTAS
- Email tem link para confirmar no SharePoint
- Pode ser enviado múltiplas vezes (recurrence)

### WF 103b - Cálculo de Datas
- Calcula quando é "prazo final" para responder
- Calcula quando é "penúltimo dia" (aviso)
- Usa para reminders automáticos

### WF 104 - Verificação de Confirmação
- **Crítico**: Verifica se TODOS trainers confirmaram
- Se SIM: Pode ir embora para contratos
- Se NÃO: Aguarda próxima iteração (próxima madrugada)
- Pode ter timeout (se após 3 dias ninguém confirmou, escala)

### WF 105-110 - Contratos e Eventos
- WF 105: Envia contrato (sempre que quer, mas recurrence controla)
- WF 106: Integra Adobe Sign (automático quando contrato enviado)
- WF 107: Verifica no Adobe Sign se foi assinado
- WF 108: Confirma conclusão (todos assinaram?)
- WF 109: Cria Teams meeting para cada aula
- WF 110: Cria estrutura de units para organizar conteúdo

## ⚠️ Casos Especiais

### E se um Trainer Disser "Não"?

```
Trainer falou "Não" para algumas datas
         ↓
WF 102b (Handle Rejection) dispara
         ↓
Sistema precisa:
  1. Remover trainer dessas datas
  2. Procurar trainer alternativo
  3. Se não achar: Descartar aquela aula OU
  4. Propor novas datas para o Schedule inteiro
```

**Decisão é manual** ou automática conforme configuração.

### E se Trainer Rejeitar Contrato?

```
Trainer não quer assinar
         ↓
Sistema tenta re-enviar 3x
         ↓
Se ainda não assinou: Manual intervention
```

## 🔗 Sequência Exata

```
WF 101: Cohort criado
    ↓
WF 102a: Schedule criada automaticamente
    ↓
WF 103a: Convites enviados (1:00 AM, recurrence)
    ↓
WF 103b: Cálculo de datas (paralelo)
    ↓
WF 104: Verificação confirmação (1:00 AM, loop)
    ├─ Enquanto não ALL confirmarem: espera
    └─ Quando ALL confirmarem: avança
    ↓
WF 105: Contratos enviados (1:30 AM)
    ↓
WF 106: Adobe Sign integrado (automático)
    ↓
WF 107: Verificação Adobe (diária, loop)
    ├─ Enquanto não ALL assinarem: espera
    └─ Quando ALL assinarem: avança
    ↓
WF 108: Confirmação final (diária)
    ↓
WF 109: Teams meetings criados (3:00 AM)
    ↓
WF 110: Units criadas (automático)
    ↓
✅ TRAINERS PHASE COMPLETE
    ↓
Fase 3 e 4 podem começar (Coaches & Markers)
```

## 🚀 Saída da Fase 2

Quando trainers estão prontos:
- ✅ TODOS trainers e confirmaram datas
- ✅ TODOS contratos foram assinados
- ✅ TODOS Teams meetings foram criados
- ✅ Cronograma é definitivo (não muda mais)

## 📚 Arquivo Relacionado

Consulte também:
- [FASE_1_SETUP/_README.md](FASE_1_SETUP/_README.md) - Context maior
- [00_WORKFLOW_REFERENCE.md](00_WORKFLOW_REFERENCE.md) - Todos os IDs

---

**Nota:** Tecnicamente, não existe "FASE 2" como fase separada. Trainers são parte da FASE 1. Este documento foi criado apenas para separa a lógica por clareza.

**Status:** ✅ Documentado | **Atualizado:** Março 2026
