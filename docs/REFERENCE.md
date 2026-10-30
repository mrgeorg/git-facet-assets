# Git Facet – Reference

All features, settings and keys of Git Facet in detail. For an overview, see the [README](../README.md).

- **Graph:** Open the command palette and run **Git Facet: Open Graph**. The graph opens in an editor tab. It is
  also available as the **Git Facet** view in the panel (drag it to the side bar if you like); choose its repository
  and filter set in its title bar. The title bar shows **Edit Filter Set**, **Condense**, **Fetch** and **Go to HEAD**;
  filter sets, view toggles, resets and the rest are in its **…** menu; there, **First Parent Only** / **Show All
  Parents** and **Show Stashes** / **Hide Stashes** name what the entry does, as VS Code shows no check marks for extensions. Font sizes: settings `gitFacet.fontSize` and `gitFacet.viewFontSize`.
  **Open Graph for This File or Folder** in the explorer context menu opens a graph limited to that path, with the
  filters of the last active graph of the repository.
- **Condense:** Linear chains are condensed into one node each from the start; **Stop Condensing** / **Condense** in the title bar (Q) turns this off and on. Expand a chain from its context menu or by clicking its diamond, a few commits per step (`gitFacet.squashKeep`, default 3) or fully (Shift+click).
  Only the first parent of each merge is followed from the start; **Show All Parents** in the **…** menu shows the
  merged branches too, or use **Expand Merged Branch** on a single merge.
- **Revision graph:** **Revision Graph** / **Compact Graph** in the title bar, next to **Go to HEAD**, switches the
  layout of the filter set; `gitFacet.layout` sets the default. In the revision graph, commits
  are boxes with short ID, tags, date and message, and each branch has a column with its name on its newest box;
  branches that do not overlap in time share a column (`gitFacet.revisionShareColumns`). Arrows point to the newer
  commit, from the fork into the branch and dashed from a merged branch into its merge. Condensed commits become a
  diamond with their count, merge commits too (`gitFacet.revisionCondenseMerges`); click a diamond to expand a few commits,
  Shift+click to expand it fully. Box width in characters: `gitFacet.revisionColumnWidth` (default 20); long messages wrap to 3 lines or are cut (`gitFacet.revisionSubjectOverflow`).
  Stashes and the staged and unstaged changes take no place in the grid: stashes hang as small dashed circles with
  their number at the top right of their commit's box, staged and unstaged changes as small ovals at its top left.
- **Hide and prioritize:** Hide refs or commits and move a branch to the front from the context menu; hidden
  ranges are shown as dashed edges with the number of skipped commits. This changes the filter set of the graph,
  so the filter editor shows it and saving keeps it. **Show/Hide Branches and Tags…** (V, title bar) lists suggested
  patterns, local and remote branches and tags with a check mark for what is shown: uncheck to hide, check to show
  again (a branch under a hidden pattern becomes an exception), check a branch that is not loaded to add it. Type a
  pattern such as `release/*` to add it as an entry of its own.
- **Defaults:** `gitFacet.branchOrder` (base order of branch priority), `gitFacet.defaultMinLength` (shortest chain
  that is condensed), `gitFacet.defaultIncludeRemotes` (load remote branches) and `gitFacet.defaultDiffFiles` (with a
  path filter, all files or only the matching ones) apply wherever the filter set leaves the value open.
- **Filter from the context menu:** On a commit, the **Filter** submenu offers **Hide Author**, **Only This Author**, **Hide Similar Subjects…**
  (suggests a pattern such as `^chore:` from the subject), **Show from Here** / **Show up to Here** (its commit date)
  and, with a branch marked by Alt+M, **Range from Marked Branch to Here**; on a collapsed merge **Show Deleted
  Branch**; on a branch **Condense This Branch**; in **Graph: Changed Files** **Only Commits with This File**. **Change
  Filter…** (Shift+E) lists every field of the filter set with its value, changed fields first, and changes one at a
  time; the button next to a changed field resets it to the loaded filter set.
- **Row layout:** Choose which fields stand left of the graph and right of the node, in which order (short ID,
  branches, tags, notes, subject, author, date), and whether tags and branches get a line above the commit message,
  and a maximum name length left of the graph: longer names go into a line above and reach across the graph (setting
  `gitFacet.rowLayout`, per graph with **Choose Row Layout**, remembered per repository for the editor and the
  panel). **Choose Row Layout** opens the layout as JSON next to a demo graph that shows every kind of field; each
  valid change applies at once to the demo and to the graph, and saving (Ctrl+S) makes it the default for new graphs.
  The context menu of a field in the demo graph moves it to the other side, hides it, or puts tags and branches in
  the line above; it edits the JSON, so Undo works there.
  A remote branch at the same commit as its
  local branch is shown as a cloud next to it; a local branch with an upstream shows `↑n ↓m` behind its flag, the
  commits only it has and those only its upstream has (a 0 is left out).
- **Tooltips:** Each part of a row has its own tooltip: the node symbol shows the commit details, the short ID the
  full hash, author and date the author with e-mail and the committer, a branch its upstream and remote branches
  of the same name with commits ahead and behind, a dashed edge the hidden commits and why they are hidden; a
  collapsed merge names its merged branch.
- **Cherry-pick, revert and merge:** From the context menu of a commit, cherry-pick it onto HEAD or revert it; from
  a branch or tag, merge it into HEAD, with a merge commit (M) or only as a fast-forward (Shift+M). Each asks first. Conflicts are listed first among the unstaged changes and
  marked `n !` on their node; clicking one opens the merge editor (with `git.mergeEditor`) or the file with conflict
  markers. The conflict message offers **Open Conflicts** and **Abort**, which is also in the title bar of the file
  list while the merge, cherry-pick or revert is in progress. Meanwhile the lower of *Staged changes* / *Unstaged
  changes* is the provisional result, named e.g. "Staged changes (merge in progress)", with a dashed edge to the
  commit being merged, cherry-picked or reverted.
- **Notes:** Commits with git notes show a small note symbol per category (N for Notes, `refs/notes/commits`; R
  for `reviews`, C for `ci`); the tooltip shows the text. Add, edit or delete a note from the context menu, also
  on a note symbol: the note opens in an editor, and saving replaces it. Further categories with name and letter
  are set per repository in `gitFacet.noteCategories`; which categories are shown is part of the filter set.
  Git does not push and fetch notes by default: after adding a note, the graph offers to set this up for the
  remotes that lack it (once per session; **No** is remembered for the repository), and the command **Git Facet:
  Push and Fetch Notes (Set Up Git)** does it any time. It adds `refs/notes/*` to `remote.<name>.push` (with `HEAD`
  before it if there were no push refspecs, so `git push` still pushes the current branch) and
  `+refs/notes/*:refs/git-facet/remote-notes/<name>/*` to `remote.<name>.fetch`. After each fetch from the graph,
  the local notes are fast-forwarded to the fetched ones, or merged with them if both sides changed (a note changed
  on both sides keeps the lines of both). A plain `git fetch` only brings them under `refs/git-facet/remote-notes/`.
  **Push to Remote** on a branch pushes them along to a remote set up this way, and
  **Push Notes to Remote** in the **…** menu pushes them on their own; **Push Note to Remote** on a note symbol
  pushes its category (git pushes notes per category, not one by one). A note the remote has in the same version
  shows a cloud after its symbol, like a pushed tag.
- **Diff:** Select one commit, a condensed chain or two commits (Ctrl/Cmd+click) to compare them. Shift+click
  selects a range instead: the commits on the way from the newer to the older one are highlighted, and the diff
  includes the older one's changes; if the older one is not an ancestor, it stays a comparison. The changed files appear in
  **Graph: Changed Files** in the Source Control view. Buttons there and in the title bar of a diff opened from it
  open the file at that revision or in the working tree. **Select for Compare** and **Compare with Selected** in
  the context menu of a file take its version before or after the change and combine with the explorer's commands
  of the same name, e.g. the old `a.txt` of a commit against `b.txt` in the working tree. A file diff opens beside the graph in an editor tab and in
  the active editor group for the view in the panel (settings `gitFacet.diffColumn` and `gitFacet.viewDiffColumn`).
  The file diff opened last follows the selection in the graph and shows the same file for the newly selected
  commit, marked *unchanged* if the commit does not change it; turn this off with the button in its title bar or
  by pinning the tab.
- **Staged and unstaged changes:** Above HEAD, the node *Staged changes* compares the index with HEAD and
  *Unstaged changes* the working tree with the index, with untracked files (status U, `.gitignore` respected). They
  appear only when there are such changes, at the top without an edge when HEAD is not shown. Colored tags such as
  `3 U` `7 M` `4 A` count the files per status; the tooltip of the node lists the files with `+++---` like `git diff --stat`, the tooltip of a tag only those with its status. Stage, unstage or stash
  files in **Graph: Changed Files** (several at once with Ctrl/Shift+click; a file with both staged and unstaged
  changes is not stashed); stage or unstage all of them from the node's context menu or the list's title bar;
  discard unstaged changes per file or all at once, after a confirmation (untracked files are deleted); with two nodes selected, a commit is compared with the index or the
  working tree. Turn them off with `gitFacet.showUncommitted` or per filter set.
- **Filter sets:** Edit the filter set of the graph and save it in **Graph: Filter Sets** for yourself, privately
  for the repository (`.git/gitfacet/filtersets.json`) or shared with the team (`.gitfacet/filtersets.json`;
  a `.gitgraph/filtersets.json` from before the rename applies until you move it, which the graph offers). The
  title and the status bar show the loaded filter set, with ● when the graph differs from it (in the panel at the
  bottom only the status bar; click it to load another). Save writes back to the loaded filter
  set (asking first, except in the shared file, which is versioned); Save As picks a place and a name. Loading more
  commits is not saved. In
  the editor, enter branches and tags by name (or choose them from the repository, with suggested patterns such as
  `feature/*`, `v1.*` or `refs/remotes/origin/*`); full patterns such as `refs/remotes/origin/*` go under other refs.
  Which characters split names for the suggestions is set in `gitFacet.patternSeparators.branches` (default `/`) and
  `gitFacet.patternSeparators.tags` (default `.`), or per filter set.
- **Commit:** On *Staged changes* (on *Unstaged changes* while nothing is staged), add or edit the commit message (kept in `.git/gitfacet/COMMIT_MSG` and in the
  Source Control input box; the node shows its first line, the tooltip all of it, and checking out keeps
  it with the stash until you come back) and commit; without a message, the editor opens first and saving commits. While nothing is staged,
  Shift+right-click on *Unstaged changes* offers **Commit All Unstaged Changes** (stage everything, then commit). On HEAD,
  **Undo Last Commit** turns the last commit back into staged changes, with its message.
- **Tags:** **Create Tag Here…** (T) asks for a name and a message (empty: lightweight tag); the pencil at the top
  right opens a long message in an editor, with the name in the first line; saving creates the tag.
  **Create Tag from CHANGELOG Here…** fills that editor from the changelog in the commit (`gitFacet.changelogPath`,
  default `CHANGELOG.md`): the topmost section `## <version> …` not yet tagged, e.g. `v0.11.3` with the message
  `Version 0.11.3`, headings such as `### Added` as `Added:`; sections without a version such as "Unreleased" are skipped.
- **Move a branch:** Drag a local branch onto another commit, or mark it with **Mark Branch for Move** (Alt+M),
  select the target and use **Move Marked Branch Here** (Alt+M again; Esc cancels); **Set Branch to Upstream** moves it
  to its upstream. There are no reset modes: changed files of a checked-out branch go into a stash that stays at the
  old commit, and if the old commit would be on no branch any more, a branch `<branch>-moved` keeps it. Each move
  asks first and can be undone from the message afterwards, or later with **Undo Last Rewrite** as long as the branch
  is still at the target (removing `<branch>-moved` again).
- **Fetch and update:** **Fetch** in the title bar fetches all remotes, **Fetch This Remote** on a remote branch only
  its remote. **Update from Upstream (Fast-Forward)** on a local branch is `git pull --ff-only` for the checked-out
  branch and `git fetch <remote> <branch>:<branch>` for the others; it never merges or rebases and reports when the
  branch and its upstream have diverged. Both run through the built-in Git extension and use its sign-in.
- **Push:** From the context menu of a local branch, **Push to Remote** pushes it to the branch of the same name on a
  remote (setting it as upstream if there is none), or to a remote branch of your choice; each asks first and names
  the target. **Remote ▸ Set Remote Tracking Branch…** chooses or removes the upstream of a branch.
  Shift+right-click offers force pushing with `--force-with-lease`. **Push Tag to Remote** on a tag and **Push All
  Tags to Remote** in the title bar push tags the same way, without force: a tag of the same name on another commit
  in the remote is rejected; Shift+right-click on a tag offers **Force Push Tag to Remote**, which replaces it. A pushed tag
  shows a cloud like a branch; after each fetch from the graph and after pushing or deleting a tag, the graph also
  fetches the remote's tags into `refs/git-facet/remote-tags/<remote>/`, so a push made elsewhere shows after the
  next fetch. **Delete Tag on Remote …** on
  the cloud deletes the tag on the remote and keeps the local one; **Delete Branch on Remote …** on a remote branch
  deletes the branch there and keeps local branches. The push runs through the built-in Git extension,
  so it signs in the same way as Source Control (password or token prompt, GitHub account, SSH passphrase).
- **Rewrite history:** **Rewrite History** on a commit of the checked-out branch offers moving it up or down,
  squashing it into the previous commit (with or without its message), dropping it and changing its message. The
  steps only change a preview: the affected range and everything on top of it is framed in yellow, dropped commits
  are struck through and can be restored. A squash shows on the branch as a diamond "Squashed: …" with the new
  message (**Change Message…** there); its commits stay visible with their own changes one lane to the right in a
  yellow frame, labelled squash or fixup, the oldest connected to the diamond by a dashed line. **Move Up**/**Move
  Down** move a commit within the squash, **Remove from Squash** takes it out again. A commit can also be dragged onto
  another; a menu then offers **Move Before**, **Move After**, **Squash** and **Squash (Fixup)** (↑ ↓ and Enter work
  too). **Start History Rewrite** starts the mode without a step. On a selected range
  (Shift), moving, squashing and dropping act on all its commits: the range moves as a whole, and squashing joins it
  into the commit below; two commits selected with Ctrl/Cmd are squashed together into the one below the older, or
  dropped together. A bar above the graph counts
  the steps and offers **Undo Last Step** (Ctrl+Z), **Discard** and **Apply History Rewrite**, which asks once and
  then runs a single `git rebase -i`; a message afterwards offers **Undo**. Uncommitted changes are set aside and come
  back. Commits that are already pushed can only be rewritten on branches from the setting
  `gitFacet.rewritableBranches` (e.g. `wip/me/*`); merges in the range and other local branches with commits in it
  prevent the rewrite. On conflicts the rebase stops; the message offers **Continue** and **Abort**, the preview with
  its yellow frame stays, and the bar offers **Abort** and **Continue Rebase** until the rebase is done. Once the
  conflicts are resolved and staged, **Continue Rebase** there, on the node of the changes or in the title bar of the
  changed files continues it at any time.
  **Change Message…** on the last commit outside the rewrite mode changes its message right away (`git commit
  --amend --only`, staged changes stay staged); if the commit is already pushed, it asks first.
  **Add Staged Changes to Last Commit (Amend)** on the staged changes node runs `git commit --amend --no-edit`
  right away; the message afterwards offers **Undo**, which stages the changes again. On an older commit of the
  checked-out branch, **Squash Staged Changes into This Commit** puts the staged changes into that commit (fixup,
  its message stays), **… (Edit Message)…** also asks for a new message; both ask first, rewrite the commits above
  it, and **Undo** (also aborting a conflict) stages the changes again.
- **Transplant a branch:** Mark a local branch (**Mark Branch for Move**), then on the target choose **Transplant
  Marked Branch Here (from Fork Point)** to take only the commits no other branch, remote branch or tag has, or
  **(from Common Base)** to take everything since the common base with the target, like `git rebase <target>`. To
  transplant only the upper part of a branch, select a range (Shift) ending at the branch tip, choose **Mark Range
  for Transplant** (the marked range gets a dashed frame) and then **Transplant Marked Range Here**; the rest of the
  branch stays on a new branch `<branch>-part`, which is removed again if you abort. It asks first, uses the same checks as rewriting, stashes changed files and brings them back
  afterwards, and the message offers **Undo**. On conflicts the rebase stops with **Continue** and **Abort**.
- **Checkout:** Check out branches, tags and commits from the context menu (C, Shift+C, Alt+C); uncommitted
  changes are stashed first, and when you come back to a branch, the stash made when leaving it is popped, so you can
  switch back and forth. Shift+right-click offers checking out without stash. **Apply Stash** on a stash node applies just the part that node shows (its staged or its unstaged changes); Shift+right-click offers **Apply Whole Stash**, and **Pop Whole Stash** applies and drops all of it.
- **Worktrees:** A branch checked out in another worktree gets a small folder symbol, a detached worktree gets it at
  its commit; the tooltip shows the path, a worktree whose folder is gone is grey and offers **Prune Worktrees**.
  **Create Worktree Here…** on a branch checks it out in a new folder next to the repository, together with the stash
  made when leaving the branch (staged changes staged again), on a commit with a new
  branch or detached; afterwards it can open the worktree in a new window. On the symbol or the branch, **Open
  Worktree in New Window** and **Delete Worktree…**: the branch stays, changes are kept by default in a stash of the
  branch (with untracked files) that comes back when checking out the branch, or discarded. In a linked worktree the
  main worktree is shown the same way, but cannot be deleted; checking out a branch that is checked out in another
  worktree offers to open that worktree instead.

## Keyboard

Click into the graph, then select with the keyboard; commands act on the selected node. The keys apply while the
graph has the focus and not its find bar; change them under **Keyboard Shortcuts** (search for `gitFacet`).

| Key | Action |
| --- | --- |
| ↑ ↓, Page Up/Down, Home/End | Select a node; with Shift, select the range up to it |
| Ctrl/Cmd+↑ ↓ | Like Ctrl/Cmd+click: keep the first node, select or move the second for a diff |
| ↑ ↓ in the revision graph | Follow the arrows: to the child on the same branch / the first parent; after Alt+↑ ↓, back the way you came |
| Alt+↑ ↓ in the revision graph | The other children / parents in turn while Alt is held; releasing Alt starts again at the selected node |
| Enter / Esc | Focus the changed files / clear the selection |
| → / ← | Expand the next commits of a condensed chain / collapse it except its newest commits, then fully |
| Shift+→ / Shift+← | Expand a condensed chain fully / collapse it fully |
| → / ← on a merge | Show / hide the merged branch, like first parent for this merge |
| C / Shift+C / Alt+C | Check out branch / commit (detached) / remote branch, with stash and unstash |
| A / Alt+A / Shift+A | Apply this part of the stash / apply whole stash / pop whole stash |
| K / R / M | Cherry-pick / revert / merge into HEAD (merge commit) |
| Shift+M | Merge into HEAD (fast-forward only) |
| N / Shift+N / Alt+N | Add / edit / delete note |
| U on HEAD | Undo commit (keep changes staged) |
| I / Shift+I on a commit, while changes are staged | Squash the staged changes into it / with a new message; I on *Staged changes* amends the last commit |
| N / Ctrl+Enter / U on *Staged changes* | Add or edit commit message / commit / unstage all |
| N / Ctrl+Shift+Enter on *Unstaged changes* (nothing staged) | Add or edit commit message / stage and commit all |
| A / Del on *Unstaged changes* | Stage all / discard all |
| S on *Staged* or *Unstaged changes* | Stash all changes (staged, unstaged and untracked) |
| B / Shift+B / T | Create branch / create and check out / create tag |
| F2 / Shift+F2 | Rename branch / force rename |
| Del / Shift+Del | Delete branch, tag, stash or the remote branch on its remote / force delete branch |
| Alt+P / Shift+Alt+P | Push branch / force push with lease |
| Alt+U / Shift+F5 | Update branch from upstream (fast-forward) / fetch all remotes |
| Alt+M | Mark the branch for a move / move the marked branch here |
| Shift+Alt+M / Alt+K / Shift+Alt+K | Mark the selected range for a transplant / transplant the marked branch or range here (from fork point) / (from common base) |
| Shift+Alt+U / Alt+F5 | Set branch to upstream / fetch this remote |
| Shift+R / Ctrl+Shift+Z | Start rewriting history at the commit / undo the last rewrite |
| Alt+↑ / Alt+↓ while rewriting | Move the commit up / down |
| S / F / R / D / Shift+D / Shift+Alt+S while rewriting | Squash into previous / the same discarding the message / change message / drop / restore / remove from squash |
| Ctrl+Enter / Ctrl+Z while rewriting | Apply the rewrite / undo the last step |
| H / Shift+H / P / V | Hide commit / hide ref / prioritize branch / show or hide branches |
| Shift+Alt+H / Ctrl+Alt+H / Alt+Page Up / Alt+Page Down | Hide author / only this author / show from here / show up to here |
| Shift+Q | Condense this branch |
| Ctrl+C / Ctrl+Shift+C | Copy hash / short ID |
| Shift+Alt+C / Shift+Alt+R / Shift+Alt+A | Copy subject / ref name / all hashes of a chain |
| Q / F / S | Toggle condense / first parent / stashes |
| Ctrl+F / Alt+Home / F5 | Find / go to HEAD / refresh |
| Enter / Shift+Enter / Alt+Enter in the find bar | Next / previous match / select the match and close the find bar |
| E / Shift+E / L | Edit filter set / change one filter / load filter set |
| Ctrl+S / Ctrl+Shift+S | Save to the loaded filter set / save as a new filter set |
| X / Shift+X / Alt+X | Reset expand and collapse / revert to the loaded filter set / clear all filters |
| D / O | Choose row layout / choose repository of the panel view |

At a node with several matching refs, a quick pick asks which one.

In **Graph: Changed Files** of the staged or unstaged changes, A / U stage / unstage the selected files, Del discards
them, S stashes them; in any list, O / Shift+O open the file in the working tree / at the revision and Ctrl+Enter
opens all diffs. In **Graph: Filter Sets**, F2 / Del rename / delete the selected filter set.

Requires VS Code 1.101 or later and the built-in Git extension. The user interface is available in English and
German.
