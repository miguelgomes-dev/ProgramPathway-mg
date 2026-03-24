# FASE 4: Markers (WF 301-307)

## 📋 Visão Geral

A **Fase 4** é **idêntica em padrão à Fase 3** (Coaches), mas focada em **Markers** (avaliadores). Ambas rodam **em paralelo** após a Fase 1 ser concluída.

## 🎯 Objetivo

- ✅ Contactar markers disponíveis
- ✅ Confirmar que têm capacidade para avaliar este cohort
- ✅ Enviá-los contratos
- ✅ Garantir assinatura via Adobe Sign

## 📊 Workflows da Fase 4

| ID | Nome | Trigger | Status |
|----|------|---------|--------|
| 301a | Send Capacity Confirmation | Recurrence (~4:30 AM) | "Marker Capacity Pending" |
| 301b | Handle Rejected Invitations | Event | - |
| 302 | Wait for Confirmation | Recurrence (~4:30 AM) | - |
| 303 | Check Confirmed | Recurrence (5:00 AM) | "Marker Capacity Confirmed" |
| 304 | Send Contracts | Recurrence (5:30 AM) | "Sending Contracts" |
| 305 | Send via Adobe Sign | Auto | - |
| 306 | Check Adobe Agreement | Recurrence Daily | - |
| 307 | Check Contracts Confirmed | Recurrence Daily | "Markers Confirmed" |

## ⏰ Timeline Típica

```
APÓS FASE 1 COMPLETA (dia X):
(Em paralelo com Fase 3 - Coaches)

DIA X + 1, ~4:30 AM:
  - WF 301a envia convites para markers
  - WF 302 aguarda confirmação

DIAS X+1 a X+3:
  - Markers respondem confirmando disponibilidade
  - WF 303 verifica se TODOS confirmaram

DIA X+3, 5:30 AM:
  - WF 304 envia contratos
  - WF 305-306 gerencia Adobe Sign

DIAS X+4 a X+7:
  - Markers assinam contratos
  - WF 307 confirma conclusão

RESULTADO: Todos markers prontos
```

## 🔄 Fluxo Detalhado

```
[APÓS FASE 1 CONCLUÍDA]
         ↓
    Status = "All Trainers Contracts Confirmed"
         ↓
    ┌─────────────────────────────┐
    │ FASE 3 (Coaches)            │ (Paralelo)
    │ WF 201a-207                 │ (Paralelo)
    │                             │ (Paralelo)
    └─────────────────────────────┘
         ↓
WF 301a (Recurrence Daily ~4:30 AM)
  "Enviar convites para markers"
  Email: "Precisamos sua confirmação de capacidade de avaliação"
         ↓
    Status = "Marker Capacity Pending"
         ↓
┌─ WF 301b (Event-based)
│  "Se Marker rejeitou"
│  → Re-enviar convite
│
WF 302 (Recurrence Daily ~4:30 AM)
  "Aguardar confirmação de capacidade"
         ↓
WF 303 (Recurrence Daily 5:00 AM)
  "Verificar se TODOS markers confirmaram"
  Loop até ALL confirmarem
         ↓
    Status = "Marker Capacity Confirmed"
         ↓
WF 304-307 (Recurrence 5:30 AM+)
  "Enviar contratos"
  "Adobe Sign process"
  "Verificar assinaturas"
         ↓
    Status = "Markers Contracts Confirmed"
         ↓
    ✅ FASE 4 COMPLETA
```

## 🎓 Detalhes Importantes

### O que é um Marker?
**Marker** = Avaliador/Corretor
- Responsável por avaliar desempenho dos alunos
- Fornece feedback
- Calcula notas/scores
- Essencial para conclusão do programa

### Confirmação de Capacidade (WF 301a-303)
- Marker precisa confirmar que tem TEMPO e RECURSOS para avaliar
- Não é apenas "estar presente"
- É ter capacidade educacional para dizer se aluno aprendeu ou não

### Contratos de Markers (WF 304-307)
- Mesmo padrão que Coaches e Trainers
- Adobe Sign para assinatura
- Documentação legal
- Rastreamento completo

## ⚠️ Pontos Críticos

| Ponto Crítico | O que fazemos |
|---|---|
| Marker não responde | Aguarda 2 dias, re-envia, aguarda mais 2 |
| Marker rejeita | WF 301b re-envia, até 3 tentativas |
| Muitos markers rejeitam | Manual: procurar substitutos |
| Diferença horária | Emails enviados madrugada, resposta até próxima noite |

## 🔁 Execução Paralela com Fase 3

```
MOMENTO FASE 1 TERMINA:
         ↓
    ┌──────────────────────┬──────────────────────┐
    │                      │                      │
    │ WF 201a (Coaches)    ↓ WF 301a (Markers)   │
    │ 3:45 AM              │ ~4:30 AM            │
    │                      │                     │
    └──────────────────────┴──────────────────────┘
    
AMBOS rodam SIMULTANEAMENTE
Nenhum espera o outro
Nenhum depende do outro
Horários ligeiramente diferentes para não sobrecarregar sistema
```

## 🔄 Sincronização Entre Fases

**Importante:** Fase 3 e Fase 4 rodam **independentemente**:
- ✅ Coaches podem estar confirmados enquanto Markers ainda confirmam
- ✅ Markers podem estar assinando contratos enquanto Coaches já estão prontos
- ✅ Sistema NÃO aguarda sincronização

**Próxima Fase só começa quando AMBOS estão prontos:**
```
Fase 3 Completa ✅
Fase 4 Completa ✅
         ↓
Fase 5 (Attendance) pode começar
```

## 🚀 Saída da Fase 4

Quando a Fase 4 termina:
- ✅ TODOS markers confirmados
- ✅ TODOS contratos assinados
- ✅ Sistema pronto para Attendance

## 📊 Comparativo: Coaches vs Markers

| Aspecto | Coaches | Markers |
|--------|---------|---------|
| Responsabilidade | Mentoria/Suporte | Avaliação/Feedback |
| Fase | 3 | 4 |
| Horários | 3:45-4:30 AM | ~4:30-5:30 AM |
| Processo | Idêntico | Idêntico |
| Dependência | Nenhuma | Nenhuma (paralelo) |

## 🔗 Relacionamentos

- **Paralela com:** Fase 3 (Coaches)
- **Depende de:** Fase 1 (Trainers)
- **Resultado:** Preparação de Markers para Attendance

## 📚 Comparação de Padrões

Veja **Fase 3 (Coaches)** - o padrão é exatamente o mesmo, alterando apenas:
- IDs dos workflows (201→301)
- Horários (3:45→~4:30)
- Tipo de ator (Coach→Marker)
- Campo de status (Coach_Capacity→Marker_Capacity)

---

**Status:** ✅ Documentado | **Atualizado:** Março 2026
