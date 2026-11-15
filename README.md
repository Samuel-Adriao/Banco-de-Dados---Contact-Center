# Banco de Dados - Contact Center

## Regra 1 — Origem da cobrança

Uma cobrança deve estar vinculada a uma contratação ou a uma venda, conforme sua origem. Não deve ficar sem origem nem possuir as duas simultaneamente.
Isso se deve ao fato de que o Contact Center pode vender um produto ou registrar a contratação de um serviço. Uma venda pode gerar uma cobrança unitária,
enquanto uma contratação pode gerar cobranças recorrentes, conforme a periodicidade acordada: diária, semanal, mensal, semestral, anual etc.

Por isso existem as tabelas VENDA e CONTRATACAO.

## Regra 2 — Cancelamento de contratação

O cancelamento de uma contratação não implica o cancelamento automático de cobranças anteriores.Uma contratação cancelada pode possuir cobranças vencidas ou pendentes,
que continuam representando obrigações financeiras do cliente.

## Regra 3 — Tentativas de pagamento

Toda tentativa de pagamento deve ser registrada, independentemente de seu resultado (TABELA: TENTATIVA_PAGAMENTO).
Uma tentativa recusada não representa um pagamento realizado e, portanto, não deve gerar um registro em PAGAMENTO. Ou seja, a tabela 'TENTATIVA PAGAMENTO' armazena pagamentos 
APROVADORS, PENDENTES OU REPROVADOS. Enquanto a tabela pagamentos só armazena aqueles que sejam APROVADOS.

## Regra 4 — Atendimento

Um atendimento deve identificar o cliente e o funcionário responsável, quando aplicável. As demais informações do cliente, como contratações, cobranças, pagamentos e ofertas,
podem ser consultadas por meio dos relacionamentos existentes no banco de dados. Quando uma operação for realizada durante o atendimento, a tabela correspondente poderá armazenar 
'id_atendimento' para permitir rastreabilidade. Como a contratação de um serviço durante um atendimento ou uma tentativa de pagamento durante um atendimento por exemplo. Isso implicaria as tabelas TENTATIVA_PAGAMENTO, PAGAMENTO e CONTRATACAO. 

## Regra 5 — Histórico

As alterações importantes de status devem ser registradas nas tabelas de histórico correspondentes.
Por exemplo, alterações de contratação em HISTORICO_CONTRATACAO e alterações de cobrança em HISTORICO_COBRANCA.

