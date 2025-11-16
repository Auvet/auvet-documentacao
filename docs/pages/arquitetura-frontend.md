# Auvet - Arquitetura do Frontend

## Visão Geral
Frontend em React 18 + TypeScript, bundlado por Vite. A UI utiliza Mantine UI, React Router para navegação, Context API para sessão/autenticação e um pequeno client HTTP para chamadas à API do backend e do serviço de autenticação.

## Tecnologias
- React 18 + TypeScript
- Vite (dev/build/preview)
- React Router v6
- Mantine (`@mantine/core`, `@mantine/hooks`) e `@tabler/icons-react`
- Selenium (testes end-to-end básicos)

## Scripts
```bash
npm run dev       # ambiente de desenvolvimento (Vite)
npm run build     # build de produção
npm run preview   # preview do build (porta 5173)
npm run test:selenium
```

## Estrutura de Pastas (alto nível)
```
src/
├─ App.tsx                # Rotas da aplicação
├─ main.tsx               # Bootstrap React
├─ components/
│  ├─ layout/             # Layouts por perfil (Admin, Funcionario, Tutor)
│  └─ ProtectedRoute.tsx  # Gate de autenticação
├─ contexts/
│  └─ AuthContext.tsx     # Contexto de sessão/token
├─ hooks/
│  └─ useAuth.ts          # Hook para consumir AuthContext
├─ pages/                 # Páginas (Home, Login, dashboards e CRUDs)
├─ repositories/          # Acesso a APIs (camada de dados)
├─ services/              # Orquestração de casos de uso (ex.: registro)
├─ utils/                 # httpClient, storage, jwt, enums, cpf/cnpj
└─ config/
   └─ env.ts              # Endpoints de APIs (auth e backend)
```

## Rotas e Navegação
- Definidas em `src/App.tsx` usando `react-router-dom`:
  - Públicas: `/`, `/login`, `/registrar-clinica`, `/termos-de-uso`, `/politica-de-privacidade`
  - Protegidas (via `ProtectedRoute`):  
    - `/dashboard/admin` (módulos: Funcionários, Tutores, Animais, Consultas, Vacinas, Métricas, Perfil)  
    - `/dashboard/funcionario` (CRUDs essenciais e Perfil)  
    - `/dashboard/tutor` (home, animais, consultas, vacinas, perfil)
- `ProtectedRoute` verifica token via `AuthContext` e `localStorage`; sem token, redireciona para `/login`.

## Autenticação e Sessão
- `AuthContext` gerencia `{ token, role }`, persiste/limpa token via `utils/storage.ts` e expõe `logout`.
- O token é lido no carregamento inicial e usado pelo client HTTP quando necessário.
- Perfis de navegação por layout (Admin, Funcionario, Tutor) organizam a UI conforme o papel.

## Camada de Dados
- `utils/httpClient.ts` provê um cliente HTTP genérico (fetch), com:
  - `baseUrl` configurável
  - injecção de `Authorization: Bearer <token>` quando `auth=true`
  - tratamento de erros padronizado
- `repositories/*Repository.ts` encapsulam chamadas REST para cada recurso (Animal, Clinica, Consulta, etc.).
- `services/*Service.ts` orquestram fluxos de alto nível (ex.: `RegistrationService` para onboarding).

## Integração com Backends
- `config/env.ts` define endpoints por ambiente:
  - Dev (proxy via Vite):  
    - `AUTH_API_BASE_URL = /auth-api`  
    - `BACKEND_API_BASE_URL = /backend-api`
  - Produção: URLs públicas (`onrender.com`).
- O token JWT é enviado no header `Authorization` para recursos protegidos.

## Páginas Principais
- Públicas: `Home`, `Login`, `RegisterClinica`, `Terms`, `Privacy`
- Dashboards:
  - Admin: `Metricas`, `FuncionariosList/Create`, `TutoresList/Create`, `AnimaisList/Create`, `ConsultasList/Create`, `VacinasList/Create`, `PerfilConfiguracoes`
  - Funcionario: `FuncionarioHome`, CRUDs essenciais, `FuncionarioPerfil`
  - Tutor: `TutorHome`, `TutorAnimais`, `TutorConsultas`, `TutorVacinas`, `TutorPerfil`

## Estilo e UI
- Mantine para componentes e hooks de UI
- Assets estáticos em `src/assets`

## Testes
- Testes E2E com Selenium em `tests/selenium` (ex.: `home.test.js`, `admin-flow.test.js`)

## Build e Deploy
- Vite gera `dist/` (assets otimizados).  
- Arquivos de deploy: `Dockerfile`, `docker-compose.yml`, `netlify.toml`, `vercel.json`, `docker/nginx.conf`.

## Variáveis/Configuração
- Configuração centralizada em `config/env.ts`. Em desenvolvimento, usar proxy do Vite para `/auth-api` e `/backend-api` (ajustar `vite.config.*` se necessário).

## Histórico de Versões
| Versão | Data | Autor | Descrição |
|--------|------|-------|-----------|
| 1.0.0 | 2025-11-16 | Izabella Alves | Documentação completa do frontend baseada no código atual |

