# 📖 Glossário de Termos

## Entidades Principais

### Cohort (Turma/Turma)
**Definição:** A unidade central do sistema - representa uma turma/grupo de alunos que seguem um programa de treinamento específico com cronograma definido.

**Atributos principais:**
- ID único
- Programa (qual programa será ministrado)
- Data de Início
- Status (varia de 0 a 33+)
- Cronograma (schedule)
- Alunos associados

**Ciclo de vida:** Criação → Setup → Trainers → Coaches → Markers → Attendance → Encerramento

**Exemplo:** "Cohort Q1-2026: Python Avançado, começando 15 de Abril"

---

### Program (Programa)
**Definição:** Modelo educacional que define quais masterclasses serão ministradas e em qual sequência.

**Atributos principais:**
- Nome do programa
- Lista de masterclasses
- Duração total
- Objetivos de aprendizado

**Relação:** Um Cohort é baseado em um Program específico

**Exemplo:** "Python Avançado 2026", "Full Stack Web Development"

---

### Masterclass
**Definição:** Aula/módulo individual que faz parte de um programa.

**Atributos principais:**
- Tema/título
- Instrutor (Trainer) responsável
- Duração
- Pré-requisitos (se houver)

**Relação:** Múltiplas masterclasses compõem um Program, e todas precisam estar agendadas em um Cohort

**Exemplo:** "Módulo 1: Fundamentos de Python", "Módulo 5: Testes e Debugging"

---

### Schedule (Agenda)
**Definição:** Cronograma que define QUANDO cada masterclass será ministrada dentro de um cohort.

**Atributos principais:**
- Datas e horários das aulas
- Trainer responsável por cada data
- Sala/equipe do Teams
- Status de confirmação

**Criação:** Automática pelo WF 102, baseado em:
- Disponibilidade dos trainers
- Estrutura do programa
- Início do cohort

**Importante:** A Schedule deve ser confirmada por TODOS os trainers antes de prosseguir

---

## Atores do Sistema

### Trainer (Instrutor)
**Definição:** Profissional responsável por ministrar as masterclasses de um cohort.

**Responsabilidades:**
- Confirmar sua disponibilidade para as datas propostas
- Assinar contrato
- Ministrar as aulas conforme agendado

**Comunicações:**
- Recebe convites via Email/Teams
- Acessa SharePoint para detalhes
- Assina contratos via Adobe Sign

**Status durante processo:**
- "Inviting Trainers" (convirtando)
- "All Trainers Dates Confirmed" (confirmou disponibilidade)
- "Sending Out Trainers Contracts" (recebendo contrato)
- "All Trainers Contracts Confirmed" (contrato assinado)

**Workflows associados:** WF 102-110

---

### Coach (Facilitador)
**Definição:** Profissional que fornece suporte, mentoria e facilita o aprendizado dos alunos durante o cohort.

**Responsabilidades:**
- Confirmar sua capacidade de atendimento
- Assinar contrato
- Acompanhar e orientar alunos

**Comunicações:**
- Recebe convites de "Capacity Confirmation"
- Acessa documentação no SharePoint
- Assina contratos via Adobe Sign

**Status durante processo:**
- "Coach Capacity Pending" (aguardando resposta)
- "Coach Capacity Confirmed" (confirmou disponibilidade)
- "Coaches Contracts Confirmed" (contrato assinado)

**Workflows associados:** WF 201-207

---

### Marker (Avaliador)
**Definição:** Profissional responsável por avaliar, corrigir e fornecer feedback sobre o desempenho dos alunos.

**Responsabilidades:**
- Confirmar sua disponibilidade
- Assinar contrato
- Realizar avaliações conforme cronograma

**Comunicações:**
- Recebe convites de "Capacity Confirmation"
- Acessa tarefas de avaliação no SharePoint
- Assina contratos via Adobe Sign

**Status durante processo:**
- "Marker Capacity Pending" (aguardando resposta)
- "Marker Capacity Confirmed" (confirmou)
- "Markers Contracts Confirmed" (contrato assinado)

**Workflows associados:** WF 301-307

---

### Aluno (Student)
**Definição:** Participante que está cursando o programa dentro de um cohort específico.

**Atributos principais:**
- ID/CPF
- Nome
- Email
- Cohort que frequenta
- Progresso/Notas

**Comunicações:**
- Recebe informações sobre cronograma
- Acesso aos materiais via Teams/SharePoint
- Participa das masterclasses

**Nota:** Organização de alunos por coaches/markers é feita automaticamente pelo sistema

---

## Conceitos Operacionais

### Availability/Disponibilidade
**Definição:** A capacidade de um trainer/coach/marker estar presente nas datas propostas.

**Processo:**
1. Sistema propõe datas baseado no programa
2. Ator confirma sua disponibilidade (sim/não)
3. Se "não": workflow de rejeição é acionado (WF 102b, 201b, 301b)
4. Se "sim": progresso para próxima fase

**Impacto:** Afeta todo o cronograma do cohort

---

### Capacity Confirmation
**Definição:** Confirmação de que um coach/marker TEM CAPACIDADE (recursos, tempo) para atender o cohort.

**Diferença de "Availability":**
- Availability = Presença nas datas específicas
- Capacity = Recursos gerais para fazer o trabalho

**Workflows:** WF 201a (Coaches), WF 301a (Markers)

---

### Contract (Contrato)
**Definição:** Documento legal que formaliza a relação entre a empresa e o trainer/coach/marker.

**Características:**
- Gerado automaticamente via Word template
- Enviado via email com link de assinatura
- Assinado digitalmente via **Adobe Sign**
- Armazenado no SharePoint
- Cria trilha de auditoria completa

**Fluxo:**
1. WF 105/204/304 = Envia contrato
2. WF 106/205/305 = Integra Adobe Sign
3. WF 107/206/306 = Verifica assinatura
4. WF 108/207/307 = Confirma conclusão

---

### Adobe Sign Signature
**Definição:** Assinatura digital de documentos usando Adobe Sign.

**Benefícios:**
- ✅ Legal e válida
- ✅ Rastreamento de quando foi assinado
- ✅ Sem necessidade de imprimir/escanear
- ✅ Integração automática com Power Automate

**Fluxo no sistema:**
1. Contrato é enviado ao ator
2. Ator recebe email com link do Adobe Sign
3. Ator assina digitalmente
4. Webhook notifica Power Automate
5. Sistema verifica conclusão

---

### Online Event / Teams Meeting
**Definição:** Reunião/evento criado automaticamente no Microsoft Teams para cada masterclass.

**Criação:** WF 109 (automático)

**Propósito:**
- Local centralizado para aulas
- Gravação automática
- Facilita comunicação aluno-trainer
- Compartilhamento de tela/documentos

**Integração:** Teams ↔ SharePoint ↔ Schedules

---

## Status & Ciclos

### Status Codes (Cohort)
```
0 - Pending
1 - Preprocessing Data Validation
2 - Creating Cohort Schedule
3 - Inviting Trainers
4 - All Trainers Dates Confirmed
5 - Sending Out Trainers Contracts
6 - All Trainers Contracts Confirmed
8 - All Online Events Created
10 - All Cohorts Units Created
20-23 - Coach related statuses
30-33 - Marker related statuses
40+ - Attendance & Beyond
```

### Validações (Lógica de Prevenção de Erros)

**Validação de Dados:**
- Datas futuras
- Emails válidos
- Documentos completos
- Rejeições tratadas

**Validação de Usuário:**
- Confirma atividade (não deixa default)
- Rejeições disparam re-invites
- Timeouts iniciam escalonamento

---

## Comunicações & Notificações

### Email
**Usado para:**
- Convites iniciais
- Envio de contratos
- Confirmações
- Lembretes

**Tecnologia:** Office 365 Connector

---

### Teams Notifications
**Usado para:**
- Notificações de eventos
- Criação de canais/reuniões
- Comunicação em tempo real

**Tecnologia:** Teams Connector

---

### SharePoint
**Usado para:**
- Armazenamento central de dados
- Documentos (contratos, materiais)
- Acesso sem autenticação duplicada

**Permissões:** Controladas por grupo/função

---

## Abreviações & Siglas

| Sigla | Significado |
|-------|------------|
| WF | Workflow (fluxo de automação) |
| PR | Programme Pathway (nome do projeto) |
| API | Interface de Programação Application |
| PDF | Formato de documento portável |
| CSV | Valores separados por vírgula |
| GUID | Identificador único global |
| UTC | Tempo Universal Coordenado |
| Adobe Sign | Serviço de assinatura digital |

---

## Referências Rápidas

- **Fases**: Setup (WF 101-110) → Trainers → Coaches (paralelo) + Markers (paralelo)
- **Triggers**: Criação de Cohort (WF 101) → Automático conforme estados
- **Banco de Dados**: SharePoint Online (listas + bibliotecas)
- **Comunicação**: Email + Teams + Adobe Sign

Para fluxos detalhados, consulte [ARQUITETURA.md](ARQUITETURA.md)
