# Promed Hospital — Deployment Reference

Quick reference for the live setup. Last verified: **2026-09-21** (site confirmed live over HTTPS).

## Live URLs
- **Primary:** https://promedhospital.co.in ✅ live
- **www:** https://www.promedhospital.co.in ✅ live (redirects to primary)
- **Vercel default:** https://promedhospital.vercel.app ✅ live

## Stack
| Layer | Provider | Details |
|-------|----------|---------|
| Domain registrar | GoDaddy | `promedhospital.co.in` (.co.in) |
| DNS | GoDaddy nameservers | `ns63.domaincontrol.com`, `ns64.domaincontrol.com` |
| Hosting | Vercel | Auto-deploys from GitHub `main` |
| Source code | GitHub | https://github.com/lochanlochi/Promedhospital |
| SSL/HTTPS | Vercel (auto) | Free, auto-renewing certificate |

## DNS records (at GoDaddy)
| Type | Name | Value | Purpose |
|------|------|-------|---------|
| `A` | `@` | `216.198.79.1` | Apex → Vercel |
| `CNAME` | `www` | `cname.vercel-dns.com` | www → Vercel (legacy value, works) |

> Vercel's newer recommended www value is `9d5696fd4c840440.vercel-dns-017.com` — optional; switching to it clears the "DNS Change Recommended" flag but the current value works fine.

Leave untouched at GoDaddy: the two `NS` records, `SOA`, `CNAME _domainconnect`, and `TXT _dmarc`.

## How to update the website
1. Edit files locally (e.g. `index.html`).
2. Commit and push:
   ```bash
   git add -A && git commit -m "your message" && git push
   ```
3. Vercel auto-deploys within seconds. No manual step needed.

## How to check the site is live (future checks)
- **In a browser:** open https://promedhospital.co.in (use incognito to avoid cache).
- **DNS propagation:** https://dnschecker.org → enter `promedhospital.co.in`, type `A` → should show `216.198.79.1`.
- **From PowerShell:**
  ```powershell
  Resolve-DnsName promedhospital.co.in -Type A
  Invoke-WebRequest https://www.promedhospital.co.in -UseBasicParsing | Select StatusCode
  ```
  Note: testing the apex over HTTPS from Windows PowerShell 5.1 may show an SSL trust error — that is a PowerShell quirk, not a real problem. Real browsers load it fine.

## Troubleshooting notes (from initial setup)
- **"Status hold" / IDLE on the domain:** newly bought `.co.in` domains require registrant email verification and sometimes KYC. Until lifted, the domain won't resolve. Resolved via GoDaddy (support: 040-49187600).
- **Can't edit GoDaddy's "Parked" A record:** don't edit it — add your own `A @` record; GoDaddy replaces the parked one.
- **"Invalid Configuration" in Vercel:** normal until DNS propagates; click Refresh after ~15 min.

## TODO before wide launch
- [ ] Replace placeholder phone number `+918000000000` in Call / WhatsApp / Emergency links (search `918000000000` in `index.html`).
- [ ] Swap SVG placeholder illustrations for real photos of the hospital and Dr. Madhusudan.
