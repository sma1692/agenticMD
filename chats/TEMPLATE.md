# Inter-Agent Session Log: `<session-date>` — `<session-id>`

- **Main Conversation / Session ID**: `<root-conversation-id>`
- **Active File Rule**: **ONE log file per chat session.** All handoffs across all projects and hierarchy levels in this chat session are appended chronologically below.
- **Started At**: `YYYY-MM-DD HH:MM:SS`

## Active Agents in This Session
| Level | Role | Agent Name | Subagent Conversation ID | Assigned Scope |
| :--- | :--- | :--- | :--- | :--- |
| Root | Chief of Staff | `warden` | Main Chat | All Company Units |
| Level 0 | Head of Tech (CTO) | `cto` | `[<id>](conversation://<id>)` | All Tech Projects (`Model: pro`) |
| Level 1 | Product Manager | `pm` | `[<id>](conversation://<id>)` | Backlog (`worklist/*`), Product & GTM (`context/<project>.md`) |
| Level 1 | Project Tech Lead | `tech_lead` | `[<id>](conversation://<id>)` | `<project>` (`context/<project>/<repo>.md`, `contracts/*`) |
| Level 2 | Backend Developer | `backend_dev` | `[<id>](conversation://<id>)` | `<backend-folder-path>` |
| Level 2 | Frontend Developer | `frontend_dev` | `[<id>](conversation://<id>)` | `<frontend-folder-path>` |

---

## Chronological Hierarchy Transcript

### [YYYY-MM-DD HH:MM:SS] `warden` -> `cto`
**Direction**: Executive Dispatch (`Root -> Level 0`)
**Target Agent**: `cto` (`<subagent-conversation-id>`)
**Message Sent**:
> <Exact prompt / directive sent from Chief of Staff to CTO>

---

### [YYYY-MM-DD HH:MM:SS] `cto` -> `pm` & `tech_lead` (`<project>`)
**Direction**: Product & Tech Directive (`Level 0 -> Level 1`)
**Target Agents**: `pm` (`<id>`), `tech_lead` (`<id>`)
**Message Sent**:
> <Exact instructions from CTO to Product Manager and Project Tech Lead>

---

### [YYYY-MM-DD HH:MM:SS] `pm` <-> `tech_lead` (or Escalation to `cto` -> `warden`)
**Direction**: Backlog Assignment / GTM Alignment (`Level 1 <-> Level 1` or `Level 1 -> Level 0`)
**Backlog Item**: `worklist/todo/<slug>.md`
**Message Sent**:
> <Priorities, acceptance criteria, or upward business/GTM questions>

---

### [YYYY-MM-DD HH:MM:SS] `tech_lead` -> `backend_dev` & `frontend_dev`
**Direction**: Repo Recon / Contract / Execution Dispatch (`Level 1 -> Level 2`)
**Target Agents**: `backend_dev` (`<id>`), `frontend_dev` (`<id>`)
**Message Sent**:
> <Exact instructions handed to Backend Dev and Frontend Dev>

---

### [YYYY-MM-DD HH:MM:SS] `backend_dev` & `frontend_dev` -> `tech_lead`
**Direction**: Level 2 Repo Findings / Execution Report (`Level 2 -> Level 1`)
**Backend Dev Report**:
> <Granular findings, endpoints, schemas, or files modified>
**Frontend Dev Report**:
> <Granular findings, UI state, client calls, or files modified>

---

### [YYYY-MM-DD HH:MM:SS] `tech_lead` & `pm` -> `cto`
**Direction**: Compressed Project & Backlog Report (`Level 1 -> Level 0`)
**Context & Backlog Updated**: `context/<project>/<repo>.md`, `context/<project>.md`, `worklist/todo/*` -> `worklist/done/*`
**Message Received**:
> <Dense project-level summary of current state, shipped backlog items, and decisions needed>

---

### [YYYY-MM-DD HH:MM:SS] `cto` -> `warden`
**Direction**: Executive Tech & Product Brief (`Level 0 -> Root`)
**Context Updated**: `context/cto.md`
**Message Received**:
> <High-density executive brief ready for the CEO>
