# Como Subir o Projeto (Autenticação, Backend e Frontend)

Este guia mostra como executar os três serviços localmente (dev) e via Docker.

## Requisitos
- Node.js 18+ e npm
- Docker e Docker Compose
- MySQL (opcional se usar Docker)

## Ordem de inicialização
1) Autenticação (porta 4000)
2) Backend (porta 3000)
3) Frontend (porta 5173)

---

## 1) Serviço de Autenticação (auvet-autenticacao)

Pasta: `auvet-autenticacao`

Variáveis de ambiente (arquivo `.env`):
```
PORT=4000
DATABASE_URL="mysql://user:pass@host:3306/auvet_auth"
JWT_SECRET="troque-em-producao"
JWT_EXPIRATION="24h"
CORS_ORIGINS="http://localhost:5173"
```

Rodar em desenvolvimento:
```bash
cd auvet-autenticacao
npm install
npm run db:migrate
npm run dev
```

Docker:
```bash
cd auvet-autenticacao
docker-compose up --build
```

URLs úteis:
- Health: `http://localhost:4000/health`
- Swagger: `http://localhost:4000/api-docs`

---

## 2) Backend (auvet-backend)

Pasta: `auvet-backend`

Variáveis de ambiente (arquivo `.env`):
```
PORT=3000
DATABASE_URL="mysql://user:pass@host:3306/auvet_backend"
AUTH_API_URL="http://localhost:4000"
# Aceitar chamadas do frontend local
CORS_ORIGINS="http://localhost:5173"
```

Rodar em desenvolvimento:
```bash
cd auvet-backend
npm install
npm run db:migrate
npm run dev
```

Docker:
```bash
cd auvet-backend
docker-compose up --build
```

URLs úteis:
- Health: `http://localhost:3000/health`
- Swagger: `http://localhost:3000/api-docs`
- API base: `http://localhost:3000/api`

Observação: o backend valida JWT consultando o serviço de Autenticação em `AUTH_API_URL`.

---

## 3) Frontend (auvet-frontend)

Pasta: `auvet-frontend`

Configuração de ambientes (`src/config/env.ts`):
- Em desenvolvimento, o projeto usa proxies do Vite:
  - `AUTH_API_BASE_URL = /auth-api`
  - `BACKEND_API_BASE_URL = /backend-api`

Certifique-se de que o `vite.config.*` está configurado com proxies para:
```
/auth-api     -> http://localhost:4000/api
/backend-api  -> http://localhost:3000/api
```

Rodar em desenvolvimento:
```bash
cd auvet-frontend
npm install
npm run dev
```

Build/preview:
```bash
npm run build
npm run preview
```

URL:
- App: `http://localhost:5173`

Em produção (sem proxy), o `env.ts` já aponta para as URLs públicas render.com.

---

## Fluxos rápidos (teste manual)
- Login: usar o frontend (`/login`), que chama `POST /api/auth/login` no serviço de Autenticação via proxy.
- Rotas protegidas: após login, o frontend envia `Authorization: Bearer <token>` para o Backend.
- Validação de token no Backend: middleware chama `POST /api/auth/validatetoken` no serviço de Autenticação.

---

## Problemas comuns (Troubleshooting)
- CORS bloqueando requisições: ver `CORS_ORIGINS` nos dois servidores (4000 e 3000) e incluir `http://localhost:5173`.
- Banco não conecta: valide `DATABASE_URL` e se o container/serviço MySQL está no ar.
- Proxy do Vite não funciona: confira `vite.config.*` (targets de `/auth-api` e `/backend-api`) e se os serviços 4000/3000 estão de pé.
- Swagger não abre: confirme `PORT` e acesse `/api-docs` no serviço correto.

---

## Resumo de comandos

Autenticação:
```bash
cd auvet-autenticacao && npm i && npm run db:migrate && npm run dev
```

Backend:
```bash
cd auvet-backend && npm i && npm run db:migrate && npm run dev
```

Frontend:
```bash
cd auvet-frontend && npm i && npm run dev
```

---

## Histórico de Versões

| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.0.0 | 2025-11-16 | Izabella Alves | Guia para subir Autenticação, Backend e Frontend (dev e Docker) |


