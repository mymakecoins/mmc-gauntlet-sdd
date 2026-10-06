# Template de Descricao de Pull Request (Sob Demanda)

Este template deve ser utilizado para compor a descricao do Pull Request quando o usuario solicitar explicitamente sua abertura (`/mmc-gauntlet-sdd pr` ou pedido no chat).

> **Avisos de Conformidade Mandatorios:**
> - Nao abrir PR automaticamente; apenas sob demanda explicita.
> - Destino do PR sempre para a branch `develop`.
> - Revisor obrigatorio: `@thiagobotelhorodoind` (`--add-reviewer thiagobotelhorodoind`).
> - Assignee obrigatorio: `@marcos-rodoind` (`--add-assignee marcos-rodoind`).
> - Card no GitHub Projects movido para `In review`.
> - **NUNCA usar `Closes #id`, `Fixes #id` ou `Resolves #id`**. Utilizar exclusivamente `Ref #<task-id>` ou `Issue: #<task-id>`.
> - Evidencias visuais incorporadas via tags HTML apontando para a branch de origem: `<img src="https://github.com/TibaldiHolding/loggiq/blob/<branch-do-pr>/tests/evidence/<issue-id>/<arquivo>.png?raw=true" alt="<descricao>" width="100%" />`.
> - Zero emojis, emoticons ou icones graficos.

---

```markdown
## Descricao Consolidada das Entregas
[Sintese clara e estruturada das mudancas implementadas, agrupando as entregas de uma ou multiplas issues trabalhadas na branch].

## Referencias Informativas
- Ref #<issue-id-1>
- Ref #<issue-id-2>

## Evidencias Visuais (Screenshots da Suite E2E)
<img src="https://github.com/TibaldiHolding/loggiq/blob/<branch-do-pr>/tests/evidence/<issue-id-1>/01-fluxo-concluido.png?raw=true" alt="Evidencia visual da entrega 1" width="100%" />

<img src="https://github.com/TibaldiHolding/loggiq/blob/<branch-do-pr>/tests/evidence/<issue-id-2>/02-validacao-sucesso.png?raw=true" alt="Evidencia visual da entrega 2" width="100%" />

## Lista de Commits
- feat: #<issue-id-1> [descricao do commit em pt-BR]
- test: #<issue-id-1> [descricao dos testes em pt-BR]
- fix: #<issue-id-2> [descricao do ajuste em pt-BR]

## Validacao do Portao de Qualidade (Gates Aprovados)
- **MMC Gauntlet SDD:** 100% de aprovacao unanime (Adversarial QA, Integridade de Contratos e Analise Estatica).
- **Playwright E2E:** Suite executada com sucesso (Exit Code: 0) com capturas de tela persistidas.
- **Tipagem & Linter:** Zero erros de compilacao TypeScript e zero warnings de linter.
```
