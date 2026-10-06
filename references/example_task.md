# TASK-003: Implementar geração de payload PIX com cálculo de chave e expiração

> **Camada:** Serviço/Aplicação
> **Estimativa:** M
> **Status:** Pendente
> **Spec Origem:** `docs/specs/2026-08-14-checkout-pix-spec.md`

---

## 1. Contexto & Motivação
A funcionalidade de checkout exige a geração dinâmica de cobranças instantâneas PIX. Esta tarefa implementa o serviço que monta o payload EMV QRCPS-MPM, calcula o CRC16 e define a data de expiração (padrão 15 minutos). As tabelas de banco já foram criadas na TASK-001 e as interfaces de domínio na TASK-002.

## 2. Escopo Atômico
- Criar a classe/função `PixPayloadService.generatePayload(params: PixChargeParams): PixPayloadResult`.
- Implementar algoritmo de cálculo de CRC16 CCITT.
- Aplicar TTL de expiração configurável (default 900 segundos).

### Fora do Escopo
- Persistência direta no banco de dados → coberto pelo repositório (TASK-002).
- Controller HTTP e rotas de webhook → TASK-004 e TASK-005.

## 3. Dependências & Bloqueios
- **Depende de:** TASK-002 (Tipos e interfaces de domínio de PIX)
- **Bloqueia:** TASK-004 (Controller `POST /api/pix/create`)

## 4. Premissas & Invariantes
- Chave PIX e identificador de transação devem ser validados antes da montagem do payload.
- **Invariante:** Não alterar suítes de teste de outros gateways de pagamento (ex: cartão/boleto).

## 5. Critérios de Aceite (Acceptance Criteria)
- [ ] **Dado** parâmetros válidos (chave, valor, txId), **quando** `generatePayload` for chamado, **então** retorna a string copia-e-cola no padrão EMV com CRC16 válido ao final.
- [ ] **Dado** valor menor ou igual a zero, **quando** chamado, **então** lança `InvalidAmountException`.
- [ ] **Dado** txId com caracteres especiais não permitidos, **quando** chamado, **então** lança `InvalidTxIdException`.

## 6. Guia TDD para o Builder (Red -> Green -> Refactor)
- **Alvo do Teste Primeiro (RED):** `tests/unit/services/pix-payload.service.test.ts`
- **Comando de Teste:** `npm test -- tests/unit/services/pix-payload.service.test.ts`
- **Comportamento Mínimo (GREEN):** Implementar em `src/services/pix-payload.service.ts` e utilitário `src/utils/crc16.ts`.
- **Foco do Refactor:** Garantir funções puras, sem efeitos colaterais e 100% tipadas com TypeScript estrito.

## 7. Rubrica Adversarial da Tarefa (The Gauntlet Vectors)
*Vetores que o Adversarial Critic irá cobrar especificamente nesta tarefa:*
- [ ] **Boundary & Edge Cases:** Valores monetários com mais de 2 casas decimais (ex: `10.999`), strings com emojis ou caracteres UTF-8 invisíveis na chave.
- [ ] **Validação & Erros:** Verificação se a chave PIX nula ou indefinida lança erro descritivo em vez de quebrar a aplicação (`TypeError`).
- [ ] **Concorrência & Idempotência:** Função deve ser pura e imutável para suportar múltiplas chamadas concorrentes sem colisão de estado.

## 8. Notas Técnicas & Arquivos Afetados
- **Arquivos esperados:** `src/services/pix-payload.service.ts`, `src/utils/crc16.ts`, `tests/unit/services/pix-payload.service.test.ts`
- **Padrões do projeto:** Validação com schemas Zod caso necessário.

## 9. Rastreabilidade
- **Requisito(s) Origem:** RF-001 (Cobrança PIX imediata), RN-002 (Cálculo de CRC16 e TTL de 15min).
