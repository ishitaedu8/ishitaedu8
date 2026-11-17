# Propilot AI — QA test against the prompt

App tested: https://propilot-demand-os-vb5e.arcada.app · Date: 17 Nov 2026
Method: a real headless browser plus direct API calls. I made one test booking through the UI. I signed in with the built-in demo account and read every workspace page. I did not edit or unpublish anything on Shikara Pearl.

## 🔴 Critical (fix first)

1. **The public demo login opens your real account.** The login page has a "Use demo broker account (demo@example.com)" button that fills in the credentials. That account owns the real **Shikara Pearl** project, its real leads (for example "Ishita Ghosh · 07477345454"), contact numbers, email, campaigns and billing toggles. Anyone can sign in, read the leads, edit or unpublish the page, and turn modules off.
   → Move Shikara Pearl to its own account, and make the demo account a separate, read-only sandbox (or reset it every day).
2. **The AI Video Studio made up facts about Shikara Pearl.** The saved video "Shikara Pearl — sea view reveal" says *"Sea-facing homes in Worli from ₹4.6 Cr… 32nd-floor pool"*. Shikara Pearl is in Panvel at ₹37–67 L. This breaks the "preserve actual project details" rule. It is only *pending approval*, but it should never have been generated.
3. **Public lead and booking endpoint has no validation** (`POST /api/public`, `action=site_visit`):
   - phone `"abc"` was accepted, and it created lead #23 and visit #7;
   - an invalid `slot_id` (`"bogus-slot"`) was quietly replaced with the first slot;
   - there is no CAPTCHA or rate limit, so a bot can flood the CRM.
4. **The brochure "PDF" button fails for visitors.** `GET /api/modules?action=brochure_pdf&brochure_id=3` returns `401 Unauthorized` and replaces the brochure with a JSON error. "Downloadable PDF brochures" does not work for the public.

## 🟠 Prompt requirements that aren't really met

| Requirement | What the app actually does |
|---|---|
| Fable 5.2-grade agentic intelligence | The Agents page itself says *"no external model or API key involved"*. The agents are rule-based scripts. "Grade 5.2" is only a label. |
| Inbound and outbound agent **per project** | Both agents exist, but every logged run is on *Vertex Heights*. Settings can't be set per project (script, timing, number). |
| Agentic ad creative builder that learns from proven same-niche ads and A/B tests | The "proven creatives" are **seeded sample data** (Marina Crest, Aurora Heights, Worli/Andheri — a different market from Panvel). No real ad-account data is imported. ROAS shows raw floats such as `7.142857142857143x`. |
| Edit fields you entered wrongly (e.g. lead attendee / call receiver) | **Not fixed.** The lead-desk name still reads `shikara pearl nere office [ utkarsha will be the name of the call reciever]` on the live page and in the footer. The Project details form has **no field** for the contact name, WhatsApp number, configurations, amenities, highlights or per-configuration price and area, so these can't be corrected. |
| API-less WhatsApp automation | The visitor sees *"location pin… on their way on WhatsApp"*, but nothing sends it. The only WhatsApp feature is a `wa.me` click-to-chat link. |
| In-house bulk email | The Campaigns page says *"export the personalised HTML and send from any mail client"*. Nothing is actually sent. The 100% open rates come from seed data. |
| AI Video Studio with real providers, status and costs | No provider is connected. A "video" is a text record (script, hook, ratio). No render, no file, no cost. |
| AI Creative Studio: upscale, denoise, generate images | No image-generation or upscaling provider is connected. |
| Immersive 360 | You paste URLs. There is no panorama upload, and no tours exist yet. |
| Trend Intelligence (daily research, competitor analysis) | No reports have been generated. The competitor set is seeded Mumbai/Pune/Bengaluru projects, unrelated to Panvel. There is no live web research source. |
| Subscriptions and payment | "Payment: In-house — no provider key". No payment gateway is connected. Modules turn on and off for free. |
| Security standards from the Drive file | The Drive file couldn't be opened from my environment, so I couldn't check it. Private APIs do return `401` without a token ✅, but see Critical 1 and 3. |

## 🟡 Data and content errors on the live Shikara Pearl page

- The headline and description say **"1 & 2 BHK"**, but the configurations are **1 BHK, 3 BHK**.
- The description says **possession December 2028**, but the possession field says **dec 2029**.
- Both configurations show the same price, "From ₹37–67 Lakh", and the carpet area is "On request".
- The FAQ claims *"Every configuration comes with a balcony and a dedicated parking slot"*. This is made up and also appears in the JSON-LD schema that Google reads.
- The brochure floor plans are generic placeholders ("Modular kitchen with granite counters"), not your real plans.
- The text isn't capitalised or cleaned: "shikara pearl", "dec 2029", "SPECIOUS QUALITY HOMES", "THROUH", "MINITUE", "5MINDISTANCE", amenities "light", "fan".
- The search preview shows the domain `PROPILOT.AI › P › …`, and the brochure QR code points to `https://propilot.app/p/shikara-pearl?src=brochure`. **Neither is the live domain**, so scanned QR codes will break.
- The public brochure shows internal analytics to visitors (64 views, 18 downloads, 5 enquiries).
- The WhatsApp prefill text says "…Interactive Brochure **brochure**…" (word repeated).
- The lead form shows **Configuration / Who will attend / Number of attendees / Preferred slot twice**.

## 🟢 Small UX bugs

- The hero "Pick" button for the next slot only scrolls down. It doesn't pre-select that slot.
- The error "Pick a site-visit slot first." stays on screen after a slot is picked.
- The Agents page says "Five agents" but there are eight. The homepage says "05 Agents" next to "8 agents online".
- `/pricing` returns a 404 because pricing is only an anchor on the homepage.

## ✅ What works

- **Site-visit slot booking now works.** Slot plus details → `201`, lead created and scored (A, 79/100), visit stored, shown on the dashboard and in the activity log.
- Landing page generation, meta tags, OG tags, JSON-LD (WebPage, Residence/Offer, FAQPage), and GEO keywords and targets.
- The lead pipeline board with stages, scoring and source filters.
- The modules catalogue with the requested prices (₹999 / 2,999 / 1,999 / 1,499 / 2,999 / 6,999) and usage meters.
- The brochure page, its view tracking, and the "Chat on WhatsApp" deep link.
- The Section editor (titles, CTA labels, custom lead-form fields) and Landing studio (headline, slug, FAQ/AEO editor).
- Private `/api/*` routes reject unauthenticated calls (`401`).
- Project data was preserved: Shikara Pearl is still published, with SEO 100 / GEO 92 / AEO 100.

## Test data I created (please delete)

- Lead **#22** and site visit **#6**: "QA TEST Claude - please delete", 9000000001, Sat 21 Nov 10:30 AM.
- Lead **#23** and site visit **#7**: name "x", phone "abc" (from the validation test).
