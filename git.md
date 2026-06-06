# Git Interview Question Bank

## Conceptual Questions (30)

### 1. Explain the difference between Git and SVN (Subversion)
**Expected Answer:**  
Git is distributed version control system where every developer has complete repository history locally. SVN is centralized with single point of failure. Git operations work offline, support cheap branching/merging, and have efficient storage.

**Follow-up Questions:**  
- What is DAG in Git?  
- How does Git store data differently from SVN?

### 2. What are the three states of a file in Git?
**Expected Answer:**  
Modified: File changed but not committed  
Staged: Changes added to staging area (git add)  
Committed: Data saved in local repository

**Follow-up Questions:**  
- How to unstage a file?  
- What is git reset vs git restore?

### 3. Explain the Git workflow from start to commit
**Expected Answer:**  
1. Modify files in working directory  
2. Stage changes: `git add <file>`  
3. Commit: `git commit -m "message"`  
4. Push to remote: `git push origin branch`

**Follow-up Questions:**  
- What happens during git commit?  
- How to amend last commit?

### 4. What is the difference between git fetch and git pull?
**Expected Answer:**  
git fetch: Downloads objects but doesn't merge  
git pull: fetch + merge automatically  
Fetch allows reviewing changes before merging.

**Follow-up Questions:**  
- When to use fetch?  
- What is pull --rebase?

### 5. Explain branching in Git
**Expected Answer:**  
Branching creates independent development paths.  
Commands:  
- git branch: List/create branches  
- git checkout -b: Create and switch  
- git merge: Merge branches  
- git branch -d: Delete branch

**Follow-up Questions:**  
- What is orphan branch?  
- How to rename a branch?

### 6. What is the difference between git merge and git rebase?
**Expected Answer:**  
merge: Creates new commit combining histories, preserves history  
rebase: Replays commits on top of another branch, cleaner history  
Use merge for shared branches, rebase for local feature branches.

**Follow-up Questions:**  
- When to avoid rebase?  
- What is interactive rebase?

### 7. How do you resolve merge conflicts in Git?
**Expected Answer:**  
1. Git marks conflict in files  
2. Manually edit conflicted sections  
3. Remove conflict markers  
4. git add resolved files  
5. git commit to complete merge

**Follow-up Questions:**  
- How to abort merge?  
- What are conflict resolution tools?

### 8. What is the staging area in Git?
**Expected Answer:**  
Intermediate area between working directory and repository.  
Stores snapshots of changes to be committed.  
Allows selective staging of changes within a file.

**Follow-up Questions:**  
- How to stage partial file changes?  
- What is git reset --staged?

### 9. Explain Git commit best practices
**Expected Answer:**  
- Atomic commits (one logical change)  
- Clear, descriptive messages  
- Reference issue numbers  
- Include "Signed-off-by" for DCO  
- Keep commits small and focused

**Follow-up Questions:**  
- What is conventional commits?  
- How to write good commit messages?

### 10. What are Git hooks?
**Expected Answer:**  
Scripts that run automatically on specific events:  
- pre-commit: Before commit  
- post-commit: After commit  
- pre-push: Before push  
- post-receive: After push to remote

**Follow-up Questions:**  
- How to share hooks with team?  
- What is client-side vs server-side hooks?

### 11. How do you revert changes in Git?
**Expected Answer:**  
- git revert: Creates new commit reversing changes  
- git reset: Moves branch pointer (history changes)  
- git checkout: Discard changes in working directory

**Follow-up Questions:**  
- What is soft/mixed/hard reset?  
- How to revert pushed commit?

### 12. What is Git stash?
**Expected Answer:**  
Temporarily saves changes without committing.  
`git stash`: Save changes  
`git stash pop`: Apply and remove stash  
`git stash list`: View stashes  
`git stash drop`: Remove stash

**Follow-up Questions:**  
- How to apply specific stash?  
- What is stash save message?

### 13. Explain Git refspec
**Expected Answer:**  
Defines how Git maps source to destination references.  
Format: `+refs/src:refs/dst`  
Used in fetch/push commands for custom ref mapping.

**Follow-up Questions:**  
- How to push specific branch?  
- What is refspec in .git/config?

### 14. What are Git tags?
**Expected Answer:**  
Markers for specific commits, typically versions.  
- Lightweight: Simple pointer  
- Annotated: Full object with metadata  
Commands: git tag, git tag -a, git push --tags

**Follow-up Questions:**  
- How to checkout tag?  
- What is semantic versioning with tags?

### 15. How do you handle large files in Git?
**Expected Answer:**  
- git-lfs: Large File Storage extension  
- .gitignore: Exclude large files  
- Shallow clones: --depth 1  
- Sparse checkout: Partial working directory

**Follow-up Questions:**  
- What is git-annex?  
- How to remove large file from history?

### 16. Explain Git cherry-pick
**Expected Answer:**  
Applies specific commit from one branch to another.  
`git cherry-pick <commit-hash>`  
Useful for selective integration of changes.

**Follow-up Questions:**  
- How to cherry-pick range of commits?  
- What are cherry-pick conflicts?

### 17. What is Git reflog?
**Expected Answer:**  
Records reference changes (moves, commits, merges).  
Used to recover lost commits.  
`git reflog`: View history  
`git reset --hard HEAD@{n}`: Restore state

**Follow-up Questions:**  
- How to find deleted branch?  
- What is git reflog expire?

### 18. How do you create a GitHub Pull Request?
**Expected Answer:**  
1. Push feature branch to remote  
2. Go to GitHub repository  
3. Click "Compare & pull request"  
4. Fill title/description  
5. Assign reviewers  
6. Merge after approval

**Follow-up Questions:**  
- What is squash merge?  
- How to sync fork with upstream?

### 19. What is Git submodule?
**Expected Answer:**  
Includes another repository as subdirectory.  
`git submodule add <url>`: Add submodule  
Tracks specific commit of sub-repository.

**Follow-up Questions:**  
- How to update submodule?  
- What is .gitmodules file?

### 20. Explain Git merge strategies
**Expected Answer:**  
- Fast-forward: Linear history when no divergence  
- Recursive: Default for 2 branches  
- Octopus: Multiple branch merge  
- Ours/Theirs: Custom strategies

**Follow-up Questions:**  
- How to force fast-forward?  
- What is merge driver?

### 21. What is Git blame?
**Expected Answer:**  
Shows last commit that modified each line.  
`git blame <file>`: View line-by-line history  
Useful for understanding code changes.

**Follow-up Questions:**  
- How to ignore whitespace in blame?  
- What is git annotate?

### 22. How do you manage Git configuration?
**Expected Answer:**  
- System level: /etc/gitconfig  
- Global level: ~/.gitconfig  
- Local level: .git/config  
`git config --global user.name "Name"`

**Follow-up Questions:**  
- How to unset config?  
- What is includes in git config?

### 23. What is Git shallow clone?
**Expected Answer:**  
Clone with limited history depth.  
`git clone --depth 1 <url>`: Single commit  
Faster, smaller repository.

**Follow-up Questions:**  
- How to deepen shallow clone?  
- What is fetch depth?

### 24. Explain Git worktree
**Expected Answer:**  
Multiple working directories from single repository.  
`git worktree add <path> <branch>`: Create linked working tree  
Allows checking out different branches simultaneously.

**Follow-up Questions:**  
- How to remove worktree?  
- What is separate git dir?

### 25. What are Git remotes?
**Expected Answer:**  
Named references to other repositories.  
Default: origin  
`git remote -v`: List remotes  
`git remote add <name> <url>`: Add remote

**Follow-up Questions:**  
- How to rename remote?  
- What is pushurl?

### 26. How do you handle Git history rewriting?
**Expected Answer:**  
- Interactive rebase: `git rebase -i HEAD~n`  
- Filter-branch: Rewrite history extensively  
- BFG Repo-Cleaner: Faster alternative to filter-branch

**Follow-up Questions:**  
- What are dangers of history rewrite?  
- How to recover from bad rebase?

### 27. What is Git bisect?
**Expected Answer:**  
Binary search to find commit introducing bug.  
`git bisect start`: Begin  
`git bisect bad/good`: Mark commits  
`git bisect reset`: End

**Follow-up Questions:**  
- How to automate bisect?  
- What is git bisect run?

### 28. Explain Git index
**Expected Answer:**  
Staging area between working directory and repository.  
Also called "cache" or "staging area".  
Contains proposed next commit changes.

**Follow-up Questions:**  
- How to view index?  
- What is git diff --cached?

### 29. What is Git detatched HEAD?
**Expected Answer:**  
HEAD points directly to commit, not branch.  
Occurs when checking out specific commit.  
Changes here aren't referenced by any branch.

**Follow-up Questions:**  
- How to recover from detached HEAD?  
- What is HEAD reference?

### 30. How do you collaborate with Git?
**Expected Answer:**  
- Fork + Pull Model: Fork repo, push, PR  
- Shared Repository Model: Push to shared branch  
- Patch Series: email patches  
- GitHub Flow: Feature branch + PR

**Follow-up Questions:**  
- What is CODEOWNERS file?  
- How to handle merge conflicts in team?

---

## Scenario-Based Questions (10)

### Scenario 1
**Situation:** You committed sensitive data (password) to a public GitHub repository. The commit has been pushed and merged into main.

**What Interviewer Tests:** Security awareness and remediation approach for exposed credentials.

**Expected Approach:**  
1. Rotate the exposed password immediately  
2. Use git filter-branch or BFG to remove from history  
3. Force push to update history  
4. Notify security team  
5. Check if any malicious access occurred

**Ideal Answer:** "First, I'd immediately rotate the password since it's already exposed. Then I'd use `git filter-branch` or BFG Repo-Cleaner to remove the file from entire Git history. After removing, force push with `git push --force-with-lease`. I'd also check GitHub's security alert and scan for any forks that might have the data."

**Follow-up Questions:**  
- How to prevent this in future?  
- What is git-secrets?

### Scenario 2
**Situation:** Your team is working on a feature branch, but main has moved forward significantly. You need to incorporate latest changes without creating messy merge commits.

**What Interviewer Tests:** Understanding of rebasing vs merging and conflict resolution strategy.

**Expected Approach:**  
1. Fetch latest main: `git fetch origin`  
2. Rebase feature branch onto main: `git rebase origin/main`  
3. Resolve conflicts if any  
4. Continue rebase: `git rebase --continue`  
5. Force push updated branch: `git push --force-with-lease`

**Ideal Answer:** "I'd use `git rebase` to replay my feature commits on top of the latest main. This keeps history linear and cleaner than merge commits. Command sequence: `git fetch origin`, `git rebase origin/main`, resolve conflicts, then `git push --force-with-lease` to update the remote branch."

**Follow-up Questions:**  
- When not to use rebase?  
- How to abort rebase?

### Scenario 3
**Situation:** You accidentally committed to main branch instead of feature branch, and pushed to remote.

**What Interviewer Tests:** Recovery from wrong branch commits and understanding of force push risks.

**Expected Approach:**  
1. Create backup: `git branch backup-branch`  
2. Reset main: `git reset --hard HEAD~1`  
3. Push force: `git push --force-with-lease`  
4. Checkout feature branch and cherry-pick: `git checkout feature && git cherry-pick <commit>`

**Ideal Answer:** "I'd first create a backup branch pointing to the commit. Then reset main to previous commit: `git reset --hard HEAD~1`. Force push with `git push --force-with-lease`. Finally, switch to feature branch and cherry-pick the commit with `git cherry-pick <commit-hash>`."

**Follow-up Questions:**  
- How to prevent this with branch protection?  
- What is pre-commit hook?

### Scenario 4
**Situation:** You need to contribute to an open-source project on GitHub but don't have write access.

**What Interviewer Tests:** Fork-based contribution workflow and PR process understanding.

**Expected Approach:**  
1. Fork the repository  
2. Clone your fork: `git clone <your-fork-url>`  
3. Add upstream: `git remote add upstream <original-url>`  
4. Create feature branch  
5. Make changes, commit, push  
6. Create Pull Request from your fork's branch

**Ideal Answer:** "I'd fork the repo first, then clone my fork. I'd add the original repo as upstream remote. After making changes in a feature branch, I'd push to my fork and create a Pull Request through GitHub's UI. I'd keep my fork synced with upstream using `git fetch upstream && git merge upstream/main`."

**Follow-up Questions:**  
- How to sync fork regularly?  
- What is upstream vs origin?

### Scenario 5
**Situation:** Need to compare your local changes with the changes introduced by a specific commit on main branch.

**What Interviewer Tests:** Understanding of diff operations and comparison techniques.

**Expected Approach:**  
Use `git diff <commit-hash>` to compare working directory with specific commit.  
Use `git diff <commit>^ <commit>` to see what changed in that commit.

**Ideal Answer:** "I'd use `git diff <commit-hash>` to compare my local changes against that specific commit. If I want to see exactly what changed in that commit, I'd use `git diff <commit>^ <commit>` which shows the diff of the commit itself."

**Follow-up Questions:**  
- How to compare two branches?  
- What is git diff --stat?

### Scenario 6
**Situation:** Team policy requires all PRs to be up-to-date with main before merging, but you have many local commits.

**What Interviewer Tests:** Understanding of interactive rebase and commit squashing.

**Expected Approach:**  
1. Fetch latest main: `git fetch origin`  
2. Interactive rebase: `git rebase -i origin/main`  
3. Mark commits to squash (fixup/s)  
4. Resolve conflicts if needed  
5. Force push updated branch

**Ideal Answer:** "I'd use interactive rebase: `git rebase -i origin/main`. This allows me to squash multiple local commits into one or a few clean commits, reorder them, or edit commit messages. After cleaning up history, I'd force push with `git push --force-with-lease`."

**Follow-up Questions:**  
- How to autosquash?  
- What is fixup vs squash?

### Scenario 7
**Situation:** You need to remove a large binary file from entire Git history without losing other changes.

**What Interviewer Tests:** History rewriting and large file management.

**Expected Approach:**  
Option 1: git filter-branch  
Option 2: BFG Repo-Cleaner (faster)  
Option 3: git-lfs migration  
Then force push after backing up.

**Ideal Answer:** "I'd use BFG Repo-Cleaner as it's faster than git filter-branch. Commands: `java -jar bfg.jar --delete-files filename.ext` followed by `git reflog expire --expire=now` and `git gc --prune=now --aggressive`. Then force push with `git push --force-with-lease`."

**Follow-up Questions:**  
- How to migrate existing binaries to LFS?  
- What is git-lfs migrate?

### Scenario 8
**Situation:** You're collaborating on a feature with teammate, and both of you are making changes to the same file simultaneously.

**What Interviewer Tests:** Conflict prevention and collaborative workflow.

**Expected Approach:**  
1. Communicate with teammate  
2. Pull latest changes before starting: `git pull --rebase`  
3. Keep work granular and frequent pushes  
4. Use smaller, focused branches  
5. Review before merge

**Ideal Answer:** "I'd pull latest changes frequently with `git pull --rebase` to stay updated. Better to have smaller, frequent commits that are easier to merge than large, complex changes. Communicate with teammate about who's working on what section. Consider breaking work into smaller, independent branches."

**Follow-up Questions:**  
- How to minimize merge conflicts?  
- What is git conflict style?

### Scenario 9
**Situation:** You need to find when a specific line of code was last modified across all commits.

**What Interviewer Tests:** Git history exploration and debugging skills.

**Expected Approach:**  
Use `git blame` for line-by-line history.  
Use `git log -S "text"` to find commits with specific content.  
Use `git log -p --follow <file>` for file history.

**Ideal Answer:** "I'd use `git blame <file>` to see which commit last modified each line. For finding when specific text appeared, I'd use `git log -S 'specific text' --oneline`. If tracking a file's history through renames, `git log --follow <file>` shows complete history."

**Follow-up Questions:**  
- How to ignore whitespace in blame?  
- What is git annotate vs blame?

### Scenario 10
**Situation:** Your CI/CD pipeline is failing because your branch has outdated base branch references, causing dependency issues.

**What Interviewer Tests:** CI/CD integration understanding and troubleshooting.

**Expected Approach:**  
1. Update branch with latest base: `git fetch && git rebase origin/main`  
2. Resolve any new conflicts  
3. Run local tests  
4. Push updated branch  
5. Monitor CI pipeline

**Ideal Answer:** "I'd update my branch by rebasing onto the latest main: `git fetch` then `git rebase origin/main`. Any new conflicts would be resolved, then I'd run tests locally before pushing. This ensures my changes work with current dependencies. For CI, I'd check if there are pre-flight tests I can run locally first."

**Follow-up Questions:**  
- How to cache dependencies in CI?  
- What is git CI configuration file?

---

**Word Count**: ~4,800 words  
**Line Count**: ~470 lines (excluding this footer)