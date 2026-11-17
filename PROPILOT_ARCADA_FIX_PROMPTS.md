# Propilot: Arcada fix prompts (paste one at a time)

**How to use this:**
1. Open the Propilot project in Arcada.
2. Copy **one** prompt (everything inside its grey box) and paste it into Arcada's chat.
3. Wait until Arcada says it has finished.
4. Tell Claude "Prompt N done". Claude re-tests the live app and tells you whether it worked before you go on.

Do **not** paste several prompts at once. Small steps are safer and easier to check.

---

## Prompt 1 — Security: stop strangers from opening my real account (MOST IMPORTANT)

```
IMPORTANT: Extend and fix the existing Propilot app. Do NOT rebuild it. Do NOT delete or change any existing project, lead, landing page, brochure or the "shikara pearl" project and its data.

Security fix: right now the "Use demo broker account (demo@example.com)" button on /login signs into the SAME account that owns my real projects (including shikara pearl and its real leads). Anyone on the internet can see and change my real data. Fix this:

1. Create a NEW, separate user account for the real owner and move ALL existing real data to it (projects, landing pages, leads, site visits, brochures, media, campaigns, agent runs, module entitlements, activity log). Keep every record's ID, URL and slug the same so /p/shikara-pearl and /b/shikara-pearl-brochure keep working exactly as now. Show me the new owner's login email on screen once and let me set its password.
2. Turn demo@example.com into a SEPARATE sandbox account that only contains sample projects (Vertex Heights, Lakeview Residences, Skyline Arc, Palm Grove Villas, Meridian Works — copies, not the originals). It must never see the real owner's data.
3. Make the demo account read-only: it can browse every page, but any save/delete/publish/disable action shows "Demo mode: sign up to make changes" and nothing is written to the database.
4. Every server API route must check that each row returned or changed belongs to the signed-in user (user_id = auth user). Add this check everywhere it is missing.
5. In public API responses (/api/public and the public brochure endpoint), remove internal fields that visitors do not need: user_id, readiness, created_at, updated_at, and the internal brochure analytics numbers.

When finished, list exactly what changed and confirm shikara pearl's live page is unchanged.
```

---

## Prompt 2 — Stop junk and fake bookings on the public form

```
IMPORTANT: Extend and fix the existing Propilot app. Do NOT rebuild it. Do NOT change existing data.

Harden the public lead/site-visit endpoint (POST /api/public, actions site_visit and lead, and the brochure enquiry):
1. Phone is required and must be a valid Indian mobile: 10 digits starting 6-9, optionally with +91 / 91 / 0 prefix and spaces/dashes. Reject anything else with a clear message: "Please enter a valid 10-digit mobile number".
2. Name is required, 2–60 characters, letters/spaces/dots only.
3. Email optional but must be valid if filled.
4. slot_id must be one of that project's real available slots. If it is not, return an error "That slot is no longer available — please pick another". Never silently switch to a different slot.
5. Add a hidden honeypot field and a rate limit: max 5 submissions per phone number per day and max 20 per IP per hour; return a friendly error when exceeded.
6. Same phone number for the same project within 24 hours must update the existing lead, not create a duplicate.
7. Show the same validation messages on the form in the browser before submitting.
8. On the landing page: remove the duplicated form fields (Configuration, Who will attend, Number of attendees and Preferred slot currently appear twice — keep one of each). Clicking the hero "Pick" button for the next slot must pre-select that slot. Clear the "Pick a site-visit slot first." error as soon as a slot is chosen.
9. Do not tell visitors "location pin and parking details are on their way on WhatsApp" unless a WhatsApp message is really sent. Change the success text to: "Site visit confirmed for <slot>. Our team will call you from <lead desk phone> to confirm." and show a "Message us on WhatsApp" button (wa.me link) underneath.
```

---

## Prompt 3 — Let me correct anything I typed wrongly in my project

```
IMPORTANT: Extend the existing Propilot app. Do NOT rebuild it. Keep all existing data.

In the project page (/app/projects/:id → Overview → Project details), the broker cannot edit several fields that were filled during intake. Add editable fields, saved to the database and reflected on the live landing page, brochure, FAQ and JSON-LD schema after saving:
- Lead desk contact name (the person who receives calls)
- Site-visit host / lead attendee name (who meets buyers at the site)
- Lead desk WhatsApp number
- Developer / company name
- Configurations as an editable list, each with its own: name (e.g. 1 BHK), carpet area (sq ft), price from, price to, units available
- Amenities (add / edit / delete / reorder)
- Highlights (add / edit / delete / reorder)
- Possession date as a month + year picker (store it consistently, display as e.g. "December 2029")
- Site-visit slots: let me choose days, times and how many visitors per slot
- Gallery photos: replace, delete, reorder and add captions

Also add an "Undo last save" button and keep a change history of each field (who changed it, old value, new value) in the activity log.

Then fix my shikara pearl project text ONLY after showing me a preview and getting my approval:
- Contact name currently reads "shikara pearl nere office [ utkarsha will be the name of the call reciever]". Proposed: contact name "Utkarsha", desk label "Shikara Pearl — Nere Site Office".
- Headline/description say "1 & 2 BHK" but configurations are "1 BHK, 3 BHK" — ask me which is correct.
- Description says possession December 2028 but the field says dec 2029 — ask me which is correct.
- Capitalise the name to "Shikara Pearl" and fix spelling in highlights (SPECIOUS→Spacious, THROUH→through, MINITUE→minute, 5MINDISTANCE→5 min distance) and remove amenities "light" and "fan".
Never change a verified fact (price, RERA, possession, configurations) without my explicit approval.
```

---

## Prompt 4 — Stop the AI from inventing facts

```
IMPORTANT: Extend the existing Propilot app. Do NOT rebuild it. Keep all existing data.

Several generators invent facts that are not in the project data. Fix this everywhere content is generated (landing FAQ, brochure, AI Video Studio, AI Creative Studio, Trend Intelligence, agent messages):
1. Generated text may only use facts present in the project's saved fields. If a fact is missing (carpet area, parking, balcony, floor count, pool, sea view), leave it out or say "Ask our team" — never invent it.
2. Remove the FAQ sentence "Every configuration comes with a balcony and a dedicated parking slot" from shikara pearl and from the generator template.
3. The existing AI Video Studio draft "Shikara Pearl — sea view reveal" (Worli, ₹4.6 Cr, 32nd-floor pool) is wrong. Mark it Rejected with note "Wrong project facts" — do not delete it.
4. Before any generated content is published, show a "Fact check" panel listing each claim and which saved field it came from; anything without a source is highlighted red and blocks publishing until I approve or remove it.
5. Brochure floor plans: do not show generic placeholder room descriptions. If no real floor plan image is uploaded, show "Floor plan available on request" with an upload button for the broker.
```

---

## Prompt 5 — Broken links and buttons

```
IMPORTANT: Extend and fix the existing Propilot app. Do NOT rebuild it. Keep all data.

1. The public brochure "PDF" button returns 401 Unauthorized for visitors (/api/modules?action=brochure_pdf). Make the PDF of a PUBLISHED brochure downloadable without login, and count each download in brochure analytics. Unpublished brochures must stay private.
2. The brochure QR code and the search preview use the wrong domain (propilot.app / PROPILOT.AI). Use the real site address the app is served from (read it from the current request / a SITE_URL setting) everywhere: QR payloads, canonical URL, og:url, sitemap, share links.
3. Add a <link rel="canonical">, og:url, and include the project page in /sitemap.xml and allow it in /robots.txt.
4. Hide brochure analytics (views, downloads, enquiries) from the public brochure page — show them only inside the workspace.
5. Fix the WhatsApp prefill text which currently says "Interactive Brochure brochure".
6. Make /pricing open the pricing section instead of a 404.
7. Fix the agent count text: the app has 8 agents but some places say "Five agents" / "05". Show the real number from the database.
8. Format ROAS in the Creative Studio to one decimal place (e.g. 7.1x).
```

---

After these five are done and re-tested, the next round covers the bigger missing pieces (per-project inbound/outbound agent settings, real WhatsApp and email sending options, payments, video and image providers). Each needs a decision from you about cost and providers, so Claude will ask before writing them.
