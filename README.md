# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

To start the app, run:

```sh
yarn install
yarn start
```
Backstage is an open platform for building developer portals, providing a single, centralized interface for all infrastructure tooling, services, and documentation.

This repository contains a full Backstage application, including backend, frontend, and plugin packages.



Backstage provides a developer portal experience for your organization. This project includes:

Backend: Node.js + Express services, integrations, and API endpoints.

Frontend: React + Material UI, providing the web interface.

Plugins: Extend functionality with Backstage plugins (internal tools, documentation, CI/CD dashboards, etc.).

Features

Centralized service catalog

Developer documentation portal

Custom plugins and extensions

Dockerized for easy deployment

Tech Stack

Backend: Node.js, TypeScript, Express

Frontend: React, Material UI

Package Manager: Yarn v4+

Database: PostgreSQL 

Containerization: Docker, Docker Compose

Getting Started
Prerequisites

Node.js >=18.x

Yarn >=4.x

Docker >=24.x (for container deployment)

PostgreSQL >=15.x (if using a database backend)

1. Clone the Repository
git clone https://github.com/<your-username>/backstage.git
cd backstage

2. Install Dependencies
yarn install


Make sure all workspace dependencies are installed correctly.

Development
Start Backend
cd packages/backend
yarn dev

Start Frontend
cd packages/app
yarn dev

Start Both Together (Monorepo)
yarn dev


This runs both backend and frontend in development mode with hot reload.

Docker Deployment
1. Build Docker Images
docker compose build --no-cache

2. Run Containers
docker compose up -d

3. Check Logs
docker compose logs -f backend
docker compose logs -f app


Backend errors like Cannot find module '@backstage/backend-defaults' often mean a dependency is missing. Run yarn install in the project root before rebuilding Docker images.

Building & Running for Production

Build frontend and backend:

yarn build


Start the production server:

yarn start

Troubleshooting
Common Issues

Module Not Found (@backstage/backend-defaults)

yarn add --cwd packages/backend @backstage/backend-defaults
yarn install


Yarn Build Errors

yarn install --immutable
yarn build


Docker Pull Failures (TLS handshake timeout)

Check your internet connection

Retry docker compose build

Increase Docker’s network timeout if needed
