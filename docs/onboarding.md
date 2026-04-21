# Onboarding: Getting set up with Cursor + GitHub

Test edit for pull request.

Welcome. This guide walks you through everything you need to work on a client project, assuming you've never used Git or GitHub before. Take it slow — the first-time setup is about 20 minutes, but after that you'll rarely think about it again.

By the end of this guide, you'll be able to:

- Open a client project in Cursor
- Create a branch for your work (so you don't overwrite anyone else)
- Save your changes
- Open a pull request for Will to review

---

## What is all this stuff, in plain terms

Before we install anything, a quick vocabulary check:

- **Git** is a tool that tracks changes to files. Think of it like Google Docs version history, but more powerful and works with any kind of file.
- **GitHub** is a website that stores Git projects in the cloud so the whole team can share them.
- **A repository** (or "repo") is just a folder of project files that Git is tracking. Each of our clients has one.
- **Cursor** is the app we use to open and edit files. It has Git and Claude built in, so most of the time you won't need to touch the command line.
- **A branch** is your own private copy of the project where you can make changes without affecting anyone else's work. You'll make a new branch every time you start something new.
- **A pull request** (or "PR") is how you ask Will to review your work and merge it into the main project.

If anything below doesn't make sense, ask in Slack — don't guess.

---

## Part 1: One-time setup

You only do this once per computer.

### Step 1: Install Cursor

Download Cursor from [cursor.com](https://cursor.com) and install it like any other app.

### Step 2: Install Git

On Mac, open the Terminal app (search Spotlight for "Terminal") and paste this:

```
git --version
```

If it prints a version number, Git is already installed. If it asks you to install developer tools, click "Install" and wait for it to finish.

On Windows, download Git from [git-scm.com](https://git-scm.com) and accept all the defaults during install.

### Step 3: Make a GitHub account

Go to [github.com](https://github.com) and sign up if you don't already have an account. Use your work email. Once you have an account, send your GitHub username to Will so he can add you to the team.

### Step 4: Sign into GitHub from Cursor

Open Cursor. Press `Cmd+Shift+P` (Mac) or `Ctrl+Shift+P` (Windows) to open the Command Palette — this is a search bar for Cursor's features that you'll use a lot.

Type `Sign in to GitHub` and press Enter. A browser window will open and ask you to authorize Cursor. Click through and approve it.

### Step 5: Tell Git who you are

Back in Terminal, run these two commands, replacing the name and email with your own. This is so your commits are attributed to you:

```
git config --global user.name "Your Name"
git config --global user.email "your.email@onedesigncompany.com"
```

You're done with setup.

---

## Part 2: Opening a client project for the first time

Every time a new client project starts, you'll need to download a copy of the repo to your computer. This is called "cloning."

1. In Cursor, open the Command Palette (`Cmd+Shift+P` or `Ctrl+Shift+P`).
2. Type `Git: Clone` and press Enter.
3. Paste the repo URL (the project lead or PM will share it) and press Enter.
4. Choose where on your computer to save it. **Important:** pick a folder that is NOT inside Google Drive, Dropbox, iCloud, or any other sync service. Those tools interfere with Git. A folder like `~/Projects/` or `Documents/project-name/` on your main drive is perfect.
5. When it finishes, Cursor will ask if you want to open the folder. Say yes.

You now have the whole project on your computer. The first thing to read is the `README.md` file in the root of the repo — it tells you how that specific client's work is organized.

---

## Part 3: The daily workflow

Here's what you do every time you sit down to work.

### Before you start: pull the latest changes

Other people may have merged work since you last opened the project. Grab their updates first.

In Cursor, click the **Source Control** icon in the left sidebar (it looks like a branching line — third icon down) or press `Ctrl/Cmd+Shift+G`. At the top of that panel, click the `...` menu and choose **Pull**. This downloads any new changes from GitHub.

### Step 1: Make a new branch for your work

Never work directly on the `main` branch. Always start by making your own branch.

1. Look at the bottom-left of the Cursor window. You'll see the current branch name (probably `main`).
2. Click it. A menu appears at the top.
3. Choose **Create new branch**.
4. Name your branch after what you're doing. Use this format: `your-area/what-youre-doing`. Examples:
   - `research/stakeholder-interview-synthesis`
   - `design/homepage-wireframes`
   - `strategy/q2-roadmap-draft`
5. Press Enter.

You're now on your new branch. Anything you change from here on is isolated from everyone else's work.

### Step 2: Do your work

Edit files, add new ones, use Claude inside Cursor to help — whatever the task is. Save normally with `Cmd+S` / `Ctrl+S`.

### Step 3: Commit your changes

A "commit" is a snapshot of your work. You can make as many as you want. A good habit is to commit every time you finish a meaningful chunk — like "finished the first draft of the interview guide" or "added three new wireframe screens."

1. Open the Source Control panel (`Ctrl/Cmd+Shift+G`).
2. You'll see a list of changed files. Hover over each one and click the `+` icon to "stage" it. (Or click the `+` next to "Changes" to stage everything.)
3. Type a short message in the box at the top describing what you did. Examples:
   - `Add draft interview guide for Wintrust stakeholders`
   - `Update homepage wireframe with new nav pattern`
4. Click the **Commit** button (checkmark icon).

### Step 4: Push your branch to GitHub

Committing saves your work locally. Pushing uploads it to GitHub so others can see it.

In the Source Control panel, click the `...` menu and choose **Push**. The first time you push a new branch, Cursor may ask "Publish branch?" — say yes.

### Step 5: Open a pull request

Once your work is ready for the project lead to review:

1. Go to the repo on github.com (or click the GitHub icon in Cursor's sidebar if it's available).
2. You'll see a yellow banner offering to open a pull request from your branch. Click it.
3. Write a short description of what you did and why. Bullet points are fine.
4. Assign the project lead as the reviewer.
5. Click **Create pull request**.

The project lead gets notified, reviews your work, and either leaves comments for revision or merges it into `main`. If he asks for changes, make them on the same branch, commit, and push again — the pull request updates automatically.

---

## Common situations

**"I started working and forgot to make a branch."**
Not a disaster. In the branch menu (bottom-left), create a new branch now. Your uncommitted changes will come with you. Then commit and push as normal.

**"I want to try two different approaches to the same thing."**
Make two branches off `main`, one for each. You can switch between them freely from the branch menu.

**"I'm afraid I'll break something."**
You won't. Git's whole purpose is to make it safe to experiment. Nothing you do on your own branch can affect `main` or anyone else's work. The worst case is throwing away your branch and starting over — which is cheap and painless.

**"Claude inside Cursor changed a bunch of files. How do I see what changed before I commit?"**
The Source Control panel shows every changed file. Click on any file name in that panel to see a side-by-side diff of what changed. Review before you commit.

**"I need to undo a change."**
In the Source Control panel, hover over the changed file and click the curved-arrow "Discard Changes" icon. This only works for changes you haven't committed yet. For committed changes, ask the project lead or post in Slack — it's recoverable but the steps depend on the situation.

---

## Working with Claude in this setup

A few things worth knowing:

- Claude automatically reads the `CLAUDE.md` file in the repo when you start a session. That file has the client-specific context — voice, tone, design conventions, things to avoid. You don't need to paste context in every time.
- If you notice Claude making the same mistake twice, tell it to add a note to `CLAUDE.md` so it remembers for next time.
- Treat the `.agent/skills/` folder as the place where project-specific workflows live. If you develop a repeatable process for this client (e.g., a specific way to structure interview synthesis), ask Claude to save it as a skill there.
- Work on your branch applies to Claude-generated files too. If Claude writes a file, commit it, push it, and let the project lead review via PR — same as any other work.

---

## Getting help

If anything goes sideways, post in the `#digital-practice` Slack channel with a screenshot and a description of what you were trying to do. Don't keep clicking things hoping it'll fix itself — Git is recoverable from almost anything if you stop and ask.

Welcome to the workflow.
