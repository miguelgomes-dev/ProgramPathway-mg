# FASE 3: Coaches (WF 201-207)

## 📋 Visão Geral

A **Fase 3** começa **em paralelo com a Fase 4** (Markers). Seu objetivo é **confirmar a disponibilidade de Coaches** e garantir que todos os **contratos foram assinados**.

## 🎯 Objetivo

- ✅ Contactar coaches disponíveis
- ✅ Confirmar que têm capacidade para atender este cohort
- ✅ Enviá-los contratos
- ✅ Garantir assinatura via Adobe Sign

## 📊 Workflows da Fase 3

| ID | Nome | Trigger | Status |
|----|------|---------|--------|
| 201a | Send Capacity Confirmation | Recurrence (3:45 AM) | "Coach Capacity Pending" |
| 201b | Handle Rejected Invitations | Event | - |
| 202 | Wait for Confirmation | Recurrence (3:45 AM) | - |
| 203 | Check Confirmed | Recurrence (4:00 AM) | "Coach Capacity Confirmed" |
| 204 | Send Contracts | Recurrence (4:30 AM) | "Sending Contracts" |
| 205 | Send via Adobe Sign | Auto | - |
| 206 | Check Adobe Agreement | Recurrence Daily | - |
| 207 | Check Contracts Confirmed | Recurrence Daily | "Coaches Confirmed" |

## ⏰ Timeline Típica

```
APÓS FASE 1 COMPLETA (dia X):

DIA X + 1, 3:45 AM:
  - WF 201a envia convites para coaches
  - WF 202 aguarda confirmação

DIAS X+1 a X+3:
  - Coaches respondem confirmando disponibilidade
  - WF 203 verifica se TODOS confirmaram

DIA X+3, 4:30 AM:
  - WF 204 envia contratos
  - WF 205-206 gerencia Adobe Sign

DIAS X+4 a X+7:
  - Coaches assinam contratos
  - WF 207 confirma conclusão

RESULTADO: Todos coaches prontos
```

## 🔄 Fluxo Detalhado

```
[APÓS FASE 1 CONCLUÍDA]
         ↓
    Status = "All Trainers Contracts Confirmed"
         ↓
WF 201a (Recurrence Daily 3:45 AM)
  "Enviar convites para coaches"
  Email: "Precision necessária sua confirmação de capacidade"
         ↓
    Status = "Coach Capacity Pending"
         ↓
┌─ WF 201b (Event-based)
│  "Se Coach rejeitou"
│  → Re-enviar convite
│
WF 202 (Recurrence Daily 3:45 AM)
  "Aguardar confirmação de capacidade"
         ↓
WF 203 (Recurrence Daily 4:00 AM)
  "Verificar se TODOS coaches confirmaram"
  Loop até ALL confirmarem
         ↓
    Status = "Coach Capacity Confirmed"
         ↓
WF 204-207 (Recurrence 4:30 AM+)
  "Enviar contratos"
  "Adobe Sign process"
  "Verificar assinaturas"
         ↓
    Status = "Coaches Contracts Confirmed"
         ↓
    ✅ FASE 3 COMPLETA
```

## 🎓 Detalhes Importantes

### O que é "Capacity"?
- Não é apenas "posso estar presente"
- É "tenho recursos/tempo/disponibilidade" para fazer o trabalho
- Coaches precisam avaliar se conseguem mentorizar este cohort

### Convite para Coaches (WF 201a)
- Descobre quais coaches não foram ainda invitados
- Envia email para cada um
- Email contém: Datas do cohort, número de alunos, contato para dúvidas

### Rejeição de Coach (WF 201b)
- Se coach marca como "Não posso", WF 201b dispara
- Re-envia em 2 dias da rejeição
- Se rejeitar 3x, escalaciona (procura outro coach)

### Contratos de Coaches (WF 204-207)
- Mesmo processo que Trainers
- Adobe Sign envia contrato
- Coach recebe por email
- Assina digitalmente
- Sistema valida assinatura automaticamente

## ⚠️ Pontos Críticos

| Ponto Crítico | O que fazemos |
|---|---|
| Coach não responde | Aguarda 2 dias, re-envia, aguarda mais 2 |
| Coach rejeita | WF 201b re-envia, até 3 tentativas |
| Muitos coaches rejeitam | Manual: procurar substitutos |
| Adobe Sign delay | Relembra 2x, depois manual |

## 🔁 Execução Paralela com Fase 4

```
MOMENTO FASE 1 TERMINA:
         ↓
    ┌──────────────────────┬──────────────────────┐
    │                      │                      │
    ↓ WF 201a (Coaches)    ↓ WF 301a (Markers)   │
    │ Recurrence 3:45 AM   │ Recurrence ~4:30 AM │
    │ (Paralelo)           │ (Paralelo)          │
    │                      │                     │
    └──────────────────────┴──────────────────────┘
    
AMBOS rodam SIMULTANEAMENTE
Nenhum espera o outro
Ambos precisam terminar antes de Fase 5
```

## 🚀 Saída da Fase 3

Quando a Fase 3 termina:
- ✅ TODOS coaches confirmados
- ✅ TODOS contratos assinados
- ✅ Coaches prontos para começar

## 📊 Diferença: Trainers vs Coaches

| Aspecto | Trainers (Fase 1) | Coaches (Fase 3) |
|--------|------------------|-----------------|
| Trigger | Quando Schedule criada | Quando Fase 1 concluída |
| Confirmação | Disponibilidade de DATAS | Capacidade GERAL |
| Responsabilidade | Ministrar aulas | Mentorizar alunos |
| Execução | Fase 1, sequencial | Paralela com Markers |

## 🔗 Relacionamentos

- **Depende de:** Fase 1 (Trainers) estar completa
- **Paralela com:** Fase 4 (Markers)
- **Resultado:** Preparação de Coaches para Attendance

## 📚 Próxima Fase

Consulte **Fase 4 (Markers)** - segue o exato mesmo padrão!

---

**Status:** ✅ Documentado | **Atualizado:** Março 2026
