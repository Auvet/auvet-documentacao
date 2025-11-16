# Auvet - Arquitetura do Serviço de Autenticação

## Visão Geral
Serviço independente em Node.js + TypeScript com Express e Prisma (MySQL) responsável por registro de usuários (Tutor e Funcionário), login e emissão/validação de JWT. Expõe endpoints REST documentados com Swagger.

## Tecnologias
- Node.js, TypeScript, Express
- Prisma ORM + MySQL
- JSON Web Token (`jsonwebtoken`)
- bcryptjs (hash de senhas)
- Swagger (swagger-jsdoc + swagger-ui-express)
- Jest + Supertest, ESLint

## Estrutura de Pastas (alto nível)
```
src/
├─ config/            # database (PrismaClient) e swagger
├─ modules/
│  └─ auth/           # controller, service, repository e tests
├─ routes/            # composição de rotas /auth
├─ utils/             # jwt, crypto (bcrypt), validators
├─ types/             # Tipagens de requests/responses
└─ server.ts          # bootstrap, CORS, health, swagger
```

## Camadas
- Controller: mapeia rotas e responses (status, mensagens, payloads)
- Service: regras de negócio (validações, geração/validação de token, perfis e permissões)
- Repository: persistência com Prisma (transações para criação de usuário + perfis)

## Endpoints (módulo Auth)
Base path: `/api/auth`
- `POST /register/tutor` — cria `Usuario` e `Tutor`, vincula às clínicas informadas
- `POST /register/funcionario` — cria `Usuario` e `Funcionario`; se cargo é “Administrador”, retorna `token` já emitido
- `POST /login` — autentica por `cpf` + `senha` e retorna `{ token, usuario, permissoes }`
- `POST /validatetoken` — valida um JWT e retorna dados do usuário/perfil
- `GET /usuarios/:cpf` — busca dados do usuário por CPF

Respostas seguem um envelope comum:
```json
{ "success": true, "data": { ... }, "message": "..." }
```
ou
```json
{ "success": false, "error": "Mensagem de erro" }
```

## JWT e Segurança
- `utils/jwt.ts`:
  - `JWT_SECRET` e `JWT_EXPIRATION` via env (padrão: 24h)
  - Geração (`sign`) e verificação (`verify`) com tratamento de token expirado/inválido
- Payload inclui: `cpf`, `email`, `tipoUsuario` (tutor|funcionario), `nivelAcesso`
- `utils/crypto.ts`: hash/compare de senha via `bcryptjs`

## Banco de Dados (Prisma)
`prisma/schema.prisma` (MySQL) define:
- `Usuario` (1:1 com `Funcionario` ou `Tutor`)
- `Funcionario` (status, nivelAcesso, registroProfissional)
- `Tutor`
- `Clinica`, `FuncionarioClinica`, `TutorClinica` (associações)

Criações usam transações (`$transaction`) no repository:
- `createUsuarioWithTutor` (vincula tutor às clínicas)
- `createUsuarioWithFuncionario`

## CORS, Health e Swagger
- `server.ts` configura CORS por `CORS_ORIGINS` (ou default) e preflight `OPTIONS`
- `/health` retorna status do serviço e conexão com banco
- `/api-docs` expõe Swagger (OpenAPI 3) com esquemas: `Tutor`, `Funcionario`, `Login`, `ValidateToken`, `LoginResponse`, etc.

## Variáveis de Ambiente
- `PORT` (default 4000)
- `DATABASE_URL`
- `JWT_SECRET`, `JWT_EXPIRATION`
- `CORS_ORIGINS` (lista separada por vírgula)

## Scripts
```bash
npm run dev        # desenvolvimento
npm run build      # compila para dist/
npm start          # executa dist/server.js
npm run db:migrate # prisma migrate dev
npm test           # jest
```

## Integração com Backend
O backend principal consome `POST /api/auth/validatetoken` para proteger rotas via middleware; o token é esperado no header `Authorization: Bearer <token>` no frontend e validado aqui.

## Histórico de Versões
| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.0.0 | 2025-11-16 | Izabella Alves | Documentação completa do serviço de autenticação |

