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
- D4: Lead alerts = instant email to operator + builder (edge function via a free-tier transactional email service) and a one-tap "Reply on WhatsApp" wa.me button with a prefilled message per lead. No WhatsApp Business API in v1.
- D5 (founder): Page creation = guided, agentic "Page Builder" flow.
  - Intake is a step-by-step wizard: one section at a time (e.g. Basics → Location → Configurations & pricing → Amenities → Highlights → Photos → Contact & site visit → Legal/RERA), each saved before moving on.
  - Every saved answer stays editable at any time via an always-visible "Changes" panel (also after publishing); edits re-flow into the page.
  - The AI writer composes copy ONLY from the founder's answers. Hard rules: add no topic/fact that was not provided; omit no fact that was provided. Enforced in code by a coverage check (every answer appears) and a no-new-facts check before preview.
  - Builder offers several original 3D-style design themes (depth hero, perspective gallery, tilt cards) inspired by — not copied from — promptbase.shop's 3D work section; mobile + reduced-motion fallbacks.
  - Preview is mandatory before publish; publish only on explicit approval.
- D6: No external AI provider in v1 (founder wants API-less). Copy is produced by a deterministic built-in writer engine (rule-based templates with multiple phrasing variants per section) that can only use intake answers. Labelled honestly as "built-in writer", not AI. Pluggable writer interface so an LLM (e.g. Claude API) can be added later behind the same fact-lock checks.
