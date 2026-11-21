# Identidade visual

O Testto Pay usa o [design system Testto](https://github.com/wmreis-labs/hostto/blob/develop/docs/DESIGN-SYSTEM.md) sem mudanças de regra. Só a marca é própria: são as barras inclinadas da Testto, com o vermelho trocado pelo verde do dinheiro entrando.

## Marca

![Marca Testto Pay](assets/testto-pay-mark-ink.svg#only-light){ width="72" }
![Marca Testto Pay](assets/testto-pay-mark-dark.svg#only-dark){ width="72" }

- São quatro barras com a inclinação de 9° do sistema (`skewX(-9deg)`). A primeira e a terceira são verdes; a segunda e a quarta, curta e no alto, são tinta.
- Ao lado vem a palavra "Testto Pay" em Space Grotesk 600, na cor do texto do tema.
- Sobre tinta (painel de entrada, ícone do app), as barras de tinta ficam brancas e o verde usa a versão do escuro.
- O ícone e o favicon usam três barras, sem a curta, num quadrado de tinta com cantos arredondados.
- No rodapé do link de pagamento, a marca aparece como endosso: "Processado por **Testto Pay**".

## Cores

O verde é o único token novo. Tudo o mais é o token Testto.

| Token | Claro | Escuro | Uso |
| --- | --- | --- | --- |
| `pay` | <span class="tp-swatch" style="background:#12B76A"></span>`#12B76A` | <span class="tp-swatch" style="background:#32D583"></span>`#32D583` | Marca e estado "pago" |
| `paySoft` | <span class="tp-swatch" style="background:#E7F8EF"></span>`#E7F8EF` | <span class="tp-swatch" style="background:#0F2E20"></span>`#0F2E20` | Fundo do estado "pago" |
| `ink` | <span class="tp-swatch" style="background:#15171C"></span>`#15171C` | <span class="tp-swatch" style="background:#ECEEF2"></span>`#ECEEF2` | Texto e botão primário |
| `brand` | <span class="tp-swatch" style="background:#D7140F"></span>`#D7140F` | <span class="tp-swatch" style="background:#FF4B45"></span>`#FF4B45` | Erro e crítico (nunca marca no Testto Pay) |

O botão primário continua em tinta. O verde fica na marca e no estado "pago", do mesmo jeito que o coral do Hostto fica só na casa do logo.

## Estados da cobrança

Os estados são os do [serviço](https://wmreis-labs.github.io/airtestto-docs/mudancas/design/2026-11-21-pagamentos-testto-pay/), mostrados com o `StatusPill` do sistema.

| Estado no serviço | Na tela | Tom |
| --- | --- | --- |
| `aberta` | <span class="tp-pill" style="background:#EAF0FD;color:#2F5BD3">Aguardando</span> | `info` |
| `paga` | <span class="tp-pill" style="background:#E7F8EF;color:#0E8A51">Pago</span> | `pay` |
| `expirada` | <span class="tp-pill" style="background:#F1F2F4;color:#5D6370">Expirado</span> | `neutro` |
| `cancelada` | <span class="tp-pill" style="background:#F1F2F4;color:#5D6370">Cancelado</span> | `neutro` |
| `estornada` | <span class="tp-pill" style="background:#FFF3E0;color:#A86100">Estornado</span> | `atencao` |
| `falhou` | <span class="tp-pill" style="background:#FDECEB;color:#D7140F">Falhou</span> | `critico` |

Uma tentativa recusada não é um estado: a cobrança continua "Aguardando" e o link continua valendo.

## Tipografia

Os títulos e os valores em destaque usam Space Grotesk. A interface usa Inter, com números tabulares em tabelas.
