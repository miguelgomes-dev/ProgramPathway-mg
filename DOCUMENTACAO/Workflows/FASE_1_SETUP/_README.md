# FASE 1: Setup do Cohort (WF 101-110)

## 📋 Visão Geral

A **Fase 1** é onde tudo começa. Quando um novo Cohort é criado, este conjunto de workflows **prepara toda a estrutura necessária** para as fases seguintes.

## 🎯 Objetivo

Transformar a criação simples de um Cohort em uma **estrutura completa com:**
- ✅ Agenda montada (Schedule)
- ✅ Trainers convidados e confirmados
- ✅ Contratos assinados
- ✅ Eventos/reuniões Teams criadas
- ✅ Unidades/módulos do cohort estruturados

## 📊 Workflows da Fase 1

| ID | Nome | Duração Típica |
|----|----|--------------|
| 101 | Quando Cohort é criado | Instantâneo |
| 102a | Quando Schedule é criada | Instantâneo |
| 102b | Trata rejeição de trainer | On-demand |
| 103a | Envia convites trainers | 1-2 dias |
| 103b | Calcula datas | On-demand |
| 104 | Verifica confirmação | 1-3 dias |
| 105 | Envia contratos | 1-2 dias |
| 106 | Adobe Sign | Paralelo |
| 107 | Verifica assinatura Adobe | 2-3 dias |
| 108 | Confirma contratos | Paralelo |
| 109 | Cria eventos Teams | Instantâneo |
| 110 | Cria units | Instantâneo |

## ⏰ Timeline Típica

```
DIA 1:
  - Usuário cria Cohort
  - WF 101 dispara
  - WF 102a cria Schedule
  - WF 103a envia convites

DIAS 2-3:
  - Trainers confirmam disponibilidade
  - WF 104 verifica confirmações

DIAS 3-4:
  - WF 105 envia contratos
  - WF 106-108 gerencia Adobe Sign

DIAS 4-5:
  - WF 107 verifica assinaturas
  - WF 109 cria eventos
  - WF 110 cria units

RESULTADO: Setup completo, pronto para Coaches & Markers
```

## 🔄 Fluxo Detalhado

```
WF 101 (Event-based)
  "Um novo Cohort foi criado"
         ↓
    Status = "Preprocessing Data Validation"
         ↓
WF 102a (Event-based)
  "Quando Schedule é criada para este Cohort"
         ↓
    Status = "Creating Cohort Schedule"
         ↓
┌─ WF 102b (Event-based)
│  "Se Trainer rejeitou convite"
│  → Re-enviar com lógica de rejeição
│
WF 103a + 103b (Recurrence Daily 1:00 AM)
  "Enviar convites para trainers"
  "Calcular datas para reminders"
         ↓
    Status = "Inviting Trainers"
         ↓
WF 104 (Recurrence Daily 1:00 AM)
  "Verificar se TODOS confirmaram"
  Loop até ALL confirmarem
         ↓
    Status = "All Trainers Dates Confirmed"
         ↓
WF 105-108 (Recurrence 1:30 AM)
  "Enviar contratos"
  "Processo Adobe Sign"
  "Verificar assinatura"
         ↓
    Status = "All Trainers Contracts Confirmed"
         ↓
WF 109 (Recurrence 3:00 AM)
  "Criar eventos Teams para cada masterclass"
         ↓
    Status = "All Online Events Created"
         ↓
WF 110 (Automático)
  "Criar units/módulos do cohort"
         ↓
    Status = "All Cohorts Units Created"
         ↓
    ✅ FASE 1 COMPLETA
         ↓
    Pronto para Fase 2 (Coaches & Markers)
```

## 🎓 Detalhes Importantes

### Trigger Inicial (WF 101)
- **Quando:** Usuário clica "Criar Cohort" e preenche: Program + Data Início
- **Result:** Sistema cria Cohort no SharePoint, status = "Pending"
- **Próxima ação:** WF 102a é disparada automaticamente

### Convites para Trainers (WF 103a)
- Envia email TODOS os trainers do programa
- Qual email? Do trainer convidado
- Com quais informações? Datas propostas, link para confirmar
- Como confirma? No SharePoint ou responde email

### Verificação de Confirmações (WF 104)
- Verifica ao mesmo tempo se TODOS trainers confirmaram
- Se não: espera até amanhã e verifica de novo
- Se sim: progride para envio de contratos
- Timeout opcional: após X dias, escala para manual

### Adobe Sign Integration (WF 106-108)
- Contrato é gerado via template Word
- Convertido para PDF
- Adobe Sign envia para assinatura
- Ator assina usando email
- Sistema valida assinatura automaticamente

### Criação de Eventos (WF 109)
- Para cada masterclass no cronograma
- Cria reunião Teams com:
  - Título: Nome da masterclass
  - Data/Hora: Conforme Schedule
  - Participantes: Trainer + Alunos
  - Gravação: Automática

### Criação de Units (WF 110)
- Cria estrutura: Cohort → Unit → Masterclass
- Organiza alunos por coach/marker (será usado em Fase 3)
- Prepara repositório de materiais

## ⚠️ Pontos Críticos

| Ponto Crítico | O que fazemos | Por quê |
|---|---|---|
| Trainer não confirma | Espera 3 dias, depois escalaciona | Não quer travar o processo |
| Trainer rejeita | WF 102b re-envia | Talvez tenha perdido primeiro email |
| Adobe Sign não assinado | Relembra 3x, depois manual | Contrato é legal, precisamos |
| Email inválido | Valida antes de enviar | Não quer enviar para email errado |

## 🚀 Saída da Fase 1

Quando Fase 1 termina, sistema está em estado:
- ✅ Cohort totalmente estruturado
- ✅ Todos trainers confirmados
- ✅ Todos contratos assinados
- ✅ Todos eventos Teams criados
- ✅ Unidades/módulos prontos

## 🔗 Próximas Fases

- **Fase 2 (não existe)** - Trainers já foram processados na Fase 1
- **Fase 3: Coaches** - Começa em paralelo com Fase 4
- **Fase 4: Markers** - Começa em paralelo com Fase 3

## 📚 Arquivo Próximo

Veja a tabela de referência completa aqui: [00_WORKFLOW_REFERENCE.md](../00_WORKFLOW_REFERENCE.md)

Para entender termos específicos, consulte: [GLOSSARIO.md](../../GLOSSARIO.md)

---

**Status:** ✅ Documentado | **Atualizado:** Março 2026
