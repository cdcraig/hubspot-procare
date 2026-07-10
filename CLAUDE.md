# hubspot-procare

Client engagement: HubSpot → ProCare lead integration. Their HubSpot plan can't fire webhooks, so a form submission emails the data; a self-hosted n8n layer parses/reformats it, then a Playwright browser bot auto-fills the ProCare Connect registration form. Smartify owns and manages the whole pipeline end to end. First deliverable: a single-page, mobile-first proposal landing page (Smartify-branded) to send the client, deployed as a preview then moved to Coop's Railway.


## Knowledge Storage — Universal Rule

The wiki (`~/General/llm-wiki/`) is the canonical knowledge store for all agents.

- **Project knowledge** (configs, IDs, endpoints, decisions, learned context) → `update_project_knowledge` (writes to `wiki/agents/hubspot-procare/`)
- **Deliverables** (reports, analysis, drafts) → write directly to `wiki/agents/hubspot-procare/`
- **Cross-project facts** → `store_memory` (dual-writes to wiki + Pinecone)
- **NEVER** save project knowledge to Claude Code memory files (`.claude/projects/*/memory/`)
- **NEVER** write deliverables to standalone folders outside the wiki

Your wiki folder: `~/General/llm-wiki/wiki/agents/hubspot-procare/`
- `index.md` — overview
- `active-work.md` — current priorities and in-progress items (update as work progresses)
- `knowledge.md` — durable facts: configs, IDs, endpoints, architectural decisions, behavioral rules (update whenever you learn something)

**Progress vs durable knowledge:**
- Progress entries ("fixed X", "deployed Y") → `active-work.md`
- Durable facts (voice IDs, API endpoints, rules like "always export before patching") → `knowledge.md`
- After significant work, scan active-work for durable facts and promote them to knowledge.md

**At session start:** read your `active-work.md` and `knowledge.md` to catch up on current state before acting. When a topic comes up and you don't remember, call `get_project_knowledge` with project="hubspot-procare" to reload. Your wiki folder IS your memory.
