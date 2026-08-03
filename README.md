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
│   ├── page-icon/        ← brand (chỉ 2 file)
│   │   ├── title.png     ← favicon / tab
│   │   ├── login.png     ← logo trên page (nav)
│   │   └── README.txt
│   ├── _redirects
│   └── _headers
├── wrangler.toml         ← project name + pages_build_output_dir = "docs"
├── package.json          ← dev / deploy / check
└── README.md
```

### Brand (`page-icon/`)

| File | Role |
|------|------|
| `title.png` | Favicon / title tab |
| `login.png` | Logo page (nav) |

Ghi đè 2 file rồi `npm run deploy`. Starter từ `Linm.RMMS.Data/logo/web-icon/icons/` (`logo-64` → title, `logo-login` → login).

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
2. Cloudflare → **Workers & Pages** → project **Pages** (not a plain Worker) → **Connect to Git**.
3. Build settings:

### Option A — recommended (no custom deploy)

| Field | Value |
|-------|--------|
| Framework preset | **None** |
| **Build command** | *(empty)* |
| **Deploy command** | *(empty / None)* |
| **Build output directory** | `docs` |
| Root directory | `/` |
| Production branch | `main` |

### Option B — custom Deploy command (CI log style)

| Field | Value |
|-------|--------|
| Build command | *(empty)* |
| **Deploy command** | `npm run deploy` |
| Root directory | `/` |

`npm run deploy` = `wrangler pages deploy docs` (see `package.json`).

### ❌ Do not use

```text
npx wrangler deploy
```

That is for **Workers** (needs `main` script or `[assets]`). This repo is **Pages** + `pages_build_output_dir = "docs"`.

Log will say:

- *run `wrangler deploy` on a Pages project, use `wrangler pages deploy`*
- *Missing entry-point to Worker script or to assets directory* → FAIL

Use instead:

```text
npm run deploy
# equivalent:
npx wrangler pages deploy docs --project-name=linm-jfms-page-index
```

4. Default URL: `https://linm-jfms-page-index.pages.dev` (project name).

### Custom domain

1. Pages project → **Custom domains** → set host (e.g. `demo.example.com`).
2. Zone must use **Cloudflare DNS** (CNAME → `{project}.pages.dev`).
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
