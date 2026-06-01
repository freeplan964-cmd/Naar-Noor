# 🍽️ Naar & Noor

> A modern restaurant management application built with Angular and ASP.NET Core.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE.md)
[![GitHub](https://img.shields.io/badge/github-Naar--Noor-black?logo=github)](https://github.com/Mostafa-SAID7/Naar-Noor)
[![Deployment](https://img.shields.io/badge/deployment-Vercel%20%26%20Azure-success)](https://naar-noor.vercel.app)

## 🎯 Overview

Naar & Noor is a full-stack restaurant platform with a responsive Angular frontend and a clean ASP.NET Core backend. It includes menu management, reservations, reviews, contact inquiries, and deployment-ready configuration.

## 🚀 Get Started

### Quick setup

```bash
git clone https://github.com/Mostafa-SAID7/Naar-Noor.git
cd Naar-Noor
```

Then follow the detailed setup guide:

- 📘 [Detailed installation and setup](docs/GETTING_STARTED.md)
- 📂 [Project structure](docs/PROJECT_STRUCTURE.md)
- 🏗️ [Architecture](docs/ARCHITECTURE.md)

### Top-level folders

- `api-server/` — ASP.NET Core backend
- `naar-noor/` — Angular frontend
- `docs/` — project documentation

## 📚 Documentation

| Document | Purpose |
|----------|---------|
| [docs/README.md](docs/README.md) | Documentation index |
| [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md) | Setup and development guide |
| [docs/PROJECT_STRUCTURE.md](docs/PROJECT_STRUCTURE.md) | Codebase layout |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Design and patterns |
| [docs/FRONTEND.md](docs/FRONTEND.md) | Frontend development |
| [docs/BACKEND.md](docs/BACKEND.md) | Backend development |
| [docs/API.md](docs/API.md) | API reference |
| [docs/DATABASE.md](docs/DATABASE.md) | Database schema and migrations |
| [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) | Deployment instructions |
| [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) | Common issues and fixes |
| [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md) | Contribution guidelines |

## 🏗️ Tech Stack

- **Frontend**: Angular 18, TypeScript, Tailwind CSS, RxJS
- **Backend**: ASP.NET Core 8, Entity Framework Core, SQL Server, MediatR
- **Patterns**: Clean Architecture, CQRS, Dependency Injection

## 📦 Running locally

For full setup details, use [docs/GETTING_STARTED.md](docs/GETTING_STARTED.md).

### Backend

```bash
cd api-server
dotnet restore
dotnet run --project src/NaarNoor.API/NaarNoor.API.csproj
```

### Frontend

```bash
cd naar-noor
npm install
npm run dev
```

## 🚀 Deployment

See [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) for full deployment instructions.

## 🤝 Contributing

Before contributing, please review:

- [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md)
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
- [SECURITY.md](SECURITY.md)

## 📄 License

This project is licensed under the MIT License. See [LICENSE.md](LICENSE.md).

## 📞 Support

If you need help:

- Visit the documentation folder: [docs/README.md](docs/README.md)
- Open an issue: https://github.com/Mostafa-SAID7/Naar-Noor/issues
- Use GitHub Discussions: https://github.com/Mostafa-SAID7/Naar-Noor/discussions

---

## 🎉 Acknowledgments

- Built with ❤️ by the Naar & Noor team
- Inspired by modern restaurant management practices
- Thanks to all contributors and supporters

---

<div align="center">

**[⬆ Back to Top](#-naar--noor)**

Made with ❤️ | [GitHub](https://github.com/Mostafa-SAID7/Naar-Noor) | [Live Demo](https://naar-noor.vercel.app)

</div>
