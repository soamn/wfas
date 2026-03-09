<div align="center">

[![GitHub](https://custom-icon-badges.demolab.com/badge/WFAS-UI-blue?logo=desktop&style=for-the-badge)](https://github.com/soamn/wfas-ui)
[![GitHub](https://custom-icon-badges.demolab.com/badge/WFAS-Server-blue?logo=server&style=for-the-badge)](https://github.com/soamn/wfas-server)
[![GitHub](https://custom-icon-badges.demolab.com/badge/WFAS-Engine-blue?logo=github&style=for-the-badge)](https://github.com/soamn/wfas-engine)

![GitHub License](https://img.shields.io/github/license/soamn/wfas?style=for-the-badge&labelColor=yellow)
![GitHub Tag](https://img.shields.io/github/v/tag/soamn/wfas-engine)
![GitHub Repo stars](https://img.shields.io/github/stars/soamn/wfas)

# WFAS Engine

### The Unified Workflow Automation System

**WFAS** is a high-performance, node-based automation engine. This repository serves as the **Main Container**, orchestrating the Frontend (UI), Backend (Server) and Backend (engine) via Git Submodules.

</div>

---

## ⚙️ Overview

### Execution Flow Architecture

```mermaid
flowchart TD
    %% 1. UI LAYER (Submodule: wfas-ui)
    subgraph UI_Layer [WFAS UI - Next.js]
        User([User Browser]) <-->|HTTPS /api| Proxy[Next.js Middleware]
        UI_Components[React Components]
    end

    %% 2. SERVER LAYER (Submodule: wfas-server)
    subgraph Server_Layer [WFAS Server - Express]
        Proxy -->|1. Auth Request| Auth[Session Validator]
        Auth -->|Invalid| Unauthorized[401 Unauthorized]

        subgraph Server_Logic [Orchestrator]
            Auth -->|Valid| Router[API Router]
            Router -->|2. Manage| Service[Services]
            Router -->|3. Route Task| TaskQueue[Execute Workflow]
        end
    end

    %% 3. ENGINE LAYER (Submodule: wfas-engine)
    subgraph Engine_Layer [WFAS Engine - Core]
        TaskQueue -->|4. Execute| Runner[Workflow Executor]

        subgraph Engine_Logic [Logic Processing]
            Runner --> Loader[Node Executor]
            Loader --> Resolver[Node Variable Resolver]
        end
    end

    %% PERSISTENCE & EXTERNAL
    Router <-->|Session/Config| DB[( Database)]
    Engine_Logic <-->|Execution State| DB

    %% STYLING
    classDef ui fill:#e1f5fe,stroke:#01579b,color:#000
    classDef server fill:#fff3e0,stroke:#e65100,color:#000
    classDef engine fill:#f3e5f5,stroke:#4a148c,color:#000

    class UI_Layer,Proxy,UI_Components ui
    class Server_Layer,Auth,Router,Service,TaskQueue server
    class Engine_Layer,Runner,Loader,Resolver, engine
```

---

## Local Setup

Run Setup locally by this single source Rather than individual Setup

- note: similar env variable must have same values

```
git clone --recursive https://github.com/soamn/wfas
cd wfas
```

```
cd wfas-engine
cp .env.example .env
pnpm install
pnpm run prisma:generate
pnpm run build
pnpm run start

```

```
cd wfas-server
cp .env.example .env
docker build -t wfas-server .
docker run --network="host" --env-file .env wfas-server

```

```
cd wfas-server
cp .env.example .env
docker build -t wfas-server .
docker run --network="host" --env-file .env wfas-server

```

```
cd wfas-ui
cp .env.example .env
pnpm install
pnpm run build
pnpm run start
```

📄 License

This project is licensed under the GNU General Public License v3.0.
