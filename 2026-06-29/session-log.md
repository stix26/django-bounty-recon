# Bug Bounty Session — 2026-06-29 (Django)

**Target:** Django (HackerOne program)  
**HackerOne User:** stix26  

---

## Summary

Full recon pipeline against Django — open-source Python web framework, active HackerOne bounty program.
**No vulnerabilities found.** Data preserved for future reference.

## Pipeline

1. **Program scope check** — Source code repos, djangoproject.com infrastructure, Trac instance
2. **GitHub commit analysis** — Audited 100+ recent commits on main and 3 stable branches
3. **CVE/advisory research** — Checked June 3 security release and all recent advisories
4. **Subdomain enumeration** — 43 subdomains on djangoproject.com
5. **Live host probing** — 9 accessible targets
6. **Nuclei scanning** — Django detection confirmed, 0 CVE matches
7. **Trac ticket review** — 3 open tickets with "security" keyword (all feature requests)

## GitHub Commit Findings

| Date | Commit | Detail |
|------|--------|--------|
| Jun 3 | CVE-2026-6873 | Signed cookie salt namespace collisions (low) |
| Jun 3 | CVE-2026-7666 | SMTP connection initialization (low) |
| Jun 3 | CVE-2026-8404 | Cache-Control case sensitivity (low) |
| Jun 3 | CVE-2026-35193 | Authorization header caching (low) |
| Jun 3 | CVE-2026-48587 | Vary header whitespace (low) |
| Jun 19 | CVE-2026-35193 (follow-up) | Qualified cache-control directives |

All patched in Django 6.0.6 / 5.2.15. No unreleased fixes found.

## Infrastructure

- **www.djangoproject.com** — nginx + Django, strong security headers, HSTS preloaded
- **code.djangoproject.com** — Trac instance, Django 6.2 detected
- **docs.djangoproject.com** — Read the Docs hosted documentation
- **dashboard.djangoproject.com** — Django dashboard (DSF metrics)
- **status.djangoproject.com** — Status page
- **httpbin.djangoproject.com** — HTTP testing endpoint

## Files Saved
- `~/Desktop/django-recon/` — full recon folder
- `targets.txt` — in-scope asset list
- `subdomains.txt` — 43 djangoproject.com subdomains
- `scan-results.txt` — probe output + nuclei + CVE data
- This document

## Open Security Tickets (Trac)
- #37160 — Make admin views consistently raise PermissionDenied
- #31923 — Add COEP/CORP header support
- #27806 — More dynamic ALLOWED_HOSTS

All are feature enhancements, not exploitable vulnerabilities.

## Aggressive Testing Results

All false positives verified. Summary of each:

### CORS
- `developers.cloudflare.com` — Access-Control-Allow-Origin: * (docs site, intentional)
- `blog.cloudflare.com` — ACAO: https://dash.cloudflare.com (intentional)
- `www.djangoproject.com` — ACAO: https://code.djangoproject.com (intentional, Trac integration)

### Open Redirects (ALL FALSE POSITIVES)
wordpress.org returned 302 for `?url=`, `?redirect=`, `?next=`, `?return=`, `?to=`, `?dest=`, `?target=`
→ Verified: redirects to same page with URL-encoded param value. Not an open redirect.

### Exposed Files (ALL FALSE POSITIVES)
status.djangoproject.com returned 200 for .env, wp-config.php, config.json, dump.sql, etc.
→ Verified: Freshping status page returns HTML for all paths (custom catch-all).
wordpress.org/wp-config.php returned 0 bytes (empty body).

### API Endpoints
status.djangoproject.com returned 200 for /api/, /graphql, /rest/, /wp-json/
→ Freshping catch-all (same HTML status page for all).
developers.cloudflare.com/api/ → Cloudflare API docs, intentional.

### .git Exposure
All targets: .git/HEAD returns HTML (not git content). Not exposed.

### Wayback URLs
Gau failed on all three targets (tool error, not a finding).

### GitHub Secrets
No secrets found in recent commits of WordPress, Django, or workerd.
