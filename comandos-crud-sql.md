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