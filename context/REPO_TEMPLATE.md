# Repository Technical Context: `<project>` / `<repo-name>`

> **Maintained By**: `tech_lead` (Level 1 Project Tech Lead, verified via `backend_dev` & `frontend_dev`)
> **Repository Path**: `/absolute/path/to/<repo-name>`
> **Role**: `frontend` | `backend` | `fullstack` | `game-client` | `infra` | `shared-lib`
> **Production Domain / Runtime**: `<url or runtime environment>`
> **Last Synced**: `YYYY-MM-DD`

---

## 1. Architectural Overview & Tech Stack
- **Primary Stack**: `<Language, Framework, Bundler, Runtime>`
- **State / Data Layer**: `<Database, ORM, Cache, or Client State Store>`
- **Key External SDKs / Integrations**: `<Auth, Analytics, XR, Payment, Messaging>`
- **Build & Run Commands**:
  - Dev: `<command>`
  - Build: `<command>`
  - Test / Lint: `<command>`

---

## 2. Directory & Module Map
| Path / Module | Responsibility | Key Exports / Entrypoints |
| :--- | :--- | :--- |
| `src/...` | Core application bootstrap | `...` |

---

## 3. Core Flows, Endpoints, or Systems
*(For Backend: API Route Table `Code -> Method Path -> Auth -> Controller -> Service/Model`. For Frontend/Game: Route Map, State Machine, ECS Systems, and Boot Pipeline.)*

---

## 4. Data Models, Configs & Environment Variables
- **Schemas / State Stores / Global Configs**:
- **Required `.env` Variables**:

---

## 5. Cross-Repo Dependencies & Integration Touchpoints
- **Upstream / Downstream Repos**:
- **Shared Contracts / Types**:

---

## 6. Known Technical Debt, Bugs & Fragile Areas
1. **`<File:Line>`**: `<Description of known bug, drift, or fragile coupling>`

---

## 7. Architectural Changelog (Maintained by `tech_lead`)
- `YYYY-MM-DD`: Initial deep repository context indexed by `tech_lead`.
