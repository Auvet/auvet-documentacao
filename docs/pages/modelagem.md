# Modelagem

## Visão Geral

Esta seção apresenta os diagramas de modelagem do sistema AUVET, incluindo o diagrama de classes UML e o modelo de entidade-relacionamento (EER) do banco de dados.

## Diagrama de Classes UML

O diagrama de classes representa a estrutura estática do sistema, mostrando as classes principais e seus relacionamentos.

![Diagrama de Classes AUVET](https://github.com/Auvet/auvet-documentacao/blob/master/docs/imagens/diagrama-de-classes-auvet.png?raw=true)

### Classes Principais

O sistema AUVET é composto pelas seguintes classes principais:

- **Usuario**: Classe base com dados básicos de autenticação
- **Funcionario**: Herda de Usuario, representa funcionários da clínica
- **Tutor**: Herda de Usuario, representa os tutores dos animais
- **Clinica**: Representa a clínica veterinária
- **Animal**: Representa os pacientes da clínica
- **Vacina**: Representa as vacinas aplicadas nos animais
- **Consulta**: Representa as consultas realizadas

### Estrutura das Classes

#### Usuario (Classe Base)
- **Atributos**: cpf, nome, email, senha, dataCadastro
- **Métodos**: login(), atualizarPerfil()

#### Funcionario (Herda de Usuario)
- **Atributos**: cargo, registroProfissional, status, nivelAcesso
- **Métodos**: cadastrarTutor(), atualizarTutor(), cadastrarAnimal(), atualizarAnimal(), agendarConsulta(), registrarConsulta(), gerarRelatorioClinica()

#### Tutor (Herda de Usuario)
- **Atributos**: telefone, endereco
- **Métodos**: cadastrarAnimal(), atualizarAnimal(), agendarConsulta()

#### Clinica
- **Atributos**: cnpj, nome, endereco, telefone, email, dataCadastro
- **Métodos**: cadastrarFuncionario(), gerarRelatorio()

#### Animal
- **Atributos**: id, nome, especie, raca, sexo, idade, peso
- **Métodos**: adicionarVacina(), atualizarVacina(), listarVacinas(), agendarConsulta()

#### Vacina
- **Atributos**: id, nome, fabricante, dataAplicacao, dataValidade, animalId, clinicaCnpj
- **Métodos**: aplicarVacina()

#### Consulta
- **Atributos**: id, data, hora, motivo, status, observacoes, animalId, funcionarioCpf, clinicaCnpj
- **Métodos**: marcar(), cancelar(), concluir()

### Relacionamentos

1. **Herança**: Funcionario e Tutor herdam de Usuario
2. **Clinica ↔ Funcionario**: Relacionamento N:N (um funcionário pode trabalhar em várias clínicas, uma clínica pode ter vários funcionários)
3. **Clinica ↔ Tutor**: Relacionamento N:N (um tutor pode ter animais em várias clínicas, uma clínica pode ter vários tutores)
4. **Clinica ↔ Animal**: Relacionamento N:N (um animal pode estar cadastrado em várias clínicas, uma clínica pode ter vários animais)
5. **Tutor → Animal**: Um tutor pode ter vários animais (1:N)
6. **Animal → Vacina**: Um animal pode receber várias vacinas (1:N)
7. **Animal → Consulta**: Um animal pode ter várias consultas (1:N)
8. **Funcionario → Consulta**: Um funcionário pode realizar várias consultas (1:N)
9. **Clinica → Consulta**: Uma clínica pode ter várias consultas (1:N)
10. **Clinica → Vacina**: Uma clínica pode ter várias vacinas (1:N)

## Modelo de Entidade-Relacionamento (EER)

O diagrama EER representa a estrutura do banco de dados, mostrando as tabelas, seus atributos e relacionamentos.

![Diagrama EER AUVET](https://github.com/Auvet/auvet-documentacao/blob/master/docs/imagens/er-diagram-auvet.png?raw=true)

### Estrutura do Banco de Dados

O diagrama EER mostra a implementação das classes no banco de dados MySQL. As tabelas principais são:

#### Tabela: usuario
- **Chave Primária**: cpf (VARCHAR(11))
- **Atributos**: nome, email (único), senha, data_cadastro (DATETIME)
- **Índices**: idx_email (email)
- **Relacionamentos**: Pode ser Funcionario ou Tutor (1:1)

#### Tabela: tutor
- **Chave Primária**: cpf (VARCHAR(11))
- **Atributos**: telefone (VARCHAR(20)), endereco (VARCHAR(200))
- **Relacionamentos**:
  - Referencia Usuario (1:1) com ON DELETE CASCADE
  - Possui vários Animais (1:N)
  - Relacionamento N:N com Clínica através de tutor_clinica

#### Tabela: funcionario
- **Chave Primária**: cpf (VARCHAR(11))
- **Atributos**: cargo (VARCHAR(50)), registro_profissional (VARCHAR(50)), status (VARCHAR(20), default 'ativo'), nivel_acesso (INT, default 1)
- **Índices**: idx_status (status)
- **Relacionamentos**: 
  - Referencia Usuario (1:1) com ON DELETE CASCADE
  - Pode administrar uma Clínica (1:1) através de administrador_cpf
  - Relacionamento N:N com Clínica através de funcionario_clinica
  - Realiza várias Consultas (1:N)

#### Tabela: clinica
- **Chave Primária**: cnpj (VARCHAR(14))
- **Atributos**: nome (VARCHAR(100)), endereco (VARCHAR(200)), telefone (VARCHAR(20)), email (VARCHAR(100)), data_cadastro (DATETIME), administrador_cpf (VARCHAR(11))
- **Índices**: idx_nome (nome)
- **Relacionamentos**:
  - Possui um administrador (Funcionario) através de administrador_cpf
  - Relacionamento N:N com Funcionario através de funcionario_clinica
  - Relacionamento N:N com Tutor através de tutor_clinica
  - Relacionamento N:N com Animal através de animal_clinica
  - Possui várias Consultas (1:N)
  - Possui várias Vacinas (1:N)

#### Tabela: funcionario_clinica
- **Chave Primária Composta**: (funcionario_cpf, clinica_cnpj)
- **Atributos**: funcionario_cpf (VARCHAR(11)), clinica_cnpj (VARCHAR(14))
- **Índices**: idx_funcionario (funcionario_cpf), idx_clinica (clinica_cnpj)
- **Relacionamentos**:
  - Referencia Funcionario com ON DELETE CASCADE
  - Referencia Clínica com ON DELETE CASCADE
- **Propósito**: Tabela de relacionamento N:N entre Funcionário e Clínica

#### Tabela: tutor_clinica
- **Chave Primária Composta**: (tutor_cpf, clinica_cnpj)
- **Atributos**: tutor_cpf (VARCHAR(11)), clinica_cnpj (VARCHAR(14))
- **Índices**: idx_tutor (tutor_cpf), idx_clinica (clinica_cnpj)
- **Relacionamentos**:
  - Referencia Tutor com ON DELETE CASCADE
  - Referencia Clínica com ON DELETE CASCADE
- **Propósito**: Tabela de relacionamento N:N entre Tutor e Clínica

#### Tabela: animal
- **Chave Primária**: id (INT AUTO_INCREMENT)
- **Atributos**: nome (VARCHAR(50)), especie (VARCHAR(50)), raca (VARCHAR(50)), sexo (CHAR(1)), idade (INT), peso (DECIMAL(5,2)), tutor_cpf (VARCHAR(11))
- **Índices**: idx_tutor (tutor_cpf), idx_nome (nome)
- **Relacionamentos**:
  - Pertence a um Tutor (N:1) com ON DELETE CASCADE
  - Relacionamento N:N com Clínica através de animal_clinica
  - Possui várias Consultas (1:N)
  - Recebe várias Vacinas (1:N)

#### Tabela: animal_clinica
- **Chave Primária Composta**: (animal_id, clinica_cnpj)
- **Atributos**: animal_id (INT), clinica_cnpj (VARCHAR(14))
- **Índices**: idx_animal (animal_id), idx_clinica (clinica_cnpj)
- **Relacionamentos**:
  - Referencia Animal com ON DELETE CASCADE
  - Referencia Clínica com ON DELETE CASCADE
- **Propósito**: Tabela de relacionamento N:N entre Animal e Clínica

#### Tabela: consulta
- **Chave Primária**: id (INT AUTO_INCREMENT)
- **Atributos**: data (DATE), hora (DATETIME), motivo (VARCHAR(200)), status (VARCHAR(20), default 'agendada'), observacoes (TEXT), animal_id (INT), funcionario_cpf (VARCHAR(11)), clinica_cnpj (VARCHAR(14))
- **Índices**: idx_data (data), idx_status (status), idx_animal (animal_id), idx_funcionario (funcionario_cpf), idx_clinica (clinica_cnpj)
- **Relacionamentos**:
  - Pertence a um Animal (N:1) com ON DELETE CASCADE
  - É realizada por um Funcionario (N:1)
  - Pertence a uma Clínica (N:1) com ON DELETE CASCADE

#### Tabela: vacina
- **Chave Primária**: id (INT AUTO_INCREMENT)
- **Atributos**: nome (VARCHAR(50)), fabricante (VARCHAR(50)), data_aplicacao (DATE), data_validade (DATE), animal_id (INT), clinica_cnpj (VARCHAR(14))
- **Índices**: idx_animal (animal_id), idx_data_aplicacao (data_aplicacao), idx_clinica (clinica_cnpj)
- **Relacionamentos**:
  - Pertence a um Animal (N:1) com ON DELETE CASCADE
  - Pertence a uma Clínica (N:1) com ON DELETE CASCADE

### Relacionamentos no Banco

1. **usuario ↔ funcionario**: Relacionamento 1:1 (herança) com ON DELETE CASCADE
2. **usuario ↔ tutor**: Relacionamento 1:1 (herança) com ON DELETE CASCADE
3. **clinica ↔ funcionario (administrador)**: Uma clínica é administrada por um funcionário (1:1) através de administrador_cpf
4. **clinica ↔ funcionario (trabalho)**: Relacionamento N:N através da tabela funcionario_clinica (um funcionário pode trabalhar em várias clínicas, uma clínica pode ter vários funcionários)
5. **clinica ↔ tutor**: Relacionamento N:N através da tabela tutor_clinica (um tutor pode ter animais em várias clínicas, uma clínica pode ter vários tutores)
6. **tutor ↔ animal**: Um tutor pode ter vários animais (1:N) com ON DELETE CASCADE
7. **clinica ↔ animal**: Relacionamento N:N através da tabela animal_clinica (um animal pode estar cadastrado em várias clínicas, uma clínica pode ter vários animais)
8. **animal ↔ consulta**: Um animal pode ter várias consultas (1:N) com ON DELETE CASCADE
9. **funcionario ↔ consulta**: Um funcionário pode realizar várias consultas (1:N)
10. **clinica ↔ consulta**: Uma clínica pode ter várias consultas (1:N) com ON DELETE CASCADE
11. **animal ↔ vacina**: Um animal pode receber várias vacinas (1:N) com ON DELETE CASCADE
12. **clinica ↔ vacina**: Uma clínica pode ter várias vacinas (1:N) com ON DELETE CASCADE

## Histórico de Versões

| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.0.0 | 2025-09-13 | Izabella Alves | Versão inicial da modelagem com diagramas UML e EER do sistema AUVET |
| 2.0.0 | 2025-11-12 | Sistema | Atualização da modelagem com estrutura final do banco de dados, incluindo tabelas de relacionamento N:N (funcionario_clinica, tutor_clinica, animal_clinica) e relacionamentos diretos de Consulta e Vacina com Clínica |

