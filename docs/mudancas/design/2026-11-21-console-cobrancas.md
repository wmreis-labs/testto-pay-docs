# Design: console, entrar e acompanhar as cobranças

**Status:** aberta
**Data:** 2026-11-21

O problema de produto está no [PRD do Testto Pay](https://wmreis-labs.github.io/wmreis-docs/mudancas/prd/2026-11-21-testto-pay/). A decisão de ter um console próprio, com um pool de usuários próprio e papéis por empresa, está no [ADR](https://wmreis-labs.github.io/wmreis-docs/mudancas/adr/2026-11-21-testto-pay-pagamentos/). Os estados e os campos da cobrança vêm do [desenho do serviço](https://wmreis-labs.github.io/airtestto-docs/mudancas/design/2026-11-21-pagamentos-testto-pay/).

## Objetivo

O financeiro de uma empresa entra no console do Testto Pay e vê as cobranças da própria empresa: quanto foi cobrado no mês, o que já foi pago, o que está aguardando e o que venceu. Ao abrir uma cobrança, vê de onde ela veio, o link, as parcelas e o histórico.

Esta é a primeira versão do console. Ela mostra só o que o serviço já guarda hoje.

## Fora deste desenho

- **Conciliação, repasses e taxas.** Dependem do extrato EDI do PagBank (airtestto #282) e ganham um desenho próprio.
- **Gerar um link avulso pelo console**, sem reserva nem pedido. Fica para depois desta versão; ver "Perguntas em aberto".
- **Gestão de usuários, empresa, conta do provedor e chaves de API.** Por enquanto, o operador da WMReis dá acesso aos usuários das empresas pela tela de Identidade do admin (airtestto #363).
- **Operador da WMReis vendo várias empresas.** Ele entra pelo pool de operadores, que não faz parte deste console.

## Onde entra

O console é um aplicativo próprio, no repositório `testto-pay`, com a base do design system Testto:

- tela de entrada dividida;
- layout logado com a sidebar recolhível;
- `PageHeader`, `DataState`, `StatusPill` e `Segmented`.

A marca é a do Testto Pay ([identidade visual](../../identidade.md)).

| Rota | Tela |
| --- | --- |
| `/entrar` | Entrada |
| `/cobrancas` | Cobranças do mês |
| `/cobrancas/:id` | Detalhe da cobrança |

A rota `/` leva para `/cobrancas`. Quem não está logado vai para `/entrar` e, depois do login, volta para a rota que pediu.

## Papéis

A pessoa pertence a uma empresa e tem um papel nela. A empresa e o papel vêm do token do pool do Testto Pay (airtestto #341). A tela nunca escolhe a empresa: a API responde só com as cobranças da empresa do usuário.

| Papel | Nesta versão |
| --- | --- |
| Administrador | Vê as cobranças e o detalhe, inclusive os identificadores do provedor |
| Financeiro | Igual ao administrador |
| Leitor | Vê as cobranças e o detalhe, sem os identificadores do provedor |

As diferenças maiores entre os papéis entram com usuários, empresa e link avulso.

## O que a pessoa faz

### Entrar

- **A tela é dividida.** À esquerda fica o painel de tinta, com a marca Testto Pay e as barras inclinadas. À direita fica o formulário, com no máximo 400px. No celular, só o formulário aparece, com a marca no topo.
- **O formulário** pede e-mail e senha e tem o link "Esqueci a senha", que é o fluxo de código do Cognito.
- **No primeiro acesso**, quando o pool exige troca de senha, a mesma tela pede a nova senha antes de entrar.
- **Os erros** aparecem sobre o formulário, em texto curto: e-mail ou senha incorretos, usuário sem empresa, ou falha de rede.
- **Usuário sem empresa ou sem papel** não entra. A tela diz "Seu usuário ainda não foi ligado a uma empresa. Fale com a WMReis."

### Layout logado

- A sidebar tem a marca Testto Pay e um item só: **Cobranças**. Não aparecem itens de telas que ainda não existem.
- No rodapé da sidebar ficam o nome e a empresa do usuário, e o menu com Tema e Sair.
- A barra superior mostra o caminho: "Cobranças" ou "Cobranças / Reserva 0412".

### Cobranças do mês (`/cobrancas`)

O cabeçalho mostra o título "Cobranças" e, à direita, a escolha do mês (`MonthPicker`). O padrão é o mês atual, e o mês fica na URL (`?mes=2026-11`).

Logo abaixo vêm três números, todos das cobranças **criadas** no mês escolhido:

| Número | Conta |
| --- | --- |
| Recebido | Soma do valor pago das cobranças pagas |
| Aguardando | Soma do valor das cobranças aguardando que ainda estão no prazo |
| Cobranças | Quantidade de cobranças |

Os números vêm da API (airtestto #342); a tela não soma páginas. A conta é sempre pelo mês de criação. Uma cobrança criada em outubro e paga em novembro aparece em outubro. A conta pelo mês do pagamento entra com a conciliação.

O filtro por estado é um `Segmented` com Todas, Aguardando, Pagas, Expiradas, Canceladas e Estornadas. O filtro também fica na URL (`?estado=paga`). Cobranças com o estado "Falhou" aparecem só em "Todas".

A tabela traz uma linha por cobrança, da mais nova para a mais antiga:

| Coluna | Conteúdo |
| --- | --- |
| Criada em | Data e hora, no fuso de São Paulo |
| Origem | Sistema e tipo, por exemplo "Hostto · Reserva", com o identificador da origem em texto discreto |
| Descrição | A descrição que o sistema mandou, por exemplo "Reserva Ana Souza 2026-12-20 a 2026-12-27" |
| Valor | Valor da cobrança, em reais |
| Parcelas | Parcelas escolhidas pelo pagador, quando pago; senão o máximo permitido ("até 12x") |
| Estado | `StatusPill` ([estados](../../identidade.md#estados-da-cobranca)) |
| Pago em | Data do pagamento, quando houver |

- Clicar na linha abre o detalhe.
- A lista vem em páginas de 50, com o botão "Carregar mais" enquanto a API devolver um cursor.
- No celular, cada cobrança vira um cartão com a descrição, o valor, o estado e a data.

### Detalhe da cobrança (`/cobrancas/:id`)

O cabeçalho mostra a descrição como título, o valor em Space Grotesk e o estado.

| Bloco | Conteúdo |
| --- | --- |
| Pagamento | Valor, valor pago, parcelas, forma de pagamento ("Cartão de crédito"), data do pagamento |
| Link | O link de pagamento, com o botão "Copiar", e a validade. Só aparece enquanto a cobrança está aguardando e no prazo. Depois disso, o bloco mostra "Link vencido em …" ou "Link substituído", conforme o estado |
| Condições | Parcelamento máximo, parcelas sem juros, validade |
| Origem | Sistema, tipo e identificador da origem. Nesta versão não há link para o sistema de origem |
| Histórico | Linha do tempo das mudanças de estado, com data, hora e motivo quando houver ("valor alterado") |
| Provedor | Provedor (PagBank) e os identificadores do checkout, do pedido e do pagamento. Serve para o suporte. Escondido para o leitor |

Nenhum dado do cartão aparece, porque o serviço nunca o recebe. O nome e o e-mail do pagador também não aparecem nesta versão.

## Estados

| Situação | O que aparece |
| --- | --- |
| Carregando | Linhas fantasmas (`LoadingRows`) na tabela e nos números |
| Erro da API | `ErrorState` com "Não foi possível carregar as cobranças" e o botão "Tentar de novo" |
| Mês sem cobranças | `EmptyState`: "Nenhuma cobrança criada em novembro de 2026" |
| Filtro sem resultado | `EmptyState`: "Nenhuma cobrança paga em novembro de 2026", com o botão "Ver todas" |
| Cobrança não encontrada ou de outra empresa | "Cobrança não encontrada", com o botão "Voltar para cobranças" (a API responde 404 nos dois casos) |
| Sessão vencida | Volta para `/entrar` e, depois do login, para a mesma rota |

Todas as telas funcionam a 375px sem rolagem horizontal e têm modo escuro.

## O que a tela consome

As rotas de leitura e o pool são as tarefas airtestto #341 e #342. O contrato das rotas entra na seção Contrato do [desenho do serviço](https://wmreis-labs.github.io/airtestto-docs/mudancas/design/2026-11-21-pagamentos-testto-pay/) junto com a #342. A tela mostra o que a API devolve e não recalcula estado: uma cobrança aguardando e fora do prazo já vem como expirada.

## Onde roda

| Ambiente | Endereço | Branch | API |
| --- | --- | --- | --- |
| dev | `https://dev-pay.testto.com.br` | `develop` | `https://dev-api.testto.com.br/payments` |
| prod | `https://pay.testto.com.br` | `main` | `https://api.testto.com.br/payments` |

Cada ambiente é um app do Amplify Hosting na conta AWS do ambiente. Um push na branch publica o console depois de lint, testes e build. O pool do Testto Pay de cada ambiente aceita o endereço do console. Os detalhes de operação estão no README do repositório `testto-pay`.

## Perguntas em aberto

- **Link avulso pelo console:** o financeiro quer cobrar algo sem reserva nem pedido (sinal, multa, extra)? Se sim, o valor sai do console, e essa é a única exceção à regra "o valor vem do backend que vende". A decisão vai para o ADR antes da tela.
- **Mês de referência:** o financeiro prefere ver por mês de criação ou por mês de pagamento? Nesta versão é por criação, porque é o índice que existe. Por pagamento entra com a conciliação.
