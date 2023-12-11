# SQL | Mulheres em Dados | Desafio Black Friday
Aqui compartilho as minhas soluções para o desafio de SQL Black Friday criado pela comunidade Mulheres em Dados.

Recursos para o Desafio:
- [Link do dataset.](https://docs.google.com/spreadsheets/d/1ietHmiEhMIEsMw-UXapaqA2EsKbQNHrNjHpw5Y0lpVc/edit#gid=621474653)
  
Para análise do dataset, foi utilizada a conexão com o BigQuery, onde fiz as consultas utilizando SQL.


## Perguntas do Desafio:
### 1. Quantos clientes compraram produtos na categoria de computadores e não informaram a sua ocupação de estado civil?

~~~sql
SELECT COUNT(User_ID) AS qtde_clientes
FROM `sql-basico-386818.MeD_BlackFriday.bf_vendas` 
WHERE categoria_produto = 'computadores'
  AND Ocupacao = 'Não informado';
~~~
<br>

|qtde_clientes|
|-------------|
|52|

---

### 2. A vendedora Fernandina quer a lista dos IDs de todos seus clientes da Black Friday. Qual das opções abaixo contém o campo que Malu vai usar no JOIN para relacionar as tabelas "vendas" e "vendedor"?

- User_ID
- vendedor_ID
- Order_ID
- ocupacao_ID
- primeiro_nome

**Resposta**:
O campo utilizado no JOIN das tabelas "vendas" e "vendedor" será o <code>vendedor_ID</code> por ser o campo em comum entre as duas tabelas.

### 4. Qual vendedor realizou o maior número de vendas?

~~~sql
SELECT DISTINCT vendedor_ID, v2.primeiro_nome, v2.sobrenome, COUNT(Order_ID) AS qtde_vendas
FROM `sql-basico-386818.MeD_BlackFriday.bf_vendas` v1
INNER JOIN `sql-basico-386818.MeD_BlackFriday.bf_vendedor` v2
USING (vendedor_ID)
GROUP BY vendedor_ID, v2.primeiro_nome, v2.sobrenome
ORDER BY qtde_vendas DESC
LIMIT 1;
~~~

| vendedor_ID | primeiro_nome | sobrenome | qtde_vendas |
|-------------|---------------|-----------|-------------|
| VEND0295342 |    Odelinda   |   Moreno  |      61     |


### 5. Qual é o CEP com maior faturamento e qual CEP com maior número de vendas?

~~~sql
SELECT cep, COUNT(Order_ID) AS qtde_vendas
FROM `sql-basico-386818.MeD_BlackFriday.bf_cliente`
INNER JOIN `sql-basico-386818.MeD_BlackFriday.bf_vendas`
USING(User_ID)
GROUP BY cep
ORDER BY qtde_vendas DESC LIMIT 1;
~~~

|CEP          | qtde_vendas |
|-------------|-------------|
|13024-215    |     49      |

~~~sql
SELECT cep, ROUND(SUM(preco),2) AS faturamento
FROM `sql-basico-386818.MeD_BlackFriday.bf_cliente`
INNER JOIN `sql-basico-386818.MeD_BlackFriday.bf_vendas`
USING(User_ID)
GROUP BY cep
ORDER BY faturamento DESC
LIMIT 1;
~~~

|CEP          | faturamento |
|-------------|-------------|
|08441-290    |   5649.3    |


