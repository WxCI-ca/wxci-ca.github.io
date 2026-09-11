# wxci.ca — Static Site Hosting Guide

The site is a **single self-contained file** (`website/index.html`): inline CSS, no JavaScript, no build step. The only external dependency is the Raleway webfont from Google Fonts (site falls back to system serif if offline).

---

## Option A — Google Cloud Storage (static hosting)

```bash
# 1. Create a bucket (or reuse one)
gsutil mb -p <PROJECT_ID> -b on -l northamerica-north1 gs://wxci.ca

# 2. Upload
gsutil cp website/index.html gs://wxci.ca/index.html
gsutil web set -m index.html gs://wxci.ca            # main page suffix

# 3. Public read
gsutil iam ch allUsers:objectViewer gs://wxci.ca

# 4. (SSL) Serve behind a load balancer with a managed TLS cert for wxci.ca,
#    or point DNS at the bucket via Cloud Run / Firebase Hosting alternative:
firebase deploy --only hosting           # if you prefer Firebase (GCS-backed) with free HTTPS
```

Simplest fully-managed HTTPS variant: `firebase init hosting` → set public dir to `website/` → `firebase deploy`. Point wxci.ca's A record at the Firebase-provisioned IPs.

## Option B — GoDaddy Web Hosting

1. GoDaddy dashboard → your hosting plan → File Manager (or SFTP with the credentials from "Settings").
2. Upload `index.html` to `public_html/` (overwrite the GoDaddy placeholder).
3. Done — no server-side code required.

## Option C — GoDaddy domain + free static host (cleanest)

Keep the domain at GoDaddy; host the file for free:
- **GitHub Pages:** push `website/` to a repo → Settings → Pages → deploy from branch → point `wxci.ca` CNAME (GoDaddy DNS → GitHub Pages IP) → enable HTTPS.
- **Cloudflare Pages:** upload `index.html` in the dashboard → attach `wxci.ca` → auto-HTTPS.

## Files

| File | Purpose |
|---|---|
| `website/index.html` | The site (single file) |
| `website/HOSTING.md` | This guide |

## Content blocks (for future edits)

- Hero: control-plane positioning + 4 trust chips
- Stack: 5-step pipeline (Sense → Extract → Classify → Decide → Govern) + Verdict/WaveCore feature columns
- Industries: Defence & Security · Mining · Environmental Monitoring (WxCI pedigree)
- Stats row: <100 ms · <$300 · seconds OTA · 100% evidence
- Why WxCI: sovereignty + pedigree + active programs panel
- Contact: mailto CTA + 90-day pilot offer
