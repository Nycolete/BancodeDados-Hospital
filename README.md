# BancodeDados-Hospital
## Criando um Banco de Daos para um Hospital 


### <sumary> 1. Analise a seguinte descrição e extraia dela os requisitos para o banco de dados: </sumary>

<details>
 O hospital necessita de um sistema para sua área clínica que ajude a controlar consultas realizadas. Os médicos podem ser generalistas, especialistas ou residentes e têm seus dados pessoais cadastrados em planilhas digitais. Cada médico pode ter uma ou mais especialidades, que podem ser pediatria, clínica geral, gastroenterologia e dermatologia. Alguns registros antigos ainda estão em formulário de papel, mas será necessário incluir esses dados no novo sistema.

 Os pacientes também precisam de cadastro, contendo dados pessoais (nome, data de nascimento, endereço, telefone e e-mail), documentos (CPF e RG) e convênio. Para cada convênio, são registrados nome, CNPJ e tempo de carência.

 As consultas também têm sido registradas em planilhas, com data e hora de realização, médico responsável, paciente, valor da consulta ou nome do convênio, com o número da carteira. Também é necessário indicar na consulta qual a especialidade buscada pelo paciente.

 Deseja-se ainda informatizar a receita do médico, de maneira que, no encerramento da consulta, ele possa registrar os medicamentos receitados, a quantidade e as instruções de uso. A partir disso, espera-se que o sistema imprima um relatório da receita ao paciente ou permita sua visualização via internet.

Com as informações que você extrair da sua análise, desenhe um Diagrama ER para esse projeto de banco de dados.

</details>

![Untitled](https://user-images.githubusercontent.com/125493277/236573861-006fe6a4-f41b-4932-ba75-93a3b762df5f.png)

### <sumary> 2. Expandindo o Modelo ER desenvolvido, montando o banco de dados e criando as tabelas para o início dos testes. </sumary>

<details>
No hospital, as internações têm sido registradas por meio de formulários eletrônicos que gravam os dados em arquivos. 

Para cada internação, são anotadas a data de entrada, a data prevista de alta e a data efetiva de alta, além da descrição textual dos procedimentos a serem realizados. 

As internações precisam ser vinculadas a quartos, com a numeração e o tipo. 

Cada tipo de quarto tem sua descrição e o seu valor diário (a princípio, o hospital trabalha com apartamentos, quartos duplos e enfermaria).

Também é necessário controlar quais profissionais de enfermaria estarão responsáveis por acompanhar o paciente durante sua internação. Para cada enfermeiro(a), é necessário nome, CPF e registro no conselho de enfermagem (CRE).

A internação, obviamente, é vinculada a um paciente – que pode se internar mais de uma vez no hospital – e a um único médico responsável.
 
 Faça a ligação do diagrama acima ao diagrama desenvolvido na atividade antrior, construindo relacionamentos com entidades relacionadas. E eleve o seu diagrama para que já selecionando os tipos de dados que serão trabalhados e em quais situações. 

Por último, crie um script SQL para a geração do banco de dados e para instruções de montagem de cada uma das entidades/tabelas presentes no diagrama completo (considerando as entidades do diagrama da atividade anterior e as novas entidades propostas no diagrama acima). Também crie tabelas para relacionamentos quando necessário. Aplique colunas e chaves primárias e estrangeiras.
Use ferramentas, como ERPlus, Lucidchart, draw.io (via web) e MySQL Workbench, ou mesmo um editor de imagens para o diagrama. 

Você pode utilizar o MySQL Workbench ou o DBdiagram.io para montar os scripts SQL.

Importante: desse modelo já devemos gerar a etapa lógica da nossa modelagem! </details> 
  
  
![Hospital Fundamental](https://github.com/Nycolete/BancodeDados-Hospital/assets/125493277/cb64a6ad-540a-45cd-96ea-acc2c46ec8e4)


 ```
 
CREATE DATABASE hospital_fundamental ;

USE hospital_fundamental;

CREATE TABLE `Medico` (
  `id_medico` int(11) PRIMARY KEY,
  `name_medico` varchar(210),
  `data_nascimento` date,
  `telefone` int(11),
  `email` varchar(255),
  `data_formacao` date,
  `crm` varchar(12),
   `area_atuacao` varchar,
  `especialidade` varchar(12),
  `cpf` int(11),
  `created_at` timestamp
);

CREATE TABLE `Paciente` (
  `id_paciente` int(11) PRIMARY KEY,
  `name_paciente` varchar(210),
  `data_nascimento` date,
  `endereco` varchar(255),
  `telefone` int(11),
  `email` varchar(255),
  `cpf` int(11),
  `rg` int(9),
  `name_convenio` varchar(210),
  `id_convenio` int(11),
  `created_at` timestamp,
  `id_internacao` int(11),
  `numero_internacoes` int(3)
);

CREATE TABLE `Convenio` (
  `id_convenio` int(11) PRIMARY KEY,
  `name_convenio` varchar(210),
  `cnpj` int(14),
  `tempo_carencia` int(2),
  `created_at` timestamp
);

CREATE TABLE `Consultas` (
  `data_consulta` date,
  `hora_consulta` varchar(5),
  `name_medico` varchar(210),
  `id_medico` int(11),
  `name_paciente` varchar(210),
  `id_paciente` int(11),
  `valor` varchar(255),
  `name_convenio` varchar(210),
  `id_convenio` int(11),
  `numero_carteira` int,
  `especialidade_consulta` varchar(255)
);

CREATE TABLE `Receita` (
  `name_medico` varchar(210),
  `id_medico` int(11),
  `name_paciente` varchar(210),
  `id_paciente` int(11),
  `medicamentos` varchar(255),
  `quantidade_medicamentos` int(2),
  `inctrucoes` varchar(255)
);

CREATE TABLE `Internacao` (
  `id_internacao` int(11) PRIMARY KEY,
  `Data_entrada` date,
  `Data_prev_alta` date,
  `Data_alta` date,
  `Procedimento` varchar(255),
  `created_at` timestamp,
  `id_paciente` int(11),
  `name_paciente` varchar(210),
  `id_quarto` int(11),
  `id_enfermeiro` int(11)
);

CREATE TABLE `Quarto` (
  `id_quarto` int(11) PRIMARY KEY,
  `numero_quarto` int (4),
  `tipo_quarto` varchar(255)
);

CREATE TABLE `TipoDeQuarto` (
  `id_quarto` int(11),
  `tipo_quarto` varchar(255),
  `valor_diaria` varchar(10)
);

CREATE TABLE `Enfermeiro` (
  `id_enfermeiro` int(11),
  `nome_enfermeiro` varchar(210),
  `cpf_enfermeiro` varchar(14),
  `cre` varchar(255)
);

 ```
#### Código e diagrama no [dbdiagram](https://dbdiagram.io/d/64556173dca9fb07c4977fa1)

### <sumary> 3. Alimentando o Banco de dados.</sumary>

#### Crie scripts de povoamento das tabelas desenvolvidas na atividade anterior.


#### - Inclua ao menos dez médicos de diferentes especialidades. E ao menos sete especialidades (considere a afirmação de que “entre as especialidades há pediatria, clínica geral, gastrenterologia e dermatologia”).
  
![pacientes01](https://github.com/Nycolete/BancodeDados-Hospital/assets/125493277/d5366ae6-fd42-4784-abe0-8b51a21b2ccb)



```
INSERT INTO Medico (id_medico, name_medico, data_nascimento, telefone, email, data_formacao, crm, area_atuação, especialidade, cpf) VALUES
(1, 'Dr. João Silva', '1980-05-15', 1234567890, 'joao.silva@example.com', '2005-10-20', '123456', 'Clínica Geral', 'Cardiologia', 12345678901),
(2, 'Dra. Maria Santos', '1975-08-22', 9876543210, 'maria.santos@example.com', '2000-06-10', '654321', 'Clínica Geral', 'Dermatologia', 18765432109),
(3, 'Dr. Pedro Rocha', '1982-03-10', 1357924680, 'pedro.rocha@example.com', '2008-12-05', '987654', 'Clínica Geral', 'Ortopedia', 13579246801),
(4, 'Dra. Ana Lima', '1988-11-28', 2468135790, 'ana.lima@example.com', '2013-09-15', '456789', 'Clínica Geral', 'Pediatria', 14681357901),
(5, 'Dr. Ricardo Mendes', '1973-02-02', 3692581470, 'ricardo.mendes@example.com', '1998-07-25', '789012', 'Clínica Geral', 'Ginecologia', 16925814701),
(6, 'Dra. Fernanda Costa', '1985-06-18', 1592634870, 'fernanda.costa@example.com', '2010-04-02', '012345', 'Clínica Geral', 'Oftalmologia', 15926348701),
(7, 'Dr. Gabriel Santos', '1979-09-30', 3571592460, 'gabriel.santos@example.com', '2004-11-17', '345678', 'Clínica Geral', 'Neurologia', 15715924601),
(8, 'Dra. Luiza Mendonça', '1987-04-07', 2581473690, 'luiza.mendonca@example.com', '2012-01-12', '678901', 'Clínica Geral', 'Psicologia', 25814736901),
(9, 'Dr. Helena Costa', '1981-07-24', 9876543211, 'helena.costa@example.com', '2006-08-08', '890123', 'Clínica Geral', 'Pediatra', 18765432110),
(10, 'Dra. Andréa Rodrigues', '1976-12-12', 1234567891, 'andrea.rodrigues@example.com', '2001-03-22', '234567', 'Clínica Geral', 'Urologia', 12345678910);
```


  
  #### - Inclua ao menos 15 pacientes

![pp003](https://github.com/Nycolete/BancodeDados-Hospital/assets/125493277/fda303e0-be8b-48ee-9c2d-5183eceb8951)


```
INSERT INTO `Paciente` (`id_paciente`, `name_paciente`, `data_nascimento`, `endereco`, `telefone`, `email`, `cpf`, `rg`, `name_convenio`, `id_convenio`, `id_internacao`, `numero_internacoes`)
VALUES
(100, 'Maria Silva', '1990-03-15', 'Rua das Flores, 123', 1122334455, 'maria.silva@email.com', 12345678900, 123456789, 'Convênio A', 1, 101, 3),
(200, 'João Santos', '1985-07-10', 'Avenida dos Anjos, 456', 9988776655, 'joao.santos@email.com', 98765432100, 987654321, 'Convênio B', 20, NULL, 0),
(300, 'Ana Souza', '2000-01-20', 'Travessa das Estrelas, 789', 7788995566, 'ana.souza@email.com', 45678912300, 456789123, 'Convênioe C', 30, 103, 1),
(400, 'Pedro Oliveira', '1982-09-05', 'Praça dos Sonhos, 654', 6655443322, 'pedro.oliveira@email.com', 65498732100, 654987321, 'Convênio A', 10, NULL, 0),
(500, 'Camila Rodrigues', '1995-11-25', 'Alameda dos Ventos, 987', 5544332211, 'camila.rodrigues@email.com', 78945612300, 789456123, 'Convênio B', 20, NULL, 0),
(600, 'Rafael Lima', '1988-04-08', 'Rua das Montanhas, 321', 3344556677, 'rafael.lima@email.com', 32165498700, 321654987, 'Convênio C', 30, 105, 2),
(700, 'Fernanda Costa', '1992-06-12', 'Avenida das Águas, 753', 7766554433, 'fernanda.costa@email.com', 98765432100, 987654321, 'Convênio A', 10, NULL, 0),
(800, 'Luiz Pereira', '1980-02-03', 'Travessa dos Bosques, 159', 8899776655, 'luiz.pereira@email.com', 65478932100, 654789321, 'Convênio B', 20, 107, 1),
(900, 'Carolina Fernandes', '1997-12-10', 'Rua das Praias, 753', 5533441122, 'carolina.fernandes@email.com', 12378945600, 123789456, 'Convênio C', 30, NULL, 0),
(1000, 'Gustavo Santos', '1993-08-18', 'Avenida dos Campos, 357', 9988776655, 'gustavo.santos@email.com', 98732165400, 987321654, 'Convênio A', 10, NULL, 0),
(1100, 'Juliana Almeida', '1987-05-30', 'Travessa das Ruas, 456', 8899776655, 'juliana.almeida@email.com', 78945612300, 789456123, 'Convênio B', 20, 109, 4),
(1200, 'Marcos Oliveira', '1984-03-09', 'Rua dos Parques, 852', 6677889966, 'marcos.oliveira@email.com', 32165498700, 321654987, 'Convênio C', 30, NULL, 0),
(1300, 'Larissa Costa', '1998-01-28', 'Avenida das Flores, 753', 5544332211, 'larissa.costa@email.com', 98765432100, 987654321, 'Convênio A', 10, NULL, 0),
(1400, 'Renato Sousa', '1981-09-14', 'Travessa dos Ventos, 951', 8899776655, 'renato.sousa@email.com', 65478932100, 654789321, 'Convênio B', 20, 111, 2),
(1500, 'Isabela Lima', '1994-11-22', 'Rua dos Bosques, 753', 6677889966, 'isabela.lima@email.com', 12378945600, 123789456, 'Convênio C', 30, NULL, 0);

```



#### - Registre 20 consultas de diferentes pacientes e diferentes médicos (alguns pacientes realizam mais que uma consulta). As consultas devem ter ocorrido entre 01/01/2015 e 01/01/2022. Ao menos dez consultas devem ter receituário com dois ou mais medicamentos.

![consultasdb](https://github.com/Nycolete/BancodeDados-Hospital/assets/125493277/8028388e-4e31-4a45-a09a-1d723735a18c)



```
INSERT INTO Consultas (data_consulta, hora_consulta, name_medico, id_medico, name_paciente, id_paciente, valor, name_convenio, id_convenio, numero_carteira, especialidade_consulta) VALUES
('2015-03-15', '10:00', 'Dr. João Silva', 1, 'Maria Silva', 100, '150.00', 'Convênio A', 10, 123456, 'Cardiologia'),
('2016-05-20', '14:30', 'Dra. Maria Santos', 2, 'João Santos', 200, '100.00', 'Convênio B', 20, 789012, 'Dermatologia'),
('2017-09-02', '11:15', 'Dr. Pedro Rocha', 3, 'Ana Souza', 300, '120.00', 'Convênio C', 30, 345678, 'Ortopedia'),
('2017-12-10', '09:30', 'Dr. João Silva', 1, 'Pedro Oliveira', 400, '150.00', 'Convênio A', 10, 901234, 'Cardiologia'),
('2018-02-14', '16:45', 'Dra. Ana Lima', 4, 'Camila Rodrigues', 500, '80.00', 'Convênio B', 20, 567890, 'Clínico Geral'),
('2018-05-05', '14:00', 'Dra. Maria Santos', 2, 'Maria Silva', 100, '100.00', 'Convênio A', 10, 123456, 'Dermatologia'),
('2018-06-20', '10:30', 'Dr. Pedro Rocha', 3, 'João Santos', 200, '120.00', 'Convênio B', 20, 789012, 'Ortopedia'),
('2019-01-08', '08:45', 'Dr. João Silva', 1, 'Ana Souza', 300, '150.00', 'Convênio C', 30, 345678, 'Cardiologia'),
('2019-04-17', '13:15', 'Dra. Ana Lima', 4, 'Pedro Oliveira', 400, '80.00', 'Convênio A', 10, 901234, 'Clínico Geral'),
('2019-09-30', '15:30', 'Dra. Andréa Rodrigues', 10, 'Camila Rodrigues', 500, '100.00', 'Convênio B', 20, 567890, 'Urologia'),
('2020-02-12', '11:00', 'Dr. Pedro Rocha', 3, 'Maria Silva', 100, '120.00', 'Convênio A', 10, 123456, 'Ortopedia'),
('2020-07-25', '09:45', 'Dra. Fernanda Costa', 6, 'João Santos', 200, '150.00', 'Convênio B', 20, 789012, 'Oftalmologia'),
('2020-08-30', '16:30', 'Dra. Ana Lima', 4, 'Ana Souza', 300, '80.00', 'Convênio C', 30, 345678, 'Clínico Geral'),
('2021-02-05', '14:20', 'Dra. Maria Santos', 2, 'Pedro Oliveira', 400, '100.00', 'Convênio A', 10, 901234, 'Dermatologia'),
('2021-03-10', '09:50', 'Dr. Pedro Rocha', 3, 'Camila Rodrigues', 500, '120.00', 'Convênio B', 20, 567890, 'Ortopedia'),
('2021-06-15', '12:10', 'Dr. João Silva', 1, 'Maria Silva', 100, '150.00', 'Convênio A', 10, 123456, 'Cardiologia'),
('2021-07-20', '15:40', 'Dra. Ana Lima', 4, 'João Santos', 200, '80.00', 'Convênio B', 20, 789012, 'Clínico Geral'),
('2022-01-05', '10:20', 'Dra. Maria Santos', 2, 'Ana Souza', 300, '100.00', 'Convênio C', 30, 345678, 'Dermatologia'),
('2022-01-10', '13:30', 'Dra. Luiza Mendonça', 8, 'Pedro Oliveira', 4, '120.00', 'Convênio A', 10, 901234, 'Psicologia'),
('2022-01-15', '11:50', 'Dr. João Silva', 1, 'Camila Rodrigues', 5, '150.00', 'Convênio B', 10, 567890, 'Cardiologia'),
('2022-01-20', '14:15', 'Dra. Ana Lima', 4, 'Maria Silva', 1, '80.00', 'Convênio A', 40, 123456, 'Clínico Geral');

```



#### - Inclua ao menos quatro convênios médicos, associe ao menos cinco pacientes e cinco consultas.

![conveniodb](https://github.com/Nycolete/BancodeDados-Hospital/assets/125493277/7f9e44e6-18f0-4398-a320-abec153f80b5)


  ```
INSERT INTO `Convenio` (`id_convenio`, `name_convenio`, `cnpj`, `tempo_carencia`) VALUES
(10, 'Convênio A', 12345678901234, 6),
(20, 'Convênio B', 23456789012345, 3),
(30, 'Convênio C', 34567890123456, 12),
(40, 'Convênio D', 45678901234567, 0);
```
 
