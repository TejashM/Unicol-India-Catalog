# Unicol India Product Catalogue

Single self-contained HTML catalogue (`index.html`) for Unicol India adhesive products. No build step, no dependencies beyond Google Fonts and an inlined EmailJS SDK — the whole site is one file.

## Live site

Once GitHub Pages is enabled on this repo (Settings → Pages → Deploy from branch → `main` / root), the catalogue is available at:

```
https://<your-github-username>.github.io/<repo-name>/
```

## Dealer-branded links

One file serves every dealer. The dealer is selected by a `?dealer=` parameter in the URL — no separate files to maintain.

| Dealer | Link |
|---|---|
| Unicol direct | `https://<your-github-username>.github.io/<repo-name>/` |
| Siddhi Vinayak Trading Co. | `https://<your-github-username>.github.io/<repo-name>/?dealer=svtco` |
| Saptagiri Interio Products | `https://<your-github-username>.github.io/<repo-name>/?dealer=saptagiri` |
| Yash Tooling | `https://<your-github-username>.github.io/<repo-name>/?dealer=yashtooling` |
| Yash Tooling System | `https://<your-github-username>.github.io/<repo-name>/?dealer=yashtoolingsystem` |

## Adding a new dealer

1. Open `index.html`, find the `const DEALERS = { ... }` block (search for "DEALER CONFIGURATION").
2. Add one entry: `slug: { name: '...', email: '...', whatsapp: '91XXXXXXXXXX' }`.
3. Commit and push. Send the dealer their link: `.../?dealer=slug`. No other file changes needed.

## Zoho CRM lead capture

Every enquiry (Contact form + Quick Enquiry) also pushes to Zoho CRM as a Lead via a Deluge webhook, alongside the existing EmailJS notification. See the setup guide kept outside this repo (not committed — contains a webhook key) for deployment steps.
