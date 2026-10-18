## Profile
- Purpose: GitHub Pages host for the generated draft (Entwurf) preview build of the frontier-design website
- Stage: prototype (repository standard 2026-10-18)
- Gate: `scripts/gate` (CI job `gate`)
- Landing: direct to main after a green gate; agents land: yes
- Release: GitHub Pages deploys from the `deploy` job after `gate`; agents release: no
- Outside issues and PRs: agents triage; outside PRs merge: owner approves
- Sensitive paths: `.github/` (CI and Pages deployment), `.agents/` (orb setup), `scripts/` (the gate and its gitleaks installer), `AGENTS.md` (agent policy)

Deviation from the standard: no `LICENSE` yet in this public repository; the owner decides in issue #1.

## Map
- `index.html`, `en/`, `impressum.html`, `datenschutz.html`, `assets/`, `favicon.svg`, `og.jpg`, `llms.txt`, `robots.txt`, `.nojekyll`: generated build output. The `deploy` job publishes every tracked file of `main` except the repository's own files (`.github/`, `.agents/`, `scripts/`, `AGENTS.md`, `README.md`, `.gitignore`) at https://frontierdsgn.github.io/frontier-design-entwurf/.
- `README.md`: German; also written by the source repository's preview publish step.
- `scripts/gate`: the gate. `scripts/install-gitleaks`: installs the pinned gitleaks it needs.
- `.github/workflows/gate.yml`: CI job `gate`, and job `deploy` (needs `gate`; pushes to `main` and manual runs on `main` only). `.agents/setup`: orb setup.

## Landing (prototype)
Small commits straight to `main` with Conventional Commits subjects; run `scripts/gate` first and push at the end of every session. Commit with a GitHub noreply address (public repository). A change to a sensitive path gets an independent review of its diff (another agent or the owner) before it is pushed, with the verdict recorded in the commit body.

## Rules
- The build comes from the private repository `frontierdsgn/frontier-design` (branch `entwurf-preview`, `ENTWURF=1 npm run build` in `site/`). Fix site content there and republish; do not hand-edit generated files here, they are overwritten by the next publish.
- This is a draft: every page keeps `noindex,nofollow`, `robots.txt` keeps `Disallow: /`, and offers, prices and the Impressum stay placeholders. Never add real personal data. The gate checks the first two.
- References must be relative: the site is served under `/frontier-design-entwurf/`, so root-relative paths break.
- Only the `deploy` job publishes, and only after `gate` passed for the same commit; it never runs on `pull_request`. A new repository file that is not part of the site goes into its exclusion list.

## Checks
`scripts/gate` runs everything (needs `gitleaks` on `PATH`; `.agents/setup` or `scripts/install-gitleaks` installs it). To look at the site, run `python3 -m http.server 8000 --bind 127.0.0.1` in the repository root and open http://127.0.0.1:8000/.
