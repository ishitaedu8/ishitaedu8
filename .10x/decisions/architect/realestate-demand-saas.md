# Architect — VisitLoop
Tables (Phase 1): clients_c, projects_c, unit_configs_c (Lookup project), visit_slots_c (Lookup project), leads_c (Lookups project, client), site_visits_c (Lookups lead, project, slot).
Profiles: Admin_c (operator, root), Client_c (builder, read-only own client rows), PublicProfile (published pages + public submit fn).
Modules: src/lib/writer.js (built-in writer + validators), src/lib/scoring.js (lead score), public page /p/:slug, Page Builder /projects/:id/builder.
Edge function: public-submit (requireAuthentication false) — validates and writes lead/visit.
