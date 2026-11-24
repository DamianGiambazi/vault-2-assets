# Git and GitHub: the notary and the courier

Git records versions on your own computer. GitHub carries them to anywhere else. They are two different jobs, and Vault 2 uses both.

## 1. Who does what

| Tool | Job | In Vault 2 |
|---|---|---|
| **git** | the notary: records what you commit, and when, on this computer | seals the prediction before the run; `seal-check.ps1` proves the order |
| **GitHub** | the courier: holds a copy others can reach | carries the book, the rules and the receipts to a browser, a phone, a fresh clone |
| **Git Credential Manager** | signs git in to GitHub (part of Git for Windows) | opens a browser sign-in on the first push; Windows stores the result |

## 2. The commands, in the order Vault 2 used them

| Command | What it does |
|---|---|
| `git init` | makes the folder a repository: a hidden `.git` folder that will hold its history |
| `git add .` | stages every file in the folder for the next commit |
| `git status --short` | shows what is staged |
| `git commit -m "…"` | records a version, with a message and the time |
| `git log --format='%h  %cI  %s'` | lists the versions: short hash, time, message |
| `git remote add origin <url>` | links the local repository to one on GitHub |
| `git push -u origin main` | sends the commits to GitHub (`-u` remembers the link) |
| `git pull` | brings down changes made elsewhere (a browser, a phone) |
| `git clone <url> <folder>` | copies a whole repository, with its history, into a new folder |

Git records **what you commit, when you commit it**. It does not watch the folder in between.

## 3. The seal

The prediction is committed *before* the run. vector-snake records when the run started. `seal-check.ps1` compares the two:

```
prediction sealed : 2026-11-01 16:02:34Z
run started       : 2026-11-01 16:14:05Z
SEALED_BEFORE_RUN
```

`Z` means UTC. Once the repository is on GitHub, anyone can check the order.

## 4. Receipts in, vectors out

The repository holds the **source** (the chapters, the questions, the rules) and the **recipe** (vector-snake's receipts: the model, the budget and every cut's fingerprint). It does **not** hold the vectors. `.gitignore` excludes them because they are build output, which anyone with the recipe can rebuild. In software terms: source and lockfile in the repository, build output ignored.

The proof is the clone. A fresh copy rebuilt every passage, and `compare.sql` returned `CUTS_IDENTICAL 33`, `VECTORS_MATCH` (minimum cosine 1.0) and `REBUILD_MATCHES_RECEIPT`. Rebuilding needs vector-snake (available on request) and the same model snapshot, recorded in `CORPUS.md`. On another machine the last decimals of the numbers can differ; the cuts can't.

## 5. Line endings

Git for Windows converts line endings by default (`core.autocrlf true`). A clone would then differ, byte for byte, from the original, and its cuts could too. Add one line to `.gitattributes` before the first commit:

```
* text=auto eol=lf
```

## 6. Editing from anywhere

In the video, a pacing rule was added to `SSOT.md` from the **GitHub mobile app**, and committed there. Back on the laptop, `git pull` brought it down: `Fast-forward · SSOT.md | 1 +`. The words and rules travel to any device. Rebuilding the meaning search needs a computer with the tools.

## 7. Habits

1. **Before deleting a local copy:** push, then check that `git status` reports a clean tree.
2. **On a borrowed computer:** remove the stored sign-in afterwards (`cmdkey /delete:git:https://github.com`).
3. **Your own manuscript:** keep the repository **Private**. The teaching novel is Public so that you can clone it.
4. **Privacy in a public repository:**
   - commit with GitHub's no-reply address (Settings → Emails → *Keep my email addresses private*, then `git config --global user.email` with the address it shows);
   - search for personal paths before the first push: `Select-String receipts\*.json -Pattern 'Users\\'` should find nothing.
5. **Follow your own runbook, not GitHub's quick-setup page.** Its suggested `first commit` would have replaced the seal.
