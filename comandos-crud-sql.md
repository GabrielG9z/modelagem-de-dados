# Comandos SQL para CRUD - Referência 

## Resumo

- C -> Create (Inserir dados)
- R -> Read (Ler dados)
- U -> Update (Atualizar dados)
- D -> Delete (Deletar dados)

## INSERT
<!-- Inserindo dados na TABELA, em seguida o CAMPO, em seguida os VALORES. -->
```sql
INSERT INTO fabricantes (nome) VALUES('Asus');
INSERT INTO fabricantes (nome) VALUES('Dell');
INSERT INTO fabricantes (nome)
VALUES('Apple'),('LG'),('Samsung'), ('Brastemp');
```

### Produtos
```sql
INSERT INTO produtos(nome, descricao, preco, quantidade, fabricante_id) VALUES(
    'Ultrabook',
    'Ultrabook Asus, com processador Intel core i9, memória RAM 16 GB e Windows 11PRO.',
    6500.99,
    25,
    1
);
```

```sql
INSERT INTO produtos(nome, descricao, preco, quantidade, fabricante_id) VALUES(
    'Tabela Android',
    'Tablet com a versão 12 do sistema operacional da Google, possui tela de 10 polegadas e armazenamento 64GB.',
    4999.99,
    5,
    5 # fabricantes_id(Samsung)
);
```

```sql
INSERT INTO produtos(nome, descricao, preco, quantidade, fabricante_id) VALUES(
   'Geladeira',
   'Refrigerador Frost-Free com acesso à internet das Coisas e bla bla bla',
   1600,
   10,
   6 # Brastemp
),
(
    'Iphone 13 Pro Max',
    'Alta durabilidade, Processador Bionic 14, 128 GB de armazenamento',
    6999.99,
    3,
    3 # Apple
),
(
    'iPad Mini',
    'Tablet Apple com tela retina display de 4k, memória interna de 64GB, acesso ao iCloud.',
    5000,
    8,
    3 #Apple
);
```

## SELECT

### Ler dados da tabela produtos
```sql
SELECT * FROM produtos;
SELECT nome, preco FROM produtos;
SELECT nome FROM produtos WHERE preco < 5000;
SELECT nome, descricao FROM produtos WHERE fabricante_id = 3;
```

### Operadores Lógicos: E, OU, NÃO
```sql
SELECT * FROM produtos WHERE preco >= 5000 AND preco < 8000;
SELECT nome, preco FROM produtos WHERE fabricante_id = 3 OR fabricante_id = 8;
WHERE fabricante_id IN(3, 8);
```

```sql
 # o operador (!=) também funciona para comparar
SELECT nome, preco, quantidade FROM produtos WHERE NOT  fabricante_id = 3;
SELECT nome, preco, quantidade FROM produtos WHERE fabricante_id != 3;
```

### Filtros
```sql 
SELECT nome, preco FROM produtos ORDER BY nome; --ASC(Ordem crescente. Já e o modo de busca padrão)
SELECT nome, preco FROM produtos ORDER BY nome DESC; --(Ordem decrescente.)
SELECT nome, descricao FROM produtos WHERE descricao LIKE '%processador%'; --LIKE (Como) (% Operador Coringa- Significa qualquer texto antes e depois da palavra selecionada)
```

### Operações e Funções de agregação
```sql
SELECT SUM(preco) FROM produtos;
SELECT SUM(preco) AS TOTAL --ALIAS (APELIDO)
FROM produtos;
SELECT SUM(quantidade) AS "Quantidade em Estoque" FROM produtos;
SELECT SUM(quantidade) AS "Quantidade em Estoque" FROM produtos WHERE fabricante_id = 3; --Comando WHERE (específica a área de busca desejada.) Apple
-- AVG (AVERAGE) MÉDIA
SELECT AVG(preco) AS "Média dos Preços" FROM produtos;
--ROUND(arredonda os valores, a vírgula determina as casas decimais)
SELECT ROUND(AVG(preco), 2) AS "Média dos Preços" FROM produtos; 
-- DISTINCT (Comando para evitar que o SQL recupere os números que tem uma ocorrência de id (Evita duplicatas). maior que 1)
SELECT COUNT(DISTINCT fabricante_id) AS "Qtd de Produtos" FROM produtos;
SELECT COUNT(DISTINCT fabricante_id) AS "Qtd de Fabricantes" FROM fabricantes;
SELECT nome, preco, quantidade, (preco * quantidade) AS Total FROM produtos;

-- Comando via SQL
INSERT INTO `produtos` (`id`, `nome`, `descricao`, `preco`, `quantidade`, `fabricante_id`) VALUES (NULL, 'Teclado Gamer', 'Teclado de última geração com teclas quânticas e mecânicas, ótimo tempo de resposta e bla bla bla. Ah e led embutido. teclado rosa que toca música da barbie.', '300', '8', '8'), (NULL, 'Placa Mãe', 'Placa com diversos slots de memória RAM, com diversos Slots DDR6, suporte a processadores Intel i9 de 11°Geração', '1200', '5', '1')

```

### Agrupamento
```sql
SELECT SUM(preco),AS Total FROM produtos;
-- GROUP BY Permite segmentar resultados da consulta. Neste caso, somamos todos os preços e segmentamos/ agrupamos por cada fabricante.
SELECT  fabricante_id, SUM(preco) AS Total FROM produtos GROUP BY fabricante_id; 
```

### UPDATE

## Atualizar dados de uma tabela
## Update Sempre com WHERE
```sql
UPDATE fabricantes SET nome = 'Microsoft Brasil' WHERE id =8;

--Mudar o preço do Ultrabook da Positivo para 5200.
UPDATE produtos SET preco = 5200 WHERE id = 7;
--Mudar a Quantidade dos produtos da Asus para 15.
UPDATE produtos SET quantidade = 15 WHERE fabricante_id = 1 OR fabricante_id = 3;
```
### Excluir dados de uma tabela
```sql
DELETE FROM fabricantes WHERE id = 4;

DELETE FROM produtos WHERE preco <= 2000 AND preco > 500;
```

### Consultas em duas ou mais tabelas (JUNÇÃO)
```sql
-- SELECT nomeDaTabela.nomeDaColuna
SELECT produtos.nome, fabricantes.nome FROM
--INNER JOIN é o comando que permite JUNTAR tabelas
 produtos INNER JOIN fabricantes
 -- ON comando para indicar o critério da junção
  ON produtos.fabricante_id = fabricantes.id;

SELECT produtos.nome AS Produto, fabricantes.nome AS Fabricante FROM
 produtos INNER JOIN fabricantes
  ON produtos.fabricante_id = fabricantes.id ORDER BY produtos.nome, fabricantes.nome;

-- Fabricante, soma dos preços e quantidade de produtos
SELECT fabricantes.nome AS Fabricante, SUM(produtos.preco) AS Total,
COUNT(produtos.fabricante_id) AS "Qtd de Produtos" FROM produtos INNER JOIN fabricantes ON produtos.fabricante_id = fabricantes.id GROUP BY Fabricante
ORDER BY Total;
-- Trazer a quantidade de produtos de cada fabricante
SELECT fabricantes.nome AS Fabricante,COUNT(produtos.quantidade) AS "Qtd de Produtos" FROM produtos INNER JOIN fabricantes ON produtos.quantidade = quantidade GROUP BY quantidade
ORDER BY quantidade;
--Correção

-- INNER JOIN Traz os registros somente daqueles fabricantes que tem produtos, os que não correspondem a condição não são exibidos
SELECT
    fabricantes.nome AS Fabricante
    COUNT(produtos.id) AS "Quantidade de Produtos",
FROM produtos INNER JOIN fabricantes ON produtos.fabricante_id = fabricantes.id GROUP BY Fabricante

--RIGHT JOIN Traz os registros mesmo daqueles fabricantes que não tem produtos.
SELECT
    fabricantes.nome AS Fabricante,
    COUNT(produtos.id) AS "Quantidade de Produtos"
FROM produtos RIGHT JOIN fabricantes ON produtos.fabricante_id = fabricantes.id GROUP BY Fabricante

--LEFT JOIN Traz os registros mesmo daqueles fabricantes que não tem produtos.
SELECT
    fabricantes.nome AS Fabricante,
    COUNT(produtos.id) AS "Quantidade de Produtos",
FROM produtos LEFT JOIN fabricantes ON produtos.fabricante_id = fabricantes.id GROUP BY Fabricante
```