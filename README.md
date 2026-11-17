# Banco de Dados — Contact Center

## Sobre o projeto

Este projeto consiste na modelagem de um banco de dados relacional para um **Contact Center**, contemplando operações de cobrança, controle de dívidas, atendimentos, negociações, pagamentos e histórico de alterações.

A estrutura foi desenvolvida para organizar as informações dos devedores, credores e funcionários, permitindo acompanhar as dívidas desde sua atribuição para cobrança até a quitação ou devolução ao credor.

O projeto também contempla situações em que uma única negociação ou atendimento envolve múltiplas dívidas, utilizando entidades associativas para representar esses relacionamentos.

## Diagrama Entidade-Relacionamento

O diagrama abaixo representa visualmente a estrutura do banco de dados e seus relacionamentos.

![Diagrama Entidade-Relacionamento do Contact Center](contact-center.svg)

> **Arquivo:** `contact-center.svg`

---

# 1. ESTRUTURA ORGANIZACIONAL

## CARGO

Representa os cargos existentes dentro do Contact Center. Em vez de cadastrar salário, comissão e função diretamente em cada funcionário, essas informações são centralizadas no cargo.

| Atributo | Função |
|---|---|
| `id_cargo (PK)` | Identificador único do cargo. |
| `nome_cargo` | Nome do cargo, como Atendente, Supervisor ou Gerente. |
| `salario_cargo` | Salário associado ao cargo. |
| `tipo_cargo` | Classificação do cargo, conforme as regras da empresa. |
| `porcentagem_comissao` | Percentual de comissão previsto para o cargo, caso exista. |
| `horas_cargo` | Carga horária prevista. |
| `desc_cargo` | Descrição das responsabilidades do cargo. |

**Relacionamento:** CARGO 1:N FUNCIONARIO.

## FUNCIONARIO

Representa as pessoas que trabalham no Contact Center.

| Atributo | Função |
|---|---|
| `cpf_cnpj_funcionario (PK)` | Identificador do funcionário. |
| `nome_funcionario` | Nome completo. |
| `data_nascimento_funcionario` | Data de nascimento. |
| `logradouro_funcionario` | Rua, avenida ou outro tipo de logradouro. |
| `numero_funcionario` | Número do endereço. |
| `complemento_funcionario` | Complemento do endereço. |
| `bairro_funcionario` | Bairro. |
| `cidade_funcionario` | Cidade. |
| `estado_funcionario` | Estado. |
| `cep_funcionario` | CEP. |
| `tel_funcionario` | Telefone principal. |
| `email_funcionario` | E-mail principal. |
| `id_cargo (FK)` | Identifica o cargo ocupado pelo funcionário. |

## DEVEDOR

Representa a pessoa física ou jurídica que possui uma obrigação financeira.

| Atributo | Função |
|---|---|
| `cpf_cnpj_devedor (PK)` | Identificador do devedor. |
| `nome_devedor` | Nome ou razão social. |
| `data_nascimento_devedor` | Data de nascimento, quando pessoa física. |
| `logradouro_devedor` | Rua, avenida ou outro tipo de logradouro. |
| `numero_devedor` | Número do endereço. |
| `complemento_devedor` | Complemento. |
| `bairro_devedor` | Bairro. |
| `cidade_devedor` | Cidade. |
| `estado_devedor` | Estado. |
| `cep_devedor` | CEP. |
| `tel_devedor` | Telefone principal. |
| `email_devedor` | E-mail principal. |

## CREDOR

Representa a instituição ou pessoa que possui o direito de receber o valor da dívida.

| Atributo | Função |
|---|---|
| `cpf_cnpj_credor (PK)` | Identificador do credor. |
| `nome_credor` | Nome ou razão social. |
| `logradouro_credor` | Logradouro. |
| `numero_credor` | Número do endereço. |
| `complemento_credor` | Complemento. |
| `bairro_credor` | Bairro. |
| `cidade_credor` | Cidade. |
| `estado_credor` | Estado. |
| `cep_credor` | CEP. |
| `tel_credor` | Telefone. |
| `email_credor` | E-mail. |

**Relacionamentos:**
- DEVEDOR 1:N DIVIDA.
- CREDOR 1:N DIVIDA.

## CONTATO_ALTERNATIVO_DEVEDOR

Armazena outros meios de contato de um devedor, permitindo cadastrar diferentes telefones, endereços de e-mail ou outros meios de comunicação.

| Atributo | Função |
|---|---|
| `id_contato_alt_devedor (PK)` | Identificador do contato alternativo. |
| `tipo_contato` | Informa se o contato é TELEFONE, EMAIL etc. |
| `valor_contato` | O número ou endereço de e-mail propriamente dito. |
| `obs_contato_alternativo` | Observações sobre aquele contato. |
| `cpf_cnpj_devedor (FK)` | Identifica a quem pertence o contato. |

## CONTATO_ALTERNATIVO_FUNCIONARIO

Tem a mesma finalidade de CONTATO_ALTERNATIVO_DEVEDOR, mas permite armazenar meios de contato alternativos dos funcionários.

---

# 2. CONTROLE DAS DÍVIDAS

## DIVIDA

Representa uma obrigação financeira específica entre um devedor e um credor.

| Atributo | Função |
|---|---|
| `id_divida (PK)` | Identificador único da dívida. |
| `titulo_divida` | Identificação do título associado à dívida. |
| `numero_documento` | Número do documento que originou ou identifica a cobrança. |
| `valor_original` | Valor inicial da obrigação. |
| `data_recebimento` | Quando o Contact Center recebeu a dívida do credor. |
| `data_devolucao` | Quando a dívida foi devolvida ao credor. |
| `status_divida` | Situação geral da dívida. |
| `cpf_cnpj_devedor (FK)` | Identifica o devedor. |
| `cpf_cnpj_credor (FK)` | Identifica o credor. |

## ATRIBUICAO_DIVIDA

Determina qual funcionário foi ou é responsável por cobrar uma dívida e em qual período de tempo.

| Atributo | Função |
|---|---|
| `id_atribuicao (PK)` | Identificador da atribuição. |
| `data_atribuicao` | Quando a dívida foi atribuída ao funcionário. |
| `prazo_dias` | Quantidade de dias concedida para recuperação. |
| `data_limite_recuperacao` | Data final do prazo. |
| `status_atribuicao` | Situação da atribuição, como ATIVA, CONCLUIDA ou TRANSFERIDA. |
| `data_fim_atribuicao` | Caso a dívida seja transferida para outra pessoa, informa o fim do período da atribuição de um funcionário específico. |
| `id_divida (FK)` | Identifica a dívida atribuída. |
| `cpf_cnpj_funcionario (FK)` | Identifica o funcionário responsável. |

## PARCELA_DIVIDA

Representa as divisões de uma dívida em vencimentos individuais. Cada parcela possui suas próprias informações financeiras e situação de pagamento.

| Atributo | Função |
|---|---|
| `id_parcela_divida (PK)` | Identificador da parcela. |
| `numero_parcela` | Número sequencial da parcela. |
| `tipo_parcela` | Identifica se a parcela é ORIGINAL (`O`) ou decorrente de um ACORDO (`A`). |
| `data_vencimento` | Data em que a parcela deve ser paga. |
| `valor_original` | Valor original daquela parcela. |
| `valor_atualizado` | Valor atualizado considerando juros, multas e descontos. |
| `status_parcela` | Situação da parcela. |
| `porcentagem_juros` | Percentual de juros aplicado. |
| `valor_multa` | Valor da multa. |
| `valor_desconto` | Valor de desconto concedido. |
| `id_divida (FK)` | Identifica a dívida à qual a parcela pertence. |

**Relacionamento:** DIVIDA 1:N PARCELA_DIVIDA.

---

# 3. ATENDIMENTOS E NEGOCIAÇÕES

## ATENDIMENTO_COBRANCA

Representa uma interação entre o Contact Center e o devedor, registrando informações sobre o contato realizado e seu resultado.

| Atributo | Função |
|---|---|
| `id_atendimento_cobranca (PK)` | Identificador do atendimento. |
| `data_hora_inicio` | Quando o atendimento começou. |
| `data_hora_fim` | Quando terminou. |
| `canal_atendimento` | Meio utilizado: telefone, e-mail, WhatsApp etc. |
| `tipo_atendimento` | Classifica o atendimento como ativo ou receptivo. |
| `descricao_atendimento` | Descrição do que aconteceu. |
| `resultado_atendimento` | Resultado: contato realizado, não atendeu, promessa de pagamento etc. |
| `obs_atendimento` | Observações adicionais. |
| `cpf_cnpj_funcionario (FK)` | Funcionário que realizou o atendimento. |
| `cpf_cnpj_devedor (FK)` | Devedor que participou do atendimento. |

## ATENDIMENTO_DIVIDA

Essa tabela é uma entidade associativa. Sua função é relacionar atendimentos com dívidas.

Ela permite que, durante um único atendimento, sejam abordadas várias dívidas diferentes, sem que seja necessário criar um novo registro de atendimento para cada dívida.

Dessa forma, um atendimento pode estar relacionado a várias dívidas, e uma mesma dívida pode aparecer em diferentes atendimentos ao longo do tempo.

| Atributo | Função |
|---|---|
| `id_atendimento_divida (PK)` | Identificador do registro associativo. |
| `id_atendimento_cobranca (FK)` | Identifica o atendimento relacionado. |
| `id_divida (FK)` | Identifica a dívida abordada. |

## NEGOCIACAO

Representa uma negociação realizada entre o Contact Center e o devedor. Essa tabela registra possíveis propostas ou acordos realizados entre funcionário e devedor.

| Atributo | Função |
|---|---|
| `id_negociacao (PK)` | Identificador da negociação. |
| `motivo_negociacao` | Motivo pelo qual a negociação ocorreu. |
| `data_negociacao` | Data em que foi realizada. |
| `valor_original` | Valor considerado antes da negociação. Se for abordada mais de uma dívida, esse valor será a soma de todas elas. |
| `valor_negociacao` | Valor proposto ou acordado. Também poderá representar a soma dos valores negociados. |
| `status_negociacao` | Situação da negociação: PENDENTE, ACEITA, RECUSADA, CANCELADA etc. |
| `obs_negociacao` | Observações adicionais. |
| `cpf_cnpj_devedor (FK)` | Identifica o devedor envolvido. |
| `cpf_cnpj_funcionario (FK)` | Identifica o funcionário responsável pela negociação. |

## NEGOCIACAO_DIVIDA

Essa tabela tem uma função semelhante à ATENDIMENTO_DIVIDA, mas relaciona negociações com dívidas.

Se um funcionário realiza uma negociação envolvendo duas ou mais dívidas, propondo um acordo total, essa tabela identifica exatamente quais dívidas participaram da negociação.

Ou seja, a tabela NEGOCIACAO registra as informações gerais da negociação, enquanto NEGOCIACAO_DIVIDA identifica quais dívidas estão relacionadas a ela.

| Atributo | Função |
|---|---|
| `id_negociacao_divida (PK)` | Identificador do registro associativo. |
| `id_negociacao (FK)` | Identifica a negociação relacionada. |
| `id_divida (FK)` | Identifica a dívida incluída na negociação. |

---

# 4. PAGAMENTOS

## TIPO_PAGAMENTO

Tem como objetivo centralizar os métodos de pagamento disponíveis, evitando que cada registro de pagamento precise armazenar uma descrição textual livre do método utilizado.

| Atributo | Função |
|---|---|
| `id_tipo_pagamento (PK)` | Identificador do tipo de pagamento. |
| `nome_tipo_pagamento` | Nome do método, como PIX, CARTÃO ou BOLETO. |
| `descricao_tipo_pagamento` | Explicação do tipo de pagamento. |

## PAGAMENTO

Registra os pagamentos realizados para quitar uma parcela de uma dívida.

O modelo permite registrar pagamentos à vista ou pagamentos parciais de uma parcela, mantendo cada operação individualmente registrada.

| Atributo | Função |
|---|---|
| `id_pagamento (PK)` | Identificador do pagamento. |
| `valor_pagamento` | Valor efetivamente pago. |
| `data_hora_pagamento` | Quando o pagamento foi realizado ou confirmado. |
| `descricao_pagamento` | Informações adicionais. |
| `status_pagamento` | Situação: PENDENTE, CONFIRMADO, RECUSADO etc. |
| `quantidade_parcelas_cartao` | Quantidade de parcelas da operação no cartão. |
| `id_parcela_divida (FK)` | Parcela da dívida à qual o pagamento se refere. |
| `id_tipo_pagamento (FK)` | Método de pagamento utilizado. |

**Relacionamento:** PARCELA_DIVIDA 1:N PAGAMENTO.

Esse relacionamento também permite registrar pagamentos parciais. Por exemplo, uma parcela de R$ 1.000,00 pode receber um pagamento de R$ 400,00 e, posteriormente, outro de R$ 600,00.

Assim, se um devedor possui uma dívida total de R$ 3.000,00, dividida em três parcelas de R$ 1.000,00, essas informações são armazenadas em DIVIDA e PARCELA_DIVIDA.

Os registros em PAGAMENTO identificam as operações financeiras realizadas para quitar cada parcela, permitindo acompanhar os valores efetivamente pagos.

---

# 5. HISTÓRICO E AUDITORIA

## HISTORICO_PARCELA

Essa tabela registra alterações no estado de uma parcela, permitindo acompanhar seu histórico e identificar quando uma mudança aconteceu.

| Atributo | Função |
|---|---|
| `id_historico (PK)` | Identificador do registro histórico. |
| `status_anterior` | Estado da parcela antes da alteração. |
| `status_novo` | Estado depois da alteração. |
| `data_hora_alteracao` | Quando a mudança aconteceu. |
| `observacao` | Explicação da alteração. |
| `id_parcela_divida (FK)` | Parcela que teve seu estado alterado. |
| `cpf_cnpj_funcionario (FK)` | Funcionário que realizou a alteração, quando aplicável. |

Se, durante um atendimento, um devedor realizar o pagamento de uma dívida ou parcela e o funcionário alterar manualmente o status da parcela de PENDENTE ou ATRASADA para PAGO, essa tabela registra quem realizou a alteração e por qual motivo.

Se apenas o campo `status_parcela` da tabela PARCELA_DIVIDA fosse atualizado, o estado anterior seria perdido.

É importante destacar que a chave estrangeira `cpf_cnpj_funcionario` pode ser nula, pois a alteração também pode ser realizada automaticamente pelo sistema, sem a intervenção de um funcionário.

