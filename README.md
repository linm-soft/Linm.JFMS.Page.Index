# Linm.JFMS.Page.Index

Static landing + workflow demo for **J-FMS**. Deploy: **Cloudflare Pages**.

| URL | File |
|-----|------|
| `/` | `index.html` |
| `/workflow-demo.html` | `workflow-demo.html` |
| `/jfms/` | rewrite → `/` (`_redirects`) |

---

## Structure

```
Linm.JFMS.Page.Index/
├── index.html
├── workflow-demo.html
├── page-icon/
│   ├── title.png     ← favicon
│   └── login.png     ← logo nav
├── _headers
├── _redirects
├── wrangler.toml
├── package.json
└── README.md
```

Static files live at **repo root** — Cloudflare default (no `docs/` subfolder).

### Brand

| File | Role |
|------|------|
| `page-icon/title.png` | Favicon / tab |
| `page-icon/login.png` | Nav logo |

---

## Local

```bash
npm run check
npm run dev    # http://127.0.0.1:8788
```

Or open `index.html` in a browser.

---

## Cloudflare Pages (Git)

| Field | Value |
|-------|--------|
| Framework preset | **None** |
| Build command | *(empty)* |
| Deploy command | *(empty)* |
| **Build output directory** | `/` or `.` or empty (repo root) |
| Root directory | `/` |
| Production branch | `main` |

URL: `https://linm-jfms-page-index.pages.dev`

**Cấm CI:** `npx wrangler deploy` (Workers). Local optional: `npm run deploy` after `wrangler login`.

### Custom domain

Pages → Custom domains → Cloudflare DNS CNAME → `{project}.pages.dev`.

---

## GitHub Pages (legacy)

Source: branch `main` / folder **`/` (root)** — not `/docs`.
