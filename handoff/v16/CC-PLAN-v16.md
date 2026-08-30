# CC-PLAN v16 — AccreditaPass Rename

Deploy `P-CC-S-RN-AccreditaPassRename-v1_0` · repo `jdsaire/fifa-press-app` · branch target `main`

---

## Context

Stage S-RN of the portfolio closure. Amendment **A16** renames the application from *FIFA Press App* to **AccreditaPass** — repository slug, GitHub Pages URL, shipped display name, npm package and browser storage keys. The reason is stated in A16 and is not cosmetic: *"FIFApp"* reads as a football app for fans, plausibly commercial, carrying a rights-holder's name it has no relationship to. None of that describes the application. The rename removes the misread at source rather than managing it with disclaimers on every surface.

Three name layers exist in this repository and **exactly one changes**:

1. **Product name — CHANGES.** `fifa-press-app`, `FIFA Press App`, `FIFApp`, `fifapp` where they name this application.
2. **Namespace — DOES NOT CHANGE, by decision.** 133 files carry the `FifaPressApp` C# identifier. A16 never names the namespace, and run v7 already established that namespace and product name are separate concerns. The live site currently serves `FifaPressApp.t6c7ohebcw.wasm` and `FifaPressApp.styles.css` — namespace-derived asset names that must keep working. A namespace rename is a different deploy with its own build verification.
3. **FIFA as subject matter — NEVER CHANGES.** The app is about accreditation for the 2026 World Cup. `FIFA` appears as the name of a real organisation the app is not affiliated with, and per **D12** the name travels where its disclaimer travels.

The known failure mode is **invisible to CI**: GitHub redirects renamed *repository* URLs but not *Pages* URLs, and the workflow rewrites `<base href="/" />` at deploy time, so the tracked source looks correct locally whatever the workflow says. A wrong base path yields a **blank live page with a fully green build**. The live URL is the check, not CI.

**Outcome sought:** `https://jdsaire.github.io/accreditapass/` renders the running application; the namespace and `handoff/` history are untouched; the non-affiliation disclaimer survives; two PRs are opened and left unmerged for the principal.

---

## Task 0 — Preflight: complete, all PASS

| Check | Result |
|---|---|
| `gh` at `~/bin/gh`, authenticated | **PASS** — `jdsaire` (keyring); scopes `gist, read:org, repo, workflow` |
| Rename capability (checked, not performed) | **PASS** — `viewerPermission: ADMIN` + `repo` scope |
| Required inputs readable | **PASS** — 4/4 |
| HEAD | **PASS** — `79e32200bd3fc2eb997ba92080068003d272bca5`, equals `origin/main` |
| Working tree | **PASS** — clean |
| Baseline build | **PASS** — 4 projects, 0 warnings, 0 errors |
| Baseline tests | **PASS** — **545 passed** (512 frontend + 33 backend), 0 failed |
| Baseline live URL | **PASS** — see below |
| Baseline internal md links | **PASS** — **358 of 358** resolve |

**Live baseline verified beyond the status code.** `https://jdsaire.github.io/fifa-press-app/` → HTTP 200; `<base href="/fifa-press-app/" />` correctly injected; SPA decode script present; importmap intact; `.nojekyll` 200; `_framework/dotnet.o9c6oxclqj.js`, `_framework/blazor.webassembly.66stpp682q.js` and app payload `_framework/FifaPressApp.t6c7ohebcw.wasm` all 200. No pre-existing outage. (`blazor.boot.json` 404 is expected — .NET 10 carries boot config inside `dotnet.js`.)

**Toolchain gap — `node`/`npm` not installed** (absent from PATH, nvm and Homebrew). The prompt's guardrail applies: hand-edit the compiled JS to match the TypeScript exactly, verify via the interop test, add no dependency, record in the Completion Report.

---

## Approved resolutions

| # | Decision |
|---|---|
| **R1** | **Q1/Q2 historical occurrences — leave all 11, list as criterion-5 exceptions.** These files' own doctrine (*"preserved as historical record"*) is the doctrine the hard rules cite for `handoff/v1`. Cost accepted: 11 of 24 doc files untouched; the old name survives in 11 places outside `handoff/`. |
| **R2** | **`handoff/README.md` conflict.** It is one of the frozen 27 *and* task 9 needs a v16 row. Resolution: 26 of 27 byte-identical; `handoff/README.md` changes **only by an appended v16 row**, its pre-existing old-name occurrence untouched. |
| **R3** | **Per-commit-green vs. commit grouping.** Each test moves **with its subject** — `LocaleServiceTests` into the display-name commit, `InteropTests` into the storage-key commit. Task 3(c) is absorbed, not skipped. |
| **R4** | **Portuguese harmonized to pt-BR**, full scope, in its own dedicated commit. |
| **R5** | **`resolutions-v4.json` `N4.title` unchanged.** Task 7 scopes the edit to `resolution`; `title` and `precedent` both stay. |

---

## Measured scope — 40 files, 57 occurrences

Independent re-measurement reproduces `verified_state`'s table exactly, file for file. **All 40 are Layer 1.** Exclusions confirmed: **27** files under `handoff/`; **133** files carrying `FifaPressApp` (77 `.cs`/`.csproj`/`.razor`).

| Group | Files | Occ. |
|---|---|---|
| CI/deploy — `.github/workflows/deploy-pages.yml` (`:32`, `:39`, `:57`, `:72`) | 1 | 4 |
| Shipped UI — `index.html:8`, `{en,es,pt}.json:40`, `src/backend/Program.cs:1,138,139` | 5 | 7 |
| Interop — `theme.ts:26`, `locale.ts:31`, `theme.js:19`, `locale.js:27`, `package.json:2`, `package-lock.json:2,8` | 6 | 7 |
| Tests — `InteropTests.cs:102,177,178`, `LocaleServiceTests.cs:134,141` | 2 | 5 |
| Documentation (13 edited, 11 exempt per R1) | 24 | 34 |

**One occurrence the pattern misses:** `tests/frontend/LocaleServiceTests.cs:134` wraps `"FIFA Press` / `App"` across two comment lines, so no line-based grep finds it. Layer 1, will be edited. **57, not 56.** Repo-wide sweep found no other wrapped instance.

### Discrepancy against A17 — record as a finding

A17 step 3 says *"the **29 files** referencing the old name across `*.md`, `*.html`, `*.json`, `*.yml`."* **Measured: 40 live files.** A17 under-counts and mis-scopes in both directions — its four extensions **miss `.ts`, `.js` and `.cs` entirely**, which is exactly where the storage keys and their guard tests live (11 files A17's runbook would never reach); its 29 also appears to fold in `handoff/` history this run excludes. A17 further orders the repo rename as step 1, where this prompt sequences it at task 6 after PR A merges — following the prompt, since renaming first would take the live site down for the length of review.

### Criterion-5 exceptions (R1) — 11 occurrences, unedited

*v7-pinned rename statements:* `ux-ui/00-initial-evaluation/{accessibility-audit,heuristic-evaluation,usability-assessment,usability-test-protocol}.md:3`, `findings-register.md:4`, `00-initial-evaluation/README.md:3` — all read *"since renamed to the FIFA Press App / `src/FifaPressApp/` in v7"*; the `in v7` governs both halves, so renaming would assert something that did not happen.
*Event references:* `ux-ui/README.md:8`, `01-design-research/README.md:41`, `02-ideation/README.md:30`, `03-ui-prototyping/README.md:47` — *"before the FIFA Press App reframing."*
*Code citation:* `03-ui-prototyping/07_BUILD-BRIEF.md:39` — *"Brand string `EventEase` → `FIFA Press App` (line 3)"*; the literal no longer exists in `NavMenu.razor`, which now reads `@L[Locale, "app.name"]`, so changing `app.name` moves the navbar brand automatically.

**Not an occurrence:** `ux-ui/05-iteration/00_RECONCILIATION.md:153` — *"the top bar's FIFA equivalent of `CartSummary`"*. Bare `FIFA`, not one of the four patterns. Untouched.

---

## Replacement strings

| Context | Old | New |
|---|---|---|
| Prose / display | `FIFA Press App` | `AccreditaPass` |
| Repo slug, URL path | `fifa-press-app` | `accreditapass` |
| Live URL | `…github.io/fifa-press-app/` | `…github.io/accreditapass/` |
| Pages base path | `/fifa-press-app/` | `/accreditapass/` |
| SPA `basePath` | `"/fifa-press-app"` | `"/accreditapass"` |
| Storage keys | `fifa-press-app.theme` / `.locale` | `accreditapass.theme` / `.locale` |
| npm package | `fifa-press-app-interop` | `accreditapass-interop` |
| ZIP folder (`docs/setup-guide.md`) | `fifa-press-app-main` | `accreditapass-main` |
| API name (`Program.cs`) | `FIFA Press App API` | `AccreditaPass API` |

---

## Preserve-verbatim, as amended by R4

**Frozen, byte-identical, all three languages** — diffed after every commit:

- `:140 landing.disclosureStrong` — EN `"This is a portfolio demonstration, not a FIFA product."`
- `:141 landing.disclosureBody` — EN `"It is not affiliated with, endorsed by, or connected to FIFA. Every record, holder and decision in it is simulated. The match schedule is real; nothing else is."`
- `:345 help.contact.fifaStrong` — `"FIFA Event Media Operations"` (identical in all three)
- `:346 help.contact.fifaBody` — EN and ES variants
- The key names `help.contact.fifaStrong` / `help.contact.fifaBody`
- `README.md:7`, the non-affiliation paragraph

**Authorized exception (R4):** the **Portuguese** forms of `:140`, `:141` and `:346` change in commit 4 only, and only by pt-BR variant correction. Their meaning, structure and every `FIFA` reference are preserved. EN and ES stay byte-identical throughout. Verbatim check therefore runs as: all three languages frozen through commit 3, then EN+ES frozen and PT diffed to the exact expected three-line delta in commit 4.

Arithmetic note: `FIFA` appears **4×** per dictionary — `:40 app.name` is Layer 1 and changes; `:140`, `:141`, `:345` are Layer 3 and do not.

---

## Portuguese harmonization (R4) — 31 lines, own commit

`pt.json` is **already internally inconsistent**, which is the justification for this pass rather than an argument against it: `"Minhas solicitações"` (×3, at `nav.record`, `record.title`, `matches.requestPending`) is Brazilian — European Portuguese would be *"As minhas solicitações"* — and `tests/frontend/LocaleServiceTests.cs:124` **already pins it as a test expectation**. Everything else is European. `es.json` already uses `registro` ×16, so pt-BR also brings the sibling dictionaries into line.

**Lexical map — 35 occurrences:**

| Old (EP) | New (pt-BR) | n |
|---|---|---|
| `registo` / `Registo` / `registos` | `registro` / `Registro` / `registros` | 19 |
| `registado` / `registada` | `registrado` / `registrada` | 4 |
| `ecrã` | `tela` | 4 |
| `contacto` | `contato` | 3 |
| `utilizador` | `usuário` | 2 |
| `equipa` | `equipe` | 2 |
| `portefólio` | `portfólio` | 1 |

**Grammar — 4 constructions:**

- `:156` `"Escreve o teu e-mail ou nome de utilizador."` → `"Escreva o seu e-mail ou nome de usuário."` — a stray `tu`-form that already clashes with the dictionary's own *seu/você* register
- `:275` `"Está a solicitar acesso a:"` → `"Está solicitando acesso a:"` — EP progressive → BP gerund
- `:162` `não lhe pode dar` → `não pode lhe dar` — clitic placement
- `:314` `não o pode encurtar` → `não pode encurtá-lo` — clitic placement

**Deliberately NOT changed** — valid in Brazilian Portuguese, merely more frequent in European usage; changing them would be a style rewrite rather than a variant correction, and this pass is bounded to forms that are wrong or markedly non-native in pt-BR: `aplicação` ×17, `sessão` ×7, `candidatura` ×6, `guardado` ×2, `credencial`, `jogo`.

**The three disclaimer strings, before → after** (called out in PR A's body for the principal's eyeball):

- `:140` `"…demonstração de portefólio…"` → `"…demonstração de portfólio…"`
- `:141` `"…Todos os registos, titulares…"` → `"…Todos os registros, titulares…"`
- `:346` `"…ter sido registada contra…"` → `"…ter sido registrada contra…"`

Key **names** are untouched, so `TheThreeFilesDefineExactlyTheSameKeys` stays green; `nav.record` is unchanged, so `AStringResolvesInItsOwnLocale` stays green.

---

## Commit sequence

| # | Branch | Commit |
|---|---|---|
| 1 | `deploy/v16-accreditapass-rename` | `fix(ci): point Pages base path and SPA routing at the new project path` |
| 2 | ″ | `feat(i18n): show AccreditaPass as the application display name` |
| 3 | ″ | `chore(rename): move interop storage keys and package name to accreditapass` |
| 4 | ″ | `fix(i18n): harmonize the Portuguese dictionary to Brazilian Portuguese` |
| 5 | ″ | `docs: rename the application to AccreditaPass across the documentation set` |
| 6 | `deploy/v16-accreditapass-rename-archive` | `docs: archive the AccreditaPass rename plan and completion report` |

Build clean and all **545 tests green after every commit**, verified individually. Author *and* committer `jdsaire`; no `Co-authored-by`, no `Generated with`, no AI or vendor reference in any commit message, branch name, PR title or PR body.

---

## Execution

**Task 2 — CI.** Branch `deploy/v16-accreditapass-rename` from `main`. Update the four `deploy-pages.yml` occurrences (`:32` subpath comment, `:39` `sed` base-href rewrite, `:57` `basePath` constant, `:72` pathname comment) to `/accreditapass/`. Most consequential edit in the run. → commit 1.

**Task 3 — runtime.**
- *(a)* `index.html:8` title; `app.name` in `{en,es,pt}.json:40`; `Program.cs:1,138,139`. Per R3, also `LocaleServiceTests.cs:141` assertion **and** its wrapped comment at `:134`. Diff preserve-verbatim across all three languages immediately after. → commit 2.
- *(b)* `theme.ts:26`, `locale.ts:31` → new keys; **hand-edit** `wwwroot/js/{theme,locale}.js` to match exactly (no Node); `package.json` + `package-lock.json:2,8` → `accreditapass-interop`. Per R3, also the three `InteropTests.cs` assertions. Rename `TheThemeStorageKeyIsUnchangedByTheConversion` → `TheThemeStorageKeyMatchesBetweenSourceAndCompiledOutput`, assert the new key in **both** `Source("theme.ts")` and `Compiled("theme.js")` — the current test only reads the compiled side, so this makes it genuinely guard agreement, which matters more than usual with the JS hand-edited. Test is strengthened, never deleted. → commit 3.
- *(c)* Portuguese harmonization per R4. → commit 4.

**Task 4 — docs.** The 13 non-exempt documentation files. Product-name occurrences only. `README.md` title → AccreditaPass with `:7` byte-identical. Do not enter `handoff/`. Leave any `FifaPressApp` identifier inside a code citation or path pointing at unchanged source. Re-count internal markdown links, report N of N against the 358 baseline. → commit 5.

**Task 5 — Gate 1.** Push, open **PR A** via `gh`. Body carries: the 40 files by name layer; the deliberate namespace and `handoff/` exclusions; the preserve-verbatim diff result including the three approved PT disclaimer deltas; link count N of N; build and test results; the 11 criterion-5 exceptions; and **as its first line**, that merging will break the live site until the task-6 rename completes. **Do not merge.** Report number and URL, then wait.

**Task 6 — Gate 2, only after the principal confirms PR A merged.** `gh repo rename accreditapass`; update local `origin`. Await the Pages workflow, then **load `https://jdsaire.github.io/accreditapass/` and confirm it paints** — landing view, disclaimer, language switch. Confirm the old URL no longer serves and record what it returns. Confirm the app writes `accreditapass.*` and no `fifa-press-app.*` key. If blank: report exact console/network errors and name which of the four `deploy-pages.yml` occurrences is the likely cause, checking in order — the four occurrences, then `.nojekyll`, then the SPA `404.html` fallback — and **wait**, do not iterate blindly.

**Task 7 — closure folder.** `~/Downloads/designops-closure/knowledge/strategy/resolutions-v4.json`, key `N4`: in `resolution` replace the dead URL and the product name. `precedent` and `title` unchanged (R5). Report before/after. Committed to no repository.

**Task 9 — archive.** Branch `deploy/v16-accreditapass-rename-archive` from post-merge `main`. Create `handoff/v16/` on the v1–v15 convention: this plan as approved (renamed to `CC-PLAN-v16.md`), `Completion-Report-v16.md`, and a folder `README.md` indexing both. Append one prose v16 row to `handoff/README.md` in the existing voice (R2). Completion Report carries, in order: ordered commit list with short SHAs, branch and both PR numbers; a dense outcome paragraph naming the three-layer invariant and stating it held; a PASS/FAIL table against the prompt's 15 success criteria with **evidence in each cell**; authorised deviations; autonomous decisions; open items. Findings recorded: the A17 discrepancy, the namespace exclusion, the `handoff/` exclusion, the storage-key reset and its one-time cost to visitors, the absent Node toolchain, the 57th wrapped occurrence, and the pt-BR harmonization with its pre-existing-inconsistency rationale. → commit 6. Open **PR B**, **do not merge**, report number and URL.

---

## Verification

Report **PASS/FAIL with evidence** per check:

1. `https://jdsaire.github.io/accreditapass/` renders the running application — verified by loading it and confirming the landing view, disclaimer and language switch paint, plus `_framework/*` and app `.wasm` returning 200. A green workflow is not the check.
2. All four `deploy-pages.yml` occurrences updated.
3. Display name reads AccreditaPass in `<title>` and all three dictionaries.
4. Storage keys `accreditapass.theme` / `.locale` in TypeScript **and** committed JS; interop test asserts the new keys on both sides and still guards source-vs-compiled agreement; full suite green.
5. Zero `fifa-press-app` / `FIFApp` / `fifapp` / `FIFA Press App` outside `handoff/`, **except the 11 exceptions listed above**, enumerated explicitly.
6. `git diff 79e3220 -- handoff/` touches nothing but the new `handoff/v16/` and the appended v16 row (R2) — 26 of 27 byte-identical, the 27th changed only by that row.
7. All 133 `FifaPressApp` identifiers unchanged; `git diff` touches no `.cs`/`.csproj`/`.razor` except the two test files.
8. `:140`/`:141`/`:345`/`:346` byte-identical in **EN and ES**; `:345` byte-identical in PT; PT `:140`/`:141`/`:346` differ **only** by the three approved variant corrections; README `:7` unchanged; `help.contact.fifa*` key names unchanged.
9. Build clean and 545 tests green after **each** of the six commits, verified individually.
10. Internal markdown links N of N against the 358 baseline.
11. Both PRs open and **unmerged** on the two exactly-named branches; `git log` shows `jdsaire` sole author and committer; no trailers; zero AI reference in any message, branch name, PR title or body.
12. Repo rename occurred at task 6, after PR A merged — never earlier.
13. `resolutions-v4.json` `N4.resolution` updated; `N4.precedent` and `N4.title` unchanged.
14. `handoff/v16/` holds plan, Completion Report and README; `handoff/README.md` carries a v16 row.
15. No delegated execution; no personal access token requested, printed or referenced.

**Commands:** build/test each project with `dotnet build -c Release` and `dotnet test -c Release` (no `.sln` at root — four `.csproj` under `src/{frontend,backend}` and `tests/{frontend,backend}`). Live checks via `curl` against the Pages URL for status, `<base href>`, `.nojekyll` and `_framework/*` assets, then a rendered load to confirm paint. All GitHub operations through `gh`.

---

## Stop conditions

Stop and report without proceeding if: HEAD drifts; the tree is dirty; a preserve-verbatim string would change **outside the R4-approved PT delta**; a `FifaPressApp` identifier would change; any `handoff/` file other than the new `v16/` and the appended index row would change; the rename fails or is refused; the live URL does not render after the rename; a build or test fails without immediate confident attribution to the commit in hand; a credential would need to be printed; or a blanket find-replace on `FIFA` is about to run.
