USE sch;


-- TRABALHO REALIZADO POR:

-- AFONSO JACINTO a22208667
-- TOMÁS MATOS a22209049


-- CRIAÇÃO DAS TABELAS 

CREATE TABLE medico (
    id_medico INT(11),
    nome VARCHAR(200),
    data_nascimento DATE,
    telemovel VARCHAR(15),
    cedula_profissional VARCHAR(15),
    observacoes VARCHAR(2000)
);

CREATE TABLE especialidade_medico (
    id_especialidade INT(11),
    id_medico INT(11)
);

CREATE TABLE especialidade (
    id_especialidade INT(11),
    nome VARCHAR(100),
    descricao VARCHAR(2000)
);

CREATE TABLE horario_base (
    id_medico INT(11),
    dia_semana ENUM('SEG', 'TER', 'QUA', 'QUI', 'SEX', 'SAB', 'DOM'), -- PERGUNTA 1.3
    hora_inicio INT(4),
    hora_fim INT(4),
    duracao_consulta INT(3)
);

CREATE TABLE consulta (
    id_consulta INT(11),
    id_medico INT(11),
    id_gabinete INT(11),
    dia DATE,
    hora_inicio INT(4),
    hora_fim INT(4),
    id_utente INT(11)
);

CREATE TABLE anotacoes_consulta (
    id_anotacao INT(11),
    anotacao VARCHAR(2000),
    id_consulta INT(11)
);

CREATE TABLE exame_medico (
    id_exame_medico INT(11),
    id_consulta INT(11),
    id_tipo_exame_medico INT(11),
    data_exame DATE,
    relatorio VARCHAR(2000),
    imagem VARCHAR(500)
);

CREATE TABLE tipo_exame_medico (
    id_tipo_exame_medico INT(11),
    descricao VARCHAR(200)
);

CREATE TABLE gabinete (
    id_gabinete INT(11),
    descricao VARCHAR(2000)
);

CREATE TABLE utente (
    id_utente INT(11),
    nome VARCHAR(200),
    morada VARCHAR(200),
    codigo_postal VARCHAR(100),
    telefone VARCHAR(15),
    id_seguro VARCHAR(30),
    id_tipo_seguro VARCHAR(10)
);

CREATE TABLE tipo_seguro (
    id_tipo_seguro VARCHAR(10),
    descricao VARCHAR(1000)
);

-- ------------------------------------------

-- ------------------------------------------

-- ETAPA 1
-- PERGUNTA 1.1 (CRIAÇÃO DE PK)


ALTER TABLE medico
    ADD PRIMARY KEY (id_medico);

ALTER TABLE especialidade_medico
    ADD PRIMARY KEY (id_especialidade, id_medico);

ALTER TABLE especialidade
    ADD PRIMARY KEY (id_especialidade);

ALTER TABLE consulta
    ADD PRIMARY KEY (id_consulta);

ALTER TABLE anotacoes_consulta
    ADD PRIMARY KEY (id_anotacao);

ALTER TABLE exame_medico
    ADD PRIMARY KEY (id_exame_medico);

ALTER TABLE tipo_exame_medico
    ADD PRIMARY KEY (id_tipo_exame_medico);

ALTER TABLE gabinete
    ADD PRIMARY KEY (id_gabinete);

ALTER TABLE utente
    ADD PRIMARY KEY (id_utente);

ALTER TABLE tipo_seguro
    ADD PRIMARY KEY (id_tipo_seguro);

   
-- ------------------------------------------

-- ------------------------------------------
   
-- PERGUNTA 1.2 (CRIAÇÃO DE UK - MÉDICO)
   

 ALTER TABLE medico
    ADD UNIQUE (telemovel);

 ALTER TABLE medico
    ADD UNIQUE (cedula_profissional);
    
-- ------------------------------------------
   
-- ------------------------------------------
   
-- PERGUNTA 1.3  (CRIAÇÃO DIAS DA SEMANA)
   
   -- FEITA NA CRIAÇÃO DA TABELA ACIMA ('SEG', 'TER', 'QUA', 'QUI', 'SEX', 'SAB', 'DOM'))
   
-- ------------------------------------------
   
-- ------------------------------------------
   
-- PERGUNTA 1.4  (CRIAÇÃO LIMITES DA HORA)
   
ALTER TABLE horario_base
    ADD CHECK (hora_inicio >= 0 AND hora_inicio <= 2359),
    ADD CHECK (hora_fim >= 0 AND hora_fim <= 2359);
   
   
   -- LIMITE DAS HORAS DA TABELA CONSULTA FEITO NA PERGUNTA 1.7
   
   
   
-- ------------------------------------------

-- ------------------------------------------
   
    -- PERGUNTA 1.5
   
   ALTER TABLE consulta
    ADD UNIQUE (id_medico, id_gabinete, dia, hora_inicio);
   
-- ------------------------------------------   
   
-- ------------------------------------------  
 
 -- PERGUNTA 1.6 (CRIAÇÃO LIMITES DOS MINUTOS)
   
   ALTER TABLE horario_base
    ADD CHECK (duracao_consulta >= 5 AND duracao_consulta <= 130);
   
-- ------------------------------------------   
   
-- ------------------------------------------  
   
 -- PERGUNTA 1.7 (CRIAÇÃO LIMITES HORA E DIFERENÇA DE MINUTOS)
   
ALTER TABLE consulta
    ADD CHECK (hora_inicio >= 0 AND hora_inicio <= 2359),   -- (PERGUNTA 1.4)
    ADD CHECK (hora_fim >= 0 AND hora_fim <= 2359);
   
ALTER TABLE consulta
    ADD CHECK (hora_inicio < hora_fim),
    ADD CHECK ((hora_fim - hora_inicio) BETWEEN 5 AND 130); -- 130 PORQUE CORRESPONDE A 90 MINUTOS
   
-- ------------------------------------------     
   
-- ------------------------------------------  
   
   -- PERGUNTA 1.8 (CRIÇÃO CAMPOS FACULTATIVOS)

ALTER TABLE utente
    MODIFY id_seguro VARCHAR(30) NULL,
    MODIFY id_tipo_seguro VARCHAR(10) NULL;

ALTER TABLE consulta
    MODIFY id_utente INT(11) NULL;

ALTER TABLE medico
    MODIFY observacoes VARCHAR(2000) NULL;

ALTER TABLE exame_medico
    MODIFY relatorio VARCHAR(2000) NULL,
    MODIFY imagem VARCHAR(500) NULL;
    
-- ------------------------------------------  
   
-- ------------------------------------------ 
   
-- PERGUNTA 1.9  (CRIAÇÃO DE UK - ID_SEGURO)
   ALTER TABLE utente
    ADD UNIQUE (id_seguro);

-- ------------------------------------------ 
   
   -- PERGUNTA 1.10  (CRIAÇÃO DE UTENTES)
INSERT INTO utente (id_utente, nome, morada, codigo_postal, telefone, id_seguro, id_tipo_seguro)
VALUES
(1, 'Tony Carreira', 'Av. de Madrid Lote 45, 3º B', 'XXXXX-XXX', '123456789', 'ABC123', 'ALTF1'),
(2, 'Quim Barreiros', 'Av. de Madrid Lote 45, 1º C', 'XXXXX-XXX', '987654321', 'DEF456', 'ALTF2'),
(3, 'José Cid', 'Outra Morada', 'YYYYY-YYY', '111222333', NULL, 'ALTF3'),
(4, 'Emanuel', 'Av. de Lisboa, 10', 'ZZZZZ-ZZZ', '444555666', 'GHI789', 'ALTF4'),
(5, 'José Malhoa', 'Rua Sem Nome, 25', 'WWWWW-WWW', '777888999', 'JKL012', 'ALTF5');   
   
-- ------------------------------------------ 

-- ------------------------------------------ 

-- PERGUNTA 2  (CRIAÇÃO DE MEDICOS)
INSERT INTO medico (id_medico, nome, data_nascimento, telemovel, cedula_profissional, observacoes)
VALUES
(1, 'Cristiano Ronaldo', '1980-01-01', '123456789', 'MP12345', 'Observações Médico 1'),
(2, 'Luis Figo', '1985-02-15', '987654321', 'MP67890', 'Observações Médico 2'),
(3, 'Sylvester Stallone', '1990-05-20', '111222333', 'MPABCDE', 'Observações Médico 3'),
(4, 'Bruce Lee', '1975-09-10', '444555666', 'MPFGHIJ', 'Observações Médico 4'),
(5, 'Maria José Valério', '1988-12-30', '777888999', 'MPKLMNO', 'Observações Médico 5');

-- ------------------------------------------ 

-- ------------------------------------------ 

-- PERGUNTA 3  (CRIAÇÃO DE ESPECIALIDADES)
INSERT INTO especialidade (id_especialidade, nome, descricao)
VALUES
(1, 'Cardiologia', 'Especialidade em doenças cardíacas'),
(2, 'Dermatologia', 'Especialidade em doenças da pele'),
(3, 'Ortopedia', 'Especialidade em problemas musculoesqueléticos'),
(4, 'Neurologia', 'Especialidade em doenças do sistema nervoso'),
(5, 'Pediatria', 'Especialidade em doenças dedicadas a crianças');


-- ------------------------------------------ 

-- ------------------------------------------ 

-- PERGUNTA 4  (CRIAÇÃO DE GABINETES)
INSERT INTO gabinete (id_gabinete, descricao)
VALUES
(1, 'Gabinete 1 - Descrição 1'),
(2, 'Gabinete 2 - Descrição 2'),
(3, 'Gabinete 3 - Descrição 3'),
(4, 'Gabinete 4 - Descrição 4'),
(5, 'Gabinete 5 - Descrição 5');

-- ------------------------------------------ 

-- ------------------------------------------ 

-- PERGUNTA 5  (CRIAÇÃO DE TIPOS DE SEGUROS)
INSERT INTO tipo_seguro (id_tipo_seguro, descricao)
VALUES
('ALTF1', 'Descrição do Seguro 1'),
('ALTF2', 'Descrição do Seguro 2'),
('ALTF3', 'Descrição do Seguro 3'),
('ALTF4', 'Descrição do Seguro 4'),
('ALTF5', 'Descrição do Seguro 5');

-- ------------------------------------------ 

-- ------------------------------------------ 

-- PERGUNTA 6  (CRIAÇÃO DE TIPOS DE EXAMES MEDICOS)
INSERT INTO tipo_exame_medico (id_tipo_exame_medico, descricao)
VALUES
(1, 'Raio-X'),
(2, 'Ressonância Magnética'),
(3, 'Ecocardiograma'),
(4, 'Hemograma'),
(5, 'Colonoscopia');

-- ------------------------------------------ 

-- ------------------------------------------ 

-- PERGUNTA 7  (VERIFICAÇÃO DAS LINHAS )

SELECT * FROM utente;
SELECT * FROM medico;
SELECT * FROM especialidade;
SELECT * FROM gabinete;
SELECT * FROM tipo_seguro;
SELECT * FROM tipo_exame_medico;   

-- ------------------------------------------

-- ------------------------------------------

-- PERGUNTA 8 (VERIFICAR VALORES UNICOS)
-- se todas as instruções retornarem zero resultados, isso indica que não há valores repetidos nos campos

SELECT id_seguro, COUNT(id_seguro) AS count_seguro
FROM utente
GROUP BY id_seguro
HAVING count_seguro > 1;


SELECT telemovel, COUNT(telemovel) AS count_telemovel
FROM medico
GROUP BY telemovel
HAVING count_telemovel > 1;


SELECT cedula_profissional, COUNT(cedula_profissional) AS count_cedula
FROM medico
GROUP BY cedula_profissional
HAVING count_cedula > 1;

-- ------------------------------------------

-- ------------------------------------------

-- PERGUNTA 9 (ALTERAÇÃO DA MORADA)
UPDATE utente
SET morada = REPLACE(morada, 'Av. de Madrid Lote 45', 'Av. de Madrid, nº14')
WHERE morada LIKE 'Av. de Madrid Lote 45%';
SELECT * FROM utente;

-- ------------------------------------------

-- ------------------------------------------

-- ETAPA 2

-- PERGUNTA 10 (CRIAÇÃO DE FK)

ALTER TABLE especialidade_medico
    ADD FOREIGN KEY (id_especialidade) REFERENCES especialidade(id_especialidade),
    ADD FOREIGN KEY (id_medico) REFERENCES medico(id_medico);

ALTER TABLE horario_base
    ADD FOREIGN KEY (id_medico) REFERENCES medico(id_medico);

ALTER TABLE consulta
    ADD FOREIGN KEY (id_medico) REFERENCES medico(id_medico),
    ADD FOREIGN KEY (id_gabinete) REFERENCES gabinete(id_gabinete),
    ADD FOREIGN KEY (id_utente) REFERENCES utente(id_utente);

ALTER TABLE anotacoes_consulta
    ADD FOREIGN KEY (id_consulta) REFERENCES consulta(id_consulta);

ALTER TABLE exame_medico
    ADD FOREIGN KEY (id_consulta) REFERENCES consulta(id_consulta),
    ADD FOREIGN KEY (id_tipo_exame_medico) REFERENCES tipo_exame_medico(id_tipo_exame_medico);

ALTER TABLE utente
    ADD FOREIGN KEY (id_tipo_seguro) REFERENCES tipo_seguro(id_tipo_seguro);

-- ------------------------------------------

-- ------------------------------------------
   
-- PERGUNTA 11 (HORARIO DOS MEDICOS)
   
INSERT INTO horario_base (id_medico, dia_semana, hora_inicio, hora_fim, duracao_consulta)
VALUES
(1, 'SEG', 900, 1700, 130),
(1, 'QUA', 900, 1700, 70),
(2, 'TER', 1000, 1800, 45),
(2, 'QUI', 1000, 1800, 50),
(3, 'SEG', 800, 1600, 25),
(3, 'SEX', 800, 1600, 5),
(4, 'TER', 1100, 1900, 65),
(4, 'QUI', 1100, 1900, 15),
(5, 'QUA', 900, 1700, 80),
(5, 'SEX', 900, 1700, 15);

SELECT * FROM horario_base;  

-- ------------------------------------------

-- ------------------------------------------

-- PERGUNTA 12 (MEDICOS COM ESPECIALIDADES)
INSERT INTO especialidade_medico (id_especialidade, id_medico)
VALUES
(1, 1), 
(2, 1), 
(3, 2), 
(4, 2), 
(5, 3), 
(1, 4), 
(3, 5); 

SELECT * FROM especialidade_medico;

-- ------------------------------------------

-- ------------------------------------------

-- PERGUNTA 13 (CONSULTAS DOS MEDICOS)
-- MEDICO 1
INSERT INTO consulta (id_consulta , id_medico, id_gabinete, dia, hora_inicio, hora_fim, id_utente)
VALUES
(1,1, 1, '2024-01-27', 1000, 1030, 1),
(2,1, 1, '2024-01-28', 1100, 1130, 2),
(3,1, 1, '2024-01-29', 1200, 1230, 3),
(4,1, 1, '2024-01-30', 1300, 1330, 4),
(5,1, 1, '2024-01-31', 1400, 1430, 5);
-- MEDICO 2
INSERT INTO consulta (id_consulta , id_medico, id_gabinete, dia, hora_inicio, hora_fim, id_utente)
VALUES
(6,2, 2, '2024-01-27', 1000, 1030, 1),
(7,2, 2, '2024-01-28', 1100, 1130, 2),
(8,2, 2, '2024-01-29', 1200, 1230, 3),
(9,2, 2, '2024-01-30', 1300, 1330, 4),
(57,2, 2, '2024-01-31', 1400, 1430, 5);
-- MEDICO 3
INSERT INTO consulta (id_consulta , id_medico, id_gabinete, dia, hora_inicio, hora_fim, id_utente)
VALUES
(134,3, 3, '2024-01-27', 1000, 1030, 1),
(24,3, 3, '2024-01-28', 1100, 1130, 2),
(33,3, 3, '2024-01-29', 1200, 1230, 3),
(42,3, 3, '2024-01-30', 1300, 1330, 4),
(51,3, 3, '2024-01-31', 1400, 1430, 5);
-- MEDICO 4
INSERT INTO consulta (id_consulta , id_medico, id_gabinete, dia, hora_inicio, hora_fim, id_utente)
VALUES
(123,4, 4, '2024-01-27', 1000, 1030, 1),
(256,4, 4, '2024-01-28', 1100, 1130, 2),
(35,4, 4, '2024-01-29', 1200, 1230, 3),
(46,4, 4, '2024-01-30', 1300, 1330, 4),
(58,4, 4, '2024-01-31', 1400, 1430, 5);
-- MEDICO 5
INSERT INTO consulta (id_consulta , id_medico, id_gabinete, dia, hora_inicio, hora_fim, id_utente)
VALUES
(10,5, 4, '2024-01-27', 1000, 1030, NULL), -- CRIADO COM O ID_UTENTE A NULL, PARA TESTAR NA PERGUNTA 14
(28,5, 4, '2024-01-28', 1100, 1130, 2),
(36,5, 4, '2024-01-29', 1200, 1230, 3),
(43,5, 4, '2024-01-30', 1300, 1330, 4),
(53,5, 4, '2024-01-31', 1400, 1430, 5);

SELECT * FROM consulta;

-- ------------------------------------------

-- ------------------------------------------

-- PERGUNTA 14 (ADICIONAR UMA TABELA COM O NOME DOS UNTENTES MEDICOS E O ID DA CONSULTA)



CREATE TEMPORARY TABLE view_pergunta14_temp_resultados_consulta (
    id_consulta INT(11),
    nome_medico VARCHAR(200),
    nome_utente VARCHAR(200)
);

INSERT INTO view_pergunta14_temp_resultados_consulta
SELECT
    c.id_consulta,
    m.nome AS nome_medico,
    u.nome AS nome_utente
FROM
    consulta c
LEFT JOIN
    medico m ON c.id_medico = m.id_medico
LEFT JOIN
    utente u ON c.id_utente = u.id_utente;


SELECT * FROM view_pergunta14_temp_resultados_consulta;


-- CRIAÇÃO DE UMA TABELA DE CONSULTAS SEM UTENTES
CREATE TABLE IF NOT EXISTS view_pergunta14_consultas_sem_utente (
    id_medico INT(11),
    nome_medico VARCHAR(200),
    quantidade_consultas_vagas INT
);


INSERT INTO view_pergunta14_temp_resultados_consulta
SELECT
    c.id_consulta,
    m.nome AS nome_medico,
    u.nome AS nome_utente
FROM
    consulta c
LEFT JOIN
    medico m ON c.id_medico = m.id_medico
LEFT JOIN
    utente u ON c.id_utente = u.id_utente;


INSERT INTO view_pergunta14_consultas_sem_utente (id_medico, nome_medico, quantidade_consultas_vagas)
SELECT
    c.id_medico,
    m.nome AS nome_medico,
    COUNT(c.id_consulta) AS quantidade_consultas_vagas
FROM
    consulta c
LEFT JOIN
    medico m ON c.id_medico = m.id_medico
WHERE
    c.id_utente IS NULL
GROUP BY
    c.id_medico, m.nome;


SELECT * FROM view_pergunta14_consultas_sem_utente;

-- ------------------------------------------

-- ------------------------------------------
-- PERGUNTA 15 (ADICIONAR NAS CONSULTAS EXAMES MEDICOS E OBSERVAÇÕES)

-- CRIAÇÃO DE OBSERVAÇÕES
INSERT INTO anotacoes_consulta (id_anotacao, anotacao, id_consulta)
VALUES
(1, 'Observações da consulta 1', 1),
(2, 'Observações da consulta 2', 2),
(3, 'Observações da consulta 3', 3),
(4, 'Observações da consulta 4', 4),
(5, 'Observações da consulta 5', 5),
(6, 'Observações da consulta 6', 6),
(7, 'Observações da consulta 7', 7),
(8, 'Observações da consulta 8', 8),
(9, 'Observações da consulta 9', 9),
(10, 'Observações da consulta 10', 10);

-- CRIAÇÃO EXAMES MEDICOS
INSERT INTO exame_medico (id_exame_medico, id_consulta, id_tipo_exame_medico, data_exame, relatorio, imagem)
VALUES
(1, 1, 1, '2024-01-27', 'Relatório do exame 1', 'imagem1.jpg'),
(2, 2, 2, '2024-01-28', 'Relatório do exame 2', 'imagem2.jpg'),
(3, 3, 3, '2024-01-29', 'Relatório do exame 3', 'imagem3.jpg'),
(4, 4, 4, '2024-01-30', 'Relatório do exame 4', 'imagem4.jpg'),
(5, 5, 5, '2024-01-31', 'Relatório do exame 5', 'imagem5.jpg'),
(6, 6, 1, '2024-01-27', 'Relatório do exame 6', 'imagem6.jpg'),
(7, 7, 2, '2024-01-28', 'Relatório do exame 7', 'imagem7.jpg'),
(8, 8, 3, '2024-01-29', 'Relatório do exame 8', 'imagem8.jpg'),
(9, 9, 4, '2024-01-30', 'Relatório do exame 9', 'imagem9.jpg'),
(10, 10, 5, '2024-01-31', 'Relatório do exame 10', 'imagem10.jpg');



SELECT c.id_consulta, c.dia, c.hora_inicio, c.hora_fim,
       a.anotacao AS observacao,
       em.id_tipo_exame_medico, em.data_exame, em.relatorio, em.imagem
FROM consulta c
LEFT JOIN anotacoes_consulta a ON c.id_consulta = a.id_consulta
LEFT JOIN exame_medico em ON c.id_consulta = em.id_consulta;


-- ------------------------------------------

-- ------------------------------------------

-- VIEW PERGUNTA 16 (NUMERO CONSULTAS POR SEMANA DOS MEDICOS)
SELECT
    m.id_medico,
    m.nome AS nome_medico,
    COUNT(c.id_consulta) AS total_consultas
FROM
    medico m
JOIN consulta c ON m.id_medico = c.id_medico
WHERE
    c.dia BETWEEN '2024-01-27' AND '2024-01-31' -- DADA ESCOLHIDA (SEMANA 27-31)
GROUP BY
    m.id_medico
ORDER BY
    total_consultas DESC;


-- ------------------------------------------

-- ------------------------------------------
   
   
-- PERGUNTA 17 (CONSULTAS MEDICO HORARIO BASE)
INSERT INTO consulta (id_consulta , id_medico, id_gabinete, dia, hora_inicio, hora_fim, id_utente)
VALUES
(55567,5 , 4, '2024-01-28', 500, 530 , 2);

CREATE TEMPORARY TABLE view_pergunta17_consultas_fora_horario_base AS
SELECT DISTINCT c.id_consulta,
       m.id_medico,
       m.nome AS nome_medico,
       c.dia,
       c.hora_inicio,
       c.hora_fim
FROM consulta c
JOIN medico m ON c.id_medico = m.id_medico
JOIN horario_base hb ON c.id_medico = hb.id_medico
WHERE c.hora_inicio < hb.hora_inicio OR c.hora_fim > hb.hora_fim;


SELECT * FROM view_pergunta17_consultas_fora_horario_base;

-- ------------------------------------------

-- ------------------------------------------

-- ETAPA 3
   
-- PERGUNTA 18 (APLICAÇÃO DO VIEW)
   
-- VIEW PERGUNTA 11 (HORARIO DOS MEDICOS)
CREATE VIEW view_pergunta11_medicos_horarios AS
SELECT m.id_medico, m.nome, hb.dia_semana, hb.hora_inicio, hb.hora_fim, hb.duracao_consulta
FROM medico m
JOIN horario_base hb ON m.id_medico = hb.id_medico;

SELECT * FROM view_pergunta11_medicos_horarios;


-- VIEW PERGUNTA 12 (MEDICOS COM ESPECIALIDADES)
CREATE VIEW view_pergunta12_medicos_especialidades AS
SELECT
    m.nome AS nome_medico,
    e.nome AS nome_especialidade,
    e.descricao AS descricao_especialidade
FROM
    medico m
JOIN especialidade_medico em ON m.id_medico = em.id_medico
JOIN especialidade e ON em.id_especialidade = e.id_especialidade;

SELECT * FROM view_pergunta12_medicos_especialidades;

-- VIEW PERGUNTA 13 (CONSULTAS DOS MEDICOS)
CREATE VIEW view_pergunta13_consultas_por_medico AS
SELECT
    c.id_consulta,
    m.id_medico,
    m.nome AS nome_medico,
    c.id_gabinete,
    c.dia,
    c.hora_inicio,
    c.hora_fim,
    c.id_utente
FROM
    consulta c
JOIN medico m ON c.id_medico = m.id_medico;
SELECT * FROM view_pergunta13_consultas_por_medico;



-- VIEW PERGUNTA 14 (CONSULTAS SEM UTENTES)
CREATE VIEW view_pergunta14_consultas AS
SELECT
    c.id_consulta,
    m.nome AS nome_medico,
    u.nome AS nome_utente,
    c.dia,
    c.hora_inicio,
    c.hora_fim,
    ac.anotacao AS observacao
FROM
    consulta c
LEFT JOIN
    medico m ON c.id_medico = m.id_medico
LEFT JOIN
    utente u ON c.id_utente = u.id_utente
LEFT JOIN
    anotacoes_consulta ac ON c.id_consulta = ac.id_consulta;
SELECT * FROM view_pergunta14_consultas;



-- ------------------------------------------

-- ------------------------------------------


-- PERGUNTA 19 (FUNCAO CONTAGEM CONSULTAS E APLICAÇÃO NA PERGUNTA 16)
-- CRIAÇÃO DA FUNÇÃO
DELIMITER //

CREATE FUNCTION contarConsultasMedico(
    medico_id INT,
    data_inicial DATE,
    periodo_contagem INT
) RETURNS INT
BEGIN
    DECLARE total_consultas INT;

    SELECT COUNT(id_consulta)
    INTO total_consultas
    FROM consulta
    WHERE id_medico = medico_id
      AND dia BETWEEN data_inicial AND DATE_ADD(data_inicial, INTERVAL periodo_contagem DAY);

    RETURN total_consultas;
END //

DELIMITER ;

-- EXEMPLO DO USO DA FUNCAO (ID MEDICO | DATA INCIAL | NUMERO DE DIAS)
SELECT contarConsultasMedico(1, '2024-01-27', 1) AS total_consultas_medico_1;

-- PERGUNTA 16 APLICANDO A FUNÇÃO

SELECT
    m.id_medico,
    m.nome AS nome_medico,
    contarConsultasMedico(m.id_medico, '2024-01-27', 1) AS total_consultas_semana
FROM
    medico m
ORDER BY
    total_consultas_semana DESC;

   
   
-- ------------------------------------------

-- ------------------------------------------
   
-- PERGUNTA 20 (CRIAÇÃO DO PROCEDIMENTO)

DELIMITER //

CREATE PROCEDURE marcarConsulta(
    IN medico_id INT,
    IN paciente_id INT,
    IN dia_consulta DATE
)
BEGIN
    DECLARE hora_inicio_consulta INT;
    DECLARE hora_fim_consulta INT;
    DECLARE duracao_consulta INT;
    DECLARE vaga_disponivel BOOLEAN;


    SELECT hora_inicio, hora_fim, duracao_consulta
    INTO hora_inicio_consulta, hora_fim_consulta, duracao_consulta
    FROM horario_base
    WHERE id_medico = medico_id AND dia_semana = UPPER(DAYNAME(dia_consulta));


    SET vaga_disponivel = NOT EXISTS (
        SELECT 1
        FROM consulta
        WHERE id_medico = medico_id
          AND dia = dia_consulta
          AND hora_inicio < hora_fim_consulta
          AND hora_fim > hora_inicio_consulta
    );

    IF vaga_disponivel THEN
        INSERT INTO consulta (id_medico, id_gabinete, dia, hora_inicio, hora_fim, id_utente)
        VALUES (medico_id, 1, dia_consulta, hora_inicio_consulta, hora_inicio_consulta + duracao_consulta, paciente_id);
    ELSE
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Não há vaga disponível para a consulta nesse horário.';
    END IF;
END //

DELIMITER ;


CALL marcarConsulta(1, 1, '2024-01-27'); 
SELECT * FROM consulta WHERE id_medico = 1 AND dia = '2024-01-27';

-- ----------------------------------------

-- ----------------------------------------

-- PERGUNTA 21 (CRIAÇÃO DE UM TRIGGER)

DELIMITER //

CREATE TRIGGER valida_horario
BEFORE INSERT ON horario_base
FOR EACH ROW
BEGIN
    DECLARE hora_inicio_valida INT;
    DECLARE hora_fim_valida INT;

    -- VALIDAÇÃO HORA INICIAL
    SET hora_inicio_valida = NEW.hora_inicio - (NEW.hora_inicio DIV 100) * 100;
    IF NEW.hora_inicio < 0 OR NEW.hora_inicio > 2359 OR hora_inicio_valida < 0 OR hora_inicio_valida > 59 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Hora de início inválida. Deve estar entre 00:00 e 23:59.';
    END IF;

    -- VALIDAÇÃO HORA FINAL
    SET hora_fim_valida = NEW.hora_fim - (NEW.hora_fim DIV 100) * 100;
    IF NEW.hora_fim < 0 OR NEW.hora_fim > 2359 OR hora_fim_valida < 0 OR hora_fim_valida > 59 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Hora de fim inválida. Deve estar entre 00:00 e 23:59.';
    END IF;
END //

DELIMITER ;


INSERT INTO horario_base (id_medico, dia_semana, hora_inicio, hora_fim, duracao_consulta)
VALUES
    (1, 'SAB', 800, 1600, 30),
    (2, 'SAB', 900, 1700, 45),
    (3, 'SAB', 1000, 1800, 60);

   
   
   
   -- TESTES:
   -- HORARIO VÁLIDO:
   
   INSERT INTO horario_base (id_medico, dia_semana, hora_inicio, hora_fim, duracao_consulta)
VALUES (4, 'SAB', 1200, 1800, 40);

   -- HORARIO INVÁLIDO (HORA INICIAL NEGATIVA):
    
-- INSERT INTO horario_base (id_medico, dia_semana, hora_inicio, hora_fim, duracao_consulta)
-- VALUES (5, 'SAB', -100, 1800, 40); 

   -- HORARIO INVÁLIDO (HORA SUPERIOR A 23):
-- INSERT INTO horario_base (id_medico, dia_semana, hora_inicio, hora_fim, duracao_consulta)
-- VALUES (6, 'SAB', 1200, 2600, 40);

   -- HORARIO INVÁLIDO (MINUTOS SUPERIORES A 59):
-- INSERT INTO horario_base (id_medico, dia_semana, hora_inicio, hora_fim, duracao_consulta)
-- VALUES (6, 'SAB', 1200, 2260, 40);

 -- EXEMPLOS COM COMENTÁRIO PARA NÃO HAVER ERROS DE COMPILAÇÃO






-- --------------------------------------------------------------------------------

-- FIM -- TRABALHO PRÁTICO DE AVALIAÇÃO - ÉPOCA DE RECURSO
-- AFONSO JACINTO a22208667
-- TOMÁS MATOS a22209049

-- --------------------------------------------------------------------------------

