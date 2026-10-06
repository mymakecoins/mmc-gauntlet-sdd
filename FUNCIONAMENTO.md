# MMC Gauntlet SDD: Funcionamento e Arquitetura

> **Lema:** O modelo propoe. O harness decide. O Gauntlet verifica. O log de eventos prova.

---

## 1. Introducao e Visao Geral

O **MMC Gauntlet SDD** e um framework de engenharia de software agentica de alta precisao que combina a disciplina do **Spec-Driven Development (SDD)**, o rigor de execucao do **Test-Driven Development (TDD)** e a capacidade de auto-correcao e auditoria do **Gauntlet Loop**.

### O Problema que Resolve
Em fluxos tradicionais de desenvolvimento autonomo com modelos de linguagem (LLMs), agentes frequentemente apresentam patologias operacionais graves:
1. **Deriva Conversacional (Drift):** Perda progressiva do escopo original apos multiplas interacoes.
2. **Declaracoes Prematuras de Sucesso:** Afirmacao de que o codigo "esta pronto e testado" sem qualquer execucao real no terminal ou logs comprovatorios.
3. **Cegueira do Autor (Author Bias):** O mesmo agente que escreve o codigo cria testes unitarios superficiais, cobrindo apenas o caminho feliz (*happy path*).
4. **Adulteracao de Contratos (Test Dilution / Anti-Cheat):** O agente enfraquece, remove ou comenta assercoes de testes existentes para mascarar regressoes e alcancar status verde artificial.
5. **Falta de Evidencias Visuais e Negociais:** Codigo entregue sem validacao visual de telas e sem explicacao compreensivel para o time de negocios e homologacao.

O **MMC Gauntlet SDD** neutraliza essas patologias atraves de uma maquina de estados deterministica com 7 Gates, segregacao estrita de papeis entre construtores e criticos adversariais, testes ponta a ponta (Playwright E2E) com capturas de tela autenticadas, e integracao nativa ao ciclo de vida de cards no GitHub Projects.

---

## 2. Arquitetura da Maquina de Estados (Os 7 Gates)

O ciclo de vida de qualquer demanda e governado por uma maquina de estados formal:

```
[ Gate 0: Stack Discovery & Board Sync ]
           | (Card movido para "In progress")
           v
[ Gate 1: Spec & Rubric Inception ] <----------------+
           |                                         | (Revisao da Spec)
           v                                         |
[ Gate 2: Spec Approval Gate ] ----------------------+
           | (Aprovado / Congelado)
           v
[ Gate 3: Task Atomization & Waves ]
           |
           v
+------------------------------------------------------------+
| GAUNTLET EXECUTION & REFINEMENT LOOP                       |
|                                                            |
|   [ Builders TDD ] ----> [ The Gauntlet (Critics Swarm) ]  |
|          ^                  - Adversarial Critic           |
|          |                  - Contract & Anti-Cheat QA     |
|          |                  - Static/Types & Security      |
|          |                  - Playwright E2E & Visual      |
|          |                               |                 |
|          +--- (Refactor / Falha) --------+ (100% Pass)     |
+------------------------------------------|-----------------+
                                           |
                                           v
                          [ Gate 4: Circuit Breakers Check ]
                                           |
                                           v
                          [ Gate 5: Evidence & Certification ]
                                           |
                                           v
                          [ Gate 6: Issue Comment & Hand-off ]
                                           |
                       (Decisao de Merge / PR Sob Demanda)
```

### Detalhamento dos Gates:

* **Gate 0 (Stack Profiling & Board Sync):**
  * Detecta runtime, gerenciadores de pacotes, linters, compiladores e suites de teste (`vitest`, `playwright`, `pytest`, `cargo`, etc.).
  * Valida se a arvore de trabalho Git esta limpa (`git status --porcelain`).
  * Se acionado a partir de uma issue ou card do GitHub Projects, sincroniza imediatamente o status para **`In progress`**.
* **Gate 1 (Spec & Gauntlet Rubric Inception):**
  * Gera o documento imutavel de especificacao em `docs/specs/YYYY-MM-DD-<topic>-spec.md`.
  * Define contexto, escopo (*In-Scope* vs *Non-Goals*), criterios de aceitacao mensuraveis e a **Gauntlet Rubric** (matriz com vetores adversariais obrigatorios, testes E2E e requisitos de analise estatica).
* **Gate 2 (Spec Approval Gate):**
  * Bloqueio deterministico para validar e congelar a especificacao antes de qualquer linha de codigo.
* **Gate 3 (Task Atomization & Execution Waves — `/mmc-gauntlet-sdd atomizer`):**
  * Decompoe a especificacao em tarefas atomicas isoladas em `docs/tasks/TASK-NNN.md` e gera o indice estruturado em ondas de execucao `docs/tasks/INDEX.md`.
  * Cada tarefa contem o guia TDD completo (RED/GREEN/REFACTOR) e rubrica com vetores adversariais dedicados.
* **Gate 4 (Circuit Breaker & Safety Control):**
  * Monitora o numero de ciclos do Gauntlet Loop. Limite maximo estrito de 5 iteracoes.
  * Dispara alerta e pausa se houver tentativa de adulteracao de testes, violacao de arquivos protegidos ou regressoes em modulos externos.
* **Gate 5 (Evidence & Certification):**
  * Consolida os logs literais de execucao no terminal (`Exit Code: 0`), o diff consolidado e a matriz com parecer unanime de aprovacao de todos os criticos.
* **Gate 6 (Issue Registration & Final Hand-off):**
  * Registra o comentario obrigatorio na issue com Nota para o Time de Negocios, Roteiro de Validacao, screenshots autenticadas e Resumo Tecnico.
  * Formula a pergunta de decisao sobre o destino do merge da branch.
  * Prepara o caminho para abertura de Pull Request sob demanda explicita do usuario.

---

## 3. O Swarm Multi-Agente e a Segregacao de Papeis

O sistema opera distribuindo tarefas entre agentes especializados com incentivos rigorosamente segregados:

```
                      +----------------------+
                      |     ORQUESTRADOR     |
                      |  (Lider do Processo) |
                      +----------+-----------+
                                 |
                 +---------------+---------------+
                 v                               v
       +------------------+            +------------------+
       |   BUILDER TDD    |            |   THE GAUNTLET   |
       | (Construtor Dev) |            | (Criticos Swarm) |
       +------------------+            +--------+---------+
                                                |
               +-------------------+------------+-----------+-------------------+
               v                   v                        v                   v
      +-----------------+ +-----------------+      +-----------------+ +-----------------+
      | Adversarial QA  | |   Contract QA   |      | Security/Static | |  E2E & Visual   |
      | (Stress/Bordas) | | (Audita Diffs)  |      | (Linters/Types) | |  (Playwright)   |
      +-----------------+ +-----------------+      +-----------------+ +-----------------+
```

### 1. Agente Orquestrador
* Controla as transicoes da maquina de estados e a contagem de iteracoes do Gauntlet.
* Sincroniza o status dos cards no GitHub Projects v2.
* Despacha tarefas para os Builders em ondas paralelas e aciona os Criticos apos cada ciclo.
* Compila o pacote final de certificacao e emite os relatorios sem emojis.

### 2. Builder Subagents (Construtores)
* Operam no ciclo estrito de **Red -> Green -> Refactor**.
* Escrevem o codigo minimo estritamente necessario para satisfazer o teste unitario/integracao.
* Permissao de escrita restrita exclusivamente aos arquivos da tarefa ativa.

### 3. The Gauntlet (Swarm de Criticos Adversariais)
Os criticos **nunca escrevem codigo de producao**; seu unico papel e buscar ativamente defeitos, quebras e inconformidades:
* **Adversarial QA Critic:** Submete o sistema a entradas nulas, vazias, malformadas, tipos anomalos, condicoes de concorrencia e latencia externa simulada.
* **Contract & Anti-Cheat QA Critic:** Audita o `git diff` contra a Spec formal. Reprova imediatamente caso detecte reducao de assercoes, comentarios em testes antigos ou violacao de assinaturas de interfaces publicas.
* **Static, Types & Security Critic:** Executa checagem estrita de tipos (`tsc`, `mypy`), linters (`eslint`, `ruff`) e analise de seguranca de dependencias.
* **E2E & Visual Critic (Playwright):** Executa testes ponta a ponta reais com gravacao automatica de capturas de tela em `tests/evidence/<issue-id>/`, garantindo a validacao das jornadas do usuario e interface.

---

## 4. Integracao entre SDD e TDD (O Casamento Macro vs. Micro)

No **MMC Gauntlet SDD**, o **SDD (Spec-Driven Development)** e o **TDD (Test-Driven Development)** atuam de forma simbiotica em niveis de abstracao complementares:

> **O SDD governa o nivel MACRO** *(o que construir, contratos, limites de escopo e portoes de qualidade)*.  
> **O TDD governa o nivel MICRO** *(a tecnica de codificacao modular e desacoplada pelos construtores)*.

```
+-----------------------------------------------------------------------------+
| SDD (Spec-Driven Development) — Nivel do Sistema e do Contrato              |
|                                                                             |
|  Define: Escopo, Criterios de Aceitacao (AC), Invariantes e Rubrica Gauntlet|
|  Controla: A Maquina de Estados (Gates 0 a 6) e o Board GitHub Projects     |
+--------------------------------------+--------------------------------------+
                                       |
                         (Decompoe em Tarefas Atomicas)
                                       |
                                       v
+-----------------------------------------------------------------------------+
| TDD (Test-Driven Development) — Nivel da Implementacao (Gate 4)             |
|                                                                             |
|  Para cada tarefa atomica por onda:                                         |
|   1. [RED]: Escreve o teste da tarefa e comprova a falha no terminal        |
|   2. [GREEN]: Escreve o codigo minimo indispensavel para passar o teste     |
|   3. [REFACTOR]: Limpa, otimiza e mantem 100% dos testes verdes             |
+--------------------------------------+--------------------------------------+
                                       |
                           (Entrega para o Gauntlet)
                                       |
                                       v
+-----------------------------------------------------------------------------+
| THE GAUNTLET — Prova de Fogo Adversarial & E2E (Gate 5)                     |
|                                                                             |
|  Subagentes Criticos testam se o codigo resiste a estresse, regressoes,     |
|  analise estatica e jornadas visuais ponta a ponta no Playwright            |
+-----------------------------------------------------------------------------+
```

### Dinamica da Integracao Passo a Passo:
1. **A Spec alimenta as Tarefas Atomicas:** No Gate 1, a Spec define os Criterios de Aceitacao (AC). No Gate 3, o comando `atomizer` converte esses criterios em tarefas atômicas isoladas em `docs/tasks/`.
2. **O Builder aplica o ciclo TDD:**
   * **[RED]:** O Builder implementa o teste direcionado e comprova a falha no terminal (`Exit Code: 1`).
   * **[GREEN]:** Escreve a implementacao minima necessria para passar o teste (`Exit Code: 0`).
   * **[REFACTOR]:** Refatora com tipagem estrita e sem introduzir acoplamentos.
3. **O Gauntlet audita a entrega global:**
   * O **Contract Critic** audita a conformidade total com a Spec.
   * O **Adversarial Critic** bombardeia o servico com casos limites.
   * O **E2E Visual Critic** executa a jornada no navegador real via Playwright gerando as evidencias visuais.

---

## 5. Ciclo de Vida do Board no GitHub Projects v2

O fluxo operacional e mapeado diretamente nas 9 colunas do board oficial:

1. **`Backlog`:** Atividades recem-criadas, liberadas apenas para refinamento inicial.
2. **`Refinement`:** Atividades sob analise de requisitos e arquitetura.
3. **`Ready`:** Atividades refinadas e aprovadas para atuacao do desenvolvedor.
4. **`In progress`:** Atividades em desenvolvimento ativo. Disparado automaticamente ao iniciar `/mmc-gauntlet-sdd <url-issue>`.
5. **`In review`:** Atividades com Pull Request aberto para a branch `develop`.
6. **`Await Staging`:** Atividades cujo PR foi integrado na `develop`, aguardando deploy em staging. As issues continuam abertas.
7. **`In Staging`:** Atividades publicadas no ambiente de homologacao (`staging`) para teste pelo time de QA/Homologacao.
8. **`Blocked`:** Atividades com qualquer tipo de impedimento operacional, tecnico ou de negocio.
9. **`Done`:** Atividades publicadas em Producao (`staging -> main`). Apenas nesta etapa as issues sao fechadas.

---

## 6. Governanca de GitFlow, Commits e Pull Requests

### 1. Nomenclatura de Branches
* Branch base para todo desenvolvimento: **`develop`**.
* Branches de feature/task: `marcos/<task-id>/<titulo-curto-kebab>` (exemplo: `marcos/45/validacao-cnpj`).
* Nunca realizar commits diretos em `main` ou `staging`.

### 2. Formato Padrao de Commit
* Estrutura: `<type>: #<task-id> <descricao em pt-BR>`
* Exemplo: `feat: #45 implementa validacao de unicidade cadastral`
* Tipos aceitos: `docs`, `feat`, `fix`, `perf`, `refact`, `style`, `test`, `chore`, `ci`.
* Cada tarefa atomica deve gerar seu commit correspondente e isolado.

### 3. Abertura de Pull Request (Estritamente Sob Demanda)
* **Regra Fundamental:** O agente NUNCA deve abrir Pull Request automaticamente ao encerrar uma issue ou tarefa. O PR so pode ser criado mediante solicitacao expressa do usuario.
* **Revisor Obrigatorio:** Tech Leader `@thiagobotelhorodoind` (`--add-reviewer thiagobotelhorodoind`).
* **Solicitante / Assignee:** `@marcos-rodoind` (`--add-assignee marcos-rodoind`).
* **Vinculacao ao Board:** Definir status do PR e das issues correspondentes como **`In review`**.
* **Referencias Nao-Destrutivas:** NUNCA utilizar `Closes #id` ou `Fixes #id` na descricao do PR, pois o merge em `develop` transiciona para `Await Staging` e nao encerra a issue. Utilizar exclusivamente `Ref #<task-id>` ou `Issue: #<task-id>`.
* **Evidencias Visuais Incorporadas:** O corpo do PR deve incluir tags HTML com imagens apontando para a branch de origem autenticada no GitHub:
  `<img src="https://github.com/TibaldiHolding/loggiq/blob/<branch-do-pr>/tests/evidence/<issue-id>/<arquivo>.png?raw=true" alt="<descricao>" width="100%" />`.

---

## 7. Comentario Obrigatorio de Encerramento na Issue

Imediatamente apos a conclusao com sucesso do Gauntlet (Gate 5 e 6), o agente deve publicar um comentario na issue correspondente com a seguinte estrutura padronizada:

1. **Nota para o Time de Negocios:** Explicacao em linguagem acessivel e nao tecnica detalhando o que foi implementado e o beneficio operacional.
2. **Passo a Passo de Validacao:** Roteiro claro para testes funcionais e homologacao pelo usuario.
3. **Evidencias Visuais com Tags HTML Autenticadas:** Screenshots do Playwright referenciadas atraves da URL de blob autenticado do repositorio privado:
   `<img src="https://github.com/TibaldiHolding/loggiq/blob/<branch>/tests/evidence/<issue-id>/<arquivo>.png?raw=true" alt="<descricao>" width="100%" />`.
   *(E terminantemente proibido o uso de URLs `raw.githubusercontent.com` em repositorios privados)*.
4. **Resumo Tecnico & Testes:** Detalhes da implementacao, arquivos alterados e status das suites unitarias, integracao e E2E.
5. **Texto Limpo em Markdown:** Proibicao absoluta de emojis, emoticons ou icones graficos no comentario.

---

## 8. Decisao de Merge ao Final do Desenvolvimento

Ao finalizar a implementacao e registrar o comentario na issue:
* Se o desenvolvimento ocorreu em uma branch especifica da issue (`marcos/<task-id>/...`), o agente **deve perguntar ao usuario para onde essa branch deve ser mergeada**.
* Se o desenvolvimento foi comitado diretamente na branch `develop`, o agente **deve informar explicitamente** que os commits estao consolidados na `develop`, sem questionar destino de merge.

---

## 9. Circuit Breakers e Travas Anti-Drift

1. **Max Iterations (5 Loops):** Se apos 5 rodadas o Gauntlet nao atingir 100% PASS, o harness interrompe o processo, congela o estado e executa diagnostico de causa-raiz.
2. **Spec Tamper Detection:** Qualquer modificacao em testes pre-existentes ou na Spec congelada sem aprovacao e bloqueada e revertida.
3. **Regression Hard-Stop:** Falhas em modulos alheios ao escopo da tarefa disparam rollback imediato.
4. **Validacao Visual Mandatoria:** Features com impacto de tela que nao gerem screenshots validas em `tests/evidence/<issue-id>/` sao reprovadas pelo E2E Visual Critic.
