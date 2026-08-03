# Linm.JFMS.Page.Index

Static **J-FMS** site at **repo root**. Host: Cloudflare.

| URL | File |
|-----|------|
| `/` | `index.html` |
| `/workflow-demo.html` | `workflow-demo.html` |
| `/jfms/` | `_redirects` rewrite |

---

## Structure

```
index.html · workflow-demo.html
page-icon/title.png · login.png
_headers · _redirects
wrangler.toml      # [assets] directory = "."
package.json
README.md
```

---

## Cloudflare (chốt — khớp Deploy command hiện tại)

| Field | Value |
|-------|--------|
| Framework | None |
| Build command | *(empty)* |
| **Deploy command** | `npx wrangler deploy` *(ok with current wrangler.toml)* |
| Root directory | `/` |
| Branch | `main` |

Repo root is the public site. Config:

```toml
[assets]
directory = "."
```

**Local:** `npx wrangler login` → `npm run deploy` · `npm run dev`.

Optional simpler CI: empty Deploy command + Build output directory empty/root — also fine.

### Custom domain

Pages/Workers → Custom domains → DNS CNAME → project hostname.

---

## Brand

| File | Role |
|------|------|
| `page-icon/title.png` | Favicon |
| `page-icon/login.png` | Nav logo |
