# Status

**Project:** Real-estate demand SaaS "VisitLoop" — rebuild of the Propilot concept on Apper.io
**Feature slug:** realestate-demand-saas
**Phase:** 0 — Brainstorming (in progress)

## Context gathered
- Reference app tested: Propilot on Arcada (see PROPILOT_QA_REPORT.md). Concept to reproduce, gaps to avoid.
- Target platform: Apper.io via MCP connector. Capabilities found: React front end, Apper data tables with row-level security, built-in auth/teams, file storage (Files fields), edge functions for server work (secrets, third-party APIs, email, payments). No built-in scheduler/cron, no built-in LLM, no built-in WhatsApp.
- Existing Apper projects: "Rot Check" (unrelated, untouched).
- "freebuff" connector: not available in this session.

## Open questions
1. MVP scope (which modules ship first)
2. Product name
3. AI provider for agents / copy (key needed?)
4. WhatsApp + email sending route

## Decisions so far
- D1 (founder, 2026-11-17): MVP = Lead engine core — project intake + photos, SEO/GEO/AEO landing page per project, lead form + site-visit booking, scored lead pipeline, private broker accounts. Premium modules deferred to later rounds.
- D2: Product name = VisitLoop.
- D3: User model = Agency mode first (founder operates all builder workspaces; each builder client gets view-only access to their own workspace). Data model is workspace-per-builder with team members so "owner + sales team" self-serve can be switched on later without rebuild. Rationale: done-for-you retainer (setup + monthly) produces cash earlier than low-ticket SaaS.
