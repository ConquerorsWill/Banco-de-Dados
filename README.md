# Banco-de-Dados
SELECT * FROM produtos;

-- DDL - Linguagem de definição de dados (Interagir com a tabela inteira)
-- criação de tabela
CREATE TABLE produtos(
	id INT PRIMARY KEY NOT NULL,
	nome VARCHAR(255),
	preco DECIMAL(10,2)
);


-- Excluir uma tabela
DROP TABLE produtos;

--alterar tabela
ALTER TABLE produtos ADD descricao TEXT;
--excluir coluna
ALTER TABLE produtos DROP descricao;

--DML - Linguagem de manipulação de dados (mexer com linha)

--Inserir dado na tabela
INSERT INTO produtos(id, nome, preco) VALUES
(1, 'Nescau Radical', 8.00),
(2, 'Yakult activia', 4.00),
(3, 'Alpino amargo', 5.00),
(4, 'Miojo Monica', 1.00);

--Alterar dado da tabela
UPDATE produtos SET preco = 15.00 WHERE id=1;
UPDATE produtos SET nome = 'Toddy' WHERE nome LIKE 'Nescau Radical';

UPDATE produtos SET descricao = 'Armazenar em ambiente gelado'
WHERE id=2 or id=3;

UPDATE produtos SET descricao = 'Promoção'
WHERE preco <= 4.00;

--Deletar dado de um tabela
DELETE FROM produtos WHERE nome LIKE 'Alpino amargo';
DELETE FROM produtos WHERE id = 1;

-- Comandos DQL (Linguagem de consulta de dados)
--selecionar todos(*) produtos, ordene ascendente
SELECT * FROM produtos ORDER BY id ASC;

--Ordenar pelo preço crescente(menor para maior)
SELECT * FROM produtos ORDER BY preco ASC;

--Ordenar pelo preço descrescente(maior para menor)
SELECT * FROM produtos ORDER BY preco DESC;

--Selecionar Colunas
SELECT preco, nome FROM produtos ORDER BY id;

--Selecionar produtos maior que R$4,00
SELECT preco, nome FROM produtos WHERE preco > 4.00;

--Selecionar produto mais caro
SELECT MAX(preco) AS titulo FROM produtos;
SELECT preco, nome FROM produtos ORDER BY preco DESC LIMIT 1;

--simulando busca por nome exato
SELECT * FROM produtos WHERE nome LIKE 'Nescau Radical';

--busca pelos primeiros caracteres, o resto não importa
SELECT * FROM produtos WHERE nome LIKE 'Ne%';
--A parte anterior não importa, busca o ultimo
SELECT * FROM produtos WHERE nome LIKE '%Fit';
--busca por qualquer parte do nome
SELECT * FROM produtos WHERE nome LIKE '%Nestle%';

-- Fase 2 --------------------------------------------------------
-- Criando tabela Funcionário ------------------------------------

CREATE TABLE funcionario(
	id SERIAL PRIMARY KEY,
	nome VARCHAR(255) NOT NULL,
	data_nasc DATE,
	sexo CHAR(1),
	salario DECIMAL(10, 2),
	ativo BOOLEAN
);

INSERT INTO funcionario (nome, data_nasc, sexo, salario, ativo) VALUES
('Bob','1990-05-15', 'M', 2000.00, true),
('Augusto', '1970-04-01', 'M', 1500.00, false),
('Joanir', '2000-01-01', 'F', 1800.00, true),
('Elisa', '1995-10-02', 'F', 1900.00, true);

SELECT * FROM funcionario;

-- Seleciona funcionários que nasceram após 01-01-1992
SELECT * FROM funcionario WHERE data_nasc > '1992-01-01';

-- Seleciona funcionários em um período
SELECT nome, data_nasc FROM funcionario
WHERE data_nasc BETWEEN '1990-01-01' AND '1997-01-01';

--Contabilizar
SELECT sexo,
COUNT(*) AS total_pessoas,
AVG(salario) AS media_salarial
FROM funcionario GROUP BY sexo;

--Seleciona funcionario inativo
SELECT *FROM funcionario WHERE ativo = false;

-----------------------------------------------------------------------------------------------------------

CREATE TABLE clientes(
	id SERIAL PRIMARY KEY,
	nome VARCHAR(255) NOT NULL,
	email VARCHAR(255) NOT NULL		
);

ALTER TABLE clientes ADD telefone VARCHAR(20);

ALTER TABLE clientes DROP email;

CREATE TABLE itens(
	id SERIAL PRIMARY KEY,
	nome VARCHAR(255) NOT NULL,
	preco DECIMAL (10,2)
);

INSERT INTO clientes (id, nome) VALUES
('1', 'rodrigo');

INSERT INTO itens (id, nome, preco) VALUES
('1', 'farinha' , '1200.00'),
('2', 'trigo' , '1300.00'),
('3', 'café' , '1100.00');

UPDATE clientes SET nome = 'roderson' WHERE nome LIKE 'rodrigo';

UPDATE itens SET preco = 15.00 WHERE id=1;


DELETE FROM clientes WHERE nome LIKE 'roderson';

DELETE FROM itens WHERE id = 2;

--criação das tabelas
CREATE TABLE estados(
	id_estado SERIAL PRIMARY KEY,
	nome_estado VARCHAR(50) NOT NULL,
	sigla CHAR(2)
);

CREATE TABLE cidades (
	id_cidade SERIAL PRIMARY KEY,
	nome_cidade VARCHAR(50),
	cep VARCHAR(50),
	id_estado INT REFERENCES estados(id_estado)
);

--INSERINDO DADOS NAS TABELAS
INSERT INTO estados(id_estado, nome_estado, sigla) VALUES
(1, 'Paraná', 'PR'),
(2, 'São Paulo', 'SP'),
(3, 'Minas Gerais', 'MG');


INSERT INTO cidades(id_cidade, nome_cidade, cep, id_estado) VALUES
(10, 'Nova Londrina', '879770-000', 1),
(11, 'Marilena', '87960-000', 1),
(12, 'Itaúna', '87980-000', 1),
(13, 'Primavera', '19273-000', 2),
(14, 'Rosana', '19274-000', 2),
(15, 'Cachoeira da Prata', '35765-000', 3);

SELECT * FROM estados;

SELECT * FROM cidades;

--SELECIONAR TODAS AS CIDADES E SEUS ESTADOS
SELECT cidades.nome_cidade, estados.nome_estado
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estado;
WHERE estados.sigla LIKE 'SP' OR estados.sigla LIKE 'PR'
ORDER BY cidades.nome_cidade ASC;

--SELECIONA OS ESTADOS COM MAIS CIDADES
SELECT estados.nome_estado, COUNT(cidades.id_cidade) AS total_cidades
FROM estados INNER JOIN cidades
ON estados.id-estado = cidades.id_estado
GROUP BY estados.nome_estado
ORDER BY total_cidades DESC;

CREATE TABLE pessoa(
	id_pessoa SERIAL PRIMARY KEY,
	nome_pessoa VARCHAR(60),
	data_nascimento DATE,
	sexo CHAR(1),
	estado_civil VARCHAR(60),
	profissao VARCHAR(60),
	id_cidade INT REFERENCES cidades(id_cidade)
);

INSERT INTO pessoa
(id_pessoa, nome_pessoa, data_nascimento, sexo, estado_civil, profissao, id_cidade) VALUES
(1, 'Marcele', '2000-01-01', 'f', 'solteira', 'Arquiteta', 10),
(2, 'Ananias', '1980-02-20', 'm', 'casado', 'Carpinteiro', 10),
(3, 'Silvio', '1950-10-10', 'm', 'casado', 'Dublador', 11),
(4, 'Léo', '2001-01-02', 'm', 'casado', 'Entregador', 11),
(5, 'Chris', '1990-05-05', 'm', 'solteiro', 'Fisiculturista', 12),
(6, 'Ana', '1997-04-01', 'f', 'solteira', 'cantora', 13),
(7, 'Jaime', '1970-03-01', 'm', 'casado', 'Carteiro', 14),
(8, 'Alice', '2002-01-10', 'f', 'solteira', 'Advogada', 15),
(9, 'Valentina', '1995-05-03', 'f', 'solteira', 'Zootecnista', 15),
(10, 'Laura', '1998-05-09', 'f', 'solteira', 'Balconista', 15);

SELECT * FROM pessoa;

SELECT pessoa.nome_pessoa, cidades.nome_cidade
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade;

--Seleciona Estado, Cidade, Pessoa
SELECT pessoa.nome_pessoa, cidades.nome_cidade, estados.nome_estado
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados
ON cidades.id_estado = estados.id_estado;

--Seleciona pessoas do Estado do Paraná
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'Paraná';

--Selecione o nome da pessoa, profissão e qual cidade pertence
SELECT pessoa.nome_pessoa, cidades.nome_cidade, pessoa.profissao
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade

--Selecionar cidade com mais mulher/homem
SELECT cidades.nome_cidade, COUNT(*) AS Quantidade
FROM cidades INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE pessoa.sexo = 'm'
GROUP BY cidades.id_cidade
ORDER BY Quantidade desc;


--6
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'São Paulo';

--4
SELECT pessoa.nome_pessoa, cidades.nome_cidade, pessoa.profissao
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade;

--3
SELECT pessoa.nome_pessoa, cidades.nome_cidade, pessoa.estado_civil
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
WHERE pessoa.estado_civil LIKE 'solteiro';

--5
SELECT pessoa.nome_pessoa, data_nascimento FROM pessoa
WHERE data_nascimento BETWEEN '1980-01-01' AND '2000-01-01';

--Selecione as pessoas de Marilena
SELECT pessoa.nome_pessoa, cidades.nome_cidade
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
WHERE cidades.nome_cidade LIKE 	'Marilena';
