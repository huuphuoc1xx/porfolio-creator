# Portfolio Creator

A full-stack application for creating and sharing professional portfolios. Users can register, sign in, and edit their portfolio (profile, hero, experience, education, contact) with support for English and Vietnamese.

## Tech Stack

| Layer    | Stack |
|----------|--------|
| Frontend | React 19, Vite 7, TypeScript, Ant Design, react-i18next (EN/VI), react-router-dom |
| Backend  | NestJS 10, TypeORM, MySQL 8, JWT (Passport), Swagger |
| DevOps   | Docker Compose (optional): MySQL, Backend, Frontend (nginx) |

## Project Structure

```
Me/
├── portfolio-frontend/   # React SPA (Vite)
├── portfolio-backend/    # NestJS API
├── docker-compose.yml    # MySQL + Backend + Frontend
├── .env.example
└── README.md
```

## Prerequisites

- Node.js 18+
- MySQL 8 (or use Docker for DB only)
- Yarn or npm

## Getting Started

### Option 1: Local development

**1. Database**

Create a MySQL database and run migrations:

```bash
# If using Docker for DB only (from repo root):
docker compose up -d db

# Backend .env (portfolio-backend/.env) – when DB is in Docker:
# DB_HOST=localhost
# DB_PORT=10003
# DB_USER=portfolio
# DB_PASSWORD=portfolio
# DB_NAME=portfolio
```

```bash
cd portfolio-backend
cp .env.example .env   # edit with your DB and JWT_SECRET
npm install
npm run migration:run  # or use TypeORM sync in dev
npm run seed          # optional: seed user + sample portfolio
npm run start:dev
```

**2. Frontend**

```bash
cd portfolio-frontend
cp .env.example .env   # set VITE_API_URL=http://localhost:10001
npm install
npm run dev
```

- **App:** http://localhost:5173 (or Vite port)
- **API:** http://localhost:10001/api
- **Swagger:** http://localhost:10001/api/docs

### Option 2: Docker Compose (full stack)

From the repo root:

```bash
cp .env.example .env   # optional: set DB_*, JWT_SECRET
docker compose up -d --build
```

- **App:** http://localhost:10002
- **API:** http://localhost:10001/api
- **Swagger:** http://localhost:10001/api/docs

See [README.docker.md](./README.docker.md) for more Docker details (ports, env vars, DB-only mode).

## Environment Variables

### Backend (`portfolio-backend/.env`)

| Variable         | Description |
|------------------|-------------|
| PORT             | API port (default 10001) |
| APP_CORS_ORIGINS | Allowed origins (e.g. http://localhost:5173) |
| DB_HOST, DB_PORT | MySQL host and port |
| DB_USER, DB_PASSWORD, DB_NAME | MySQL credentials |
| JWT_SECRET       | Secret for JWT (change in production) |

### Frontend (`portfolio-frontend/.env`)

| Variable     | Description |
|--------------|-------------|
| VITE_API_URL | Backend base URL (e.g. http://localhost:10001) |

## Main Features

- **Home page:** Welcome screen with link to sample portfolio and Get started (Login).
- **Auth:** Register (with confirm password), Login; JWT stored in localStorage.
- **Portfolio:** One portfolio per user (created on register). Edit profile, hero, experiences, education, contact; EN/VI content via tabs.
- **Public view:** Share link `/p/:slug`; locale and theme (light/dark) in navbar; section nav (Home, About, Skills, etc.) on portfolio page only.
- **Settings (navbar):** Theme switch, language (EN/VI), Login or (when authenticated) My portfolio, Edit, Logout. Locale and theme persisted in localStorage.

## Scripts

| Location   | Command           | Description |
|------------|-------------------|-------------|
| Backend    | `npm run start:dev` | Run API with watch |
| Backend    | `npm run seed`     | Seed user + sample portfolio |
| Backend    | `npm run migration:run` | Run TypeORM migrations |
| Frontend   | `npm run dev`      | Vite dev server |
| Frontend   | `npm run build`    | Production build |

## API Overview

| Method | Path                      | Auth   | Description |
|--------|---------------------------|--------|-------------|
| POST   | /api/auth/register        | No     | Register (email, password) |
| POST   | /api/auth/login           | No     | Login → access_token + user |
| GET    | /api/portfolios/me        | Bearer | Current user's portfolio |
| GET    | /api/portfolios/:slug     | No     | Public portfolio by slug |
| PUT    | /api/portfolios/:id       | Bearer | Update portfolio (owner only) |

Use Swagger at `/api/docs` for full request/response schemas.

## License

Private / internal use.
