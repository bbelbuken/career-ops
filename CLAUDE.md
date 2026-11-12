@AGENTS.md

<!-- Add Claude Code-specific guidance here only when it has no AGENTS.md counterpart. -->

# Fork-local rules (Batu & Gökçe) — these OVERRIDE AGENTS.md where they conflict

This checkout is a private-use fork (`origin` = Batu's fork). Two candidates run it,
each on their own PC with their own user-layer files. Be token-efficient: the two
share one Claude account.

## 1. Language

- **Chat replies: always Turkish.** Short, table-formatted where it helps.
- **Files: English.** Code, configs, comments, mode files, commit messages.
- **Candidate-facing output follows the job posting's language.** Per job: if the
  JD is written in Turkish, write the report, tailored CV, cover letter, emails
  and form answers in Turkish; otherwise write them in English. This per-job
  choice overrides `language.output` in `config/profile.yml` (keep that at `en`).
  For Turkish-market postings also load the Turkish market vocabulary from
  `modes/tr/` on top of the English modes.

## 2. Updates — "sistemi güncelle"

- **Skip the AGENTS.md "Update Check" entirely.** Never run
  `node update-system.mjs check|apply|rollback` on this fork: `apply` installs
  upstream release files over the fork's own changes.
- When the user says **"sistemi güncelle"** (or "update", "güncelle"):
  1. `git fetch origin`
  2. `git status --porcelain --untracked-files=no` — if any *tracked* file is
     modified, stop and explain in Turkish (do not stash, reset or discard).
  3. `git pull --ff-only origin main` — if it is not a fast-forward, stop and
     tell the user to ask Batu; never merge, rebase or force anything.
  4. Summarize in Turkish: `git log --oneline ORIG_HEAD..HEAD` and
     `git diff --stat ORIG_HEAD..HEAD` (what changed, in plain words).
     "Already up to date" → say it is already current.
  Personal files are gitignored, so a pull never touches them.
- **Upstream merges (`career-ops-hq/career-ops`) happen only on Batu's PC, only
  when Batu asks:** `git fetch upstream && git merge upstream/main`, resolve
  conflicts keeping these fork-local rules, then push to `origin`.

## 3. Git safety

- **Never stage, commit or push personal files:** `cv*.md`, `config/profile.yml`,
  `config/*.json`, `portals.yml`, `modes/_profile.md`, `modes/_custom.md`,
  `modes/_brief.md`, `article-digest.md`, `voice-dna.md`, `data/`, `reports/`,
  `output/`, `interview-prep/`, `jds/`, `documents/`, `writing-samples/`, `.env`,
  any PDF/DOCX/photo. Before any commit, show `git status` and check this list.
- **Gökçe's PC is pull-only.** If `git remote get-url --push origin` prints
  `DISABLED`, this is Gökçe's PC: never commit, never push, never change remotes
  or credentials. Only "sistemi güncelle" touches git there.
- On Batu's PC, commit/push only when Batu explicitly asks.

## 4. Archetypes and portals (shared definitions)

- `templates/archetypes.yml` is the shared archetype library (HR/L&D and tech).
  During intake/onboarding, select the candidate's archetypes **by `id`** into
  `config/profile.yml` → `target_roles.archetypes` (with `fit`), and render the
  selected entries into the "Your Target Roles" table of `modes/_profile.md`
  (columns: name → Archetype, `axes` → Thematic axes, `buys` → What they buy),
  plus their `jd_signals` / `ats_keywords` for detection and CV keyword checks.
  Never add personal data to `templates/archetypes.yml`.
- `templates/portals.hr.yml` / `templates/portals.tech.yml` are the scanner
  starting points by archetype `family`. During intake, copy the matching one to
  `portals.yml` and narrow `location_filter` to the candidate's answers.
- Kariyer.net, LinkedIn and Indeed are not scannable: for Turkish jobs the main
  flow is "paste URL / JD → evaluate".

## 5. Help menu

- When the user types **"yardım"**, **"menü"**, **"help"**, **"ne yapabilirim"**
  (or runs `/yardim`), re-read `YARDIM.md` from disk (never from memory) and print it verbatim as the whole reply: no
  summary, no additions, no other commands.
- `YARDIM.md` is the one file written in Turkish (it is end-user text). When a
  mode is added or a site's support changes, update it in the same commit.

## 6. Calibration

- Keep the 4.0/5 apply threshold at first. Once the candidate has recorded
  outcomes (`outcome` mode) for **10 applications**, suggest running `calibrate`
  once, then again every ~10 further outcomes.
