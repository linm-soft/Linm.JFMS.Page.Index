# Linm.JFMS.Page.Index

Static landing + workflow demo for **J-FMS** (Fleet Management).

| URL | File (SSOT) |
|-----|-------------|
| `/` | `docs/index.html` |
| `/workflow-demo.html` | `docs/workflow-demo.html` |
| `/jfms/` | rewrite → `/` (Cloudflare `_redirects`) |

Deploy target: **[Cloudflare Pages](https://developers.cloudflare.com/pages/)**.

Skill: **`/set-up-deploy-static-page`** (Linm.Development.Rules).

---

## Structure (single SSOT)

```
Linm.JFMS.Page.Index/
├── docs/                 ← publish root (edit here only)
│   ├── index.html
│   ├── workflow-demo.html
│   ├── _redirects
│   └── _headers
├── wrangler.toml         ← project name + pages_build_output_dir = "docs"
├── package.json          ← dev / deploy / check
└── README.md
```

**Rules**

- HTML lives **only** under `docs/` — no parallel copies at repo root.
- No Node bundler / no `yarn install` for runtime — static files deploy as-is.
- Node 18+ is only for `wrangler` preview/deploy.

---

## Local preview

```bash
npm run check   # required files present
npm run dev     # → http://127.0.0.1:8788
```

Or open `docs/index.html` in a browser (relative links work offline).

---

## Deploy — Cloudflare Dashboard (Git)

1. Push to GitHub (`{org}/Linm.JFMS.Page.Index`).
2. Cloudflare → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
3. Build settings:

| Field | Value |
|-------|--------|
| Framework preset | **None** |
| Build command | *(empty)* |
| Build output directory | `docs` |
| Root directory | `/` |
| Production branch | `main` |

4. Default URL: `https://linm-jfms-page-index.pages.dev` (or chosen project name).

### Custom domain

1. Pages project → **Custom domains** → set host (e.g. `demo.example.com`).
2. Zone must use **Cloudflare DNS**.
3. Wait SSL **Active**. Prefer **one** host (Cloudflare **or** GitHub Pages) per custom domain.

---

## Deploy — CLI (`wrangler`)

```bash
npx wrangler login   # one-time
npm run deploy
```

Project name: `linm-jfms-page-index` (`wrangler.toml` + `--project-name`).

---

## Notes

| Topic | Detail |
|-------|--------|
| Secrets | None (pure static) |
| SPA | N/A — multi HTML pages |
| Cache | HTML `must-revalidate` via `_headers` |
| GitHub Pages (legacy) | Source = `/docs` on `main` — avoid dual-host same domain |

---

## Not this pattern

| Pattern | Use instead |
|---------|-------------|
| Webpack MFE → versioned Pages Index | `/review-mfe-setup` · `deploy-pages-index.cjs` `@v2` |
| Demo HTML in app repo → push into shared Pages Index | `static-html-deploy-pages-index.md` |
