# [TASK-NNN]: [Titulo curto e acionavel, iniciando com verbo no infinitivo]

> **Camada:** [Banco/Migracao | Dominio/Tipos | Servico/Aplicacao | API/Controller | Integracao/E2E | UI/Frontend | Setup/Infra]
> **Estimativa:** [P | M | G]
> **Status:** Pendente
> **Spec Origem:** [`docs/specs/YYYY-MM-DD-<topic>-spec.md` | Documento de Requisitos]

---

## 1. Contexto & Motivacao
[Por que esta tarefa existe. O suficiente para que o Builder Subagent entenda o proposito sem precisar reler o documento de requisitos completo. Cite a regra de negocio ou a motivacao tecnica.]

## 2. Escopo Atomico
[O que esta tarefa faz — e apenas isso. Uma responsabilidade bem definida e isolada.]

### Fora do Escopo
- [O que NAO faz parte desta tarefa. Aponte para a TASK que cobre, se houver.]

## 3. Dependencias & Bloqueios
- **Depende de:** [TASK-NNN] | Nenhuma
- **Bloqueia:** [TASK-NNN] | Nenhuma

## 4. Premissas & Invariantes
- [Premissas assumidas ou interpretacoes de requisitos ambiguos.]
- **Invariante:** Nao modificar testes pre-existentes de outros modulos; manter compatibilidade estrita de contratos.
- **Invariante:** Nao utilizar emojis ou icones graficos em mensagens de commit, codigo ou documentacao.

## 5. Criterios de Aceite (Acceptance Criteria)
- [ ] **Dado** [contexto inicial], **quando** [acao/evento], **entao** [resultado esperado observavel]
- [ ] [Criterio adicional verificavel em formato checklist ou Gherkin]

## 6. Guia TDD para o Builder (Red -> Green -> Refactor)
- **Alvo do Teste Primeiro (RED):** `tests/...` ou `loggiq/tests/e2e/...`
- **Comando de Teste:** `npm test -- ...` ou `npm run test:e2e`
- **Comportamento Minimo (GREEN):** Arquivo(s) a criar/modificar em `src/...`.
- **Foco do Refactor:** Limpeza de codigo, tipagem estrita e zero warnings sem quebrar a suite de testes.
- **Evidencias Visuais (se aplicavel):** Em tarefas E2E ou de interface, salvar capturas de tela comprobatorias em `tests/evidence/<issue-id>/`.

## 7. Rubrica Adversarial da Tarefa (The Gauntlet Vectors)
*Vetores que o Adversarial Critic ira cobrar especificamente nesta tarefa:*
- [ ] **Boundary & Edge Cases:** [ex: valores limitrofes, strings vazias, numeros negativos, payloads truncados]
- [ ] **Validacao & Erros:** [ex: lancamento de excecoes tipadas, status HTTP 400/409/422 correto, mensagens claras]
- [ ] **Concorrencia & Idempotencia:** [ex: idempotencia de chamadas, controle de concorrencia ou locking se aplicavel]

## 8. Notas Tecnicas & Arquivos Afetados
- **Arquivos esperados:** `src/...`, `tests/...`
- **Padroes do projeto:** [ex: schemas Zod, TypeScript estrito, sem emojis]

## 9. Rastreabilidade
- **Requisito(s) Origem:** [RF-001 | RN-003 | AC-1 da Spec]
