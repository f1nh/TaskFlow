# TaskFlow

A Kanban-style project and task management app — a from-scratch, portfolio rebuild of the **Balady Division Task Management System** I originally built for internal use at WSMCO, done with a full SDLC process so the reasoning behind every decision is documented, not just the code.

## Status

🚧 **Early development.** The ASP.NET Core MVC scaffold is committed and running locally. Backend implementation (auth, projects, tasks) is in progress — see [Roadmap](#roadmap) below for exactly what's done and what's next.

## About

The original system was an internal ASP.NET tool for assigning, tracking, and managing departmental tasks. TaskFlow rebuilds the same core idea as a generic, public project: register or log in, create projects, and manage each project's tasks on a drag-and-drop board with **To Do / In Progress / Done** columns.

The goal of this rebuild isn't just a working app — it's to practice and demonstrate a complete software development lifecycle: requirements, system design, implementation, testing, and deployment, each done deliberately and documented as it happens.

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | ASP.NET Core MVC (.NET 10), C# |
| ORM / Data access | Entity Framework Core |
| Database | SQL Server |
| Auth | ASP.NET Core Identity |
| Frontend | Razor Views, Bootstrap, vanilla JavaScript |
| Source control / CI | Git, GitHub, GitHub Actions |

## Architecture

```
Browser (Razor Views + Bootstrap + JS)
   │  HTTPS request + auth cookie
   ▼
ASP.NET Core MVC — AccountController / ProjectsController / TasksController
   │  [Authorize] + explicit ownership check on every read/write
   ▼
Entity Framework Core
   │  LINQ → SQL
   ▼
SQL Server — Users, Projects, Tasks
```

Every `Project` belongs to exactly one owning user; every `Task` belongs to exactly one project. Controllers verify `Project.OwnerId` matches the current user before returning or modifying any row — this is the one piece of business logic the design deliberately centers on, since it's the same class of check (broken object-level authorization) that shows up in real security reports.

Full diagrams (ERD, sequence diagrams, state diagram, deployment diagram, C4 container view) live alongside this repo's design docs.

## Features

- [ ] Email/password authentication (ASP.NET Core Identity)
- [ ] Create, view, and delete projects, scoped to the logged-in user
- [ ] Create, update, and delete tasks within a project
- [ ] Drag-and-drop Kanban board (status + ordering persisted to the database)
- [ ] Automated tests (unit + integration)
- [ ] CI pipeline (build, test on every push)

## Getting Started

**Prerequisites:** [.NET 10 SDK](https://dotnet.microsoft.com/download), SQL Server (LocalDB is fine for development)

```bash
git clone https://github.com/<your-username>/TaskFlow.git
cd TaskFlow
dotnet run
```

The app starts on `http://localhost:5084` (port may vary — check your terminal output).

## Roadmap

This project follows a deliberate SDLC process rather than jumping straight to code:

- [x] **Requirements** — feature scope and success criteria defined
- [x] **System design** — ER diagram, API/route design, and the ownership-check decision documented
- [x] **Repo & environment setup** — ASP.NET Core MVC scaffold committed and running
- [ ] **Backend implementation** — Identity auth, EF Core models + migrations, Projects/Tasks controllers
- [ ] **Frontend implementation** — Razor views, Bootstrap styling, drag-and-drop board
- [ ] **Testing & CI/CD** — unit tests, GitHub Actions workflow
- [ ] **Documentation & polish** — finalize this README, deployment notes

## License

Personal portfolio project — feel free to explore the code, but this isn't intended for production use as-is.
