
CREATE DATABASE IF NOT EXISTS oficina_mec;
USE oficina_mec;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Cliente`(
  idCliente INT NOT NULL AUTO_INCREMENT,
  Nome VARCHAR(160) NOT NULL,
  Endereco VARCHAR(100) NOT NULL,
  Email VARCHAR(70) NOT NULL UNIQUE,
  Telefone VARCHAR(12) NOT NULL,
  PRIMARY KEY (idCliente)) 
  ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Veiculo`(
  idVeiculo INT NOT NULL AUTO_INCREMENT,
  Modelo VARCHAR(45) NOT NULL,
  Ano SMALLINT NOT NULL CHECK (Ano >= 1900),
  Montadora VARCHAR(45) NOT NULL,
  Valor DECIMAL(10,2) NOT NULL CHECK (Valor >= 0),
  Cliente_idCliente INT NOT NULL,
  PRIMARY KEY (idVeiculo),
  INDEX fk_Veiculo_Cliente_idx (Cliente_idCliente ASC),
  CONSTRAINT fk_Veiculo_Cliente
    FOREIGN KEY (Cliente_idCliente)
    REFERENCES Cliente (idCliente)
    ON DELETE CASCADE
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Filial`(
  idFilial INT NOT NULL AUTO_INCREMENT,
  Nome VARCHAR(160) NOT NULL,
  Localidade VARCHAR(45) NOT NULL,
  PRIMARY KEY (idFilial)) 
  ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Status_OS`(
  idStatus_OS INT NOT NULL AUTO_INCREMENT,
  Descricao VARCHAR(90) NOT NULL,
  PRIMARY KEY (idStatus_OS)) 
  ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Mecanico` (
  idMecanico INT NOT NULL AUTO_INCREMENT,
  Nome VARCHAR(70) NOT NULL,
  Especialidade VARCHAR(90) NOT NULL,
  Filial_idFilial INT NOT NULL,
  PRIMARY KEY (idMecanico),
  INDEX fk_Mecanico_Filial_idx (Filial_idFilial ASC),
  CONSTRAINT fk_Mecanico_Filial
    FOREIGN KEY (Filial_idFilial)
    REFERENCES Filial (idFilial)
    ON DELETE CASCADE
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Ordem_Servico`(
  idOS INT NOT NULL AUTO_INCREMENT,
  Data_emissao DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  Data_Conclusao DATETIME NULL DEFAULT NULL,
  OS_autorizada ENUM('S','N') NOT NULL DEFAULT 'N',
  Data_Autorizacao DATETIME NULL DEFAULT NULL,
  Valor_itens DECIMAL(10,2) NULL DEFAULT NULL,
  Cliente_idCliente INT NOT NULL,
  Status_OS_idStatus_OS INT NOT NULL,
  Veiculo_idVeiculo INT NOT NULL,
  PRIMARY KEY (idOS),
  INDEX fk_Ordem_Servico_Cliente_idx (Cliente_idCliente ASC),
  INDEX fk_Ordem_Servico_Status_idx (Status_OS_idStatus_OS ASC),
  INDEX fk_Ordem_Servico_Veiculo_idx (Veiculo_idVeiculo ASC),
  CONSTRAINT fk_Ordem_Servico_Cliente
    FOREIGN KEY (Cliente_idCliente)
    REFERENCES Cliente (idCliente)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT fk_Ordem_Servico_Status
    FOREIGN KEY (Status_OS_idStatus_OS)
    REFERENCES Status_OS (idStatus_OS)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  CONSTRAINT fk_Ordem_Servico_Veiculo
    FOREIGN KEY (Veiculo_idVeiculo)
    REFERENCES Veiculo (idVeiculo)
    ON DELETE RESTRICT
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Pecas`(
  idPecas INT NOT NULL AUTO_INCREMENT,
  Nome VARCHAR(90) NOT NULL,
  Marca VARCHAR(90) NOT NULL,
  Valor DECIMAL(10,2) NOT NULL CHECK (Valor >= 0),
  PRIMARY KEY (idPecas)) 
  ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Servicos_Ofertados`(
  idServico INT NOT NULL AUTO_INCREMENT,
  Nome VARCHAR(60) NOT NULL,
  Descricao VARCHAR(160) NOT NULL,
  Valor DECIMAL(10,2) NOT NULL CHECK (Valor >= 0),
  PRIMARY KEY (idServico)) 
  ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Estoque`(
  idEstoque INT NOT NULL AUTO_INCREMENT,
  Disponibilidade SMALLINT NOT NULL CHECK (Disponibilidade >= 0),
  Filial_idFilial INT NOT NULL,
  PRIMARY KEY (idEstoque),
  INDEX fk_Estoque_Filial_idx (Filial_idFilial ASC),
  CONSTRAINT fk_Estoque_Filial
    FOREIGN KEY (Filial_idFilial)
    REFERENCES Filial (idFilial)
    ON DELETE CASCADE
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Mecanico_Preenche_OS`(
  Ordem_Servico_idOS INT NOT NULL,
  Mecanico_idMecanico INT NOT NULL,
  PRIMARY KEY (Ordem_Servico_idOS, Mecanico_idMecanico),
  CONSTRAINT fk_MecanicoPreenche_OS
    FOREIGN KEY (Ordem_Servico_idOS)
    REFERENCES Ordem_Servico (idOS)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT fk_MecanicoPreenche_Mecanico
    FOREIGN KEY (Mecanico_idMecanico)
    REFERENCES Mecanico (idMecanico)
    ON DELETE CASCADE
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Mecanico_Executa_OS`(
  Ordem_Servico_idOS INT NOT NULL,
  Mecanico_idMecanico INT NOT NULL,
  PRIMARY KEY (Ordem_Servico_idOS, Mecanico_idMecanico),
  CONSTRAINT fk_MecanicoExecuta_OS
    FOREIGN KEY (Ordem_Servico_idOS)
    REFERENCES Ordem_Servico (idOS)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT fk_MecanicoExecuta_Mecanico
    FOREIGN KEY (Mecanico_idMecanico)
    REFERENCES Mecanico (idMecanico)
    ON DELETE CASCADE
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Estoque_has_Pecas`(
  Estoque_idEstoque INT NOT NULL,
  Pecas_idPecas INT NOT NULL,
  PRIMARY KEY (Estoque_idEstoque, Pecas_idPecas),
  CONSTRAINT fk_EstoquePecas_Estoque
    FOREIGN KEY (Estoque_idEstoque)
    REFERENCES Estoque (idEstoque)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT fk_EstoquePecas_Pecas
    FOREIGN KEY (Pecas_idPecas)
    REFERENCES Pecas (idPecas)
    ON DELETE CASCADE
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Ordem_Servico_has_Pecas`(
  Ordem_Servico_idOS INT NOT NULL,
  Pecas_idPecas INT NOT NULL,
  Quantidade INT NOT NULL CHECK (Quantidade > 0),
  Valor_unitario DECIMAL(10,2) NOT NULL CHECK (Valor_unitario >= 0),
  Subtotal DECIMAL(10,2) GENERATED ALWAYS AS (Quantidade * Valor_unitario) STORED,
  PRIMARY KEY (Ordem_Servico_idOS, Pecas_idPecas),
  CONSTRAINT fk_OS_Pecas_OS
    FOREIGN KEY (Ordem_Servico_idOS)
    REFERENCES Ordem_Servico (idOS)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT fk_OS_Pecas_Peca
    FOREIGN KEY (Pecas_idPecas)
    REFERENCES Pecas (idPecas)
    ON DELETE CASCADE
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS `oficina_mec`.`Servicos_Ofertados_has_Ordem_Servico`(
  Servicos_Ofertados_idServico INT NOT NULL,
  Ordem_Servico_idOS INT NOT NULL,
  Quantidade INT NOT NULL CHECK (Quantidade > 0),
  Valor_unitario DECIMAL(10,2) NOT NULL CHECK (Valor_unitario >= 0),
  Subtotal DECIMAL(10,2) GENERATED ALWAYS AS (Quantidade * Valor_unitario) STORED,
  PRIMARY KEY (Servicos_Ofertados_idServico, Ordem_Servico_idOS),
  CONSTRAINT fk_Servico_OS_Servico
    FOREIGN KEY (Servicos_Ofertados_idServico)
    REFERENCES Servicos_Ofertados (idServico)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  CONSTRAINT fk_Servico_OS_OS
    FOREIGN KEY (Ordem_Servico_idOS)
    REFERENCES Ordem_Servico (idOS)
    ON DELETE CASCADE
    ON UPDATE CASCADE) 
    ENGINE = InnoDB;