---
type: truth
domain: git
priority: low
status: in-progress
audience: developer
---

# Git Rules
1.  **Main is Sacred.** Never commit to main, Broken links or `fixme` tags on `main` = Failed Build.
2.  **One Topic per Branch/PR.** Don't mix "Update Legal Policy" with "Fix typo in Runbook".
3.  **Write for the Diff.** Write atomic sentences/paragraphs. Huge paragraphs = unreadable diffs = missed errors in review.
4.  **No "WIP" Commits on Main.** Squash merge cleans this, but keep topic branches clean too.
5.  **Large Files = Git LFS.** Configure `git lfs track "assets/**/*.pdf" "assets/**/*.png"` *before* adding big files.
6.  **Archive, Don't Delete.** Move to `99_Archive/` with a `DEPRECATED.md` note explaining why. History is legal evidence.
7.  **Sync `.obsidian/plugins` via PR.** Adding a plugin? PR it. Removing? PR it. Team must agree on tooling.
