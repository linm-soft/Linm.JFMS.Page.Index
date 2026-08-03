# Linm.JFMS.Page.Index

Static landing + workflow demo for **J-FMS** (Fleet Management).

| Path | File |
|------|------|
| `/` | `docs/index.html` |
| `/workflow-demo.html` | `docs/workflow-demo.html` |
| `/jfms/` | same as `/` (Cloudflare rewrite) |

Deploy target: **[Cloudflare Pages](https://developers.cloudflare.com/pages/)**.

---

## Structure

```
Linm.JFMS.Page.Index/
├── docs/                 ← build output (static assets only)
│   ├── index.html
│   ├── workflow-demo.html
│   ├── _redirects
│   └── _headers
├── wrangler.toml
├── package.json
└── README.md
```

No Node bundler — publish `docs/` as-is.

---

## Local preview

```bash
# requires Node 18+
npm run dev
# → http://127.0.0.1:8788
```

Or open `docs/index.html` in a browser (relative links work offline).

---

## Deploy — Cloudflare Dashboard (Git)

1. Push this repo to GitHub (`linm-soft/Linm.JFMS.Page.Index`).
2. Cloudflare Dashboard → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
3. Select repo `Linm.JFMS.Page.Index`.
4. Build settings:

| Field | Value |
|-------|--------|
| Framework preset | **None** |
| Build command | *(empty)* |
| Build output directory | `docs` |
| Root directory | `/` (repo root) |
| Production branch | `main` |

5. **Save and Deploy**.
6. Default URL: `https://linm-jfms-page-index.pages.dev` (or project name you chose).

### Custom domain (e.g. `demo.rmms.vn`)

1. Pages project → **Custom domains** → **Set up a domain** → `demo.rmms.vn`.
2. Domain `rmms.vn` must use **Cloudflare DNS** (zone **Active**). Cloudflare will add CNAME for `demo`.
3. Wait SSL **Active**.
4. Remove GitHub Pages custom domain for the same host if still set.

If zone `rmms.vn` is not on Cloudflare yet: add site + set Mat Bao Name Servers to the two `*.ns.cloudflare.com` values Cloudflare shows.

---

## Deploy — CLI (`wrangler`)

```bash
# one-time: login
npx wrangler login

# deploy production
npm run deploy
```

Project name in `wrangler.toml` / `--project-name`: `linm-jfms-page-index`.

---

## Environment notes

| Topic | Detail |
|-------|--------|
| Node | Not required at runtime (static). Node 18+ only for `wrangler`. |
| Secrets | None |
| SPA | N/A — multi HTML pages |
| Cache | HTML `must-revalidate` via `_headers` |

---

## GitHub Pages (legacy)

If you still use GitHub Pages: Source = `/docs` on `main`. Prefer **one** host (Cloudflare **or** GitHub) for a given custom domain.
