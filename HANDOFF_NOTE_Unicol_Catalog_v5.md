# Unicol India Product Catalogue v5 — Handoff Note
**File:** `unicol_india_catalog_v5.html`
**Date:** June 2026
**Status:** COMPLETE AND LIVE. EmailJS contact form verified working with confirmed test submission delivered to info@unicolindia.com on June 7, 2026.

> **Note:** This handoff contains the EmailJS keys for the live contact form. The Public Key is meant to be public (it ships in client-side HTML). The Service ID and Template ID are operational identifiers, not auth credentials — but the document as a whole should not be shared externally.

---

## Summary of v3 → v5 Evolution

This session moved the catalogue from a feature-complete-but-incomplete v3 to a fully live v5. Three structured rounds of changes:

### Round 1: v3 → v4 (product & structural changes)

| # | Change |
|---|---|
| 1 | Wrapped filter-bar + cat-strip in a new `.sticky-header` div so the application image tiles stay visible while scrolling through products (previously the strip scrolled away under the sticky filter-bar) |
| 2 | Unibord 677.25 tag: "VERSATILE HIGH FILL" → "VERSATILE" |
| 3 | Unibord 784: now Holz-Her AND Festool compatible; description, specs and meta updated |
| 4 | Both Unibord 784 and 786 now show full cartridge dimensions: ø63 × 79 mm, Box of 24 (≈15 kg) |
| 5 | Reordered: Unibord 786 now appears before 784 |
| 6 | Reordered: UniPUR 8310 now appears before 8311; highlight class moved to 8310 (first-in-section pattern) |
| 7 | UniPUR 8310 packing: "2 kg / 20 kg / 200 kg" → "2 kg / 20 kg" |
| 8 | New application filter: **Laminate Pressing** added; Nunivil 1004, 1108, 135AV all tagged (existing apps retained) |
| 9 | New application filter: **Acrylic Pressing** added; Nunivil 211E tagged (existing apps retained) |
| 10 | Nunivil 270 BV — XT6 prominence: tag updated to "HARDWOODS & EXOTIC WOODS — D4 / XT6 LOAD-BEARING"; description rewritten with all three standards (EN 204/205 D4, EN 14257-WATT 91, EN 17619-XT6) bolded; first spec pill is now a green "EN 17619-XT6 STRUCTURAL" badge |
| 11 | Nunipur 7029: "Post-forming" badge removed; data-app simplified to "3D Membrane Press" only |
| 12 | **New product: Nunivil 80** added — PVAc assembly card placed after Nunivil 118, with prominent green "UNI EN 12456 — CHAIRS" spec pill |
| 13 | Email domain corrected throughout: info@unicolindia.in → **info@unicolindia.com** (Microsoft 365 mailbox) |

Product count: **40 → 41**.

### Round 2: v4 → v5 (EmailJS form went live)

Three rounds of debugging:

| Issue | Cause | Fix |
|---|---|---|
| `emailjs is not defined` error on page load | Deprecated EmailJS CDN URL (`cdn.emailjs.com`) was phased out by EmailJS | Switched to `cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js`, then **inlined the 3.9KB SDK directly into the HTML** for zero CDN dependency |
| init() API broken | EmailJS v4 changed `emailjs.init('KEY')` to `emailjs.init({ publicKey: 'KEY' })` | Updated to v4 object syntax |
| `400 Bad Request — The template ID not found` | User provided the template's **name** (`UnicolIndiaCatalogInquiry`), not its auto-generated **ID** | Used the actual EmailJS-generated Template ID: `template_1pz6edj` |

---

## File Architecture

Self-contained single HTML file, ~155 KB. No external dependencies — all images are base64-embedded, the EmailJS SDK is inlined, and only Google Fonts are loaded from CDN (graceful degradation if blocked).

### Structure (3 pages, horizontal slide transitions)

| Page | Description |
|---|---|
| **Cover** | Factory aerial background, Unicol India branding, ISO 9001 / ISO 14001 / ISCC certification badges, "View Catalogue" CTA |
| **Catalogue** | Sticky header (dual filter bar + 12-tile category image strip), product card grid, footer strip |
| **Contact & Enquiry** | Split layout — company info + Google Maps link on left; live EmailJS-powered product enquiry form on right |

Navigation: top nav bar with 3 tabs (Cover · Catalogue · Contact & Enquiry); smooth horizontal slide animation between pages.

---

## EmailJS Configuration (LIVE)

### Live keys embedded in the HTML

```javascript
const EMAILJS_PUBLIC_KEY  = 'xJBuCekPm9MhIXedP';
const EMAILJS_SERVICE_ID  = 'service_hlk76ui';
const EMAILJS_TEMPLATE_ID = 'template_1pz6edj';

emailjs.init({ publicKey: EMAILJS_PUBLIC_KEY });
```

### Service details
- **Provider:** EmailJS (free tier — 200 emails/month)
- **Email Service:** Microsoft 365 (Outlook), connected to `info@unicolindia.com`
- **Service name in dashboard:** Unicol_Microsoft365
- **SDK version:** @emailjs/browser v4.4.1 (inlined directly in HTML, no CDN dependency)
- **Backup service available (not active):** Gmail `unicolind@gmail.com` — can be wired in as fallback if Microsoft OAuth expires

### Current template — "UnicolIndiaCatalogInquiry" (Template ID `template_1pz6edj`)

| Field | Value |
|---|---|
| **To Email** | `info@unicolindia.com` |
| **From Name** | `Unicol India Website Enquiry` |
| **Reply To** | `{{from_email}}` ← so hitting reply in Outlook goes to the enquirer |
| **Subject** | `New Enquiry from {{from_name}} — {{from_company}} ({{product}})` |
| **Body** | See below |

**Current body content:**

```
New product enquiry received from the Unicol India website.

NAME:        {{from_name}}
COMPANY:     {{from_company}}
EMAIL:       {{from_email}}
PHONE:       {{phone}}
LOCATION:    {{city}}
INTEREST:    {{product}}

MESSAGE:
{{message}}

---
Submitted via www.unicolindia.com contact form
```

### Form field → Template variable mapping

| Form field (HTML element ID) | Template variable | Required |
|---|---|---|
| `f-name` | `{{from_name}}` | Yes |
| `f-company` | `{{from_company}}` | Yes |
| `f-email` | `{{from_email}}` | Yes |
| `f-phone` | `{{phone}}` | No (defaults to "Not provided") |
| `f-city` | `{{city}}` | Yes |
| `f-product` | `{{product}}` | No (defaults to "Not specified") |
| `f-message` | `{{message}}` | No (defaults to "No message provided") |

### How to modify the template

1. Log into https://dashboard.emailjs.com
2. Click **Email Templates** in left sidebar
3. Open the template named "UnicolIndiaCatalogInquiry"
4. Edit subject, body, or any field; click **Save**
5. Changes take effect immediately — no HTML re-deploy needed (as long as variable names aren't changed)

### If adding new template variables

If you add a new variable to the template (e.g., `{{enquiry_source}}`), you must also update the JS code in the HTML file (around line 1086) to send that variable:

```javascript
emailjs.send(EMAILJS_SERVICE_ID, EMAILJS_TEMPLATE_ID, {
  from_name:    name,
  from_company: company,
  // ... existing fields ...
  enquiry_source: 'Website Catalogue',  // NEW
})
```

---

## Products Catalogue — 41 Products in 9 Chemistry Categories

### EVA / PO — Edgebanding (10 cards)
| Code | Tag |
|---|---|
| Unibord 625.20 | Super Transparent — HIGHLIGHT |
| Unibord 607M.25 | All-Round Standard |
| Unibord 688.25 | Multilayer & Soft-forming |
| Unibord 677.25 | Versatile *(was: Versatile High Fill — corrected v5)* |
| Unibord 694.25 | Soft-forming |
| Unibord 637.25 | Slow Feed — Low Temperature |
| Unibord 771.25 | High Speed Polyolefin (PO) |
| Unibord 635P.25 | Manual — PVC Edges |
| Unibord 786 | Cartridge — Transparent; ø63 × 79 mm; Holz-Her & Festool *(reordered v5)* |
| Unibord 784 | Cartridge — Holz-Her & Festool; ø63 × 79 mm *(Festool added v5)* |

### HMPUR — Edgebanding (3 cards)
| Code | Tag |
|---|---|
| UniPUR 8310 | Transparent Glue Line — HIGHLIGHT *(reordered v5; was after 8311)* |
| UniPUR 8311 | High Speed Ivory/White |
| UniPUR 8320 | High Speed Mini Slugs |

### HMPUR — Flat Lamination (4 cards)
| Code | Tag |
|---|---|
| UniPUR 8401PT | UV Traceable — HIGHLIGHT |
| UniPUR 8401 | Sandwich Elements |
| UniPUR 8412 | Anti-Yellowing |
| UniPUR 8411 | Long Open Time (also Profile Wrapping) |

### HMPUR — Profile Wrapping (2 cards)
| Code | Tag |
|---|---|
| UniPUR 8220 | ABS/PVC/Veneer — HIGHLIGHT |
| UniPUR 8217 | PVC/Aluminium |

### PVAc — Nunivil Range (8 cards) *(was 7 — Nunivil 80 added v5)*
| Code | Tag |
|---|---|
| Nunivil 1004 | D3/D4 Water Resistant — HIGHLIGHT *(now also tagged Laminate Pressing)* |
| Nunivil 1008 | D3/D4 Parquet & Veneer |
| Nunivil 1028 | D3/D4 Fingerjoint & Creep Resistant |
| Nunivil 1108 | D4 One-Component Parquet *(now also tagged Laminate Pressing)* |
| Nunivil 118 | Dowel Gluing Automatic Machines |
| **Nunivil 80** | **Hardwoods & Chairs — UNI EN 12456 Creep Test certified** *(NEW v5)* |
| Nunivil 135AV | Post-forming Thermo-reactivation *(now also tagged Laminate Pressing)* |
| Nunivil 211E | Foil Covering PVC/Aluminium *(now also tagged Acrylic Pressing)* |

### EPI (1 card)
| Code | Tag |
|---|---|
| Nunivil 270 BV | Hardwoods & Exotic Woods — D4 / XT6 Load-Bearing — HIGHLIGHT *(XT6 emphasis added v5)* |

### PU Dispersion (1 card)
| Code | Tag |
|---|---|
| Nunipur 7029 | Membrane Press PVC & Synthetic Foils — HIGHLIGHT *(Post-forming tag removed v5)* |

### UF Resin (3 cards)
| Code | Tag |
|---|---|
| Resina 404 | E1 Class Veneering & Plywood — HIGHLIGHT |
| Resina 405 | E0.5 Class Low Emission |
| Resin 506 | Veneer Splicing |

### EVA — Packaging (1 card)
| Code | Tag |
|---|---|
| Unimelt 468 | Packaging & Architrave Short Open Time |

### Hardeners & Catalysts (2 cards)
| Code | Tag |
|---|---|
| Hardener 302 | Catalyst for PVAc & PU Dispersion |
| Hardener 303 | Catalyst for PVAc / EPI Dispersions |

### Edgebanding Process Agents (4 cards)
| Code | Tag |
|---|---|
| Release Agent U111 | Panel Surface Protection Transparent — HIGHLIGHT |
| Protecting Agent U112 | Pressure Roller Protection Green |
| Antistatic Agent U113 | Glue Joint Cooling Light Blue |
| Cleaning Agent U114 | Edge Cleaning & Shine Pink |

### PUR Machine Cleaners (2 cards)
| Code | Tag |
|---|---|
| Purmelt Cleaner Red | PUR Tank & Gun Cleaner |
| PUR Roll Cleaner | PUR Roller Cleaner |

---

## Filter System

The catalogue uses **two-dimensional filtering**: Category (chemistry type) AND Application (use case). Both filters use AND logic — a card is visible only if both filters match.

### Category Filter Buttons
| ID | Value |
|---|---|
| `cat-all` | all |
| `cat-EVA` | EVA |
| `cat-PO` | PO |
| `cat-HMPUR` | HMPUR |
| `cat-PVAc` | PVAc |
| `cat-EPI` | EPI |
| `cat-PUDisp` | PU Dispersion |
| `cat-UF` | UF Resin |
| `cat-Aux` | Auxiliary |

### Application Filter Buttons
| ID | Value |
|---|---|
| `app-all` | all |
| `app-EB` | Edgebanding |
| `app-FL` | Flat Lamination |
| `app-PW` | Profile Wrapping |
| `app-3D` | 3D Membrane Press |
| `app-Asm` | Assembly |
| `app-Ven` | Veneering |
| `app-Pq` | Parquet |
| `app-PF` | Post-forming |
| `app-LamP` | **Laminate Pressing** *(NEW v5)* |
| `app-AcrP` | **Acrylic Pressing** *(NEW v5)* |
| `app-MC` | Machine Cleaning |
| `app-Pkg` | Packaging |

### Filter Functions (in `<script>` block)
- `setCat(val)` — sets category filter, updates button active state, calls `applyFilters()`
- `setApp(val)` — sets application filter, updates button active state, calls `applyFilters()`
- `applyFilters()` — AND logic across `data-cat` (single value) and `data-app` (comma-separated)
- `resetFilters()` — resets both to 'all'

---

## Category Image Strip — 12 Tiles

The image strip is INSIDE the `.sticky-header` wrapper — stays pinned at top while scrolling.

| Tile | Image Source | Filter on Click |
|---|---|---|
| Edge banding | Base64 embedded | App: Edgebanding |
| HMPUR Edge banding | unicol.it URL | Cat: HMPUR, App: Edgebanding |
| Liquid release agents | Base64 embedded | Cat: Auxiliary, App: Edgebanding |
| Profile wrapping | Base64 embedded | App: Profile Wrapping |
| HMPUR Profile Wrapping | unicol.it URL | Cat: HMPUR, App: Profile Wrapping |
| 3D Vacuum press | Base64 embedded | App: 3D Membrane Press |
| Veneer panels | Base64 embedded | App: Veneering |
| Curved panels | Base64 embedded | App: Veneering |
| Kitchen counter tops | Base64 embedded | App: Post-forming |
| **Hard woods and assembly** | unicol.it URL — **PLACEHOLDER** | App: Assembly |
| **Parquet production** | unicol.it URL — **PLACEHOLDER** | App: Parquet |
| Auxiliary products | unicol.it URL — confirmed accurate | Cat: Auxiliary |

---

## Pending Items

| Priority | Item |
|---|---|
| MEDIUM | Replace placeholder photo for "Hard woods & assembly" tile |
| MEDIUM | Replace placeholder photo for "Parquet production" tile |
| LOW | Optional: replace HMPUR Edge banding tile with a process photo instead of product pack shot |
| LOW | Decide whether Unimelt 468 (packaging) deserves its own section heading or merge with Auxiliary |
| FUTURE | Host on `unicolindia.com` as a permanent product browser page |
| FUTURE | Add individual product detail modal/popup on card click |
| FUTURE | Add WhatsApp enquiry button linking to +91 73067 02322 |

---

## Suggested Next Enhancements

### Enhancement 1: Replace the two placeholder tile images

**What's needed:** Two photos to replace the unicol.it stock pack shots currently used for the "Hard woods & assembly" and "Parquet production" tiles.

**Image specs:**
- Square aspect ratio (1:1) or close — will be displayed as a 64×64 px circle (cropped centre)
- Minimum 200×200 px source for clean rendering on Retina/HiDPI screens
- JPG or PNG; will be base64-embedded into the HTML to maintain self-contained file
- Compress to under ~30 KB each to keep total file size reasonable

**Photo subject suggestions:**
- *Hard woods & assembly*: a customer's chair or table assembly line with hardwood furniture; or a close-up of a Nunivil 270 BV glue joint on a hardwood frame
- *Parquet production*: a close-up of parquet planks being glued, or a finished oak/teak parquet floor — ideally something showcasing an Indian flooring customer

**Implementation:** Once Tejash provides the two images (or links), Claude can base64-encode them and inline them into the HTML in one edit each, replacing the current `<img src="https://www.unicol.it/wp-content/uploads/...">` references.

---

### Enhancement 2: Auto-acknowledgement email to the enquirer

When a customer submits the enquiry form, they should automatically receive a thank-you email confirming receipt and setting expectations on response time. This reduces follow-up "did you get my message?" calls and improves professional perception.

**Two implementation approaches:**

#### Approach A: Second EmailJS template (recommended)

1. **Create a second EmailJS template** named "UnicolEnquiryAutoReply"
2. Configure:
   - **To Email:** `{{to_email}}` (the customer's email — will be passed from form)
   - **From Name:** `Unicol India`
   - **Reply To:** `info@unicolindia.com`
   - **Subject:** `Thank you for your enquiry — Unicol India`
   - **Body:**
   ```
   Dear {{to_name}},

   Thank you for reaching out to Unicol India. We have received your
   enquiry regarding {{product}} and our team will respond within 1
   business day.

   In the meantime, if your enquiry is urgent, please feel free to
   contact us directly:

   Phone: +91 73067 02322
   Email: info@unicolindia.com

   Warm regards,
   The Unicol India Team

   www.unicolindia.com
   ```
3. **Note the auto-reply Template ID** (will look like `template_xxxxxxx`)
4. **Update the HTML JavaScript** — add a second `emailjs.send()` call chained after the first:

```javascript
// After successful first send (notification to info@unicolindia.com)
emailjs.send(EMAILJS_SERVICE_ID, 'template_1pz6edj', { /* main template params */ })
  .then(() => emailjs.send(EMAILJS_SERVICE_ID, 'TEMPLATE_ID_OF_AUTO_REPLY', {
    to_email: email,
    to_name:  name,
    product:  product || 'our adhesive solutions',
  }))
  .then(() => { /* success UI */ })
  .catch(err => { /* error UI */ });
```

**Tradeoffs:** Uses 2 EmailJS credits per submission instead of 1. With 200 emails/month free tier, that means 100 enquiries/month max. If projected volume exceeds this, upgrade to EmailJS Personal plan ($7/month for 1,000 emails) or Business ($15/month for 5,000).

#### Approach B: EmailJS built-in Auto-Reply feature

EmailJS dashboards have a built-in auto-reply toggle on each template. Less customizable but simpler:
1. Open the existing template "UnicolIndiaCatalogInquiry"
2. Look for "Auto-Reply" or "Send Auto-Reply to Sender" option
3. Configure the auto-reply body separately
4. No HTML/code changes needed

**Recommendation:** Try Approach B first (simpler, fewer credits). If you need more customization, switch to Approach A.

---

### Enhancement 3: Auto-capture enquiries as Zoho CRM leads

Every catalogue submission should automatically create a Lead in Zoho CRM, slotting into the "marketing operations control tower" architecture already configured.

**Two implementation approaches:**

#### Approach A: Outlook rule → Zoho email-to-lead (simplest)

Zoho CRM has a built-in feature called "Email to Lead" — any email forwarded to a specific Zoho address auto-creates a lead.

1. In Zoho CRM: Setup → Channels → Email → Email-to-Lead Configuration → enable for the user/team that should receive these
2. Note the generated forwarding address (looks like `leadcapture_xxxxxxxx@zohocrm.com`)
3. In Outlook (info@unicolindia.com): create a Rule:
   - When: subject contains "New Enquiry from"
   - Action: forward to the Zoho leadcapture address
4. Zoho parses the email and creates a Lead with mapped fields

**Tradeoffs:** Email parsing is approximate — Zoho will try to extract name/company/email from email body, but mapping may be imperfect. Custom fields like "Product Interest" may end up in the Description instead.

#### Approach B: EmailJS Webhook → Zoho Flow → Zoho CRM (cleanest)

EmailJS supports webhooks that POST structured JSON to a URL when an email is sent.

1. **In Zoho Flow**: create a new flow with a Webhook trigger; copy the generated webhook URL
2. **In EmailJS Dashboard**: Account → Webhooks → add new webhook → paste Zoho Flow webhook URL → trigger on template `template_1pz6edj` "After Send"
3. **In Zoho Flow**: map the JSON fields from EmailJS to Zoho CRM Lead fields:
   - `from_name` → Lead Name (split into First/Last)
   - `from_company` → Company
   - `from_email` → Email
   - `phone` → Phone
   - `city` → City
   - `product` → custom field "Product Interest" (or Lead Source description)
   - `message` → Description
4. **Add Zoho CRM action**: "Create Lead" with mapped fields

**Tradeoffs:** Requires Zoho Flow which may need a paid plan if you're already at the free task limit. Cleanest structured-data mapping.

**Recommendation:** Approach B (webhook → Flow → CRM) is structurally cleaner and matches Tejash's preference for proper data architecture. Approach A is acceptable as a temporary stopgap.

---

## Key Design Decisions (do not change without review)

1. **EmailJS SDK is inlined** — version 4.4.1 (~3.9 KB) is embedded directly in the HTML, not loaded from CDN. This eliminates a CDN dependency point of failure (which we hit during testing — `cdn.emailjs.com` was deprecated). If updating to a new SDK version, replace the entire inline script with the new version's minified content.

2. **All images base64 embedded (mostly)** — the HTML is self-contained. Only 5 images currently load from `unicol.it` URLs (3 confirmed accurate as Unicol product shots, 2 placeholders pending replacement). Do not switch back to relative file paths.

3. **Sticky header wraps both filter-bar and cat-strip** — they MUST stay in the `.sticky-header` div for the application image tiles to stay visible during scroll. Moving them out will break the scroll behaviour.

4. **Filter IDs are unique** — buttons use `id="cat-EVA"` etc., not class-based detection. The `setCat()`/`setApp()` mapping objects in JavaScript must be updated if new categories are added. Both new app filters (`app-LamP`, `app-AcrP`) have been wired into the map.

5. **data-app is comma-separated** — e.g. `data-app="Assembly,Veneering,Laminate Pressing"`. The filter uses `.split(',').map(s=>s.trim()).includes()`. Do not change the delimiter.

6. **Card highlight class** — first/most important product in each section has class `card highlight` which adds navy top border and light blue background. Do not arbitrarily move highlight between cards without strategy reason.

7. **TDS rule** — product descriptions must come verbatim from official Unicol TDS PDFs only. Do not invent or paraphrase specifications. Exception: Unibord 784/786 dimensions were sourced from Unicol distributors (machines4u.com.au, beyondtools.com, jaccraindustries.com) when TDS not available — verified ø63 × 79 mm.

8. **Brand colour** — Unicol India navy blue is `#001F3F`. Green accent badges for prominent certifications (e.g., XT6, EN 12456) use `#1d6a3a`. Do not substitute.

9. **EmailJS init() uses v4 object syntax** — `emailjs.init({ publicKey: KEY })`, NOT `emailjs.init(KEY)`. The string-argument form is deprecated.

10. **Email domain is `.com`** — `info@unicolindia.com` is the live business mailbox on Microsoft 365. The previous `.in` address is retired. All 6 references in the HTML use `.com`.

---

## Contact Information (embedded in footer, contact page, and EmailJS template)

```
Unicol India Private Limited
A57, Giriraj Industrial Estate
Mahakali Caves Road, Andheri (East)
Mumbai 400093, Maharashtra, India

Tel:      +91 73067 02322
Email:    info@unicolindia.com  (Microsoft 365)
Website:  www.unicolindia.com
LinkedIn: https://www.linkedin.com/company/unicol-india-private-limited/
YouTube:  https://www.youtube.com/@unicolindia

Principal: Unicol SRL, Fontanelle (TV), Italy — www.unicol.it
```

Certifications displayed: ISO 9001 · ISO 14001 · ISCC

Google Maps link: `https://maps.google.com/?q=A57+Giriraj+Industrial+Estate+Mahakali+Caves+Road+Andheri+East+Mumbai+400093`

---

## TDS Files Used (verified against official Unicol data sheets)

| File | Product |
|---|---|
| N80ST_ENG_rev04_set22.pdf | **Nunivil 80** *(added v5)* |
| N1004ST_ENG_rev08_nov19.pdf | Nunivil 1004 |
| N1008ST_ENG_rev08_nov19.pdf | Nunivil 1008 |
| N1028ST_ENG_rev03_ott22.pdf | Nunivil 1028 |
| N1108ST_ENG_rev03_jun16.pdf | Nunivil 1108 |
| N118ST_ENG_rev03_jul09.pdf | Nunivil 118 |
| N135AV_ST_ENG_rev00_nov14.pdf | Nunivil 135AV |
| N211EST_ENG_rev01_gen23.pdf | Nunivil 211E |
| N270_BV_ST_ENG_rev01_gen25.pdf | Nunivil 270 BV |
| NUNIPUR_7029_ST_ENG_rev01_apr26.pdf | Nunipur 7029 |
| Resin_404.pdf | Resina 404 |
| R405_ST_ENG_rev05_jan26.pdf | Resina 405 |
| Resin_506.pdf | Resin 506 |
| U468_ST_ENG_rev00_mar24.pdf | Unimelt 468 |
| RELEASE_AGENT_U111_ST__ENG_rev01_Jan23.pdf | Release Agent U111 |
| PROTECTING_AGENT_U112_ST_ENGrev01_jan23.pdf | Protecting Agent U112 |
| ANTISTATIC_AGENT_U113_ST_ENG_rev01_jan23.pdf | Antistatic Agent U113 |
| CLEANING_AGENT_U114_ST_ENG_rev01_jan23.pdf | Cleaning Agent U114 |
| PUR_Melt_Cleaner_Red.pdf | Purmelt Cleaner Red |
| PUR_ROLL_CLEANER.pdf | PUR Roll Cleaner |
| Hardener_I302ST_ENG_rev05_nov14.pdf | Hardener 302 |
| Hardener_I303ST_ENG_rev07_nov19.pdf | Hardener 303 |
| Unipur_8310_PDF.PDF | UniPUR 8310 |

**Note:** EVA/PO Unibord and UniPUR 83xx/84xx/82xx grades — TDS not uploaded in this session. Card content carried forward from earlier sessions.

---

## How to Continue in a New Chat

To resume this work seamlessly:

1. Upload both files to the new chat:
   - `unicol_india_catalog_v5.html` (the live catalogue)
   - `HANDOFF_NOTE_Unicol_Catalog_v5.md` (this document)
2. State which enhancement you want to tackle:
   - "Help me fine-tune the EmailJS template content" → Enhancement 2 area
   - "I have the two replacement photos — please inline them" → Enhancement 1
   - "Let's set up the Zoho CRM integration" → Enhancement 3
   - "Add a WhatsApp button to the catalogue" → from FUTURE pending list
3. The new Claude session will have full context to act immediately.

---

*End of handoff. Next session: pick up at any of the three enhancements above, or address the pending items list.*
