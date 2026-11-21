# Lesson 0 — Setting Up Your Developer Environment

**Goal:** By the end of this lesson you'll have Python, Git, uv and VS Code installed, a GitHub account connected securely, your first project on GitHub, and your first Pull Request merged.

**Time:** 2–3 hours

---

## 0.1 Open the Terminal

The Terminal is where you type commands for your computer. Developers use it every day.

Press `Cmd + Space`, type **Terminal**, press Enter.

Check what you already have:

```bash
python3 --version   # Is Python installed? Which version?
git --version       # Is Git installed?
uname -m            # Which chip? arm64 = Apple Silicon, x86_64 = Intel
sw_vers             # Which macOS version?
```

> 💡 **Tip:** Type each command and press **Enter**. The output appears below the command. Copy it with `Cmd + C` when asking for help — exact error text matters.

---

## 0.2 Install the tools

You need three tools:

| Tool | What it does |
|---|---|
| **Git** | Tracks every change to your code, so you can undo, branch and collaborate |
| **uv** | Manages Python versions, project environments and packages — the modern standard |
| **VS Code** | Your code editor |

### Option A — Apple Silicon Macs (M1/M2/M3/M4): use Homebrew

[Homebrew](https://brew.sh) is the standard Mac package manager.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install git uv
brew install --cask visual-studio-code
```

### Option B — Intel Macs: install each tool directly

Homebrew is phasing out Intel Macs, and its installer may refuse with:

```
Homebrew on macOS is only supported on Apple Silicon processors!
```

That's fine — install each tool from its official source instead.

**Git** (via Apple's Command Line Tools). If your Git is old, reinstall the tools:

```bash
sudo rm -rf /Library/Developer/CommandLineTools   # removes only the old developer tools
xcode-select --install                            # click "Install" in the pop-up
```

> When the Terminal asks for your password, nothing appears on screen as you type. That's normal — type it and press Enter.

**uv** (official installer):

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**VS Code:** download it from [code.visualstudio.com](https://code.visualstudio.com), then drag **Visual Studio Code** into your **Applications** folder.

### Enable the `code` command

This lets you open projects in VS Code from the Terminal.

1. Open VS Code **from the Applications folder**.
2. Press `Cmd + Shift + P`, type **shell command**, choose **Install 'code' command in PATH**.

### Verify

Quit and reopen the Terminal (`Cmd + Q`), then:

```bash
git --version
uv --version
code --version
```

All three should print a version number.

---

## 0.3 Tell Git who you are

Git stamps your name and email on every commit.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global --list      # check it
```

- `--global` applies the setting to every project on this computer.
- `init.defaultBranch main` makes new projects start on a branch called `main` (the modern standard).

> 🔒 **Privacy:** this email becomes public on every commit you push. To keep it private, use the "noreply" address from GitHub → **Settings → Emails → Keep my email addresses private**.

---

## 0.4 Connect to GitHub with an SSH key

Create a free account at [github.com](https://github.com). Pick your username carefully — it's your public developer profile.

An **SSH key** is like a digital ID card: your computer keeps the *private* half, GitHub gets the *public* half, and they recognize each other without passwords.

**1. Create the key**

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

Press **Enter** to accept the default location. Choose a passphrase you'll remember.

**2. Let macOS remember it**

```bash
code ~/.ssh/config
```

Paste this, save (`Cmd + S`):

```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Then:

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

**3. Give GitHub your public key**

```bash
pbcopy < ~/.ssh/id_ed25519.pub     # copies the public key to your clipboard
```

On GitHub: **Settings → SSH and GPG keys → New SSH key**, paste, save.

**4. Test**

```bash
ssh -T git@github.com
```

The first time, you'll see:

```
The authenticity of host 'github.com' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
```

This is your computer asking whether it trusts the server. That fingerprint matches [GitHub's published key](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints), so type **yes**. If it ever changes unexpectedly, SSH will warn you — that's the protection.

Success looks like:

```
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 0.5 Your first project

```bash
mkdir -p ~/code          # a folder for all your projects
cd ~/code                # move into it
uv init python-journey   # create a Python project (also starts a Git repo)
cd python-journey
ls -a                    # list everything, including hidden files
```

Depending on your uv version, you'll get one of two layouts.

**Newer uv — "src layout"** (the structure real Python packages use):

```
python-journey/
├── src/python_journey/__init__.py   ← your code
├── pyproject.toml                   ← project settings and dependencies
├── uv.lock                          ← exact package versions (for rebuilding anywhere)
├── .venv/                           ← this project's private Python environment
├── .python-version                  ← which Python the project uses
└── .gitignore                       ← files Git should ignore
```

Run it with:

```bash
uv run python-journey
```

**Older uv — single file:** you'll see `main.py` instead of `src/`. Run it with `uv run main.py`.

Either way you should see:

```
Hello from python-journey!
```

Open the project in VS Code:

```bash
code .        # "." means "this folder"
```

> 💡 Always open the **project folder**, not single files. The Explorer (`Cmd + Shift + E`) shows the file tree, and the built-in Terminal (`` Ctrl + ` ``) opens right inside your project.

---

## 0.6 Commit and push

```bash
git status                            # see what changed (red = not staged)
git add .                             # stage everything (green = staged)
git commit -m "Initial project setup" # save a snapshot
```

On GitHub, click **+ → New repository**, name it `python-journey`, keep it **Public**, and **don't** add a README, .gitignore or license (your project already has them).

```bash
git remote add origin git@github.com:YOUR-USERNAME/python-journey.git
git push -u origin main
```

- `origin` is a nickname for your GitHub repo's address.
- `-u` links your local `main` to GitHub's `main`, so next time `git push` is enough.

Refresh GitHub — your files are there.

---

## 0.7 Your first Pull Request

On real teams, nobody edits `main` directly. You make changes on a **branch**, then ask for them to be merged through a **Pull Request (PR)**, where code review happens.

```bash
git switch -c update-readme      # create a branch and move to it
```

Edit `README.md` — describe what you're learning and add a progress checklist. Save, then:

```bash
git add README.md
git commit -m "Add learning goals and progress to README"
git push -u origin update-readme
```

On GitHub, click **Compare & pull request**. Write a description using this template:

```markdown
## What
Adds learning goals and a progress checklist to the README.

## Why
To track my progress through the course.

## How I tested
Previewed the README on GitHub; checkboxes render correctly.
```

Check the **Files changed** tab (red = removed, green = added), then **Merge pull request**.

Finally, update your local copy:

```bash
git switch main
git pull
```

### Local vs remote branches

There are two copies of `main`:

- **Local `main`** — on your computer. Changes only when you commit or `git pull`.
- **`origin/main`** — on GitHub. Merging a PR changes only this one.

That's why you `git pull` after merging. In VS Code, the **bottom-left status bar** shows your current branch (click it to switch) and sync arrows: `↓` commits to pull, `↑` commits to push.

---

## Troubleshooting

| Error | Cause | Fix |
|---|---|---|
| `Homebrew on macOS is only supported on Apple Silicon processors!` | Intel Mac | Use Option B in section 0.2 |
| `zsh: permission denied: code` | The `code` command was installed while VS Code was running from Downloads | Move VS Code to Applications, open it from there, run **Uninstall 'code' command**, then **Install 'code' command** |
| `error: Failed to spawn: main.py` / `No such file or directory` | Wrong folder, or newer uv created a `src` layout | Run `pwd` and `ls -a`. If you see `src/`, use `uv run <project-name>` |
| Typed password but nothing appears | Normal Terminal behavior | Keep typing and press Enter |

> 💡 **Reading errors is a core skill.** `pwd` (where am I?) and `ls -a` (what's here?) solve a surprising number of problems.

---

## Commands learned

| Command | What it does |
|---|---|
| `pwd` | Show the current folder |
| `ls -a` | List files, including hidden ones |
| `cd <folder>` | Move into a folder |
| `mkdir -p <folder>` | Create a folder |
| `code .` | Open the current folder in VS Code |
| `uv init <name>` | Create a new Python project |
| `uv run <name>` | Run the project |
| `git status` | Show what changed |
| `git add <file>` / `git add .` | Stage changes |
| `git commit -m "msg"` | Save a snapshot |
| `git switch -c <branch>` | Create and switch to a branch |
| `git switch <branch>` | Switch branch |
| `git push -u origin <branch>` | Upload a branch to GitHub |
| `git pull` | Download changes from GitHub |

---

## ✅ Checklist

- [ ] Git, uv and VS Code installed
- [ ] Git configured with name and email
- [ ] SSH key added to GitHub; `ssh -T git@github.com` says "Hi"
- [ ] `python-journey` repo pushed to GitHub
- [ ] First Pull Request opened, reviewed and merged
- [ ] Local `main` updated with `git pull`

**Next:** Lesson 1 — Variables, types and your first real program.
