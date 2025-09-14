# AUVET - Arquitetura Modular

## Visão Geral

O Auvet foi desenvolvido seguindo uma arquitetura modular baseada em camadas (layered architecture) e padrões de design que promovem separação de responsabilidades, reutilização de código e manutenibilidade.

## Princípios Arquiteturais

### 1. Separação de Responsabilidades
- Cada camada tem uma responsabilidade específica e bem definida
- Baixo acoplamento entre módulos
- Alta coesão dentro de cada módulo

### 2. Modularidade
- Organização por domínios de negócio
- Módulos independentes e reutilizáveis
- Facilita manutenção e evolução

### 3. Inversão de Dependência
- Dependências apontam para abstrações, não implementações
- Facilita testes unitários
- Permite troca de implementações

## Estrutura de Camadas

```
┌─────────────────────────────────────────┐
│              PRESENTATION               │
│           (Controllers/Routes)          │
├─────────────────────────────────────────┤
│               BUSINESS                  │
│              (Services)                 │
├─────────────────────────────────────────┤
│               PERSISTENCE               │
│            (Repositories)               │
├─────────────────────────────────────────┤
│               DATABASE                  │
│            (Prisma ORM)                 │
└─────────────────────────────────────────┘
```

## Organização Modular

### Estrutura de Módulos

Cada módulo representa um domínio de negócio e contém:

```
src/modules/[MODULO]/
├── controller.ts    # Camada de Apresentação
├── service.ts       # Camada de Negócio
├── repository.ts    # Camada de Persistência
└── [MODULO].test.ts # Testes Unitários
```

### Módulos Implementados

1. **Usuario** - Gerenciamento de usuários do sistema
2. **Funcionario** - Gestão de funcionários da clínica
3. **Clinica** - Gestão de clínicas veterinárias
4. **Tutor** - Gestão de tutores dos animais
5. **Animal** - Gestão de animais
6. **Consulta** - Gestão de consultas veterinárias
7. **Vacina** - Gestão de vacinas

## Fluxo de Dados

### Fluxo Típico de uma Requisição

```
Cliente HTTP → Controller → Service → Repository → Prisma ORM → MySQL Database
```

### Exemplo Prático: Criação de Funcionário

1. **Controller** (`FuncionarioController.create`)
   - Recebe requisição HTTP
   - Valida dados de entrada
   - Chama Service

2. **Service** (`FuncionarioService.createFuncionario`)
   - Implementa regras de negócio
   - Orquestra criação de Usuario + Funcionario
   - Chama Repository

3. **Repository** (`FuncionarioRepository.create`)
   - Acessa banco de dados via Prisma
   - Executa operações de persistência

4. **Database** (MySQL via Prisma)
   - Armazena dados
   - Retorna resultado

## Histórico de Versões

| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.0.0 | 2025-09-13 | Izabella Alves | Versão inicial da documentação de arquitetura com estrutura modular e fluxo de dados |

