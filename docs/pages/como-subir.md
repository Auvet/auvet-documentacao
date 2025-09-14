# Como subir o projeto

## Visão Geral

O Auvet é um sistema de gerenciamento de clínicas veterinárias desenvolvido com Node.js, TypeScript, Express e Prisma ORM. Este tutorial irá guiá-lo através da instalação e configuração completa da aplicação.

## Stack Tecnológica

### Backend
- **Node.js** 18+ - Runtime JavaScript
- **TypeScript** 5.0+ - Linguagem de programação
- **Express.js** 4.18+ - Framework web
- **Prisma ORM** 5.19+ - ORM para banco de dados
- **MySQL** 8.0 - Banco de dados relacional
- **Jest** - Framework de testes
- **Docker** - Containerização

### Banco de Dados
- **MySQL 8.0** - Sistema de gerenciamento de banco de dados
- **Prisma** - ORM e migrações

### Infraestrutura
- **Docker** - Containerização
- **Docker Compose** - Orquestração de containers

## Pré-requisitos

Antes de começar, certifique-se de ter instalado:

- **Docker** (versão 20.10+)
- **Docker Compose** (versão 2.0+)
- **Node.js** (versão 18+) - apenas para desenvolvimento local
- **Git** - para clonar o repositório


##  Instalação

### 1. Clone o Repositório

```bash
git clone https://github.com/Auvet/auvet-backend
cd auvet-backend
```

### 2. Configuração do Ambiente

Crie um arquivo `.env` baseado no `env.example`:

```bash
cp env.example .env
```

Edite o arquivo `.env` com suas configurações:

```env
DATABASE_URL="mysql://auvet_user:auvet123@localhost:3307/auvet_db"
NODE_ENV=development
PORT=3000
CORS_ORIGIN=http://localhost:3000
```

### 3. Executando com Docker (Recomendado)

#### Iniciar a Aplicação

```bash
docker-compose up --build

# Ou executar em background
docker-compose up -d --build
```

#### Verificar se os Serviços Estão Rodando

```bash
# Ver status dos containers
docker-compose ps

# Ver logs do backend
docker-compose logs backend

# Ver logs do MySQL
docker-compose logs mysql
```

## Histórico de Versões

| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.0.0 | 2025-09-13 | Izabella Alves | Versão inicial do guia de instalação com instruções para Docker e configuração do ambiente |

