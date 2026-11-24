# GitHub First Push — Windows PowerShell Reference Guide

Git · GitHub · Git Credential Manager · GitHub CLI · HTTPS authentication

This guide covers the first push from a local repository to GitHub:
- linking the remote;
- understanding authentication;
- signing in;
- pushing `main`;
- verifying that it worked.

**The core principle:** git commits locally; GitHub only receives commits through an authenticated connection. These are separate operations, and a successful commit doesn't prove that a push will work.

The examples use the Vault 2 repository, `https://github.com/DamianGiambazi/novel-ssot.git`. Use your own account and repository name.

## 1. The workflow

1. **Prepare the local repository:** `git init`, `git add`, `git commit`. Confirm the branch is `main`.
2. **Create the repository on GitHub,** with **no** README, licence or `.gitignore`. Ignore the commands GitHub's quick-setup page suggests; your repository already has its first commit.
3. **Link it:** `git remote add origin <url>`.
4. **Push:** `git push -u origin main`. On the first push you'll be asked to sign in.

## 2. Step by step

### Step 1: check the local repository
```powershell
git status
git branch --show-current
git log -1 --oneline
```
Confirm that you are in the intended repository, that the branch is `main`, and that at least one commit exists. If the branch has another name, push that branch or rename it deliberately. If new repositories start on another branch, set the default once: `git config --global init.defaultBranch main`.

### Step 2: link the remote
```powershell
git remote add origin https://github.com/DamianGiambazi/novel-ssot.git
git remote -v
```
Expected:
```
origin  https://github.com/DamianGiambazi/novel-ssot.git (fetch)
origin  https://github.com/DamianGiambazi/novel-ssot.git (push)
```
`origin` is just a local nickname for the remote. If you see `error: remote origin already exists`, inspect it with `git remote -v`. Correct it with `git remote set-url origin <url>`, rather than adding a second remote.

### Step 3: the first push, and signing in
```powershell
git push -u origin main
```
GitHub no longer accepts your account password for git over HTTPS. On Windows, Git for Windows includes **Git Credential Manager**. On the first push it prints:
```
info: please complete authentication in your browser...
```
- A browser window opens. Sign in to GitHub (with two-factor authentication if it's on) and authorise.
- The push then finishes in the same command.
- Windows stores the sign-in, so later pushes don't ask again.

Check that the helper is set: `git config --show-origin --get-all credential.helper` should show `manager`.

A successful first push ends with:
```
To https://github.com/DamianGiambazi/novel-ssot.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```
After this, `git push` and `git pull` work without the extra words.

### Step 4: verify
```powershell
git status -sb
git ls-remote --heads origin main
```
`## main...origin/main` means your branch is linked and up to date. Then open the repository in the browser and look.

## 3. Other ways to sign in

| Method | When to use it | How |
|---|---|---|
| **Git Credential Manager (browser)** | the default on Windows; nothing to install | push; sign in when the browser opens |
| **GitHub CLI** | you also want GitHub commands in the terminal | install from [cli.github.com](https://cli.github.com/), then `gh auth login` (GitHub.com → HTTPS → Login with a web browser) and `gh auth setup-git`; check with `gh auth status` |
| **Personal access token** | automation, or a machine without a browser | create a fine-grained token limited to one repository ([GitHub docs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)); treat it as a password and never commit it |
| **SSH key** | many repositories on one machine, long term | [Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) |

## 4. Troubleshooting

| Error or situation | Likely cause | Response |
|---|---|---|
| `remote origin already exists` | the remote is already configured | `git remote -v`; `git remote set-url` if wrong |
| `Authentication failed` | wrong, expired or conflicting stored credentials | remove the stored entry (`cmdkey /list`, then `cmdkey /delete:git:https://github.com`) and push again |
| `Repository not found` | wrong URL, a private repository, or the wrong account | check the URL and which account you signed in with |
| `Permission denied` / HTTP 403 | the signed-in account can't write to this repository | check the account and the repository's access settings |
| `src refspec main does not match any` | no commit yet, or the branch isn't `main` | `git branch --show-current`, `git log -1` |
| `rejected — fetch first` / `non-fast-forward` | the remote has commits you don't have (e.g. a README made on GitHub) | inspect them, `git pull`, then push. Never `git push --force` as a quick fix |

## 5. Privacy before the first push to a public repository

- **Commit with GitHub's no-reply address:** Settings → Emails → *Keep my email addresses private*, then `git config --global user.email` with the address shown there. Commits already pushed keep their old address.
- **Search your files for personal paths and names** before the push.
- **On a shared or borrowed computer,** remove the stored sign-in when you're done: `cmdkey /delete:git:https://github.com`.

**The rule:** the remote is configuration; authentication is identity; repository permissions authorise the push; the push transfers commits. When something fails, check each layer separately.
