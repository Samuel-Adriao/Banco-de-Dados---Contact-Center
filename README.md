# Banco de Dados - Contact Center

# 1. ESTRUTURA ORGANIZACIONAL

## CARGO

Representa os cargos existentes dentro do contact center. Em vez de cadastrar salário, comissão e função diretamente em cada funcionário, você centraliza essas informações no cargo.

| Atributo | Função |
|---|---|
| `id_cargo (PK)` | Identificador único do cargo. |
| `nome_cargo` | Nome do cargo, como Atendente, Supervisor ou Gerente. |
| `salario_cargo` | Salário associado ao cargo. |
| `tipo_cargo` | Classificação do cargo, conforme as regras da empresa. |
| `porcentagem_comissao` | Percentual de comissão previsto para o cargo, caso exista. |
| `horas_cargo` | Carga horária prevista. |
| `desc_cargo` | Descrição das responsabilidades do cargo. |

## FUNCIONARIO

Representa as pessoas que trabalham no contact center.

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

**Relacionamento:** CARGO 1:N FUNCIONARIO

---

## DEVEDOR

Representa a pessoa física ou jurídica que possui uma obrigação financeira.

| Atributo | Função |
|---|---|
| `cpf_cnpj_devedor (PK)` | Identificador do devedor. |
| `nome_devedor` | Nome ou razão social. |
| `data_nascimento_devedor` | Data de nascimento, quando pessoa física. |
| `logradouro_devedor` | Rua, avenida etc. |
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
- DEVEDOR 1:N DIVIDA
- CREDOR 1:N DIVIDA

## CONTATO_ALTERNATIVO_DEVEDOR

Armazena outros meios de contato de um devedor.

| Atributo | Função |
|---|---|
| `id_contato_alt_devedor (PK)` | Identificador do contato. |
| `tipo_contato` | Informa se é TELEFONE, EMAIL etc. |
| `valor_contato` | O número ou endereço de e-mail propriamente dito. |
| `obs_contato_alternativo` | Observações sobre aquele contato. |
| `cpf_cnpj_devedor (FK)` | Identifica a quem pertence o contato. |

## CONTATO_ALTERNATIVO_FUNCIONARIO

Tem exatamente a mesma finalidade, mas para funcionários.

---

# 2. CONTROLE DAS DÍVIDAS

## DIVIDA

Representa uma obrigação financeira específica entre um devedor e um credor.

| Atributo | Função |
|---|---|
| `id_divida (PK)` | Identificador único da dívida. |
| `numero_documento` | Número do documento que originou ou identifica a cobrança. |
| `valor_original` | Valor inicial da obrigação. |
| `data_recebimento` | Quando o contact center recebeu a dívida do credor. |
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
| `data_fim_atribuicao` | Caso a dívida seja transferida para outra pessoa, esse campo informa o fim do período da atribuição de um funcionário específico. |
| `id_divida (FK)` | Identifica a dívida atribuída. |
| `cpf_cnpj_funcionario (FK)` | Identifica o funcionário responsável. |

## PARCELA_DIVIDA

Representa as divisões de uma dívida em vencimentos individuais.

| Atributo | Função |
|---|---|
| `id_parcela_divida (PK)` | Identificador da parcela. |
| `numero_parcela` | Número sequencial da parcela. |
| `data_vencimento` | Data em que a parcela deve ser paga. |
| `valor_original` | Valor original daquela parcela. |
| `valor_atualizado` | Valor atualizado considerando juros, multas e descontos. |
| `status_parcela` | Situação da parcela. |
| `porcentagem_juros` | Percentual de juros aplicado. |
| `valor_multa` | Valor da multa. |
| `valor_desconto` | Valor de desconto concedido. |
| `id_divida (FK)` | Identifica a dívida à qual a parcela pertence. |

**Relacionamento:** DIVIDA 1:N PARCELA_DIVIDA

---

# 3. ATENDIMENTOS E NEGOCIAÇÕES

## ATENDIMENTO_COBRANCA

Representa uma interação entre o contact center e o devedor.

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

O que ela faz é permitir que durante um único atendimento possa-se ser abordado várias dívidas diferentes sem que seja criado um novo registro para cada uma dessas dívidas ou limitar um atendimento a tratar de somente uma dívida por vez, sendo necessário encerrar o chamado para iniciar outro a respeito de outra dívida.

## NEGOCIACAO

Representa uma negociação realizada entre o contact center e o devedor. Basicamente essa tabela registra possíveis propostas ou acordos entre funcionário e devedor.

| Atributo | Função |
|---|---|
| `id_negociacao (PK)` | Identificador da negociação. |
| `motivo_negociacao` | Motivo pelo qual a negociação ocorreu. |
| `data_negociacao` | Data em que foi realizada. |
| `valor_original` | Valor considerado antes da negociação. **OBS:** se for abordado mais de uma dívida, esse valor será a soma de todas elas. |
| `valor_negociacao` | Valor proposto ou acordado. **OBS:** aqui também será somado. |
| `status_negociacao` | Situação da negociação: PENDENTE, ACEITA, RECUSADA, CANCELADA etc. |
| `obs_negociacao` | Observações adicionais. |
| `cpf_cnpj_devedor (FK)` | Devedor envolvido. |
| `cpf_cnpj_funcionario (FK)` | Funcionário responsável pela negociação. |

## NEGOCIACAO_DIVIDA

Essa tabela tem uma função semelhante à ATENDIMENTO_DIVIDA, mas relaciona negociações com dívidas.

Se um funcionário realiza uma negociação envolvendo duas ou mais dívidas, propondo um acordo total, essa tabela identifica exatamente quais dívidas foram negociadas.

Ou seja, a tabela NEGOCIACAO registra a negociação enquanto a tabela NEGOCIACAO_DIVIDA identifica quais dívidas participaram dela.

---

# 4. PAGAMENTOS

## TIPO_PAGAMENTO

Tem como objetivo evitar que cada pagamento tenha que armazenar uma descrição textual livre do método utilizado.

| Atributo | Função |
|---|---|
| `id_tipo_pagamento (PK)` | Identificador do tipo. |
| `nome_tipo_pagamento` | Nome, como PIX, CARTÃO, BOLETO. |
| `descricao_tipo_pagamento` | Explicação do tipo. |

## PAGAMENTO

Registra os pagamentos realizados pelo devedor para quitar uma parcela. Deixando claro que pagamento à vista, nada mais é do que um pagamento "dividido" em 1x.

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

**Relacionamento:** PARCELA_DIVIDA 1:N PAGAMENTO

Esse relacionamento também permite registrar pagamentos parciais. Uma parcela de R$ 1.000 pode receber um pagamento de R$ 400 e depois outro de R$ 600.

Assim, se um devedor tem uma dívida total de R$ 3.000,00, dividida em 3 parcelas de R$ 1.000,00, isso será armazenado em DIVIDA e PARCELA_DIVIDA.

Caso ele queira parcelar essa parcela da dívida de R$ 1.000,00 em 2x, agora sim essa parcela de R$ 1.000,00 será registrada em TIPO_PAGAMENTO e PAGAMENTO.

---

# 5. HISTÓRICO E AUDITORIA

## HISTORICO_PARCELA

Essa tabela registra alterações no estado de uma parcela.

| Atributo | Função |
|---|---|
| `id_historico (PK)` | Identificador do registro histórico. |
| `status_anterior` | Estado da parcela antes da alteração. |
| `status_novo` | Estado depois da alteração. |
| `data_hora_alteracao` | Quando a mudança aconteceu. |
| `observacao` | Explicação da alteração. |
| `id_parcela_divida (FK)` | Parcela que teve seu estado alterado. |
| `cpf_cnpj_funcionario (FK)` | Funcionário que realizou a alteração, quando aplicável. |

Se durante um atendimento, um devedor pagar a dívida ou parcela da dívida com um funcionário, e esse por sua vez mudar manualmente o status da parcela da dívida de PENDENTE ou ATRASADA para PAGO, essa tabela registra quem e por que fez isso.

Se simplesmente atualizar `status_parcela` na tabela PARCELA_DIVIDA, o estado anterior seria perdido.

Importante dizer que a FK do funcionário aqui pode ser nula, já que isso pode ser feito de forma automática pelo sistema sem intermédio de um funcionário.
