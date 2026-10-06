# MMC Gauntlet SDD

Framework de desenvolvimento orientado por especificação que combina SDD, TDD, execução multiagente, crítica adversarial e evidências verificáveis.

O MMC Gauntlet SDD organiza o trabalho em sete gates. Cada mudança começa por uma especificação, é decomposta em tarefas atômicas, implementada por construtores independentes e submetida a críticos especializados antes da certificação.

![Jornada do MMC Gauntlet SDD](./Jornada_do_MMC-Gauntlet_SDD.png)

## O que a skill oferece

- Especificação formal antes de alterações no código.
- Critérios de aceite e rubrica adversarial por objetivo.
- Decomposição em tarefas atômicas e ondas de paralelismo.
- Builders independentes trabalhando com Red, Green e Refactor.
- Críticos de casos extremos, contratos, análise estática, segurança e E2E.
- Circuit breaker após cinco ciclos sem aprovação.
- Evidências literais de testes, diffs e comandos executados.
- Integração com o ciclo de vida do GitHub Projects.
- Templates para tarefas, comentários de issues e Pull Requests.

## Fluxo de execução

1. **Descoberta:** identifica stack, ferramentas, estado do Git e item de trabalho.
2. **Especificação:** registra objetivo, escopo, critérios de aceite e rubrica do Gauntlet.
3. **Aprovação:** congela a especificação antes da implementação.
4. **Atomização:** divide o trabalho em tarefas pequenas, rastreáveis e ordenadas por dependência.
5. **Construção:** builders implementam cada tarefa com TDD.
6. **Gauntlet:** críticos independentes procuram falhas e devolvem lacunas aos builders.
7. **Certificação:** consolida evidências, atualiza a issue e prepara o hand-off.

Uma entrega só é certificada quando todos os críticos aprovam, os testes exigidos terminam com sucesso e as evidências correspondem ao código real.

## Instalação

Clone o repositório no diretório de skills compartilhado pelo seu agente:

```bash
git clone git@github.com:mymakecoins/mmc-gauntlet-sdd.git ~/.agents/skills/mmc-gauntlet-sdd
```

Reinicie ou recarregue a sessão do agente para que a skill seja descoberta.

## Uso rápido

Para executar o ciclo completo a partir de um objetivo:

```text
/mmc-gauntlet-sdd auto "Implementar a funcionalidade descrita na especificação"
```

Para iniciar a partir de uma issue:

```text
/mmc-gauntlet-sdd auto https://github.com/organizacao/repositorio/issues/123
```

O modo autônomo cobre descoberta, especificação, atomização, builders, críticos, testes, certificação e hand-off. O fluxo também pode ser executado gate a gate.

## Comandos

| Comando | Finalidade |
| --- | --- |
| `/mmc-gauntlet-sdd auto <objetivo\|url-issue>` | Executa o ciclo completo. |
| `/mmc-gauntlet-sdd init [url-issue]` | Descobre a stack, verifica o Git e sincroniza o board. |
| `/mmc-gauntlet-sdd spec <objetivo>` | Cria a especificação e a rubrica adversarial. |
| `/mmc-gauntlet-sdd atomizer [origem]` | Decompõe requisitos em tarefas atômicas e ondas. |
| `/mmc-gauntlet-sdd start` | Inicia os builders em ciclos TDD. |
| `/mmc-gauntlet-sdd test-gauntlet` | Executa os críticos e as validações do projeto. |
| `/mmc-gauntlet-sdd doctor` | Diagnostica falhas e bloqueios do Gauntlet. |
| `/mmc-gauntlet-sdd status` | Exibe gate, critérios e pareceres atuais. |
| `/mmc-gauntlet-sdd done` | Consolida as evidências e encerra o hand-off. |
| `/mmc-gauntlet-sdd pr` | Abre um Pull Request quando solicitado explicitamente. |

## Requisitos de uso

A skill adapta os comandos de validação à stack encontrada no projeto. Para o fluxo completo, o ambiente deve oferecer:

- Git e uma árvore de trabalho acessível.
- Ferramentas de build, teste, lint e análise estática do projeto.
- Playwright quando houver validação E2E ou visual.
- Acesso ao GitHub quando a tarefa envolver issues, Pull Requests ou Projects.
- Suporte do agente a subagentes independentes para builders e críticos.

Os comandos mostrados nos documentos são contratos operacionais. As ferramentas efetivamente executadas dependem do repositório consumidor.

## Artefatos gerados no projeto consumidor

```text
docs/
├── specs/
│   └── YYYY-MM-DD-<topico>-spec.md
└── tasks/
    ├── INDEX.md
    └── TASK-NNN.md

tests/
└── evidence/
    └── <issue-id>/
```

As especificações registram o contrato do objetivo. As tarefas mantêm dependências, critérios de aceite, alvos de teste e rastreabilidade. Evidências visuais ficam associadas à issue correspondente.

## Estrutura do repositório

| Arquivo | Conteúdo |
| --- | --- |
| [`SKILL.md`](./SKILL.md) | Regras, gates, invariantes e contratos da skill. |
| [`MANUAL_DE_USO.md`](./MANUAL_DE_USO.md) | Guia operacional e exemplos completos. |
| [`FUNCIONAMENTO.md`](./FUNCIONAMENTO.md) | Arquitetura, papéis e fluxo interno. |
| [`references/task_template.md`](./references/task_template.md) | Template de tarefa atômica. |
| [`references/index_template.md`](./references/index_template.md) | Índice de tarefas e ondas. |
| [`references/example_task.md`](./references/example_task.md) | Exemplo preenchido de tarefa. |
| [`references/issue_comment_template.md`](./references/issue_comment_template.md) | Comentário de certificação para issues. |
| [`references/pr_template.md`](./references/pr_template.md) | Descrição de Pull Request sob demanda. |

## Princípios centrais

- Especificação antes de código.
- Evidência antes de afirmação.
- Testes existentes não podem ser enfraquecidos para obter aprovação.
- Builders e críticos devem permanecer independentes.
- Pull Requests só são abertos sob solicitação explícita.
- O circuito para e solicita intervenção quando o limite de iterações é atingido.

Consulte o [manual de uso](./MANUAL_DE_USO.md) para o fluxo passo a passo e o [documento de funcionamento](./FUNCIONAMENTO.md) para os detalhes da arquitetura.
