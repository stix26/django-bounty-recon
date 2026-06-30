# Django Bug Bounty Reconnaissance

Security research repository targeting the [Django HackerOne program](https://hackerone.com/django).  
This repo documents automated reconnaissance runs, methodology, and raw data for future reference.

## Repository Structure

```
├── {YYYY-MM-DD}/           # Date-stamped session directory
│   ├── session-log.md      # Narrative of tools, findings, and blockers
│   ├── targets.txt         # In-scope asset list for the session
│   ├── subdomains.txt      # Subdomain enumeration output
│   ├── scan-results.txt    # Probe responses and scanner results
├── README.md               # (this file)
```

Each session is fully self-contained. Raw data is preserved so that a vulnerability discovered later can be traced back to the reconnaissance that enabled it.

## Scope

Django source code (github.com/django/django), djangoproject.com infrastructure, documentation site, Trac ticket tracker, and related project infrastructure.

## Sessions

| Date | Focus | Outcome | Notes |
|------|-------|---------|-------|
| 2026-06-29 | Django infrastructure + source code | No vulnerabilities identified; 43 subdomains enumerated; CVEs from June 3 release all patched | Django 6.0.6 / 5.2.15 current; 0 nuclei matches; no unreleased fixes found |

## Tools

- [subfinder](https://github.com/projectdiscovery/subfinder) — subdomain enumeration
- [nuclei](https://github.com/projectdiscovery/nuclei) — template-based vulnerability scanning
- [HackerOne API](https://api.hackerone.com/) — program scope and report submission
- [GitHub API](https://docs.github.com/en/rest) — commit analysis and code review

## Disclaimer

This repository contains output from automated security reconnaissance tools.  
No vulnerabilities were verified or exploited. All work was performed within the scope of the Django HackerOne program and in compliance with its policies.
