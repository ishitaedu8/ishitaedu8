# ADR-001: Build VisitLoop on Apper.io as a phased, fact-locked, key-free app

**Status:** Accepted · **Date:** 2026-11-17 · **Feature:** realestate-demand-saas

## Context
Founder (non-technical) wants the Propilot concept rebuilt under a new name with a working end-to-end flow. Propilot (Arcada) shipped many screens backed by seed data and fabricated content. Apper offers React pages, tables with RLS, auth/teams, Files storage and edge functions; no built-in LLM, scheduler or WhatsApp.

## Decision
- Apper app "VisitLoop", built in 4 phases; each phase fully working before the next.
- Data: Apper tables with policies; Owner-based RLS for operator data; PublicProfile read only on published projects/configs/slots; public lead + booking writes go through a `requireAuthentication:false` edge function that validates input (phone, slot capacity, honeypot, duplicate merge).
- Copy: deterministic built-in writer (pure JS module) generating headline/description/FAQ/meta/keywords/JSON-LD from answers, with coverage + no-new-facts validators. Writer interface allows an LLM later.
- WhatsApp: wa.me deep links with generated scripts (API-less). Email alerts: edge function (secret added only when founder chooses a provider; until then alerts queue in an in-app outbox).

## Alternatives Considered
| Alternative | Pros | Cons | Why Not |
|---|---|---|---|
| Keep fixing Propilot on Arcada | Existing data | No code access, prompt-only edits, fabricated modules | Founder asked for a new build |
| Single big-bang build of all modules | One delivery | Placeholders, unreviewable, Propilot's failure mode | Quality |
| LLM copy via Claude API | Better prose | Needs key + cost; founder wants API-less | Deferred behind interface |
| WhatsApp Business API | True automation | Cost + Meta verification | Deferred |

## Consequences
+ Every visible feature is real; facts can't be invented; zero running cost for core.
− Messages need one tap to send; copy is template-quality until an LLM is plugged in.
Risks: public edge function abuse → validation, rate checks, honeypot.
