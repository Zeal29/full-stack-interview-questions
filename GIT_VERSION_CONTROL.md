# Git & Version Control Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) · Intermediate (Q21–Q40) · Advanced (Q41–Q50)

---

## Basic (Q1–Q20)

### Q1: What is Git?

**Answer:** Git is a distributed version control system that tracks changes in source code, allowing multiple developers to collaborate on a project. Every developer has a full copy of the repository history locally.

**💡 Tip:** Think **G**it = **G**uardian of code history. Distributed means every clone is a full backup.

---

### Q2: What is the difference between Git and GitHub?

**Answer:** Git is the version control tool (runs locally). GitHub is a cloud-based platform that hosts Git repositories and adds collaboration features like pull requests, issues, and CI/CD.

**💡 Tip:** Git = the engine. GitHub = the garage where you park and collaborate.

---

### Q3: What are the three main areas of a Git project?

**Answer:**
1. **Working Directory** — where you edit files
2. **Staging Area (Index)** — where you mark changed files to be committed
3. **Repository (.git)** — where Git permanently stores snapshots

**💡 Tip:** Think of it as: **Edit → Stage → Save** (Working → Staging → Repo).

---

### Q4: What does `git init` do?

**Answer:** It initializes a new Git repository in the current directory, creating a hidden `.git` folder that stores all version control metadata.

**💡 Tip:** `init` = **in**itialize. It births a repo — only needed once per project.

---

### Q5: What is the difference between `git add` and `git commit`?

**Answer:** `git add` stages changes (moves them to the staging area). `git commit` permanently saves the staged changes to the repository with a message.

**💡 Tip:** `add` = put in shopping cart. `commit` = checkout and pay.

---

### Q6: What does `git status` show?

**Answer:** It displays the current state of the working directory and staging area — which files are modified, staged, untracked, or unmerged.

**💡 Tip:** `status` is your **dashboard**. Run it often to know where you stand.

---

### Q7: What is a branch in Git?

**Answer:** A branch is a lightweight, movable pointer to a commit. It lets you diverge from the main line of development and work on features independently without affecting other branches.

**💡 Tip:** Branch = parallel universe. Each branch is its own timeline that can merge back.

---

### Q8: How do you create and switch to a new branch?

**Answer:** `git checkout -b <branch-name>` creates a new branch and switches to it in one command. Alternatively, `git branch <name>` creates it, then `git switch <name>` switches.

**💡 Tip:** `-b` = **b**irth + branch. One flag does both: create + switch.

---

### Q9: What is `git merge`?

**Answer:** It integrates changes from one branch into another. Git performs a three-way merge using the two branch tips and their common ancestor.

**💡 Tip:** Merge = combining two timelines back into one. Fast-forward if no divergence; three-way merge if they diverged.

---

### Q10: What is `git clone`?

**Answer:** It creates a local copy of a remote repository, including all history, branches, and files. The remote is automatically set as `origin`.

**💡 Tip:** `clone` = downloading the entire project universe, not just the latest snapshot.

---

### Q11: What is `git pull`?

**Answer:** It fetches changes from the remote repository and merges them into the current branch. It's essentially `git fetch` + `git merge` in one step.

**💡 Tip:** `pull` = **p**ull down updates + merge. Use `fetch` first if you want to inspect before merging.

---

### Q12: What is `git push`?

**Answer:** It uploads local branch commits to the remote repository, updating the remote branch to match your local one.

**💡 Tip:** `push` = sending your work **up** to the cloud. `pull` = bringing work **down**.

---

### Q13: What is `git fetch`?

**Answer:** It downloads objects and refs from a remote repository but does **not** merge them. It updates your remote-tracking branches without modifying your working directory.

**💡 Tip:** `fetch` = look before you leap. Downloads changes but doesn't touch your code until you merge.

---

### Q14: What is a commit hash?

**Answer:** A 40-character SHA-1 checksum that uniquely identifies each commit. It's generated from the commit's content, author, timestamp, and parent commit.

**💡 Tip:** SHA-1 hash = digital fingerprint. Even a one-character change produces a completely different hash.

---

### Q15: What does `git log` do?

**Answer:** It shows the commit history of the current branch, displaying commit hashes, authors, dates, and messages in reverse chronological order.

**💡 Tip:** `git log --oneline --graph --all` gives a beautiful visual tree of all branches.

---

### Q16: What is `git stash`?

**Answer:** It temporarily saves modified tracked files and reverts the working directory to match the HEAD commit. You can later reapply the stashed changes with `git stash pop`.

**💡 Tip:** Stash = a sticky note. "Hold my changes, I'll come back for them." Great for switching branches mid-work.

---

### Q17: What is a `.gitignore` file?

**Answer:** A configuration file that tells Git which files or directories to exclude from version control (e.g., `node_modules/`, `.env`, build outputs, OS files).

**💡 Tip:** `.gitignore` = the bouncer at the club door. It decides what gets in and what stays out.

---

### Q18: What is `git diff`?

**Answer:** It shows the differences between various states: working directory vs staging, staging vs last commit, or between any two commits/branches.

**💡 Tip:** `diff` = before vs after. `git diff` (unstaged), `git diff --staged` (staged vs committed).

---

### Q19: What is the HEAD in Git?

**Answer:** HEAD is a pointer to the current branch reference, which in turn points to the latest commit on that branch. It indicates where you are in the project history.

**💡 Tip:** HEAD = "You Are Here" marker on the map. `git checkout` moves HEAD.

---

### Q20: What is `git reset` and what are its modes?

**Answer:** `git reset` moves the HEAD pointer to a specified commit. Three modes:
- **--soft** — moves HEAD, keeps changes staged
- **--mixed** (default) — moves HEAD, unstages changes but keeps them in working directory
- **--hard** — moves HEAD, discards all changes

**💡 Tip:** Soft = gentle (keep staged). Mixed = middle (keep as edits). Hard = harsh (destroy everything).

---

## Intermediate (Q21–Q40)

### Q21: What is a merge conflict and how do you resolve it?

**Answer:** A merge conflict occurs when two branches modify the same line(s) in the same file and Git can't automatically reconcile them. You must manually edit the file to choose which changes to keep, then `git add` and `git commit`.

**💡 Tip:** Conflicts are marked with `<<<<<<<`, `=======`, `>>>>>>>`. Pick a side (or combine), remove markers, stage, and commit.

---

### Q22: What is the difference between `git rebase` and `git merge`?

**Answer:** `git merge` creates a new merge commit preserving both histories. `git rebase` replays your commits on top of another branch, creating a linear history without merge commits.

**💡 Tip:** Merge = preserves truth (actual history). Rebase = rewrites history to look clean. **Never rebase shared branches.**

---

### Q23: What is `git rebase -i` (interactive rebase)?

**Answer:** It lets you modify commits in a branch interactively: reorder, edit, squash (combine), drop, or reword commit messages. Opens an editor with a list of commits to modify.

**💡 Tip:** Interactive rebase = time machine with a control panel. `pick`, `squash`, `reword`, `drop` are your commands.

---

### Q24: What is the golden rule of rebasing?

**Answer:** **Never rebase commits that have been pushed and shared with others.** Rebase rewrites history, which causes divergent histories and conflicts for collaborators.

**💡 Tip:** Rebase your own local branches. Merge shared/public branches. Period.

---

### Q25: What is `git cherry-pick`?

**Answer:** It applies changes from a specific commit onto the current branch, without merging the entire branch. Useful for selectively bringing bug fixes or features across branches.

**💡 Tip:** Cherry-pick = picking one cherry (commit) from another tree (branch) and placing it on yours.

---

### Q26: What is the difference between `git reset` and `git revert`?

**Answer:** `git reset` moves HEAD backward, rewriting history (dangerous on shared branches). `git revert` creates a new commit that undoes a previous commit's changes, preserving history (safe for shared branches).

**💡 Tip:** Reset = erase from history. Revert = add an "undo" commit. Use revert on public branches, reset on local.

---

### Q27: What are remote-tracking branches?

**Answer:** References to the state of branches on remote repositories. They're named `origin/main`, `origin/feature`, etc. They're updated only when you `fetch` or `pull` — they don't move with local operations.

**💡 Tip:** Remote-tracking branches = read-only mirrors of what's on the server. You don't move them directly — `fetch` updates them.

---

### Q28: What is `git remote` and what does `origin` mean?

**Answer:** `git remote` manages connections to remote repositories. `origin` is the default name given to the remote repository you cloned from. You can have multiple remotes (e.g., `origin`, `upstream`).

**💡 Tip:** `origin` = where you cloned from. `upstream` = the original project (common in fork-based workflows).

---

### Q29: What is a pull request (PR)?

**Answer:** A pull request is a platform feature (GitHub, GitLab, Bitbucket) that proposes merging a branch's changes into another branch. It enables code review, discussion, and CI checks before merging.

**💡 Tip:** PR = "Please Review my code." It's not a Git feature — it's a collaboration platform feature.

---

### Q30: What is the Gitflow branching model?

**Answer:** A branching strategy with:
- **main** — production-ready code
- **develop** — integration branch for features
- **feature/*** — individual features branched from develop
- **release/*** — preparation for release
- **hotfix/*** — urgent fixes branched from main

**💡 Tip:** Gitflow = structured. main → production, develop → next release, feature/* → work in progress.

---

### Q31: What is trunk-based development?

**Answer:** A branching strategy where all developers commit to a single branch (main/trunk) frequently. Short-lived feature branches may be used but are merged quickly (within a day). Emphasizes continuous integration.

**💡 Tip:** Trunk-based = everyone on the main highway, no long detours. Pairs well with CI/CD and feature flags.

---

### Q32: What are Git hooks?

**Answer:** Scripts that Git executes automatically at specific points in the workflow: pre-commit, post-commit, pre-push, etc. Used for enforcing code quality, running tests, or formatting code.

**💡 Tip:** Hooks = automated gatekeepers. `pre-commit` runs before a commit is created; `pre-push` runs before pushing.

---

### Q33: What is `git bisect` and how does it work?

**Answer:** It performs a binary search through commit history to find which commit introduced a bug. You mark commits as "good" or "bad," and Git narrows down the culprit commit.

**💡 Tip:** Bisect = binary search for bugs. `git bisect start` → `git bisect bad` (current) → `git bisect good <commit>` → Git checks out the midpoint.

---

### Q34: What is `git blame`?

**Answer:** It shows the last modification (author, commit, timestamp) for each line in a file. Useful for understanding who changed what and when.

**💡 Tip:** `blame` = who to blame for this line? Also called `annotate`. Don't actually blame people — use it for context.

---

### Q35: What is the staging area and why is it useful?

**Answer:** The staging area (index) is an intermediate zone between the working directory and repository. It lets you compose commits carefully — staging only the changes you want, leaving others out.

**💡 Tip:** Staging = packing your suitcase. You choose exactly what goes in each commit, rather than dumping everything.

---

### Q36: What does `git reflog` do?

**Answer:** It records almost every movement of HEAD — commits, checkouts, resets, merges. It's a safety net that lets you recover lost commits even after a hard reset or rebase.

**💡 Tip:** Reflog = Git's diary. Even if you "lose" commits, reflog remembers where HEAD was. `git reflog` → `git reset --hard <hash>` = recovery.

---

### Q37: What is `git stash` and when should you use it?

**Answer:** `git stash` saves your uncommitted changes (both staged and unstaged) and reverts the working directory to HEAD. Use it when you need to switch branches but aren't ready to commit.

**Answer (continued):** Restore with `git stash pop` (apply and delete) or `git stash apply` (apply and keep).

**💡 Tip:** Stash = "hold my coffee." Temporarily shelve work, do something else, then come back.

---

### Q38: What are Git tags?

**Answer:** Tags are immutable references that point to specific commits, typically used to mark release versions (e.g., `v1.0.0`). Lightweight tags are just pointers; annotated tags store metadata (author, date, message).

**💡 Tip:** Tags = bookmarks for releases. Annotated tags (`git tag -a`) are signed and stored as full objects — prefer them for releases.

---

### Q39: How do you undo the last commit without losing changes?

**Answer:** `git reset --soft HEAD~1` undoes the last commit but keeps the changes staged. `git reset --mixed HEAD~1` undoes it and unstages changes but keeps them in the working directory.

**💡 Tip:** `HEAD~1` = one commit back. `--soft` keeps staged. `--mixed` (default) unstages but preserves edits.

---

### Q40: What is `git clean`?

**Answer:** It removes untracked files from the working directory. `git clean -f` forces deletion of untracked files; `git clean -fd` also removes untracked directories; `git clean -n` does a dry run.

**💡 Tip:** `clean` = the vacuum cleaner. `-n` (dry run) first to see what would be deleted. `-f` to actually delete. **Cannot be undone.**

---

## Advanced (Q41–Q50)

### Q41: How does Git store data internally?

**Answer:** Git stores data as **objects** in the `.git/objects` directory:
- **Blob objects** — file content (no filename)
- **Tree objects** — directory listings (mapping names to blobs/trees)
- **Commit objects** — point to a tree + parent commit(s) + metadata
- **Tag objects** — annotated tag info pointing to a commit

**💡 Tip:** Git = content-addressed filesystem. SHA-1 hash of content = its address. Same content = same hash = stored once (deduplication).

---

### Q42: What is the Git object graph?

**Answer:** Git's internal data structure where commits form a directed acyclic graph (DAG). Each commit points to one or more parent commits. Branches and tags are just pointers (refs) to commits in this graph.

**💡 Tip:** Think of Git as a linked list that branches and merges — a DAG where every commit knows its parent(s).

---

### Q43: What is a detached HEAD state?

**Answer:** When HEAD points directly to a commit hash instead of a branch reference. Happens when you checkout a commit, tag, or rebase. Commits made in detached HEAD are orphaned if you switch away without creating a branch.

**💡 Tip:** Detached HEAD = walking off the marked trail. Create a branch immediately (`git checkout -b <name>`) if you want to keep the work.

---

### Q44: How do you recover a dropped commit?

**Answer:** Use `git reflog` to find the commit hash of the dropped commit, then `git checkout <hash>` or `git reset --hard <hash>` to restore it. Reflog keeps entries for 90 days by default.

**💡 Tip:** Reflog is the safety net. Even after `reset --hard`, reflog remembers. Act fast — reflog entries expire.

---

### Q45: What is `git subtree` and how does it differ from `git submodule`?

**Answer:**
- **Submodule** — links to another repo as a separate project inside a directory. The parent repo stores a reference (commit hash), not the actual files.
- **Subtree** — merges another repo's code directly into the parent repo. The code becomes part of the parent's history.

**💡 Tip:** Submodule = symlink to another repo (lightweight, complex). Subtree = copy-paste the code in (heavier, simpler).

---

### Q46: What is the difference between `git merge --no-ff` and `git merge --ff-only`?

**Answer:**
- `--no-ff` — always creates a merge commit, even when a fast-forward is possible. Preserves branch history.
- `--ff-only` — only merges if a fast-forward is possible (no divergent history). Fails otherwise.

**💡 Tip:** `--no-ff` = "I want evidence this branch existed." `--ff-only` = "only merge if it's clean, otherwise stop."

---

### Q47: How do you rewrite the history of already-pushed commits safely?

**Answer:** You generally **don't** — it's dangerous. If you must: use `git rebase -i` to modify, then `git push --force-with-lease` (safer than `--force` — it checks that the remote hasn't been updated by someone else).

**💡 Tip:** `--force-with-lease` = "force push, but only if nobody else pushed since my last fetch." Always prefer this over `--force`.

---

### Q48: What are Git worktrees?

**Answer:** Worktrees let you have multiple working directories for the same repository, each on a different branch. You can work on two branches simultaneously without stashing or cloning.

**💡 Tip:** Worktree = cloning without cloning. `git worktree add ../hotfix hotfix-branch` creates a second folder on a different branch.

---

### Q49: What is a Git LFS (Large File Storage)?

**Answer:** Git LFS is an extension that replaces large files (binaries, videos, datasets) with lightweight pointer files in the repo, storing the actual file content on a remote server. This keeps the repository size manageable.

**💡 Tip:** LFS = "Git's attic." Large files go to external storage, only pointers stay in Git. Essential for projects with binaries >50MB.

---

### Q50: How would you implement a monorepo strategy with Git?

**Answer:** A monorepo stores multiple projects in a single repository. Key strategies:
- Use **sparse checkout** to let teams fetch only their directories
- Use **path-based CI triggers** to only run relevant pipelines
- Use **Git subtree** for shared libraries
- Enforce **code ownership** via `CODEOWNERS` files

**💡 Tip:** Monorepo = one repo to rule them all. Benefits: atomic cross-project changes, shared history. Challenge: repo size and CI complexity.
