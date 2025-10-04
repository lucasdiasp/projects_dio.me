
/* Tarefa: Listar todos os pedidos com dados do cliente e status da entrega. */
/* Objetivo: Mostra a visão geral de todos os pedidos, seus clientes e o status atual da entrega. */
SELECT 
    p.idPedido,
    p.Descricao AS Descricao_Pedido,
    c.Nome AS Cliente,
    e.Status_entrega,
    e.Codigo_Rastreio,
    p.Data
FROM Pedido p
JOIN Cliente c ON p.Cliente_idCliente = c.idCliente
JOIN Entrega e ON p.Entrega_idEntrega = e.idEntrega
ORDER BY p.idPedido;

/* Tarefa: Mostrar pedidos com informações de pagamento e valor total pago. */
/* Objetivo: Mostra a forma de pagamento, o status e o valor pago por cada pedido. */

SELECT 
    p.idPedido,
    c.Nome AS Cliente,
    pg.Tipo AS Forma_Pagamento,
    php.Valor_pago,
    php.Status_pagamento
FROM Pedido p
JOIN Cliente c ON p.Cliente_idCliente = c.idCliente
JOIN Pagamento_has_Pedido php ON php.Pedido_idPedido = p.idPedido
JOIN Pagamento pg ON php.Pagamento_idPagamento = pg.idPagamento
ORDER BY php.Valor_pago DESC;

/* Tarefa: Total gasto por cliente (agregação com GROUP BY). */
/* Objetivo: Revela os clientes que mais gastaram, somando o valor total de seus pedidos. */

SELECT 
    c.Nome AS Cliente,
    SUM(php.Valor_pago) AS Total_Gasto
FROM Cliente c
JOIN Pedido p ON c.idCliente = p.Cliente_idCliente
JOIN Pagamento_has_Pedido php ON p.idPedido = php.Pedido_idPedido
GROUP BY c.Nome
ORDER BY Total_Gasto DESC;

/* Tarefa: Quantidade total de produtos disponíveis em estoque.  */
/* Objetivo: Mostra o total geral de produtos disponíveis (somando todos os estoques). */

SELECT 
    SUM(pe.Quantidade) AS Quantidade_Total_Estoque
FROM Produto_has_Estoque pe;

/* Tarefa: Quantidade de produtos por categoria. */
/* Objetivo: Mostra quantos produtos há em cada categoria. */

SELECT 
    Categoria,
    COUNT(*) AS Quantidade_Produtos
FROM Produto
GROUP BY Categoria
ORDER BY Quantidade_Produtos DESC;

/* Tarefa: Fornecedores e os produtos que disponibilizam. */
/* Objetivo: Relaciona fornecedores e os produtos fornecidos por cada um. */

SELECT 
    f.Razao_Social AS Fornecedor,
    p.Descricao AS Produto
FROM Disponibiliza_produto dp
JOIN Fornecedor f ON dp.Fornecedor_idFornecedor = f.idFornecedor
JOIN Produto p ON dp.Produto_idProduto = p.idProduto
ORDER BY f.Razao_Social;

/* Tarefa: Vendedores terceiros e seus produtos. */
/* Objetivo: Mostra quais produtos são vendidos por terceiros e as quantidades ofertadas. */

SELECT 
    ft.Razao_Social AS Vendedor_Terceiro,
    p.Descricao AS Produto,
    pv.Quantidade AS Quantidade_Disponivel
FROM Produto_por_vendedor pv
JOIN Fornecedor_terceiro ft ON pv.Fornecedor_terceiro_idFornecedor_terceiro = ft.idFornecedor_terceiro
JOIN Produto p ON pv.Produto_idProduto = p.idProduto
ORDER BY ft.Razao_Social;

/* Tarefa: Valor médio dos pedidos (atributo derivado com AVG). */
/* Objetivo: Calcula o valor médio pago por pedido. */

SELECT 
    ROUND(AVG(php.Valor_pago), 2) AS Valor_Medio_Pedidos
FROM Pagamento_has_Pedido php;

/* Tarefa: Clientes com mais de R$ 1.000 em compras (HAVING). */
/* Objetivo: Filtra apenas os clientes que gastaram mais de R$ 1.000 no total. */

SELECT 
    c.Nome AS Cliente,
    SUM(php.Valor_pago) AS Total_Gasto
FROM Cliente c
JOIN Pedido p ON c.idCliente = p.Cliente_idCliente
JOIN Pagamento_has_Pedido php ON p.idPedido = php.Pedido_idPedido
GROUP BY c.Nome
HAVING Total_Gasto > 1000
ORDER BY Total_Gasto DESC;

/* Tarefa: Produtos mais vendidos (JOIN com contagem de pedidos). */
/* Objetivo: Mostra quantas vezes cada produto aparece em pedidos (vendas). */

SELECT 
    p.Descricao AS Produto,
    COUNT(pp.Pedido_idPedido) AS Total_Pedidos
FROM Produto p
JOIN Produto_por_pedido pp ON p.idProduto = pp.Produto_idProduto
GROUP BY p.Descricao
ORDER BY Total_Pedidos DESC;

/* Tarefa: Clientes com gasto superior a R$1.000,00 */
/* Objetivo: Selecionar clientes que gastaram mais do que R$ 1.000,00 por compra */

SELECT
  c.idCliente,
  c.Nome AS Nome_Cliente,
  p.idPedido,
  php.Valor_pago AS Valor_da_Compra
FROM Cliente c
JOIN Pedido p ON p.Cliente_idCliente = c.idCliente
JOIN Pagamento_has_Pedido php ON php.Pedido_idPedido = p.idPedido
WHERE php.Valor_pago > 1000.00
ORDER BY php.Valor_pago DESC;

/* Tarefa: Listar produtos vendidos (subqueries) */
/* Objetivo: Mostrar todos os produtos que são vendidos na loja. */

SELECT DISTINCT
  pr.idProduto,
  pr.Descricao AS Produto,
  pr.Valor
FROM Produto pr
WHERE pr.idProduto IN (
    SELECT Produto_idProduto FROM Disponibiliza_produto
    UNION
    SELECT Produto_idProduto FROM Produto_por_vendedor
)
ORDER BY pr.Descricao;

/* Tarefa: Produtos por vendedor */
/* Objetivo: Mostrar todos os produtos fornecidos por determinados fornecedores  */

SELECT
  f.idFornecedor,
  f.Razao_Social AS Fornecedor,
  pr.idProduto,
  pr.Descricao AS Produto,
  pr.Valor
FROM Fornecedor f
JOIN Disponibiliza_produto dp ON dp.Fornecedor_idFornecedor = f.idFornecedor
JOIN Produto pr ON pr.idProduto = dp.Produto_idProduto
WHERE f.Razao_Social LIKE '%Fornecedor A%' OR f.Razao_Social LIKE '%Fornecedor B%'
ORDER BY f.Razao_Social, pr.Descricao;

/* Tarefa: Listar quantidade de produtos por fornecedor */
/* Objetivo: Quantos produtos cada fornecedor fornece para a loja */

SELECT
  f.idFornecedor,
  f.Razao_Social AS Fornecedor,
  COUNT(DISTINCT dp.Produto_idProduto) AS Total_Produtos_Fornecidos
FROM Fornecedor f
LEFT JOIN Disponibiliza_produto dp ON dp.Fornecedor_idFornecedor = f.idFornecedor
GROUP BY f.idFornecedor, f.Razao_Social
ORDER BY Total_Produtos_Fornecidos DESC;


/* Tarefa: Exibir produtos disponíveis por local e quantidade */
/* Objetivo: Produtos disponíveis no estoque — ordenando pelo estoque e pela quantidade disponível  */

SELECT
  pr.idProduto,
  pr.Descricao AS Produto,
  e.idEstoque,
  e.Local AS Local_Estoque,
  pe.Quantidade AS Quantidade_Disponivel
FROM Produto_has_Estoque pe
JOIN Produto pr ON pr.idProduto = pe.Produto_idProduto
JOIN Estoque e ON e.idEstoque = pe.Estoque_idEstoque
ORDER BY e.idEstoque, pe.Quantidade DESC, pr.Descricao;


/* Tarefa: Listar clientes por data de entrega */
/* Objetivo: Verificar clientes com entrega superior a N dias */

SELECT DISTINCT
  c.idCliente,
  c.Nome AS Nome_Cliente,
  p.idPedido,
  p.Data   AS Data_Pedido,
  e.Data_Envio,
  e.Data_entrega,
  DATEDIFF(e.Data_entrega, p.Data) AS Dias_Apos_Pedido
FROM Pedido p
JOIN Cliente c ON p.Cliente_idCliente = c.idCliente
JOIN Entrega e ON p.Entrega_idEntrega = e.idEntrega
WHERE e.Data_entrega IS NOT NULL
  AND DATEDIFF(e.Data_entrega, p.Data) > 1
ORDER BY Dias_Apos_Pedido DESC;


/* Tarefa: Exibir quantidade de clientes por tipo de pagamento segmentado */
/* Objetivo: Quantidade de clientes que pagaram exclusivamente com PIX */

SELECT COUNT(*) AS Clientes_Exclusivamente_PIX
FROM (
  SELECT p.Cliente_idCliente
  FROM Pedido p
  JOIN Pagamento_has_Pedido php ON php.Pedido_idPedido = p.idPedido
  JOIN Pagamento pg ON pg.idPagamento = php.Pagamento_idPagamento
  GROUP BY p.Cliente_idCliente
  HAVING SUM(CASE WHEN pg.Tipo <> 'PIX' THEN 1 ELSE 0 END) = 0
) sub;