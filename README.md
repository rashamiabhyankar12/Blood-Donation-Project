# 🩸 Blood Donation Project

A full-stack blood donation management system with a React frontend and an ASP.NET Core backend. This repository contains the client app (React) and the server (ASP.NET Core) implementing APIs, persistence, and business logic for donors, donation requests, and administrative workflows.

---

## Table of contents
- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Tech stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Getting started (local)](#getting-started-local)
  - [Backend (ASP.NET Core)](#backend-aspnet-core)
  - [Frontend (React)](#frontend-react)
- [Environment variables / configuration](#environment-variables--configuration)
- [Database & migrations](#database--migrations)
- [API (summary)](#api-summary)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License & contact](#license--contact)

---

## Overview
This project implements a platform to register donors, manage blood donation events and requests, and enable administrators to match donors with recipients. The frontend provides an interactive UI while the backend exposes REST APIs for CRUD operations, authentication, and data validation.

---

## Features
- Donor registration and profile management
- Search donors by blood group, location, and availability
- Create / fulfill blood requests
- Basic role-based access control (user / admin)
- Email notifications (placeholder / service integration)
- Data persistence with relational database and EF Core

---

## Architecture
- Client: React SPA (single-page application), communicates with backend via JSON REST APIs.
- Server: ASP.NET Core Web API, layered architecture:
  - Presentation (Controllers)
  - Application / Services (business logic)
  - Data (EF Core repositories / DbContext)
- Database: Relational (e.g., PostgreSQL / SQL Server)
- Optional: Docker for containerized runs; CI pipelines for build/test/deploy

---

## Tech stack
- Frontend: React (Create React App), React Router, axios / fetch
- Backend: ASP.NET Core (6/7/8+), C#, Entity Framework Core
- Database: PostgreSQL or SQL Server (configurable)
- Dev tools: dotnet CLI, npm / yarn, Docker (optional)

---

## Prerequisites
- .NET SDK 6.0+ (or project target) — https://dotnet.microsoft.com/
- Node.js 16+ and npm (or yarn) — https://nodejs.org/
- A database instance (Postgres / SQL Server) or Docker
- (Optional) Docker & Docker Compose

---

## Getting started (local)

Clone the repo:
```bash
git clone https://github.com/rashamiabhyankar12/Blood-Donation-Project.git
cd Blood-Donation-Project
```

### Backend (ASP.NET Core)
1. Open a terminal in the backend folder (adjust path if your server is in `server/`, `backend/` or root):
```bash
cd backend        # or the correct backend path
dotnet restore
```

2. Configure environment variables (see [Environment variables](#environment-variables--configuration)).

3. Apply EF Core migrations and update the database:
```bash
dotnet ef database update
```
(If EF tools are not installed: `dotnet tool install --global dotnet-ef`)

4. Run the backend:
```bash
dotnet run
```
By default the API will run on `https://localhost:5001` (Kestrel) or the port configured in launch settings.

### Frontend (React)
1. Install dependencies:
```bash
cd react-app
npm install        # or yarn
```

2. Configure frontend env (API base URL) in `.env` or `package.json` proxy.

3. Start the dev server:
```bash
npm start
```
Open http://localhost:3000 to view the app.

4. Build for production:
```bash
npm run build
```

---

## Environment variables / configuration
Typical variables (use `.env` files or your deployment platform secrets):

Backend (.NET):
- ConnectionStrings__DefaultConnection = "Host=...;Database=...;Username=...;Password=..."
- ASPNETCORE_ENVIRONMENT = Development
- Jwt__Key, Jwt__Issuer, Jwt__Audience (if using JWT)
- Email__Smtp__Host, Email__Smtp__Port, Email__Smtp__User, Email__Smtp__Pass (optional)

Frontend (.env):
- REACT_APP_API_BASE_URL = https://localhost:5001/api

Note: Use secrets management for production credentials.

---

## Database & migrations
This project uses EF Core migrations to manage schema changes.

Common workflow:
```bash
# create a migration
dotnet ef migrations add AddDonorEntity

# update database
dotnet ef database update
```

If switching DB provider (SQL Server ↔ PostgreSQL), update the DbContext provider and connection string in configuration.

---

## API (summary)
The backend exposes REST endpoints (example paths — adjust to actual controller routes):

- POST /api/auth/register — register user/donor
- POST /api/auth/login — authenticate
- GET /api/donors — list/search donors (query by bloodGroup, city, radius)
- GET /api/donors/{id} — donor details
- POST /api/requests — create a blood request
- GET /api/requests — list requests
- PUT /api/requests/{id}/fulfill — mark request fulfilled (admin/user flows)

For production-grade APIs:
- Use proper validation (FluentValidation or DataAnnotations)
- Return appropriate HTTP status codes and problem details
- Add OpenAPI (Swagger) for documentation:
  - In Startup/Program: services.AddSwaggerGen(); and app.UseSwagger();

---

## Testing
- Backend: add xUnit / NUnit tests for services and controllers; run via:
```bash
dotnet test
```
- Frontend: use React testing-library / Jest:
```bash
npm test
```

---

## Deployment
Options:
- Docker: create Dockerfiles for backend/frontend and orchestrate with docker-compose
- Cloud: deploy ASP.NET backend to Azure App Service / AWS Elastic Beanstalk / container registry; static frontend can be served from Netlify / Vercel / S3 + CloudFront.
- CI/CD: add GitHub Actions workflows for build, tests, and deploy.

Example Docker Compose (high-level):
```yaml
version: '3.8'
services:
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: example
      POSTGRES_DB: blooddb
  api:
    build: ./backend
    environment:
      ConnectionStrings__DefaultConnection: Host=db;Database=blooddb;Username=postgres;Password=example
    depends_on:
      - db
  web:
    build: ./react-app
    ports:
      - "3000:3000"
```

---

## Contributing
- Fork the repo, create a feature branch, and open a pull request.
- Follow a consistent commit message style.
- Add unit/integration tests for new features or bug fixes.
- Document API changes (Swagger / README updates).

---

## License & contact
- Add your chosen license (e.g., MIT) or include a LICENSE file.
- Maintainer: Rashami Abhyankar — link to GitHub profile: https://github.com/rashamiabhyankar12

---

If you want, I can:
- Commit this updated README to the repository (create a branch & PR if you prefer), or
- Customize sections (API routes, exact backend folder path, or add badges) to match your code more precisely.
