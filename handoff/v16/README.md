# v16 — The AccreditaPass Rename

Stage S-RN of the portfolio closure: the run that changed what the application is called,
and nothing else.

**Three name layers, exactly one of them changed.** The product name became **AccreditaPass**
— the repository is now `jdsaire/accreditapass`, the live site is
`https://jdsaire.github.io/accreditapass/`, the page title and all three dictionaries say
AccreditaPass, and the browser storage keys are `accreditapass.theme` and
`accreditapass.locale`. The `FifaPressApp` C# namespace did **not** change, by decision: 413
identifier occurrences across 133 files are byte-identical, no `.csproj` and no `.razor` was
touched, and the live site still serves `FifaPressApp.*.wasm` — which is precisely why it
still boots. FIFA as **subject matter** did not change either: the non-affiliation
disclaimer, the FIFA Event Media Operations contact, the `help.contact.fifa*` key names and
every World Cup reference survive. No blanket find-replace on the string `FIFA` was ever run.

**The failure mode was the point of the run.** Four hard-coded paths in
`.github/workflows/deploy-pages.yml` carry the project subpath, including the build-time
`sed` that rewrites `<base href>` and the SPA-routing `basePath`. Miss one and the deployed
Blazor app loads a blank page behind a fully green build — the tracked `index.html` looks
correct locally whatever the workflow says. Those four were the first commit and the last
verification.

**Sequencing was non-negotiable and observed.** Four rename commits, then the principal's
merge, then `gh repo rename`, then the live check. The site was knowingly down between the
merge and the rename; renaming first would have kept it down for the length of review instead.

**One thing the runbook did not predict.** A repository rename invalidates the GitHub Pages
deployment, and for about ten minutes the CDN keeps serving a stale copy that looks healthy.
The new URL returned 200 with correct content, then 404 once the cache expired. An explicit
`workflow_dispatch` redeploy fixed it, after which every check was re-run cache-busted. The
old Pages URL now returns 404 and does **not** forward, exactly as A17 warned.

**Also here, by the principal's decision:** `pt.json` harmonized to Brazilian Portuguese.
The dictionary was already inconsistent — `"Minhas solicitações"` is Brazilian and is pinned
by a test, while the rest was European — so 34 lines now read consistently as pt-BR,
including `registo` → `registro`, the app's core domain noun. Three disclaimer strings
changed by single-word variant corrections; English and Spanish stayed byte-identical.

Eleven old-name occurrences outside `handoff/` were deliberately preserved as historical
record, in the same spirit `handoff/v1` preserved the EventEase-era name. Tests: 512 → 512
frontend + 33 → 33 backend, green after every commit.

- [`CC-PLAN-v16.md`](CC-PLAN-v16.md) — the plan as approved before task 2, including the
  preflight baseline, the three name layers, and the five resolutions the principal
  approved before any file was touched.
- [`Completion-Report-v16.md`](Completion-Report-v16.md) — the five-commit table with real
  SHAs, a PASS line against all 15 success criteria with evidence in each cell, the eleven
  preserved occurrences named individually, five authorised deviations, the decisions
  resolved autonomously, and the findings — including A17's undercount, the CDN masking the
  Pages outage, and a pre-existing flaky test reproduced on the untouched baseline.

**Audited/executed against:** `main` @ `79e3220`.
