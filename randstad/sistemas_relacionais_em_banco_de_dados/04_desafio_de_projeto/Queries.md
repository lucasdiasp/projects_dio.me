
-- Listar todas as ordens de serviço com nome do cliente e status
SELECT 
  os.idOS AS "Nº OS",
  c.Nome AS "Cliente",
  s.Descricao AS "Status",
  os.Data_emissao AS "Data de Emissão",
  os.Valor_itens AS "Valor Total (R$)"
FROM Ordem_Servico os
JOIN Cliente c ON os.Cliente_idCliente = c.idCliente
JOIN Status_OS s ON os.Status_OS_idStatus_OS = s.idStatus_OS
ORDER BY os.Data_emissao DESC;

-- Listar veículos e seus respectivos donos
SELECT 
  v.idVeiculo,
  v.Modelo,
  v.Montadora,
  v.Ano,
  v.Valor AS "Valor Estimado (R$)",
  c.Nome AS "Proprietário"
FROM Veiculo v
JOIN Cliente c ON v.Cliente_idCliente = c.idCliente
ORDER BY c.Nome;

-- Quantidade de mecânicos por filial

SELECT 
  f.Nome AS "Filial",
  COUNT(m.idMecanico) AS "Qtd de Mecânicos"
FROM Filial f
LEFT JOIN Mecanico m ON f.idFilial = m.Filial_idFilial
GROUP BY f.idFilial
ORDER BY COUNT(m.idMecanico) DESC;

-- Valor total de serviços e peças em cada OS

SELECT 
  os.idOS AS "Nº OS",
  c.Nome AS "Cliente",
  SUM(sp.Subtotal) AS "Total Peças (R$)",
  SUM(ss.Subtotal) AS "Total Serviços (R$)",
  SUM(sp.Subtotal + ss.Subtotal) AS "Total Geral (R$)"
FROM Ordem_Servico os
JOIN Cliente c ON os.Cliente_idCliente = c.idCliente
LEFT JOIN Ordem_Servico_has_Pecas sp ON os.idOS = sp.Ordem_Servico_idOS
LEFT JOIN Servicos_Ofertados_has_Ordem_Servico ss ON os.idOS = ss.Ordem_Servico_idOS
GROUP BY os.idOS, c.Nome
ORDER BY "Total Geral (R$)" DESC;

-- Top 5 peças mais utilizadas
SELECT 
  p.Nome AS "Peça",
  SUM(osp.Quantidade) AS "Total Utilizado"
FROM Pecas p
JOIN Ordem_Servico_has_Pecas osp ON p.idPecas = osp.Pecas_idPecas
GROUP BY p.idPecas
ORDER BY SUM(osp.Quantidade) DESC
LIMIT 5;

-- Serviços mais lucrativos (por total de valor em OS)
SELECT 
  s.Nome AS "Serviço",
  SUM(sohs.Subtotal) AS "Valor Total (R$)",
  COUNT(DISTINCT sohs.Ordem_Servico_idOS) AS "Qtd de OS"
FROM Servicos_Ofertados s
JOIN Servicos_Ofertados_has_Ordem_Servico sohs ON s.idServico = sohs.Servicos_Ofertados_idServico
GROUP BY s.idServico
ORDER BY SUM(sohs.Subtotal) DESC
LIMIT 10;

-- Quantas OS cada mecânico executou
SELECT 
  m.Nome AS "Mecânico",
  COUNT(meo.Ordem_Servico_idOS) AS "Qtd de OS Executadas",
  f.Nome AS "Filial"
FROM Mecanico m
JOIN Mecanico_Executa_OS meo ON m.idMecanico = meo.Mecanico_idMecanico
JOIN Filial f ON f.idFilial = m.Filial_idFilial
GROUP BY m.idMecanico
ORDER BY COUNT(meo.Ordem_Servico_idOS) DESC;

-- Estoque total de peças por filial
SELECT 
  f.Nome AS "Filial",
  SUM(e.Disponibilidade) AS "Total em Estoque"
FROM Estoque e
JOIN Filial f ON e.Filial_idFilial = f.idFilial
LEFT JOIN Estoque_has_Pecas ehp ON e.idEstoque = ehp.Estoque_idEstoque
GROUP BY f.idFilial
ORDER BY "Total em Estoque" DESC;

-- Ordens de serviço com peças e serviços detalhados
SELECT 
  os.idOS,
  c.Nome AS "Cliente",
  v.Modelo AS "Veículo",
  p.Nome AS "Peça",
  osp.Quantidade AS "Qtd Peça",
  s.Nome AS "Serviço",
  sos.Quantidade AS "Qtd Serviço",
  os.Valor_itens AS "Valor Total (R$)"
FROM Ordem_Servico os
JOIN Cliente c ON os.Cliente_idCliente = c.idCliente
JOIN Veiculo v ON os.Veiculo_idVeiculo = v.idVeiculo
LEFT JOIN Ordem_Servico_has_Pecas osp ON os.idOS = osp.Ordem_Servico_idOS
LEFT JOIN Pecas p ON osp.Pecas_idPecas = p.idPecas
LEFT JOIN Servicos_Ofertados_has_Ordem_Servico sos ON os.idOS = sos.Ordem_Servico_idOS
LEFT JOIN Servicos_Ofertados s ON sos.Servicos_Ofertados_idServico = s.idServico
ORDER BY os.idOS;

-- Total gasto por cliente em todas as OS
SELECT 
  c.Nome AS "Cliente",
  SUM(os.Valor_itens) AS "Valor Total Gasto (R$)",
  COUNT(os.idOS) AS "Qtd de OS"
FROM Cliente c
JOIN Ordem_Servico os ON c.idCliente = os.Cliente_idCliente
GROUP BY c.idCliente
HAVING SUM(os.Valor_itens) > 0
ORDER BY SUM(os.Valor_itens) DESC;

-- Todas as ordens de serviço concluídas
SELECT 
  os.idOS AS "Nº OS",
  c.Nome AS "Cliente",
  v.Modelo AS "Veículo",
  os.Data_emissao AS "Data de Emissão",
  os.Data_Conclusao AS "Data de Conclusão",
  TIMESTAMPDIFF(DAY, os.Data_emissao, os.Data_Conclusao) AS "Dias até Conclusão"
FROM Ordem_Servico os
JOIN Cliente c ON os.Cliente_idCliente = c.idCliente
JOIN Veiculo v ON os.Veiculo_idVeiculo = v.idVeiculo
JOIN Status_OS s ON os.Status_OS_idStatus_OS = s.idStatus_OS
WHERE s.Descricao LIKE '%Concluída%' OR s.Descricao LIKE '%Finalizada%'
ORDER BY os.Data_Conclusao DESC;

-- Todas as OS em execução
SELECT 
  os.idOS,
  c.Nome AS "Cliente",
  v.Modelo AS "Veículo",
  s.Descricao AS "Status"
FROM Ordem_Servico os
JOIN Cliente c ON os.Cliente_idCliente = c.idCliente
JOIN Veiculo v ON os.Veiculo_idVeiculo = v.idVeiculo
JOIN Status_OS s ON os.Status_OS_idStatus_OS = s.idStatus_OS
WHERE s.Descricao LIKE '%Em Execução%' OR s.Descricao LIKE '%Aberta%'
ORDER BY os.Data_emissao DESC;

-- Clientes que solicitaram OS para mais de um carro
SELECT 
  c.Nome AS "Cliente",
  COUNT(DISTINCT v.idVeiculo) AS "Qtd de Veículos"
FROM Cliente c
JOIN Veiculo v ON c.idCliente = v.Cliente_idCliente
GROUP BY c.idCliente
HAVING COUNT(DISTINCT v.idVeiculo) > 1
ORDER BY COUNT(DISTINCT v.idVeiculo) DESC;

-- Percentual de OS concluídas em relação ao total
SELECT 
  CONCAT(ROUND(
    (SUM(CASE WHEN s.Descricao LIKE '%Concluída%' OR s.Descricao LIKE '%Finalizada%' THEN 1 ELSE 0 END) / COUNT(*)) * 100, 
  2), '%') AS "Percentual de OS Concluídas"
FROM Ordem_Servico os
JOIN Status_OS s ON os.Status_OS_idStatus_OS = s.idStatus_OS;

-- Ranking dos mecânicos mais produtivos (por número de OS executadas)
SELECT 
  m.Nome AS "Mecânico",
  f.Nome AS "Filial",
  COUNT(meo.Ordem_Servico_idOS) AS "Qtd de OS Executadas"
FROM Mecanico m
JOIN Filial f ON m.Filial_idFilial = f.idFilial
JOIN Mecanico_Executa_OS meo ON m.idMecanico = meo.Mecanico_idMecanico
GROUP BY m.idMecanico, f.Nome
ORDER BY COUNT(meo.Ordem_Servico_idOS) DESC
LIMIT 10;
