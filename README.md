
# agenticMD: The Warden Agent Hierarchy in Plain Markdown

agenticMD is a pure Markdown scaffold. It sets up a corporate-style hierarchy of AI agents inside the coding agent you already use. There is nothing to install, build, or run. You open the folder in your agent app, and the app reads the instruction files.

One human operator talks to a single executive agent, `warden`. Warden delegates across engineering, product, infrastructure, security, and growth agents, and returns compact, decision-ready briefs.

---

## 1. Quickstart: Open the Folder in Your Agent App

Clone the repository, or click **Use this template** on GitHub:

```bash
git clone https://github.com/example/agenticMD.git
```

Then open the folder in one of the apps below. Say `hi` in the chat. On the first run, `warden` sees that `.state/onboarded.json` is missing and starts the onboarding interview. The interview registers your projects and fills `projects.yaml`, `context/`, `memory/`, and `worklist/`.

### Claude Code (Desktop App)

- Open the Claude desktop app and go to the **Code** tab.
- Start a new session and select the `agenticMD` folder as the working folder.
- Claude Code loads `CLAUDE.md` automatically. `CLAUDE.md` imports `AGENTS.md`.
- The subagents in `.claude/agents/` (`cto`, `pm`, `tech_lead`, `reader_scout`, and others) become available to `warden` with no extra setup.
- `.claude/settings.json` pre-approves file reads and edits inside the folder, so you get fewer permission prompts.

### Claude Code (VS Code or JetBrains Extension)

- Install the Claude Code extension from the VS Code Marketplace or the JetBrains Marketplace.
- Open the `agenticMD` folder as your project (**File > Open Folder**).
- Open the Claude Code panel and start chatting. The same `CLAUDE.md`, subagents, and settings apply.

### Claude Code (Terminal)

```bash
cd agenticMD
claude
```

### Google Antigravity

- Open Antigravity and open the `agenticMD` folder as your workspace.
- Antigravity reads `GEMINI.md`, which maps the hierarchy in `AGENTS.md` onto its native subagent tools.
- The skills in `.agents/skills/` (onboarding, standup, pushit, orchestrator) are available as runbooks.

### Gemini CLI

```bash
cd agenticMD
gemini
```

Gemini CLI loads `GEMINI.md` from the workspace root.

### Other Agents That Read `AGENTS.md`

`AGENTS.md` holds the full hierarchy and protocol in tool-neutral Markdown. Any coding agent that reads `AGENTS.md` from the project root can run the scaffold. Agents without native subagents follow the same rules in a single context.

### Working on Your Own Repositories

Your code stays in its own repositories. During onboarding, `warden` asks for each repository path. In Claude Code, onboarding adds these paths to `.claude/settings.local.json` (`additionalDirectories`), so agents can work across folders from this one workspace. In the VS Code extension, you can also add the repositories to a multi-root workspace.

---

## 2. The Bottom-Heavy Token Pyramid

Naive multi-agent setups make the root agent read entire codebases, project histories, and backlogs on every turn. Token cost grows quickly, and so does hallucination.

Warden inverts the token flow:

- **Root (`warden`, Chief of Staff)**:
  - Budget: under 1,500 tokens per turn.
  - Never reads source code, repository directories, topology caches, or raw task lists.
  - Reads user intent, routes work down to the C-Suite, and compresses replies into briefs.
- **Level 0 C-Suite (`cto`, `cbo`)**:
  - Budget: under 4,000 tokens per turn.
  - Reads only its own `context/<agent>.md` and `memory/<agent>.md`.
  - The CTO governs architecture and chairs standups. The CBO weighs revenue, prize EV, and opportunity cost.
- **Level 1 Domain Leads (`pm`, `tech_lead`, `infra_head`, `sec_lead`, `designer`, `pa`)**:
  - Own backlogs (`worklist/`), specs, API contracts (`contracts/`), infra runbooks, and security audits.
  - Turn scout digests into small file-edit manifests and assign them to Level 2.
- **Level 2 Scouts and Developers (`reader_scout`, `frontend_dev`, `backend_dev`, `systems_cpp_dev`, `rust_lowlat_dev`, `xr_3d_dev`, `infra_jr`, `sec_auditor`)**:
  - `reader_scout` reads raw code and returns a symbol and line-range digest of 150 words or fewer.
  - Developers work on 1 to 3 assigned files per shard.
- **Upward Reply Caps**:
  - Scout to Level 1: 150 words or fewer.
  - Developer to Level 1: 200 words or fewer. Full diffs stay on disk.
  - Level 1 to Level 0: 150 words or fewer.
  - Level 0 to `warden`: 120 words or fewer, plus explicit `[NEEDS_CEO]` decision items.
- **Zero-Read Chat Logging**:
  - One append-only log per session (`chats/<YYYY-MM-DD>-session-<id>.md`).
  - Agents append without reading earlier turns back into context.

---

## 3. Scout → Plan → Apply

Every code change runs in three passes. Each pass uses the cheapest model that can do it.

- **Scout pass**: `reader_scout` reads the raw source, runs grep and AST lookups, and returns a compact digest of file paths, line ranges, and symbol signatures. Frontier models never read full source files.
- **Plan**: Level 0 and Level 1 agents read only the digest. They resolve architecture and contracts with a frontier reasoning model, then write a minimal `FileEditDirective[]` manifest.
- **Apply pass**: Level 2 developers take 1 to 3 files from the manifest each and apply the edits with a fast, cheap model (or a local model).

Supporting rules:

- **Turn limit**: a developer shard ends after 8 turns or fewer. New work starts a fresh subagent with a clean context.
- **Disjoint shards**: large features split across separate file sets, so several developers run in parallel without conflicts.
- **Cost effect**: the frontier model reads a digest of about 150 words instead of whole files. Most of the edit output comes from the cheap model.

---

## 4. Operating Modes

- **Executive Mode**: chat with `warden`. Requests flow down the hierarchy and come back as a brief.
- **Direct Mode (`@agent`)**: start a message with an agent handle, such as `@cto`, `@pm`, `@tech_lead <project>`, or `@infra_head`. You get that agent's full, uncompressed reply.
- **Standup**: type `standup`. `pm` triages `worklist/inbox.md`, updates the backlogs, and writes `standups/<date>-standup.md`. Warden then pauses for your approval.
- **pushit**: type `pushit` after a change. The agent stages, version-checks, commits, and pushes, following `.agents/skills/pushit/SKILL.md`.

---

## 5. Workspace Layout

```text
agenticMD/
├── AGENTS.md                 # Canonical hierarchy and protocol (tool-neutral)
├── CLAUDE.md                 # Claude Code entry point (imports AGENTS.md)
├── GEMINI.md                 # Antigravity / Gemini CLI entry point
├── projects.example.yaml     # Example project registry (onboarding writes projects.yaml)
├── .claude/
│   ├── agents/               # Subagent prompts for Claude Code
│   └── settings.json         # Pre-approved permissions for the workspace
├── .agents/skills/           # Runbooks: onboard, standup, pushit, orchestrator
├── .state/                   # Example topology cache and model registry
├── context/                  # Templates for per-agent and per-repo context
├── memory/                   # Template for per-agent memory
├── contracts/                # Template for API and interface contracts
├── worklist/                 # Inbox template (onboarding creates backlogs)
├── standups/                 # Standup template
├── chats/                    # Session log template
└── docs/                     # Optional guides (WhatsApp setup, privacy policy)
```

Everything you add during use (`projects.yaml`, filled `context/`, `memory/`, `worklist/`, `chats/`, `standups/`) is your private state. Keep it out of any public fork.

---

## 6. Zero-Leak Guarantee

- No credentials, proprietary logic, or customer data ship in this repository.
- Every file here is a prompt, protocol, skill, or template.
- Private state lives only in your local copy.
