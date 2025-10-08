# Go-Driver Technical Roadmap (v1.0)

**Author:** Heidi Kalske-Akintobi / Photini Family / Go-Driver Core  
**Version:** v0.5 (October 2025)  
**Document Type:** Engineering & Compliance Roadmap  
> **Note:** This roadmap describes the technical and compliance foundations of the Go-Driver Ethical AI Framework. For the narrative version, see [roadmap-vision.md](roadmap-vision.md).

---

## 1. Overview
Go-Driver is an **ethical multi-agent AI framework** designed to transform knowledge work into transparent, auditable collaboration between humans and AIs.  
The project integrates a Chrome-based frontend, Node.js backend, and a modular database to coordinate writing, research, engineering, and compliance tasks under one ethical command center.

**Mission Goal:** Empower individuals and small teams to operate with the efficiency of a corporation—while remaining fully compliant with human-rights, privacy, and AI-ethics principles.

**Current Build Phase:** v0.5 — Ethics Core Alpha  
*(Frontend and backend foundation stable; source management and policy engine in progress.)*

---

## 2. Core Principles
1. **Ethics First** – Every AI interaction passes through a policy filter and logging layer.  
2. **Transparency by Design** – All actions are observable and reversible.  
3. **Modularity & Scalability** – Each Driver module is independent but shares a common context bus.  
4. **User Sovereignty** – Data and rules belong to the user, not the provider.  
5. **Explainability** – Every decision is documented with why, what, and who authorized it.  
6. **Interoperability** – Works with OpenAI, Perplexity, Xano, n8n and future local models.  

---

## 3. Development Phases

### 3.1 Core Foundations (v0.1 → v0.3)

**Objective:** Establish the base architecture for persistent UI, activity logs, and backend services.

**Key Deliverables**
- Go-Driver panel and side drawer (UI/UX framework).  
- Thread management and project grouping.  
- Safe Clear log system (UI only clear vs data reset).  
- Watch-Me-Work observer mode (event logging).  
- Node backend with Express + Multer for file uploads.  
- IndexedDB wrapper (GoDB) for projects, threads, and snapshots.

**Dependencies**
- Node v20 +, npm 8 +.  
- Chrome Extension Manifest V3.  
- Local IndexedDB access.  

**Design Commentary**
- All data operations implemented client-first to ensure offline capability.  
- Every UI component uses vanilla JS and inline styles for zero-dependency portability.  
- Logs auto-render on `activityLog.push()` for real-time visibility.

**Success Indicators**
- Panel boots without console errors.  
- Activity Log renders automatically.  
- Backend responds on `/ping` and `/upload`.  
- Basic project/thread creation and storage persist after reload.

---

### 3.2 Driver Layer (v0.4)

**Objective:** Create the AI “team” within Go-Driver—modular agents called Drivers that share a common context and obey ethical rules.

**Drivers**
- 🧭 **Navigator** – Plans project milestones and task sequences.  
- ✍️ **Writer** – Generates and edits content while respecting Ethics Core filters.  
- ⚙️ **Engineer** – Codes extensions and back-end modules.  
- 📊 **Analyst** – Monitors logs and creates summaries and KPIs.  
- 🌐 **Connector** – Handles API integrations (GitHub, Xano, n8n).  
- ⚖️ **Ethicist** – Approves or rejects AI outputs based on rules.

**Key Deliverables**
- Driver module structure (`/drivers/`).  
- Shared context bus for inter-Driver communication.  
- Prompt routing and metadata logging.  
- Early Ethicist stub for manual approval.  
- AI provider dropdown (OpenAI / Perplexity).  

**Dependencies**
- Completion of Core Foundations and GoDB stability.  
- API keys configured in `apiConfig`.  

**Design Commentary**
- Each Driver runs as a separate JS module with a standard API: `plan()`, `execute()`, `review()`.  
- Context bus ensures Drivers don’t overwrite each other’s state.  
- Logs are tagged per Driver for traceability (`writer-log`, `engineer-log`, etc.).  
- Ethicist module acts as gatekeeper before any output enters the thread.

**Success Indicators**
- Drivers communicate via context bus without crash.  
- AI requests route correctly through provider selector.  
- Ethicist stub logs decisions to Activity Log.  
- At least one complete multi-Driver workflow executed (start → review → output).  

---

### 3.3 Ethics & Policy Core (v0.5)

**Objective:** Implement a rule-based Ethics Core that evaluates every AI action for compliance, privacy, and human-rights safety.

**Key Deliverables**
- `ethicsCore.js` service layer with:
  - PII detector and redaction utilities.
  - Policy Engine loading dynamic JSON rules per project.
  - Approval modal for manual “Ethicist review”.
  - Risk Scoring Model (low / medium / high).
- Explainability Log storing decisions: rule → action → timestamp → result.
- Compliance metadata embedded in each Activity Log entry.
- Ethics toggle in UI (“Always On / Manual / Off”).

**Dependencies**
- All Drivers functional and logging through context bus.
- Policy configuration files in `/config/policies/`.

**Design Commentary**
- The Ethics Core acts as middleware between Drivers and output.
- Rules follow a declarative JSON schema:
  ```json
  { "rule": "no_pii", "pattern": "(\\d{3}-\\d{2}-\\d{4})", "severity": "high" }
  ```
- Adds negligible latency (< 20 ms avg) per message.
- Supports extension into NLP-based detection later.

**Success Indicators**
- Every AI output tagged with an ethics status.
- Manual override creates audit record.
- All policy files load dynamically without reload.
- Zero blocked content leaks into visible threads.

---

### 3.4 Knowledge & Dashboard Layer (v0.6)

**Objective:** Build a dynamic dashboard that visualizes projects, Drivers, KPIs, and knowledge graph relations.

**Key Deliverables**
- Dashboard Tab in main panel with:
  - Project summary cards (status, last update, ethics score).
  - Timeline / calendar view for milestones.
  - KPI widgets (threads, sources, Driver activity).
  - Quick Capture modal for on-the-fly notes.
- Knowledge Graph (entity + relation JSON store).
- Search & Filter across projects, sources, and logs.
- Optional integration to Xano or Neo4j later.

**Dependencies**
- Stable GoDB schema with projects, threads, sources, logs.
- At least one Driver producing analytics data.

**Design Commentary**
- Data layer decoupled from UI for API reuse.
- Graph model designed for federated future (nodes = entities, edges = relations).
- Each widget subscribes to GoDB change events for live update.

**Success Indicators**
- Dashboard renders in under 500 ms.
- Widgets refresh automatically on Driver output.
- Graph JSON export → Xano API verified.
- Ethics status visible per project card.

---

### 3.5 Connectors & Autonomy (v0.7)

**Objective:** Expand Go-Driver beyond local boundaries—enable seamless integration with external tools and initiate semi-autonomous Driver-mode.

**Key Deliverables**
- Multi-API Key Manager (OpenAI, Perplexity, Anthropic, Xano, n8n).
- GitHub Connector with selective export (exportDocsToGitHub()).
- n8n Flow hooks for automation.
- “Driver-Mode” switch in UI (Observer → Assistant → Driver → Autopilot).
- Policy Brain for contextual rule evaluation.
- Session Summary Modal (visual, exportable Markdown).

**Dependencies**
- Ethics Core fully operational.
- API keys securely stored.
- Backend reachable for REST POST / PUT.

**Design Commentary**
- Driver-Mode uses a finite-state machine pattern with explicit permissions.
- All external calls pass through a proxy to sanitize payloads.
- GitHub Connector commits docs with signed hash for authenticity.

**Success Indicators**
- External API sync without error.
- GitHub push logs with commit SHA.
- Driver-Mode runs autonomously for ≥ 15 min without manual correction.
- All external requests logged + reviewed by Ethicist.

## 4. Technical Architecture

**Frontend**
- Vanilla JS + Chrome Extension Manifest V3.
- UI composition: TopBar / ThreadView / SidePanel / Dashboard / Modal.
- Tailwind or inline CSS for lightweight styling.
- IndexedDB for local state.

**Backend**
- Node.js v20 +, Express 5, Multer for file upload.
- Planned middleware for GitHub and Xano connectors.
- JSON API contract: /ping, /upload, /pushDocs.

**Storage & Persistence**
- IndexedDB (GoDB) → JSON mirror in Xano → Graph DB phase.
- Data hierarchy: Project > Thread > Message > Source > Log.
- Backup routine via exportDocsToGitHub().

**AI Providers**
- OpenAI (gpt-4o-mini) | Perplexity (llama-3.1-sonar-128k) | Local LLM (future).
- Configurable per project / Driver.
- Centralized rate-limit + token usage tracker.

**Data Flow Diagram (simplified)**

USER INPUT
   ↓
Go-Driver UI (Thread / Dashboard)
   ↓
Driver Modules (Navigator, Writer, Engineer…)
   ↓
Ethics Core Filter
   ↓
Backend API (Node → GitHub / Xano)
   ↓
Storage (GoDB / Knowledge Graph)

---

## 5. Security & Data Governance

**Principles**
- Local-first storage with user encryption.
- Explicit consent for external pushes.
- Zero-trust API key policy.
- Every action logged and signed with timestamp and Driver ID.

**Security Features**
- AES-256 local encryption module planned.
- Sanitization of PII before remote sync.
- Tokenized access for multi-user federation.
- Periodic “Ethics Snapshot” reports for auditors.

**Data Retention**
- Logs kept locally until export.
- Users can delete or archive sessions.
- Versioned backups tagged by session summary ID.

**Audit Readiness**
- Explainability Logs → CSV export.
- Each rule decision traceable to policy file and Driver action.
- Clear chain of custody for files and commits.

---

## 6. Versioning & Synchronization

**Objective:** Maintain continuous traceability between local work, documentation, and external repositories.

**Key Components**
- **Semantic Versioning:**  
  - `v0.x` → pre-release prototypes  
  - `v1.x` → production stable  
  - Patch numbers for each verified commit  
- **Session Snapshots:**  
  Every “End Session” modal exports a Markdown summary into `/docs/sessions/` with timestamps and ethics metadata.
- **GitHub Connector:**  
  Controlled by `exportDocsToGitHub()` allowing selective file pushes rather than global syncs.  
  Each push includes commit message, SHA hash, and driver signature.
- **Doc Flow:**  
  1. Capture → Modal summary  
  2. Save → `/docs/`  
  3. Review → Ethics Core check  
  4. Push → GitHub with signed commit  
- **Rollback:**  
  Any commit or session can be rolled back using Undo Stack (`undoLast()`), preserving data integrity.

**Design Commentary**
- Version numbers embedded in both UI and session summaries ensure cross-reference integrity.  
- Manual export keeps users in control—nothing leaves the local machine without consent.  
- When federated nodes arrive (v1.0+), these same version tokens will secure multi-user provenance.

**Success Indicators**
- Commit log shows consistent semantic versions.  
- No unverified file leaves local storage.  
- Every GitHub push includes ethics metadata block.  
- Session rollback restores exact thread state.

---

## Appendix C — Compliance, Oversight & Audit

### C.1  Purpose
To guarantee that Go-Driver remains legally, ethically, and socially accountable in all operations—from individual user actions to federated deployments.

### C.2  Oversight Framework
- **Transparency:** All Driver actions are logged with timestamp, rule applied, and outcome.  
- **Explainability:** Each AI decision links to its policy rule and rationale text.  
- **Accountability:** Human approval required for high-risk actions (manual Ethicist review).  
- **Traceability:** File and message histories include commit SHA, policy version, and reviewer ID.  
- **Non-Repudiation:** Digital signatures confirm origin and authenticity of exports.

### C.3  Audit Artifacts
1. **Activity Log** – chronological record of every event.  
2. **Explainability Log** – JSON entries detailing rule evaluations.  
3. **Session Summaries** – human-readable reports of outcomes.  
4. **Policy Files** – active rule sets at time of decision.  
5. **Ethics Snapshots** – periodic state captures for external audit.

Each artifact can be exported to CSV or JSON for third-party verification.

### C.4  Governance Workflow
1. **User or Authority Request Audit** → triggers export of relevant logs.  
2. **Go-Driver compiles chain of custody** including hashes of all related files.  
3. **Ethics Core validates integrity** against policy registry.  
4. **Auditor receives package** signed with user’s or organization’s public key.  
5. **Results stored back** into Go-Driver as immutable “Audit Record”.

### C.5  External Review Compatibility
- Complies with GDPR data-subject rights (access, deletion).  
- Aligns with EU AI Act transparency clauses (Articles 13–15).  
- Supports ISO 42001 AI-management-system certification path.  
- Provides APIs for NGO or academic oversight modules.

### C.6  Risk Management & Mitigation
| Risk                  | Mitigation                                   |
|-----------------------|----------------------------------------------|
| PII leakage           | Automatic redaction + manual approval        |
| Unauthorized export   | Tokenized push with consent gate             |
| Model bias            | Policy rule weights + audit feedback loop    |
| Data loss             | Local AES backup + GitHub mirror             |
| Misuse of Driver-Mode | Ethics escalation + manual override required |

### C.7  Ethics Review Schedule
- **Weekly internal scans** – automated risk summaries.  
- **Monthly manual review** – user-initiated.  
- **Quarterly external audit** – NGO / partner collaboration.  
- **Annual ethics report** – public transparency document.

---

## 7. Closing Summary

Go-Driver stands as a fusion of engineering precision and human ethics.  
Its roadmap defines not only software milestones but a framework for responsible autonomy.  
From the Core Foundations to the future Federation layer, every component reinforces one principle: **technology must serve humanity transparently**.

> *Next document:* [Visionary Roadmap (Founder's Edition)](roadmap-vision.md)

**Current Version:** v0.5.0 — *Ethics Core Alpha*  
**Next Target:** v0.6 — *Knowledge & Dashboard Integration*

---
