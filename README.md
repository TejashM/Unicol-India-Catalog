# Unicol India Product Catalogue

Single self-contained HTML catalogue (`index.html`) for Unicol India adhesive products. No build step, no dependencies beyond Google Fonts and an inlined EmailJS SDK — the whole site is one file.

## Live site

```
https://tejashm.github.io/Unicol-India-Catalog/
```

(GitHub Pages always serves from the lowercase `<username>.github.io` subdomain, even though the account and repo names keep their original capitalization — that's why this differs in case from the repo URL.)

## Dealer-branded links

One file serves every dealer. The dealer is selected by a `?dealer=` parameter in the URL — no separate files to maintain. This table is the master reference for every link currently live — update it whenever a dealer is added.

| Dealer | Link |
|---|---|
| Unicol direct | `https://tejashm.github.io/Unicol-India-Catalog/` |
| Siddhi Vinayak Trading Co. | `https://tejashm.github.io/Unicol-India-Catalog/?dealer=svtco` |
| Saptagiri Interio Products | `https://tejashm.github.io/Unicol-India-Catalog/?dealer=saptagiri` |
| Yash Tooling | `https://tejashm.github.io/Unicol-India-Catalog/?dealer=yashtooling` |
| Yash Tooling System | `https://tejashm.github.io/Unicol-India-Catalog/?dealer=yashtoolingsystem` |
| Anushka Sales | `https://tejashm.github.io/Unicol-India-Catalog/?dealer=anushka` |
| Safalta Trading Co | `https://tejashm.github.io/Unicol-India-Catalog/?dealer=safalta` |

## Adding a new dealer

1. Open `index.html`, find the `const DEALERS = { ... }` block (search for "DEALER CONFIGURATION").
2. Add one entry with all seven fields — the first three drive email/WhatsApp, the last four drive the Contact page's address panel:
   ```js
   slug: {
     name:     'Company Name',
     email:    'their@email.com',           // enquiries routed here (CC to Unicol)
     whatsapp: '91XXXXXXXXXX',              // digits only, no + or spaces
     tag:      'Unicol India Authorised Partner · <City>',
     phoneDisplay: '+91 XXXXX XXXXX',       // formatted for display
     addressHtml: `Line 1<br>
             Line 2<br>
             City, State PIN<br><br>
             \u{1F4DE} &nbsp;+91 XXXXX XXXXX<br>
             \u{2709}\u{FE0F} &nbsp;their@email.com`,
     mapsUrl:  'https://maps.google.com/?q=...',  // or a maps.app.goo.gl short link
   },
   ```
3. Commit, push, add a row to the table above. Send the dealer their link: `.../?dealer=slug`. No other file changes needed — the Contact page address, tag line, WhatsApp button, and email routing all pick it up automatically.

## Zoho CRM lead capture

Every enquiry (Contact form + Quick Enquiry) also pushes to Zoho CRM as a Lead via a Deluge webhook, alongside the existing EmailJS notification. See the setup guide kept outside this repo (not committed — contains a webhook key) for deployment steps.

