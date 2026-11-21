# Testto Pay

O Testto Pay é a cobrança por link do ecossistema Testto. Hostto, Zé Salgados e, mais adiante, sistemas de outras empresas pedem a cobrança ao Testto Pay. O cliente paga no checkout do provedor (hoje o PagBank), e o resultado volta para o sistema que vendeu. O financeiro de cada empresa acompanha tudo no console.

Nenhum dado de cartão passa pela Testto. O link é gerado pelo provedor e o dinheiro cai na conta da própria empresa.

## Onde está cada coisa

| Documento | Onde |
| --- | --- |
| Problema e escopo (PRD) | [Testto Pay](https://wmreis-labs.github.io/wmreis-docs/mudancas/prd/2026-11-21-testto-pay/) |
| Decisão (ADR) | [Testto Pay, pagamentos como domínio central](https://wmreis-labs.github.io/wmreis-docs/mudancas/adr/2026-11-21-testto-pay-pagamentos/) |
| Desenho do serviço (API, eventos, estados) | [Pagamentos (Testto Pay)](https://wmreis-labs.github.io/airtestto-docs/mudancas/design/2026-11-21-pagamentos-testto-pay/) |
| Identidade visual | [Identidade visual](identidade.md) |
| Telas do console | [Design docs](mudancas/design/index.md) |

## Entregas

1. **Corte 1:** a reserva da Miralis no Hostto, com o PagBank. O serviço já está em dev.
2. **Corte 2:** o console para as empresas da WMReis e o pedido do Zé Salgados. Está em andamento.
3. **Corte 3:** empresas de fora, com cadastro, conta do provedor autorizada, chave de API e webhooks de saída.
