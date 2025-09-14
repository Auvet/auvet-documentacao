# Modelagem

## Visão Geral

Esta seção apresenta os diagramas de modelagem do sistema AUVET, incluindo o diagrama de classes UML e o modelo de entidade-relacionamento (EER) do banco de dados.

## Diagrama de Classes UML

O diagrama de classes representa a estrutura estática do sistema, mostrando as classes principais e seus relacionamentos.

![Diagrama de Classes AUVET](./imagens/diagrama-de-classes-auvet.png)

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
- **Atributos**: id, nome, especie, raca, sexo, dataDeNascimento, peso
- **Métodos**: adicionarVacina(), atualizarVacina(), listarVacinas(), agendarConsulta()

#### Vacina
- **Atributos**: id, nome, fabricante, dataAplicacao, dataValidade
- **Métodos**: aplicarVacina()

#### Consulta
- **Atributos**: id, data, hora, motivo, status, observacoes
- **Métodos**: marcar(), cancelar(), concluir()

### Relacionamentos

1. **Herança**: Funcionario e Tutor herdam de Usuario
2. **Clinica → Funcionario**: Uma clínica emprega vários funcionários (1:N)
3. **Tutor → Animal**: Um tutor pode ter vários animais (1:N)
4. **Animal → Vacina**: Um animal pode receber várias vacinas (1:N)
5. **Animal → Consulta**: Um animal pode ter várias consultas (1:N)
6. **Funcionario → Consulta**: Um funcionário pode realizar várias consultas (1:N)

## Modelo de Entidade-Relacionamento (EER)

O diagrama EER representa a estrutura do banco de dados, mostrando as tabelas, seus atributos e relacionamentos.

![Diagrama EER AUVET](./imagens/eer-diagram-auvet.png)

### Estrutura do Banco de Dados

O diagrama EER mostra a implementação das classes no banco de dados MySQL usando Prisma ORM. As tabelas principais são:

#### Tabela: clinica
- **Chave Primária**: cnpj (VARCHAR(14))
- **Atributos**: nome, endereco, telefone, email, dataCadastro, administradorCpf
- **Relacionamento**: Possui um administrador (Funcionario)

#### Tabela: usuario
- **Chave Primária**: cpf (VARCHAR(11))
- **Atributos**: nome, email (único), senha, dataCadastro
- **Relacionamentos**: Pode ser Funcionario ou Tutor (1:1)

#### Tabela: funcionario
- **Chave Primária**: cpf (VARCHAR(11))
- **Atributos**: cargo, registroProfissional, status, nivelAcesso
- **Relacionamentos**: 
  - Referencia Usuario (1:1)
  - Pode administrar várias Clínicas (1:N)
  - Realiza várias Consultas (1:N)

#### Tabela: tutor
- **Chave Primária**: cpf (VARCHAR(11))
- **Atributos**: telefone, endereco
- **Relacionamentos**:
  - Referencia Usuario (1:1)
  - Possui vários Animais (1:N)

#### Tabela: animal
- **Chave Primária**: id (AUTO_INCREMENT)
- **Atributos**: nome, especie, raca, sexo, idade, peso
- **Relacionamentos**:
  - Pertence a um Tutor (N:1)
  - Possui várias Consultas (1:N)
  - Recebe várias Vacinas (1:N)

#### Tabela: consulta
- **Chave Primária**: id (AUTO_INCREMENT)
- **Atributos**: data, hora, motivo, status, observacoes
- **Relacionamentos**:
  - Pertence a um Animal (N:1)
  - É realizada por um Funcionario (N:1)

#### Tabela: vacina
- **Chave Primária**: id (AUTO_INCREMENT)
- **Atributos**: nome, fabricante, dataAplicacao, dataValidade
- **Relacionamento**: Pertence a um Animal (N:1)

### Relacionamentos no Banco

1. **clinica ↔ funcionario**: Uma clínica é administrada por um funcionário, um funcionário pode administrar várias clínicas
2. **usuario ↔ funcionario**: Relacionamento 1:1 (herança)
3. **usuario ↔ tutor**: Relacionamento 1:1 (herança)
4. **tutor ↔ animal**: Um tutor pode ter vários animais (1:N)
5. **animal ↔ vacina**: Um animal pode receber várias vacinas (1:N)
6. **animal ↔ consulta**: Um animal pode ter várias consultas (1:N)
7. **funcionario ↔ consulta**: Um funcionário pode realizar várias consultas (1:N)

## Histórico de Versões

| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.0.0 | 2025-09-13 | Izabella Alves | Versão inicial da modelagem com diagramas UML e EER do sistema AUVET |

