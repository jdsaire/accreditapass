# Completion Report — v16 (Stage S-RN: the AccreditaPass Rename)

**Branch:** `deploy/v16-accreditapass-rename` · **PR A:** [#13](https://github.com/jdsaire/accreditapass/pull/13), merged by the principal
**Archive branch:** `deploy/v16-accreditapass-rename-archive` · **PR B:** opened, left unmerged
**Base:** `main` @ `79e3220` — confirmed against `origin/main` before planning, exact match to the deploy prompt's stated HEAD
**Author and committer on every commit:** `Juan Diego S. <88201583+jdsaire@users.noreply.github.com>`

This records what was **executed**, not what was planned. Where the two differ, the
difference is named here.

---

## Outcome

The application is now **AccreditaPass**. The repository is `jdsaire/accreditapass`, the
live site is `https://jdsaire.github.io/accreditapass/`, the shipped display name reads
AccreditaPass in the page title and all three dictionaries, and the browser storage keys are
`accreditapass.theme` and `accreditapass.locale`.

The run's governing invariant was that this repository holds **three name layers** and that
exactly one of them changes. It held. Layer 1, the product name, changed in 46 occurrences
across 29 files. Layer 2, the `FifaPressApp` C# namespace, is byte-identical: 413 identifier
occurrences across 133 files, unchanged, with no `.csproj` and no `.razor` modified — which
is why the live site still serves `FifaPressApp.wg0564o0me.wasm` and `FifaPressApp.styles.css`
and still boots. Layer 3, FIFA as subject matter, is intact: the non-affiliation disclaimer,
the FIFA Event Media Operations contact, the `help.contact.fifa*` key names and every World
Cup reference survive, byte-identical in English and Spanish and semantically identical in
Portuguese. No blanket find-replace on the string `FIFA` was ever run; every edit was made
against a located occurrence whose layer had been identified first.

The rename was sequenced, not opportunistic: four rename commits, then the principal's merge,
then `gh repo rename`, then live verification. The known failure mode — a wrong Pages base
path producing a blank page behind a fully green build — was the first thing fixed and the
last thing verified.

---

## Commits

| # | SHA | Commit |
|---|---|---|
| 1 | `5f288e7` | `fix(ci): point Pages base path and SPA routing at the new project path` |
| 2 | `0224f0a` | `feat(i18n): show AccreditaPass as the application display name` |
| 3 | `08b3b24` | `chore(rename): move interop storage keys and package name to accreditapass` |
| 4 | `c78a932` | `fix(i18n): harmonize the Portuguese dictionary to Brazilian Portuguese` |
| 5 | `c0166ce` | `docs: rename the application to AccreditaPass across the documentation set` |
| — | `1ee4d89` | merge of PR #13 into `main` (created by GitHub's web merge UI) |

Build clean and the full suite green after **every** commit, verified by checking each
commit out in turn and rebuilding from a cleaned `obj/`+`bin/` rather than inferring from the
tip. Tests: 512 → 512 frontend + 33 → 33 backend.

The merge commit `1ee4d89` carries `GitHub <noreply@github.com>` as *committer* because the
principal merged through the web UI. Its author is `jdsaire`. All five authored commits carry
`jdsaire` as both author and committer, with no trailers.

---

## Success criteria

| # | Criterion | Result | Evidence |
|---|---|---|---|
| 1 | Live URL renders the running application | **PASS** | `https://jdsaire.github.io/accreditapass/` HTTP 200 on a cache-busted request (`x-cache: MISS`, `age: 0`); 66 of 66 boot-manifest assets resolve; app payload `FifaPressApp.wg0564o0me.wasm` 200, 240,405 bytes; visual paint confirmed by the principal |
| 2 | All four `deploy-pages.yml` occurrences updated | **PASS** | `:32` subpath comment, `:39` `sed` base-href, `:57` SPA `basePath`, `:72` pathname comment → `/accreditapass/`; diff was exactly 4 lines in 1 file |
| 3 | Display name reads AccreditaPass | **PASS** | `<title>AccreditaPass</title>`; `"app.name": "AccreditaPass"` live in `en`, `es`, `pt` |
| 4 | Storage keys renamed in source and compiled output; guard intact; suite green | **PASS** | `accreditapass.theme`/`.locale` in `theme.ts`/`locale.ts` and in the committed `theme.js`/`locale.js`; live shipped JS confirms both; guard strengthened and negative-tested — injecting a deliberate source/compiled drift fails the suite |
| 5 | Zero old-name occurrences outside `handoff/`, exceptions listed | **PASS** | 11 remain, all 11 the approved historical exceptions, enumerated below |
| 6 | 27 `handoff/` files byte-identical to `79e3220` | **PASS** | `git diff --name-only 79e3220 HEAD -- handoff/**` returned 0 files at PR A; this archive adds `handoff/v16/` and one appended index row (see Deviations) |
| 7 | 133 namespace identifiers unchanged; no `.cs`/`.csproj`/`.razor` but the two test files | **PASS (invariant) / PARTIAL (file list)** | 413 `FifaPressApp` occurrences across 133 files, unchanged. `src/backend/Program.cs` was also modified — mandated by task 3(a) — in product-name strings only, touching no namespace identifier |
| 8 | Disclaimer, contact, key names, World Cup matter byte-identical | **PASS (as amended)** | 10 of 10 byte-identical: EN and ES `:140`, `:141`, `:345`, `:346`, plus PT `:345` and `README.md:7`. PT `:140`/`:141`/`:346` changed only by the principal-approved pt-BR variant corrections |
| 9 | Build clean and suite green after every commit, verified individually | **PASS** | each of the five commits checked out and rebuilt from clean; 512 + 33 at each |
| 10 | Internal markdown links resolve, N of N | **PASS** | **358 of 358**, identical to the pre-run baseline |
| 11 | Two PRs on the exactly-named branches, both unmerged by the executor, jdsaire sole author, zero AI attribution | **PASS** | PR #13 opened unmerged and merged only by the principal; PR B opened unmerged. Author/committer `jdsaire` on all five commits; zero matches for AI or vendor names in any message, branch name, PR title or PR body |
| 12 | Repository rename happened at task 6, after PR A merged | **PASS** | `gh repo rename accreditapass` ran only after the principal confirmed the merge |
| 13 | `resolutions-v4.json` N4 updated; `precedent` unchanged | **PASS** | `resolution` now names AccreditaPass and `https://github.com/jdsaire/accreditapass/tree/main` (HTTP 200); `precedent` and `title` byte-identical; file still parses as valid JSON |
| 14 | `handoff/v16/` holds plan, report and README; index carries a v16 row | **PASS** | this folder |
| 15 | No delegated execution; no credential requested, printed or referenced | **PASS** | all work performed in one execution context; every GitHub operation via `gh` |

---

## The eleven preserved occurrences

Success criterion 5's exception class, approved by the principal before execution. All
eleven name the application by the name the **v7** rename produced, in prose that is a
statement about that rename rather than a live reference to the product:

- `ux-ui/00-initial-evaluation/README.md:3`, `accessibility-audit.md:3`,
  `heuristic-evaluation.md:3`, `usability-assessment.md:3`,
  `usability-test-protocol.md:3`, `findings-register.md:4` — six notes reading *"since
  renamed to the FIFA Press App / `src/FifaPressApp/` in v7"*. The `in v7` governs both
  halves of that sentence, so renaming the first half would assert a rename that did not
  happen.
- `ux-ui/README.md:8`, `01-design-research/README.md:41`, `02-ideation/README.md:30`,
  `03-ui-prototyping/README.md:47` — four references to *"before the FIFA Press App
  reframing"*, naming a specific past event.
- `ux-ui/03-ui-prototyping/07_BUILD-BRIEF.md:39` — the citation *"Brand string `EventEase`
  → `FIFA Press App` (line 3)"*. The literal no longer exists in `NavMenu.razor` at all,
  which now reads `@L[Locale, "app.name"]`, so changing `app.name` moves the navbar brand
  automatically. An accurate citation outranks a consistent name.

These files' own stated doctrine is *"preserved as historical record"* — the doctrine
`handoff/v1` already established for the EventEase-to-FifaPressApp rename, preserved rather
than revised.

Not an occurrence, and left alone: `ux-ui/05-iteration/00_RECONCILIATION.md:153`, *"the top
bar's FIFA equivalent of `CartSummary`"* — a bare `FIFA`, not one of the four patterns.

---

## Findings

**The governing amendment under-counts the work, in both directions.** A17 step 3 states
*"the 29 files referencing the old name across `*.md`, `*.html`, `*.json`, `*.yml`."*
Measured: **40 live files**. The four extensions A17 names miss `.ts`, `.js` and `.cs`
**entirely** — which is exactly where the storage keys and their guard tests live, eleven
files A17's own runbook would never have reached. A17's figure also appears to fold in
`handoff/` history, which this run excludes. A17 additionally orders the repository rename
as step 1; this prompt sequences it at task 6 after PR A merges, and the prompt is right —
renaming first would have taken the live site down for the length of review instead of for
the length of one deploy.

**The deploy prompt's own documentation group is mislabelled.** `verified_state` heads that
group *"DOCUMENTATION — 24 files"* but enumerates **26**. The 40-file total is unaffected
and correct; only the subtotal label is wrong.

**Occurrence counts in the prompt are line counts, not occurrence counts.** The true figure
is 56 pattern-matched occurrences plus one that no line-based grep can find (below), of which
11 are preserved — 46 changed.

**One occurrence is invisible to the prompt's own search.**
`tests/frontend/LocaleServiceTests.cs:134` wraps `"FIFA Press` / `App"` across two comment
lines. A repo-wide sweep found no other wrapped instance. It was renamed.

**The namespace exclusion is load-bearing, not cosmetic.** The live site publishes
`FifaPressApp.*.wasm` and `FifaPressApp.styles.css`. Renaming the namespace in this run would
have changed every published asset name at the same moment the base path changed, making any
failure far harder to attribute. It remains a separate deploy with its own build verification.

**The storage-key reset has a one-time cost, accepted.** Every existing visitor's saved theme
and language resets exactly once, because the key they were stored under no longer exists.
The guard that used to forbid this change now asserts the new key on **both** sides of the
source/compiled boundary; previously it read only the compiled file and could not have
detected a drift at all.

**No Node on the build host.** `npm ci` and `npm run build` were unavailable — `node` is
absent from PATH, nvm and Homebrew. Per the prompt's guardrail the committed JavaScript was
hand-edited to match the TypeScript exactly. No dependency was added, no bundler introduced,
and CI still does not need Node. The strengthened guard was negative-tested to prove it would
catch a hand-edit mistake.

**A pre-existing flaky test, not introduced by this run.**
`ThemeTriggerPlacementTests.ChoosingSystemClearsTheStoredChoiceRatherThanStoringAThirdValue`
intermittently fails on a cold build. It was reproduced on the untouched baseline `79e3220`,
which settles attribution. It is a bUnit timing race on an async click handler; it asserts
against a mocked module and never references a storage key name. Carried forward as an open
item.

**GitHub Pages required an explicit redeploy after the rename, and the CDN masked it.**
Immediately after `gh repo rename`, both the old and the new Pages URL returned HTTP 200 with
correct-looking content. That was a stale CDN copy of the pre-rename deploy
(`x-cache: HIT`, `cache-control: max-age=600`). Once the TTL expired the new URL returned
**404 Site not found** and the Pages API reported no active deployment. A `workflow_dispatch`
run of *Deploy to GitHub Pages* republished the site under the new path, after which every
check was re-run cache-busted. **Lesson for any future rename in this repository: a repo
rename invalidates the Pages deployment, and for roughly ten minutes the CDN will tell you
otherwise.** Verify with a cache-busting query string and confirm `x-cache: MISS`.

**The old Pages URL does not forward, as A17 warned.** After cache expiry,
`https://jdsaire.github.io/fifa-press-app/` returns **HTTP 404, "Site not found"**. The old
*repository* URL does redirect to `jdsaire/accreditapass`; the Pages URL does not. Any
published reference to the old live URL is now dead and must be updated wherever it appears.

---

## Authorised deviations

**1. A fifth commit the prompt did not schedule: the Portuguese harmonization.** The
principal directed a spelling correction in `pt.json`'s `landing.disclosureStrong`
(`portefólio` → `portfólio`), which trips the prompt's preserve-verbatim stop condition.
Investigation showed `pt.json` was **already internally inconsistent**: `"Minhas
solicitações"` is Brazilian and is pinned as an expectation by `LocaleServiceTests`, while
the rest of the dictionary was European. On that evidence the principal chose a full
harmonization to Brazilian Portuguese. 34 lines changed in one dedicated commit — 35 lexical
corrections (`registo`→`registro` ×18 being the app's core domain noun, plus `ecrã`→`tela`
with article gender agreement, `contacto`→`contato`, `utilizador`→`usuário`,
`equipa`→`equipe`, `secção`→`seção`, `registad*`→`registrad*`, `intermédias`→`intermediárias`,
`portefólio`→`portfólio`) and nine grammar corrections (a stray `tu`-form, the EP progressive
`está a solicitar`, the mesoclisis `preencher-se-á`, two enclitic constructions, and two
clitic placements). Deliberately **not** changed, because they are valid Brazilian
Portuguese and altering them would be style rewriting rather than variant correction:
`aplicação`, `candidatura`, `sessão`, `guardado`, and impersonal `escreve-se`. Key names were
untouched, so the three dictionaries still define exactly the same keys. Three
preserve-verbatim strings changed as a result — `:140` `portefólio`→`portfólio`, `:141`
`registos`→`registros`, `:346` `registada`→`registrada` — with meaning, structure and every
`FIFA` reference intact. English and Spanish remained byte-identical throughout.

**2. Tests moved with their subjects.** The prompt grouped all test updates into task 3(c),
but `LocaleServiceTests` asserts the display name and `InteropTests` asserts the storage
keys, so that grouping would have left commits 2 and 3 red until 3(c) landed — violating the
hard rule that the suite passes after *every* commit. Each test was updated in the commit
that changed what it asserts. Task 3(c) was absorbed, not skipped. Approved before execution.

**3. `handoff/README.md` receives an appended row.** It is one of the frozen 27 files *and*
task 9 requires a v16 index row. Resolved as: 26 of 27 byte-identical; `handoff/README.md`
changes only by the appended v16 row, its pre-existing old-name occurrence untouched.
Approved before execution.

**4. `src/backend/Program.cs` was modified.** Success criterion 7 permits only the two test
files among `.cs` files, but task 3(a) explicitly mandates the product-name occurrences in
`Program.cs`. The invariant criterion 7 exists to protect — that no namespace identifier
changes — holds at 413 of 413. `Program.cs` changed in three product-name strings and
comments only.

**5. `N4.title` left unchanged.** Task 7 scopes the edit to `resolution` and freezes
`precedent`; it does not mention `title`, which still reads *"FIFA App Scope & Placement"*.
Read as scope rather than oversight, and confirmed with the principal.

---

## Decisions resolved autonomously

**Layer classification of every ambiguous occurrence.** Occurrences that could not be
confidently classified were listed as questions in the plan and left unedited rather than
resolved by editing, per the hard rule. All eleven were then resolved by the principal in a
single decision (above). Occurrences classified as Layer 1 without escalation, because they
name the product rather than a past event: `backend/06_REPO-MAP.md:12`,
`docs/project-plan.md:20`, `ux-ui/05-iteration/README.md:4`,
`docs/Original-Build-Flowchart.md:1` and `:125` (both unpinned, and `:125` explicitly points
the reader at *"its current framing"*), `03_UI-DECISIONS.md:128`/`:130` and `11_I18N.md:141`
(live specifications of the current product-name treatment, whose stated policy — English,
always, unchanged across EN/ES/PT — remains true of AccreditaPass), plus the repository
slugs and URLs in `docs/grading-criteria.md:9`, `04-evaluation/00_SCOPE.md:31` and
`05-iteration/00_RECONCILIATION.md:9`, and the GitHub ZIP folder name
`fifa-press-app-main` → `accreditapass-main` in `docs/setup-guide.md`.

**The guard test was strengthened rather than merely retargeted.** The hard rule requires
`TheThemeStorageKeyIsUnchangedByTheConversion` to keep asserting that source and compiled
output agree on one key. It never actually did — it read only `Compiled("theme.js")`. It is
now `TheThemeStorageKeyMatchesBetweenSourceAndCompiledOutput` and asserts the key in both
`Source("theme.ts")` and `Compiled("theme.js")`, which matters more than usual in a run where
the JavaScript was hand-edited. Verified by injecting a deliberate drift and observing the
suite fail.

**A git identity was set repo-locally.** No `user.name` or `user.email` was configured
globally or locally on the clone. Rather than guess, the identity was taken from the only one
in the repository's history — `Juan Diego S. <88201583+jdsaire@users.noreply.github.com>` —
and set with `git config` scoped to this repository alone.

**A file-corruption mistake was caught and corrected before it left the machine.**
`src/frontend/wwwroot/index.html` is the only CRLF file in the repository. The first edit pass
normalised it to LF, rewriting all 35 lines instead of one. The commit was amended to restore
CRLF with only the `<title>` changed, and the remaining 39 in-scope files were swept for CRLF
(none). Recorded because the diff, not the test suite, is what caught it.

---

## Verification commands

```bash
# live site, cache-busted (a repo rename leaves the CDN lying for ~10 minutes)
curl -sI "https://jdsaire.github.io/accreditapass/?cb=$(date +%s)" | grep -iE 'HTTP/|x-cache|age'

# every boot-manifest asset resolves
curl -s https://jdsaire.github.io/accreditapass/_framework/dotnet.*.js \
  | grep -oE '"[A-Za-z0-9._-]+\.(wasm|dat)"' | tr -d '"' | sort -u

# the three name layers
git grep -I -o -E 'fifa-press-app|FIFApp|fifapp|FIFA Press App' -- . ':(exclude)handoff/**' | wc -l   # 11
git grep -I -o 'FifaPressApp' | wc -l                                                                 # 413
git diff --name-only 79e3220 HEAD -- 'handoff/**'                                                     # empty at PR A

# preserve-verbatim
for f in en es; do for l in 140 141 345 346; do
  diff <(git show 79e3220:src/frontend/wwwroot/i18n/$f.json | sed -n "${l}p") \
       <(sed -n "${l}p" src/frontend/wwwroot/i18n/$f.json); done; done
```

---

## Open items carried forward

- **The flaky theme test.** `ThemeTriggerPlacementTests.ChoosingSystemClearsTheStoredChoiceRatherThanStoringAThirdValue`
  fails intermittently on cold builds, on `79e3220` as well as here. A bUnit timing race
  worth fixing in a run that owns the test suite.
- **The dead live URL is published elsewhere.** `https://jdsaire.github.io/fifa-press-app/`
  now 404s and does not forward. A17 step 6 requires updating any CV, LinkedIn or portfolio
  reference already published; that is outside this repository and outside this run.
- **`N4.title`** still reads *"FIFA App Scope & Placement"* in the closure folder, by decision.
- **The namespace rename**, if ever wanted, remains a separate deploy with its own build
  verification. Nothing in this run assumes it will happen.
- **Every visitor's saved theme and language has reset once.** Expected, one-time, and not
  recoverable — the old keys are not read before being abandoned.
