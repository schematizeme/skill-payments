# Changelog — schematize-payments

Todas as mudanças relevantes deste pacote, no formato [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/),
com versionamento [SemVer](https://semver.org/lang/pt-BR/).

## [0.1.0] — 2026-10-14
Primeira versão. Pagamento move dinheiro irreversível (PIX não estorna por botão, nota emitida tem efeito
tributário, cartão cobrado gera contestação) e a base só tinha "cobrança é efeito externo" em uma linha.
Faltava a especialista: dinheiro em float, chave de produção em ambiente de teste, webhook sem assinatura,
cobrança sem idempotência e PAN em log são defeitos que viram incidente financeiro e não tinham dono nem gate.

### Adicionado
- `SKILL.md` com 14 pisos, o primeiro absoluto: **nunca cobrar fora de prd** (herda efeito externo da base), fronteira com `schematize-api`, `schematize-data`, `schematize-iam`/base de IAM, `schematize-database`, `schematize-institutional`, `schematize-qa` e `schematize-pentest`.
- `references/`: `estado-e-ledger` (inteiro em centavos, máquina de estados, ledger de dupla entrada), `pix`, `cartao-pci` (nunca tocar PAN, SAQ A, chargeback), `boleto-recorrencia` (boleto, assinatura, dunning, Pix Automático), `webhooks-idempotencia`, `split-conciliacao-fiscal` (split, conciliação, estorno, NF-e/NFS-e via provedor), `nao-cobrar-fora-de-prd` e `stack-versoes` (anexo volátil com fonte e data).
- `scripts/check-pay.sh` + `check-pay.test.sh`: 10 regras estáticas (float em dinheiro, chave live no repo, host/credencial de produção em arquivo não-prd, ambiente com default prd, webhook sem assinatura e sem deduplicação, cobrança sem idempotência, PAN em log, PAN literal por Luhn, PAN/CVV persistido), waiver rastreável `pay-ok:`, casos vermelhos e verdes plantados.
- Comandos: `pay-help`, `pay-load`, `pay-review`, `pay-flow`, `pay-webhook`, `pay-sandbox`, `pay-reconcile`, `pay-claude`, `pay-handoff`, `pay-cc`.

### Não verificado
- Texto primário das normas do BCB, API de qualquer PSP real, sandbox de PSP e emissão fiscal contra provedor (ver `references/stack-versoes.md`, seção "Faltou verificar").
