# DokiTab GTM Context OS

## Company
DokiTab (dokitab.com): offline-first healthcare operating system, hospital management system and EMR (with pharmacy POS and lab LIMS) for hospitals, clinics, diagnostic labs and pharmacies in Nigeria and West Africa; free core tier plus a ₦30,000/month plan. [company.md]

## How this folder works
- `context/icp/` — ideal customer profiles. Holds `icp.md` (5-layer ICP plus disqualifiers) and `personas.md` (Champion, Economic Buyer, Influencer). Read before writing any targeting or outreach.
- `context/positioning/` — how we frame DokiTab vs alternatives. Read before messaging or campaign work.
- `context/messaging/` — approved value props, taglines, objection handling. Read before writing any copy.
- `context/brand-kit/` — logos, colours, fonts, voice. Read before any visual or branded output.
- `context/docs/research/` — raw research and source material. Read when you need to verify a company fact.
- `knowledge-base/playbooks/` — repeatable GTM processes. Read before running a known workflow.
- `knowledge-base/learnings/` — what worked and what did not. Read at the start of planning new work.
- `meetings/` — notes and transcripts. Read when a task refers to a past discussion.
- `output/campaigns/` — finished deliverables. Read to reuse or reference past campaigns.
- `output/outbound/` — outbound deliverables (emails, sequences, lists). Read to reuse past outreach.
- `output/social/` — social posts and assets. Read to reuse past social content.
- `output/data/` — exported data and lists. Read when a task needs prior data.
- `inspiration/` — ads and examples to learn from. Read for reference when creating new copy or creative.
- `tools/` — scripts and utilities. Read when automating a repeated task.
- `weekly-priorities/` — the plan for each ISO week. Read at the start of every session.
- `work-log/` — timestamped log of every change. Read to see what was done, and append to it every session.

## Skills
Skills live in `.claude/skills/`. Available:
- `agent-browser`: browser automation for agents (fill forms, click, extract, screenshot, log in).
- `build-clay`: build, audit, repair or extend a production Clay outbound workbook (Companies and People tables).
- `build-workflow-spec`: turn one campaign idea into a full outbound workflow spec with approval gates.
- `calculate-costs`: report the dollar and credit cost of a Clay table's AI and action columns.
- `campaign-contract`: write contract.md, the build sheet for one Clay campaign, from an approved decision.
- `campaign-copy`: write and QA cold email and LinkedIn DM copy against an approved workflow spec.
- `campaign-qa`: QA a Clay table or Instantly/PlusVibe campaign against its workflow spec.
- `campaign-strategy`: generate 15 to 25 cold outbound campaign ideas with targeting, value prop and priority.
- `clay-build-plan`: turn a workflow spec into a step-by-step Clay build guide plus a swimlane diagram.
- `crawl4ai`: crawl pages or whole sites into clean Markdown or JSON, free and local.
- `design-clay-table-json`: write the importable Clay table JSON blueprint for one campaign from its contract.
- `export-tables`: export one or more Clay tables to local CSV files.
- `import-clay-table`: import a Clay table JSON, prove paid columns on sample rows, then run within budget.
- `inbox-placement-test`: run an end-to-end cold email inbox placement test (EmailBison and MailReach) and score pools.
- `miro-gtm-flowchart`: draw a GTM motion as an editable flow chart on a Miro board.
- `prompt-builder`: build or repair a reusable prompt, such as a Clay AI column's per-row prompt.
- `save-clay-schema`: export an approved Clay table to the local registry and prove it re-imports.
- `signal-matrix`: build ICP and signal matrices, segment buyers and map signals to reach them.
- `workspace-cost-audit`: report a full Clay workspace's monthly credit and AI dollar spend, read-only.

## Rules
- Cite a source for every company fact.
- Never invent numbers, metrics, or statistics.
- No em dashes in any copy.
- Ask before sending or publishing anything.
- Log every change in this week's work log.

## Tools
GTM stack I use:
- Clay: data enrichment and outbound workbook building.
- HubSpot: CRM.
- n8n: workflow automation.
- Notion (MCP connected): where we publish finished docs. GitHub stays the source of truth.
- Miro (MCP connected): diagrams and GTM flowcharts.
- Apollo: prospecting and contact data.
- Claude Code: AI assistant for this Context OS.

Local tool bundles in `tools/` (from the gtm-starter-kit):
- `tools/rex/`: Clay API client, cost tooling and the ClayLens browser extension.
- `tools/crawl4ai/`: local web crawler to Markdown or JSON.
- `tools/agent-browser/`: browser automation for agents.
