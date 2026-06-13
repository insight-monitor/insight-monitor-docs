---
type: truth
domain: git
priority: low
status: accepted
audience: developer
---
# Git Rules
1.  **Main is Sacred.** Only receives merges from `develop` via PR. Never commit directly. Broken links or `fixme` tags on `main` = Failed Build.
2.  **One Topic per Branch/PR.** Don't mix "Update Legal Policy" with "Fix typo in Runbook".
3.  **Write for the Diff.** Write atomic sentences/paragraphs. Huge paragraphs = unreadable diffs = missed errors in review.
4.  **No "WIP" Commits on Main.** Squash merge cleans this, but keep topic branches clean too.
5.  **Large Files = Git LFS.** Configure `git lfs track "assets/**/*.pdf" "assets/**/*.png"` *before* adding big files.
6.  **Archive, Don't Delete.** Move to `99_Archive/` with a `DEPRECATED.md` note explaining why. History is legal evidence.
7.  **Sync `.obsidian/plugins` via PR.** Adding a plugin? PR it. Removing? PR it. Team must agree on tooling.
8.  **Develop is the Hub.** All `topic/*` branches merge to `develop`. Periodically PR `develop` → `main` on demand for a release.
9.  **Hotfix.** Emergency fixes directly on `main` → immediately backport to `develop` via cherry-pick or PR.
