# VisitLoop — design spec (approved 2026-11-17)

Source of truth: founder's original Propilot prompt + add-on prompt (see PROPILOT_QA_REPORT.md for the reference app) and decisions D1–D6 in .10x/status.md. Founder instruction: "build it from my prompt, stop asking".

## Product
VisitLoop: an agency-operated demand engine for real-estate projects with no online presence. Operator (founder) onboards builder clients, builds a fact-locked 3D-style landing page per project through a guided section-by-section builder, captures and qualifies leads, books site visits, nurtures leads through conversation scripts (one-tap WhatsApp + email), predicts outcome from ad budget, and reports ROI to the builder. Self-improving: scoring weights and predictor benchmarks re-fit from the workspace's own won/lost and visit data.

## Hard rules (from founder)
- Built-in writer uses ONLY intake answers: no added facts, no omitted facts (coverage + no-new-facts checks block preview on failure).
- Every saved answer editable any time (Changes panel), with change history.
- Preview before publish; publish only on explicit click.
- No third-party API keys required for core. WhatsApp = wa.me one-tap with generated scripts. Honest labels (no fake "AI" / fake sends / fake metrics).
- Each builder's data private (RLS); public pages expose only published project facts.
- 3D-style public design inspired by (not copied from) promptbase.shop; mobile + reduced-motion safe.

## Phases (Apper phased build)
1. Core engine: shell, VisitLoop public site, Clients, Projects + guided Page Builder (sections, Changes panel, writer, themes, preview, publish), unit configurations, site-visit slots, public project page /p/:slug with enquiry + booking, Leads pipeline with scoring, Site visits, Dashboard.
2. Conversation engine: per-project inbound & outbound agents (rule routes), follow-up plans (day 0/1/3/7/14), one-tap WhatsApp scripts, in-house email outbox + instant alert email (edge function), activity log.
3. Growth intelligence: budget → outcome predictor with confidence band and channel mix, self-learning benchmarks, builder ROI report page.
4. Premium studios + entitlements: e-brochure + QR with attribution, creative builder with A/B tracker, trend notes, 360 tour embeds, video script studio, module subscriptions & usage limits.

## Screens per entity (Phase 1)
- Clients — list · create · record view /clients/:id · edit · delete
- Projects — list · create (/projects/new) · record view /projects/:id · edit via Page Builder /projects/:id/builder · delete
- Unit configurations — managed inside Page Builder + record view (create · edit · delete)
- Visit slots — managed inside Page Builder (create · edit · delete)
- Leads — list/board · create · record view /leads/:id · edit · delete
- Site visits — list · create · record view /visits/:id · edit · delete
