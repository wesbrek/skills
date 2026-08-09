---
name: open-pr
description: Open a GitHub pull request for the current change, with screenshots or a video demo embedded inline in the description.
---

# Open PR

Open a high-quality GitHub pull request for the current change, with images and
videos embedded **inline** in the description.

## Prerequisites

`gh` installed and authenticated (`gh auth status`).

## Process

### 1. Establish the change context

- Confirm the current branch and the base branch (usually the repo default).
- Read the diff against the base (`git diff <base>...HEAD`, `git log`, changed
  files). Summarize what changed and **why**, with every changed file accounted
  for.
- If the branch is the default branch, stop and offer to create a feature branch
  first.
- If there are uncommitted changes, ask whether to commit them first.

### 2. Gather media

- Ask for image/video paths, or use paths already produced in the session
  (e.g. Playwright output).
- If the change is visual and no media exists, offer to capture it — but do not
  block PR creation on it.

### 3. Draft the PR

Write a title and description from the change context. Match the repo's
conventions if a `PULL_REQUEST_TEMPLATE` exists.

- **Title:** concise, imperative, conventional-commit style if the repo uses it.
- **Body:** what changed, why, how to test, linked issues (`Closes #N`) when
  applicable. Leave a clear spot where media will go.

Show the draft to the user before creating anything.

### 4. Create the PR

```bash
gh pr create --base <base> --head <branch> --title "<title>" --body "<body>"
```

Capture the PR URL from the output.

### 5. Embed media inline

[`pr-media`](https://github.com/MatteoSchifano/gh-pr-media) is **optional**. When
it is installed, delegate the media step to it rather than reimplementing the
upload:

```bash
gh pr-media add <files...> --pr-url <pr-url> --to description
```

Prefer `--strategy browser` so video plays inline; it drives your already
logged-in local browser, so it assumes a developer machine (`auto` picks it
outside CI). That browser session is the credential — pass none of your own.

When `pr-media` is missing, the PR is already open: tell the user "PR created.
Install `pr-media` to embed images/video inline" (`gh extension install
MatteoSchifano/gh-pr-media` or `npm i -g pr-media`), then stop.

### 6. Report

Print the final PR URL and a one-line summary (title, files embedded, strategy).
