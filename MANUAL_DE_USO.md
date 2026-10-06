# Manual de Uso: MMC Gauntlet SDD

Guia pratico e operacional para utilizacao da skill **MMC Gauntlet SDD** no desenvolvimento de software com garantia de qualidade, TDD, Playwright E2E e integracao nativa ao GitHub Projects v2.

---

## 1. Modos de Execucao

Voce pode operar o MMC Gauntlet SDD em dois regimes:

### Modo A: Autonomo Contínuo (Recomendado)
Ideal para tarefas completas, novas features, resolucao de issues ou refatoracoes. Ao receber uma URL de issue ou descricao de objetivo, o harness executa todo o fluxo:

```bash
# Iniciar a partir de uma issue do GitHub (move card para "In progress" no board):
/mmc-gauntlet-sdd auto https://github.com/TibaldiHolding/loggiq/issues/45

# Ou iniciar a partir de um objetivo em texto:
/mmc-gauntlet-sdd auto "Implementar validacao de unicidade cadastral com middleware de seguranca e suite Playwright"
```

### Modo B: Interativo / Passo a Passo
Ideal para quando voce deseja validar a especificacao manualmente, ajustar tarefas atomicas ou inspecionar cada portao de qualidade antes de prosseguir:

1. **Inicializar ambiente e sincronizar board:**
   ```bash
   /mmc-gauntlet-sdd init https://github.com/TibaldiHolding/loggiq/issues/45
   ```
2. **Gerar e congelar a especificacao:**
   ```bash
   /mmc-gauntlet-sdd spec "Validar unicidade de documento no cadastro de transportadoras"
   ```
3. **Atomizar em tarefas TDD e ondas de execucao:**
   ```bash
   /mmc-gauntlet-sdd atomizer docs/specs/2026-09-17-unicidade-documento-spec.md
   ```
4. **Iniciar implementacao com os Builders TDD:**
   ```bash
   /mmc-gauntlet-sdd start
   ```
5. **Acionar o Gauntlet e testes Playwright E2E:**
   ```bash
   /mmc-gauntlet-sdd test-gauntlet
   ```
6. **Certificar entrega e registrar comentario na issue:**
   ```bash
   /mmc-gauntlet-sdd done
   ```
7. **Abrir Pull Request (Somente quando solicitado):**
   ```bash
   /mmc-gauntlet-sdd pr
   ```

---

## 2. Referencia de Comandos

| Comando | Parametros | Descricao da Operacao |
| :--- | :--- | :--- |
| `/mmc-gauntlet-sdd auto` | `<objetivo\|url-issue>` | Executa o ciclo completo de ponta a ponta: move card para `In progress`, gera Spec, atomiza tarefas, roda Builders TDD, executa Gauntlet Loop e Playwright E2E, publica comentario na issue e pergunta destino de merge. |
| `/mmc-gauntlet-sdd init` | `[url-issue]` | Gate 0: analisa stack, verifica limpeza da arvore Git e sincroniza status do card no GitHub Projects para `In progress`. |
| `/mmc-gauntlet-sdd spec` | `<objetivo>` | Gate 1: gera `docs/specs/YYYY-MM-DD-<topic>-spec.md` com limites de escopo e Rubrica do Gauntlet. |
| `/mmc-gauntlet-sdd atomizer` | `[origem]` | Gate 3: decompõe a spec ou documento bruto em tarefas atomicas em `docs/tasks/TASK-NNN.md` e indice com ondas em `docs/tasks/INDEX.md`. |
| `/mmc-gauntlet-sdd start` | *(nenhum)* | Gate 4: inicia a execucao concorrente das tarefas atomicas pelos Builders em ciclo TDD (`Red -> Green -> Refactor`). |
| `/mmc-gauntlet-sdd test-gauntlet`| *(nenhum)* | Gate 5: aciona o swarm de criticos adversariais, auditoria de contratos, linters e a suite Playwright E2E. |
| `/mmc-gauntlet-sdd doctor` | *(nenhum)* | Diagnostica causas de falha no Gauntlet, quebras de contrato, flakiness ou regressões. |
| `/mmc-gauntlet-sdd status` | *(nenhum)* | Exibe o Gate ativo, coluna do board GitHub Projects, checklist de AC e parecer de cada critico. |
| `/mmc-gauntlet-sdd done` | *(nenhum)* | Gate 6: consolida evidencias literais, publica o comentario oficial na issue e formula a pergunta sobre o destino do merge. |
| `/mmc-gauntlet-sdd pr` | *(nenhum)* | **Abertura de PR Sob Demanda:** Executado somente quando o usuario pedir. Cria PR para `develop`, adiciona revisor `@thiagobotelhorodoind`, assignee `@marcos-rodoind`, vincula ao board em `In review` e anexa tags HTML de imagens. |

---

## 3. Passo a Passo de um Ciclo Completo de Desenvolvimento

### Cenario Real: Resolucao da Issue #45 (Validacao de Unicidade Cadastral)

#### Passo 1: Invocacao do Gauntlet
O usuario envia no chat:
```text
/mmc-gauntlet-sdd auto https://github.com/TibaldiHolding/loggiq/issues/45
```

#### Passo 2: Sincronizacao do Board e Preparacao da Branch (Gate 0)
* O orquestrador detecta a URL da issue #45.
* O card e a issue correspondentes sao movidos imediatamente para **`In progress`** no GitHub Projects.
* E criada a branch de trabalho a partir da `develop`: `marcos/45/validacao-unicidade-cadastral`.

#### Passo 3: Criacao da Spec e Rubrica Gauntlet (Gates 1 e 2)
O harness gera o arquivo `docs/specs/2026-09-17-issue-45-unicidade-cadastral-spec.md`:
* **Problem:** Operadores estavam cadastrando registros duplicados por falta de validacao previa na camada de servico.
* **Outcome:** Rejeicao automatica com erro tipado 409 Conflict e feedback visual imediato na interface.
* **Gauntlet Rubric:**
  * Teste unitario da constraint de unicidade.
  * Vetores adversariais: chamadas concorrentes com mesmo payload, variacoes de formatacao com pontuacao e espacos extras.
  * Teste Playwright E2E garantindo exibicao do alerta de erro no frontend e gravacao de screenshots em `tests/evidence/issue-45/`.

#### Passo 4: Decomposicao Atomica (Gate 3 - Atomizer)
O comando `/mmc-gauntlet-sdd atomizer` gera:
* `docs/tasks/INDEX.md` estruturado em ondas de despacho.
* `docs/tasks/TASK-001.md`: Regra de negocio no servico e teste de unicidade (Onda 1).
* `docs/tasks/TASK-002.md`: Controller HTTP e tratamento de status 409 (Onda 2).
* `docs/tasks/TASK-003.md`: Suite Playwright E2E e capturas de tela (Onda 3).

#### Passo 5: Implementacao TDD pelos Builders (Gate 4)
* **Builder 1:** Escreve teste unitario que falha [RED] -> Implementa validacao [GREEN] -> Refatora tipagem [REFACTOR].
* Cada etapa e comitada isoladamente: `feat: #45 implementa validacao de unicidade na camada de servico`.

#### Passo 6: Prova de Fogo (The Gauntlet - Gate 5)
* **Adversarial QA Critic:** Simula disparos paralelos para verificar se ocorrem race conditions.
* **Contract QA Critic:** Audita diffs e confirma que nenhum teste pre-existente foi enfraquecido.
* **Static Critic:** Confirma `npm run lint` e `npm run typecheck` com 0 erros.
* **E2E Visual Critic:** Executa `npm run test:e2e` salvando screenshots comprovatorias em `tests/evidence/issue-45/`.
* Todos os criticos reportam `PASS`.

#### Passo 7: Certificacao e Comentario Obrigatorio na Issue (Gate 6)
O agente publica o comentario oficial na Issue #45 contendo:
* **Nota para o Time de Negocios** em linguagem clara e acessivel.
* **Passo a Passo de Validacao** para os homologadores.
* **Capturas de Tela Incorporadas** via tags HTML apontando para o blob autenticado do GitHub:
  `<img src="https://github.com/TibaldiHolding/loggiq/blob/marcos/45/validacao-unicidade-cadastral/tests/evidence/issue-45/01-alerta-duplicidade.png?raw=true" alt="Alerta de duplicidade validado" width="100%" />`
* **Resumo Tecnico e Matriz de Testes**.

#### Passo 8: Pergunta de Destino de Merge
Ao concluir, o agente pergunta ao desenvolvedor:
> "O desenvolvimento da Issue #45 foi concluido com 100% de aprovacao no Gauntlet e Playwright. Para onde voce gostaria que a branch `marcos/45/validacao-unicidade-cadastral` seja mergeada?"

#### Passo 9: Abertura de Pull Request (Sob Demanda)
Quando o usuario solicitar a abertura de PR:
* O PR e criado apontando da branch da feature para a **`develop`**.
* O Tech Leader `@thiagobotelhorodoind` e adicionado como revisor.
* `@marcos-rodoind` e adicionado como assignee.
* O card e a issue sao movidos para **`In review`** no GitHub Projects.
* A descricao do PR inclui o resumo das mudancas, referencias informativas `Ref #45` (sem `Closes #45`) e as imagens de evidencia visual em tags HTML.

---

## 4. Regras Fundamentais de Conformidade

1. **Proibicao Absoluta de Emojis:** Commits, issues, comentarios, PRs e relatorios nunca devem conter emojis, emoticons ou icones graficos. Utilize exclusivamente texto puro e Markdown padrao.
2. **Imagens Autenticadas:** Nunca utilize URLs do tipo `raw.githubusercontent.com` em repositorios privados, pois elas quebram a visualizacao para revisores. Utilize sempre o caminho blob autenticado do GitHub com parametro `?raw=true`.
3. **Nao Fechar Issues no PR:** Jamais utilize palavras-chave de fechamento automatico (`Closes #id`, `Fixes #id`) na descricao de PRs. As issues permanecem abertas ate a publicacao final em producao (`Done`).

---

## 5. Diagnostico e Solucao de Problemas (Troubleshooting)

### O que fazer se o Gauntlet falhar no Adversarial Critic?
1. Execute `/mmc-gauntlet-sdd doctor` para obter a analise da falha e o payload que causou o erro.
2. O Builder ajustara a validacao de entrada ou tratamento de excecao para neutralizar o vetor adversarial.
3. Reexecute `/mmc-gauntlet-sdd test-gauntlet`.

### O que fazer se a suite Playwright falhar?
1. Verifique se a aplicacao e o backend estao em execucao no ambiente de testes.
2. Inspecione o trace do Playwright gerado em `test-results/`.
3. Assegure que os seletores de elementos e as condicoes de espera estejam alinhados com o DOM renderizado.

### O que fazer se o Circuit Breaker (5 ciclos) disparar?
O harness pausa e emite um relatorio detalhado. Avalie se ha incompatibilidade de contratos ou se a especificacao original em `docs/specs/` precisa de revisao. Ajuste a spec e reative via `/mmc-gauntlet-sdd start`.
