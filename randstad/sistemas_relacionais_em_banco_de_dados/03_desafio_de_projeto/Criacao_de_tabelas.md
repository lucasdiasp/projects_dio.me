
CREATE DATABASE IF NOT EXISTS ecommerce;
USE ecommerce;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Cliente` (
  `idCliente` INT NOT NULL AUTO_INCREMENT,
  `Nome` VARCHAR(100) NOT NULL,
  `Endereco` VARCHAR(200) NOT NULL,
  `Email` VARCHAR(100) NOT NULL,
  `Telefone` VARCHAR(20) NOT NULL,
  PRIMARY KEY (`idCliente`))
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Cliente`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Cliente_PJ` (
  `CNPJ` CHAR(14) NOT NULL,
  `Razao_Social` VARCHAR(45) NOT NULL,
  `Cliente_idCliente` INT NOT NULL,
  PRIMARY KEY (`Cliente_idCliente`),
  INDEX `fk_Cliente_PJ_Cliente1_idx` (`Cliente_idCliente` ASC) VISIBLE,
  UNIQUE INDEX `CNPJ_UNIQUE` (`CNPJ` ASC) VISIBLE,
  CONSTRAINT `fk_Cliente_PJ_Cliente1`
    FOREIGN KEY (`Cliente_idCliente`)
    REFERENCES `ecommerce`.`Cliente` (`idCliente`)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Cliente_PJ`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Cliente_PF` (
  `CPF` CHAR(11) NOT NULL,
  `Data_Nasc` DATE NOT NULL,
  `Cliente_idCliente` INT NOT NULL,
  PRIMARY KEY (`Cliente_idCliente`),
  INDEX `fk_Cliente_PF_Cliente1_idx` (`Cliente_idCliente` ASC) VISIBLE,
  UNIQUE INDEX `CPF_UNIQUE` (`CPF` ASC) VISIBLE,
  CONSTRAINT `fk_Cliente_PF_Cliente1`
    FOREIGN KEY (`Cliente_idCliente`)
    REFERENCES `ecommerce`.`Cliente` (`idCliente`)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Cliente_PF`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Pagamento` (
  `idPagamento` INT NOT NULL AUTO_INCREMENT,
  `Tipo` VARCHAR(45) NULL,
  `Status` VARCHAR(45) NULL,
  PRIMARY KEY (`idPagamento`))
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Pagamento`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Entrega` (
  `idEntrega` INT NOT NULL AUTO_INCREMENT,
  `Status_entrega` VARCHAR(45) NULL,
  `Codigo_Rastreio` VARCHAR(45) NULL,
  `Data_Envio` DATETIME NULL,
  `Data_entrega` DATETIME NULL,
  PRIMARY KEY (`idEntrega`))
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Entrega`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Pedido` (
  `idPedido` INT NOT NULL AUTO_INCREMENT,
  `Descricao` VARCHAR(45) NULL,
  `Cliente_idCliente` INT NOT NULL,
  `Frete` DECIMAL(10,2) NULL,
  `Data` DATETIME(6) NULL,
  `Entrega_idEntrega` INT NOT NULL,
  PRIMARY KEY (`idPedido`),
  INDEX `fk_Pedido_Cliente1_idx` (`Cliente_idCliente` ASC) VISIBLE,
  INDEX `fk_Pedido_Entrega1_idx` (`Entrega_idEntrega` ASC) VISIBLE,
  CONSTRAINT `fk_Pedido_Cliente1`
    FOREIGN KEY (`Cliente_idCliente`)
    REFERENCES `ecommerce`.`Cliente` (`idCliente`)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT `fk_Pedido_Entrega1`
    FOREIGN KEY (`Entrega_idEntrega`)
    REFERENCES `ecommerce`.`Entrega` (`idEntrega`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Pedido`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Pagamento_has_Pedido` (
  `Pagamento_idPagamento` INT NOT NULL,
  `Pedido_idPedido` INT NOT NULL,
  `Valor_pago` DECIMAL(10,2) NOT NULL,
  `Status_pagamento` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`Pagamento_idPagamento`, `Pedido_idPedido`),
  INDEX `fk_Pagamento_has_Pedido_Pedido1_idx` (`Pedido_idPedido` ASC) VISIBLE,
  INDEX `fk_Pagamento_has_Pedido_Pagamento1_idx` (`Pagamento_idPagamento` ASC) VISIBLE,
  CONSTRAINT `fk_Pagamento_has_Pedido_Pagamento1`
    FOREIGN KEY (`Pagamento_idPagamento`)
    REFERENCES `ecommerce`.`Pagamento` (`idPagamento`)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT `fk_Pagamento_has_Pedido_Pedido1`
    FOREIGN KEY (`Pedido_idPedido`)
    REFERENCES `ecommerce`.`Pedido` (`idPedido`)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Pagamento_has_Pedido`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Produto` (
  `idProduto` INT NOT NULL AUTO_INCREMENT,
  `Categoria` VARCHAR(160) NULL,
  `Descricao` VARCHAR(200) NULL,
  `Valor` DECIMAL(10,2) NULL,
  PRIMARY KEY (`idProduto`))
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Produto`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Fornecedor_terceiro` (
  `idFornecedor_terceiro` INT NOT NULL AUTO_INCREMENT,
  `Razao_Social` VARCHAR(160) NULL,
  `Local` VARCHAR(100) NULL,
  PRIMARY KEY (`idFornecedor_terceiro`))
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Fornecedor_terceiro`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Fornecedor` (
  `idFornecedor` INT NOT NULL AUTO_INCREMENT,
  `Razao_Social` VARCHAR(200) NULL,
  `CNPJ` VARCHAR(45) NULL,
  PRIMARY KEY (`idFornecedor`))
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Fornecedor`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Estoque` (
  `idEstoque` INT NOT NULL AUTO_INCREMENT,
  `Local` VARCHAR(100) NULL,
  PRIMARY KEY (`idEstoque`))
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Estoque`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Produto_has_Estoque` (
  `Produto_idProduto` INT NOT NULL,
  `Estoque_idEstoque` INT NOT NULL,
  `Quantidade` INT NULL,
  PRIMARY KEY (`Produto_idProduto`, `Estoque_idEstoque`),
  INDEX `fk_Produto_has_Estoque_Estoque1_idx` (`Estoque_idEstoque` ASC) VISIBLE,
  INDEX `fk_Produto_has_Estoque_Produto1_idx` (`Produto_idProduto` ASC) VISIBLE,
  CONSTRAINT `fk_Produto_has_Estoque_Produto1`
    FOREIGN KEY (`Produto_idProduto`)
    REFERENCES `ecommerce`.`Produto` (`idProduto`)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT `fk_Produto_has_Estoque_Estoque1`
    FOREIGN KEY (`Estoque_idEstoque`)
    REFERENCES `ecommerce`.`Estoque` (`idEstoque`)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Produto_has_Estoque`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Produto_por_vendedor` (
  `Produto_idProduto` INT NOT NULL,
  `Fornecedor_terceiro_idFornecedor_terceiro` INT NOT NULL,
  `Quantidade` INT NULL,
  PRIMARY KEY (`Produto_idProduto`, `Fornecedor_terceiro_idFornecedor_terceiro`),
  INDEX `fk_Produto_has_Fornecedor_terceiro_Fornecedor_terceiro1_idx` (`Fornecedor_terceiro_idFornecedor_terceiro` ASC) VISIBLE,
  INDEX `fk_Produto_has_Fornecedor_terceiro_Produto1_idx` (`Produto_idProduto` ASC) VISIBLE,
  CONSTRAINT `fk_Produto_has_Fornecedor_terceiro_Produto1`
    FOREIGN KEY (`Produto_idProduto`)
    REFERENCES `ecommerce`.`Produto` (`idProduto`)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT `fk_Produto_has_Fornecedor_terceiro_Fornecedor_terceiro1`
    FOREIGN KEY (`Fornecedor_terceiro_idFornecedor_terceiro`)
    REFERENCES `ecommerce`.`Fornecedor_terceiro` (`idFornecedor_terceiro`)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Produto_por_vendedor`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Disponibiliza_produto` (
  `Fornecedor_idFornecedor` INT NOT NULL,
  `Produto_idProduto` INT NOT NULL,
  PRIMARY KEY (`Fornecedor_idFornecedor`, `Produto_idProduto`),
  INDEX `fk_Fornecedor_has_Produto_Produto1_idx` (`Produto_idProduto` ASC) VISIBLE,
  INDEX `fk_Fornecedor_has_Produto_Fornecedor_idx` (`Fornecedor_idFornecedor` ASC) VISIBLE,
  CONSTRAINT `fk_Fornecedor_has_Produto_Fornecedor`
    FOREIGN KEY (`Fornecedor_idFornecedor`)
    REFERENCES `ecommerce`.`Fornecedor` (`idFornecedor`)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT `fk_Fornecedor_has_Produto_Produto1`
    FOREIGN KEY (`Produto_idProduto`)
    REFERENCES `ecommerce`.`Produto` (`idProduto`)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Disponibiliza_produto`;

CREATE TABLE IF NOT EXISTS `ecommerce`.`Produto_por_pedido` (
  `Pedido_idPedido` INT NOT NULL,
  `Produto_idProduto` INT NOT NULL,
  PRIMARY KEY (`Pedido_idPedido`, `Produto_idProduto`),
  INDEX `fk_Pedido_has_Produto_Produto1_idx` (`Produto_idProduto` ASC) VISIBLE,
  INDEX `fk_Pedido_has_Produto_Pedido1_idx` (`Pedido_idPedido` ASC) VISIBLE,
  CONSTRAINT `fk_Pedido_has_Produto_Pedido1`
    FOREIGN KEY (`Pedido_idPedido`)
    REFERENCES `ecommerce`.`Pedido` (`idPedido`)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT `fk_Pedido_has_Produto_Produto1`
    FOREIGN KEY (`Produto_idProduto`)
    REFERENCES `ecommerce`.`Produto` (`idProduto`)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
ENGINE = InnoDB;

SELECT * FROM `ecommerce`.`Produto_por_pedido`;