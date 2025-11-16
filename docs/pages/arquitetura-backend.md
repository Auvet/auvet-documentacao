# Auvet- Arquitetura do Backend

## Visão Geral
Backend em Node.js com TypeScript, Express e Prisma (MySQL). A arquitetura é modular por domínio, separada em camadas Controller → Service → Repository, com middleware global de autenticação e documentação via Swagger (OpenAPI 3).

## Tecnologias e Ferramentas
- Node.js, TypeScript, Express
- Prisma ORM + MySQL
- Swagger (swagger-jsdoc + swagger-ui-express)
- Jest (unitário/integrado), Supertest
- ESLint
- Docker e Docker Compose

## Estrutura de Pastas (alto nível)
```
auvet-backend/
├─ src/
│  ├─ config/           # database (PrismaClient) e swagger
│  ├─ middleware/       # auth (valida JWT via API externa)
│  ├─ modules/          # domínios: usuario, funcionario, clinica, ...
│  │  └─ <dominio>/
│  │     ├─ controller.ts
│  │     ├─ service.ts
│  │     └─ repository.ts
│  ├─ routes/           # composição de rotas e middleware global
│  ├─ utils/            # validadores
│  └─ server.ts         # bootstrap do app, CORS, health e Swagger
├─ prisma/
│  ├─ schema.prisma     # modelos e relacionamentos
│  └─ migrations/       # histórico de migrações
└─ docs/                # artefatos de documentação (diagrama, swagger paths)
```

## Camadas
```
┌───────────── Presentation ─────────────┐  Controllers / Rotas
└──────────── Business (Service) ────────┘  Regras de negócio
└────────── Persistence (Repository) ────┘  Acesso ao banco (Prisma)
└─────────────── Database ───────────────┘  MySQL
```

## Módulos implementados
- usuario, funcionario, clinica, tutor, animal, consulta, vacina
- relacionamentos auxiliares: funcionario-clinica, tutor-clinica, animal-clinica

Cada módulo segue o padrão:
- `controller.ts`: recebe requisições, valida/organiza input, chama o service e mapeia respostas/erros.
- `service.ts`: contém as regras de negócio e orquestra múltiplos repositories.
- `repository.ts`: encapsula consultas Prisma, transações e tratamento de erros de persistência.

## Rotas e Middleware
- `src/routes/index.ts` registra os controllers em `/api/*` e aplica `authenticateToken` globalmente:
  - Todas as rotas sob `/api` exigem JWT válido.
  - Controllers por recurso: `/funcionarios`, `/usuarios`, `/tutores`, `/vacinas`, `/animais`, `/clinicas`, `/consultas`, `/funcionario-clinica`, `/tutor-clinica`, `/animal-clinica`.
- `src/middleware/auth.ts`: valida o token chamando a API de autenticação externa (`AUTH_API_URL`), anexando o usuário em `req.user` quando válido.

## Servidor, CORS e Healthcheck
- `src/server.ts`:
  - Configura `express.json()`, CORS com lista de origens permitidas (default + variáveis de ambiente) e preflight `OPTIONS`.
  - Monta as rotas em `/api` e a documentação Swagger em `/api-docs`.
  - Endpoint `/health`: verifica conectividade com o banco via `prisma.$connect()`.

## Documentação de API (Swagger)
- `src/config/swagger.ts` gera o OpenAPI 3.0 e expõe `/api-docs`.
- Esquemas documentados: `Usuario`, `Funcionario`, `Tutor`, `Animal`, `Clinica`, `Consulta`, `Vacina`, além de `ApiResponse`.
- As anotações consolidam controllers e paths de `src/docs/swagger-paths.ts`.

## Banco de Dados (Prisma)
Modelo definido em `prisma/schema.prisma` (MySQL):
- Entidades: `Usuario`, `Funcionario`, `Tutor`, `Clinica`, `Animal`, `Consulta`, `Vacina`.
- Tabelas de associação: `FuncionarioClinica`, `TutorClinica`, `AnimalClinica`.
- Regras relevantes:
  - `Usuario` 1:1 com `Funcionario` ou `Tutor`.
  - `Clinica` tem administrador (`Funcionario`) e relacionamentos N:N com funcionários, tutores e animais via tabelas de junção.
  - `Consulta` referencia `Animal`, `Funcionario` e `Clinica`.
  - Cascades configurados onde aplicável.

## Fluxo de uma requisição (ex.: criar Funcionário)
1) `POST /api/funcionarios` → Controller valida entrada e chama o Service  
2) Service aplica regras (ex.: criar `Usuario` + `Funcionario`)  
3) Repository executa as operações no Prisma (transação quando necessário)  
4) Resposta padronizada (sucesso/erro) retorna ao cliente

## Variáveis de Ambiente
- `PORT`: porta do servidor (default 3000)
- `DATABASE_URL`: conexão MySQL para Prisma
- `AUTH_API_URL`: URL da API de autenticação para validar tokens
- `CORS_ORIGINS` (ou `CORS_ORIGIN`): lista de origens permitidas (separadas por vírgula)

## Scripts úteis
```bash
npm run dev           # desenvolvimento (ts-node + nodemon)
npm run build         # compila para dist/
npm start             # executa dist/server.js
npm run db:migrate    # prisma migrate dev
npm run db:generate   # prisma generate
npm test              # jest
```

## Histórico de Versões

| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.1.0 | 2025-10-06 | Izabella Alves | Autenticação JWT externa, middleware global, CI/CD, Docker melhorado |
| 1.0.0 | 2025-09-24 | Izabella Alves | CRUDs iniciais, arquitetura modular, Docker + MySQL, testes básicos |
| 1.0.0 | 2025-11-16 | Izabella Alves | Termina documentação do backend |
