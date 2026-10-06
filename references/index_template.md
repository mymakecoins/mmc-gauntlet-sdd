# Indice de Tarefas Atomizadas — [Nome do Objetivo / Feature]

**Spec de Origem:** [`docs/specs/YYYY-MM-DD-<topic>-spec.md` | Documento de Requisitos]  
**Data de Geracao:** [YYYY-MM-DD]  
**Total de Tarefas:** [N]  
**Total de Ondas de Despacho:** [M]  

---

## 1. Tabela Geral de Tarefas

| ID | Titulo | Camada | Dependencias | Alvo de Teste (TDD) | Rastreabilidade |
|:---|:---|:---|:---|:---|:---|
| TASK-001 | [Titulo curto e acionavel] | Banco/Migracao | Nenhuma | `tests/migrations/...` | RF-001 |
| TASK-002 | [Titulo curto e acionavel] | Dominio/Tipos | TASK-001 | `tests/domain/...` | RF-001, RN-001 |
| TASK-003 | [Titulo curto e acionavel] | Servico/Aplicacao | TASK-002 | `tests/services/...` | RF-002 |
| TASK-004 | [Titulo curto e acionavel] | API/Controller | TASK-003 | `tests/api/...` | RF-002, RN-002 |
| TASK-005 | [Titulo curto e acionavel] | E2E/Validacao Visual | TASK-004 | `loggiq/tests/e2e/...` | RF-002, RN-003 |

---

## 2. Ordem de Despacho Multi-Agente (Ondas de Execucao)

> **Regra de Paralelismo:** Tarefas dentro da mesma Onda sao independentes e podem ser despachadas para **Builder Subagents em paralelo**. Cada Onda aguarda a conclusao e aprovacao da Onda anterior.

### Onda 1 — Fundacao, Migracoes & Contratos Basicos
*Tarefas de setup, infraestrutura de banco e definicoes de contratos/tipos fundamentais.*
- **TASK-001:** [Titulo da Tarefa] (`Banco/Migracao`)
- **TASK-002:** [Titulo da Tarefa] (`Dominio/Tipos`)

### Onda 2 — Servicos de Dominio & Regras de Negocio
*Executam apos a Onda 1 estar consolidada.*
- **TASK-003:** [Titulo da Tarefa] (`Servico/Aplicacao`)
- **TASK-004:** [Titulo da Tarefa] (`Servico/Aplicacao`)

### Onda 3 — Controllers, APIs & Integracoes Externas
*Executam apos servicos estarem implementados.*
- **TASK-005:** [Titulo da Tarefa] (`API/Controller`)

### Onda 4 — Suite Playwright E2E, UI & Evidencias Visuais
*Validacao ponta a ponta e submissao ao Gauntlet Swarm completo com capturas de tela.*
- **TASK-006:** [Titulo da Tarefa] (`Integracao/E2E`)

---

## 3. Matriz de Cobertura de Requisitos & Gauntlet

| Requisito / AC da Spec | Tarefas Cobertas | Vetores Adversariais Mapeados |
|:---|:---|:---|
| AC-1: [Descricao do AC] | TASK-001, TASK-003 | Inputs nulos, tipos invalidos, payload truncado |
| AC-2: [Descricao do AC] | TASK-004, TASK-005 | Concorrencia, timeout simulado, retries |
| AC-3: [Descricao do AC] | TASK-006 | Fluxo completo ponta a ponta com Playwright e screenshots |
