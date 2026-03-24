# 📋 Tabela de Referência Completa - Todos os Workflows

## Resumo

| Total | Fases | Status |
|-------|-------|--------|
| 28 Workflows | 4 Fases | ✅ Em Produção |

---

## 📊 Tabela Completa

### FASE 1: SETUP DO COHORT (WF 101-110)

| ID | Nome | Tipo | Trigger | Objetivo | Arquivo |
|---|---|---|---|---|---|
| **101** | When a Cohort is created | Setup | Event | Inicia processo quando novo Cohort é criado | [Link](FASE_1_SETUP/101-WhenaCohortiscreated.md) |
| **102a** | When a Cohort Schedule is created | Setup | Event | Dispara fluxo quando agenda é criada | [Link](FASE_1_SETUP/102a-WhenaCohortScheduleiscreated.md) |
| **102b** | When a Trainer Invitation Rejected is created | Rejection | Event | Trata rejeição de convite de trainer | [Link](FASE_1_SETUP/102b-WhenaTrainerInvitationRejectediscreated.md) |
| **103a** | Send Invitation to Trainer and Wait for Confirmation | Invitation | Recurrence (1:00 AM) | Envia convite inicial para trainers | [Link](FASE_1_SETUP/103a-SendInvitationtoTrainerandWaitforConfirmation.md) |
| **103b** | Calculate and Update Dates for Deadline Reminder | Calculation | Recurrence | Calcula datas para lembretes de deadline | [Link](FASE_1_SETUP/103b-CalculateandUpdateDatesforDeadlineReminder.md) |
| **104** | Check For Trainers Dates Confirmed | Verification | Recurrence (1:00 AM) | Verifica se TODOS trainers confirmaram datas | [Link](FASE_1_SETUP/104-CheckForTrainersDatesConfirmed.md) |
| **105** | Send Contracts to Trainers | Document | Recurrence (1:30 AM) | Envia documentos de contrato via email | [Link](FASE_1_SETUP/105-SendContractstoTrainers.md) |
| **106** | Send Contract to Trainer using Adobe Sign | Signature | Auto | Integra com Adobe Sign para assinatura digital | [Link](FASE_1_SETUP/106-SendContracttoTrainerusingAdobeSign.md) |
| **107** | Check For Trainers Adobe Sign Agreement | Verification | Recurrence Daily | Verifica status de assinatura no Adobe Sign | [Link](FASE_1_SETUP/107-CheckForTrainersAdobeSignAgreement.md) |
| **108** | Check For Trainers Contracts Confirmed | Verification | Recurrence Daily | Valida se contratos foram assinados | [Link](FASE_1_SETUP/108-CheckForTrainersContractsConfirmed.md) |
| **109** | Create Online Events | Meeting | Recurrence (3:00 AM) | Cria reuniões no Microsoft Teams | [Link](FASE_1_SETUP/109-CreateOnlineEvents.md) |
| **110** | Create Cohorts Units | Structure | Auto | Cria unidades/módulos do cohort | [Link](FASE_1_SETUP/110-CreateCohortsUnits.md) |

**Status da Fase 1:** ✅ Completa | **Duração típica:** 3-5 dias

---

### FASE 2: TRAINERS (WF 102-110 detalhado)

Veja tabela acima - WF 102-110 cobrem toda a fase de trainers

**Sequência:**
1. Convite inicial (103a)
2. Cálculo de datas (103b)
3. Verificação confirmação (104)
4. Envio contrato (105)
5. Adobe Sign (106)
6. Verificação Adobe (107)
7. Confirmação contratos (108)
8. Criação eventos (109)
9. Criação units (110)

**Status esperado ao final:** "All Trainers Contracts Confirmed" + "All Online Events Created"

---

### FASE 3: COACHES (WF 201-207)

| ID | Nome | Tipo | Trigger | Objetivo | Arquivo |
|---|---|---|---|---|---|
| **201a** | Send Capacity Confirmation to Coaches | Invitation | Recurrence (3:45 AM) | Envia convite para coaches confirmarem capacidade | [Link](FASE_3_COACHES/201a-SendCapacityConfirmationtoCoaches.md) |
| **201b** | When a Coach Confirmation Rejected is created | Rejection | Event | Trata rejeição de coach | [Link](FASE_3_COACHES/201b-WhenaCoachConfirmationRejectediscreated.md) |
| **202** | Send Capacity Confirmation to Coach and Wait for Confirmation | Confirmation | Recurrence (3:45 AM) | Aguarda confirmação de capacidade | [Link](FASE_3_COACHES/202-SendCapacityConfirmationtoCoachandWaitforConfi.md) |
| **203** | Check For Coaches Capacity Confirmed | Verification | Recurrence (4:00 AM) | Verifica se TODOS coaches confirmaram | [Link](FASE_3_COACHES/203-CheckForCoachesCapacityConfirmed.md) |
| **204** | Send Contracts to Coaches | Document | Recurrence (4:30 AM) | Envia contratos para assinatura | [Link](FASE_3_COACHES/204-SendContractstoCoaches.md) |
| **205** | Send Contract to Coach using Adobe Sign | Signature | Auto | Integra Adobe Sign para coaches | [Link](FASE_3_COACHES/205-SendContracttoCoachusingAdobeSign.md) |
| **206** | Check For Coaches Adobe Sign Agreement | Verification | Recurrence Daily | Verifica assinatura no Adobe Sign | [Link](FASE_3_COACHES/206-CheckForCoachesAdobeSignAgreement.md) |
| **207** | Check For Coaches Contracts Confirmed | Verification | Recurrence Daily | Confirma conclusão dos contratos | [Link](FASE_3_COACHES/207-CheckForCoachesContractsConfirmed.md) |

**Status esperado ao final:** "Coaches Contracts Confirmed"

**Execução:** ⏳ Paralela com Fase 4 (Markers)

---

### FASE 4: MARKERS (WF 301-307)

| ID | Nome | Tipo | Trigger | Objetivo | Arquivo |
|---|---|---|---|---|---|
| **301a** | Send Capacity Confirmation to Markers | Invitation | Recurrence (~4:30 AM) | Envia convite para markers confirmarem capacidade | [Link](FASE_4_MARKERS/301a-SendCapacityConfirmationtoMarkers.md) |
| **301b** | When a Marker Confirmation Rejected is created | Rejection | Event | Trata rejeição de marker | [Link](FASE_4_MARKERS/301b-WhenaMarkerConfirmationRejectediscreated.md) |
| **302** | Send Capacity Confirmation to Marker and Wait for Confirmation | Confirmation | Recurrence (~4:30 AM) | Aguarda confirmação de capacidade | [Link](FASE_4_MARKERS/302-SendCapacityConfirmationtoMarkerandWaitforConf.md) |
| **303** | Check For Markers Capacity Confirmed | Verification | Recurrence (5:00 AM) | Verifica se TODOS markers confirmaram | [Link](FASE_4_MARKERS/303-CheckForMarkersCapacityConfirmed.md) |
| **304** | Send Contracts to Markers | Document | Recurrence (5:30 AM) | Envia contratos para assinatura | [Link](FASE_4_MARKERS/304-SendContractstoMarkers.md) |
| **305** | Send Contract to Marker using Adobe Sign | Signature | Auto | Integra Adobe Sign para markers | [Link](FASE_4_MARKERS/305-SendContracttoMarkerusingAdobeSign.md) |
| **306** | Check For Markers Adobe Sign Agreement | Verification | Recurrence Daily | Verifica assinatura no Adobe Sign | [Link](FASE_4_MARKERS/306-CheckForMarkersAdobeSignAgreement.md) |
| **307** | Check For Markers Contracts Confirmed | Verification | Recurrence Daily | Confirma conclusão dos contratos | [Link](FASE_4_MARKERS/307-CheckForMarkersContractsConfirmed.md) |

**Status esperado ao final:** "Markers Contracts Confirmed"

**Execução:** ⏳ Paralela com Fase 3 (Coaches)

---

## 🚀 Sequência de Execução

```
WF 101: Cohort Created (TRIGGER)
   ↓
WF 102a: Schedule Created
   ↓
WF 102b: Handle Rejections (se houver)
   ↓
WF 103a-b: Send Invites + Calculate Dates (Trainers)
   ↓
WF 104: Check All Trainers Confirmed
   ↓
WF 105-108: Send & Verify Contracts (Trainers)
   ↓
WF 109: Create Events
   ↓
WF 110: Create Units
   ↓
┌─────────────────────────────┐
│  WF 201a-207 (COACHES)      │ [PARALELO]
│  WF 301a-307 (MARKERS)      │ [PARALELO]
└─────────────────────────────┘
   ↓
[FASE COMPLETA]
```

---

## 📊 Estatísticas por Tipo

| Tipo | Quantidade | IDs |
|------|-----------|-----|
| **Event Trigger** | 5 | 101, 102a, 102b, 201b, 301b |
| **Recurrence Trigger** | 17 | 103a, 103b, 104, 105, 107, 108, 109, 202, 203, 204, 206, 207, 302, 303, 304, 306, 307 |
| **Auto Trigger** | 6 | 106, 205, 305, 110 |
| **Verification** | 8 | 104, 107, 108, 203, 206, 207, 303, 306, 307 |
| **Invitation** | 5 | 103a, 201a, 202, 301a, 302 |
| **Document/Signature** | 6 | 105, 106, 204, 205, 304, 305 |
| **Other** | 8 | 102a, 102b, 103b, 109, 110, 201b, 301b |

---

## ⏰ Cronograma de Execução (Horários Típicos)

| Horário | Workflows | Função |
|---------|-----------|--------|
| **1:00 AM** | WF 103a, 104 | Enviar & verificar convites trainers |
| **1:30 AM** | WF 105, 107, 108 | Contratos trainers |
| **3:00 AM** | WF 109 | Criar eventos Teams |
| **3:45 AM** | WF 201a, 202 | Enviar convites coaches |
| **4:00 AM** | WF 203 | Verificar coaches |
| **4:30 AM** | WF 204, 206, 207, 301a, 302 | Contratos coaches + Convites markers |
| **5:00 AM** | WF 303 | Verificar markers |
| **5:30 AM** | WF 304, 306 | Contratos markers |

**Nota:** Horários podem variar conforme timezone e configuração

---

## 🔗 Links Rápidos

### Por Fase
- [Fase 1: Setup](FASE_1_SETUP/)
- [Fase 2: Trainers](FASE_2_TRAINERS/)
- [Fase 3: Coaches](FASE_3_COACHES/)
- [Fase 4: Markers](FASE_4_MARKERS/)

### Documentação Principal
- [README.md](../README.md) - Visão geral
- [ARQUITETURA.md](../ARQUITETURA.md) - Diagramas
- [GLOSSARIO.md](../GLOSSARIO.md) - Definições
- [GUIA_REUTILIZACAO.md](../GUIA_REUTILIZACAO.md) - Padrões

---

## 📝 Notas

- **Status:** Todos os 28 workflows estão ✅ em produção
- **Última atualização:** Março 2026
- **Próximos:** Workflows de Attendance (Fase 5) planejados
- **Manutenção:** Encaminhe dúvidas ao time de desenvolvimento

---

Para detalhes específicos de cada workflow, clique no link [Link] na coluna "Arquivo"
