# Bugs & Iterations — forging

Log every bug fix and iteration with date, problem, root cause, and fix.

---

## 2026-10-07: BUG-001 — ARCHIVED.md leaked a local home path into a PUBLIC repo

**Problem:** PR #1 (`fleet-p2`) added `ARCHIVED.md` containing the absolute path
`/Users/<owner>/meta_implementation.md`. This repo is public, so the file would have
published a machine-local path. The same paragraph also described the bundle as
"3-file" while listing four files and omitting the compiled `assets/` bundle and the
Pages deploy workflow.

**Root cause:** The fleet-audit agent copied the audit doc's absolute path verbatim
into prose and summarised the tree from memory instead of `git ls-tree`.

**Fix:** Reworded the reference to `meta_implementation.md` (home-relative, no absolute
path) and corrected the file inventory to match `git ls-tree -r --name-only main`
(7 tracked files, incl. `.github/workflows/deploy.yml`).

**Regression check (no test harness in this static repo):**
```
git grep -nE "/Users/" HEAD -- . ; test $? -eq 1 && echo "OK: no home paths"
```
