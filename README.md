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
