# Beginner Guide: Using the GitHub Plugin With Codex

This guide is for a light GitHub user who already knows basic Git commands such as `git clone`, `git branch`, `git add`, `git commit`, `git status`, `git push`, `git merge`, and `git rebase`, but is new to using the GitHub Plugin inside Codex.

The goal is to show how Codex, the GitHub Plugin, local `git`, and GitHub CLI `gh` work together in real software workflows.

## The Big Picture

Think of the workflow as four layers:

| Tool | What it is good for |
|---|---|
| Codex | Your coding assistant. It reads files, edits code, runs commands, explains changes, and helps plan workflows. |
| GitHub Plugin | Lets Codex inspect GitHub repositories, issues, pull requests, comments, labels, and PR metadata. |
| local `git` | Handles your local repository: branches, diffs, staging, commits, rebases, merges, and pushes. |
| `gh` | GitHub CLI. Useful for authentication, current-branch PR discovery, PR checks, and GitHub Actions logs. |

In plain language:

- Ask the **GitHub Plugin** for GitHub context.
- Ask **Codex** to edit and verify local code.
- Use **`git`** for local version-control actions.
- Use **`gh`** for GitHub CLI tasks that the plugin does not cover well.

## Before You Start

You should have:

1. A local repository.
2. A GitHub remote connected to that repo.
3. The GitHub Plugin installed and authorized in Codex.
4. GitHub CLI installed if you want PR checks or CI logs:

```bash
gh --version
gh auth status
```

If `gh auth status` says you are not logged in, run:

```bash
gh auth login
```

You do not need to use `gh` for every task. It mainly matters for current-branch PR lookup and GitHub Actions logs.

## A Simple Mental Model

When you ask Codex to do GitHub work, be explicit about the starting point:

- "Use this local repository."
- "Use issue #12."
- "Use PR #34."
- "Use this GitHub URL."
- "Use the current branch."

Good prompt:

```text
Use GitHub issue #12 as context. Summarize the task, inspect the local repo, propose a small implementation plan, then wait for my approval before editing.
```

Less useful prompt:

```text
Help with GitHub.
```

## GitHub Workflow Concepts Before The Examples

Before using the GitHub Plugin, it helps to understand the main GitHub objects you will see in common workflows.

### Repository

A repository is the project container on GitHub. It stores code, branches, issues, pull requests, discussions, releases, and project history.

Use a repository when you want Codex to understand the project as a whole:

```text
Use GitHub to summarize this repository: open issues, open PRs, recent activity, and what needs attention.
```

Codex can use the GitHub Plugin to inspect repository-level information, while local `git` handles the files in your checkout.

### Issue

An issue is a task, bug report, feature request, question, or planning item. Think of it as the "what needs to be done" record.

Use an issue when:

- You want to describe a bug or feature before coding.
- You want to discuss requirements before implementation.
- You want a trackable task that can later be linked to a PR.
- You want Codex to use the issue as the source of truth for a change.

Beginner example:

```text
Use GitHub issue #12 as context. Summarize the request, list acceptance criteria, and propose a small implementation plan. Do not edit files yet.
```

Typical issue-to-code flow:

1. Create or inspect an issue.
2. Understand the requested change.
3. Create a branch locally.
4. Implement the change.
5. Open a PR that references the issue.

### Branch

A branch is a separate line of work in Git. It lets you make changes without directly changing `main` or `master`.

Use a branch when:

- You are starting a feature or bug fix.
- You want your changes isolated from stable code.
- You plan to open a pull request.

Common beginner branch pattern:

```bash
git checkout -b codex/fix-issue-12
```

When using Codex, you can ask:

```text
If I am on main or master, create a new branch named codex/fix-issue-12 before editing. Otherwise stay on the current feature branch.
```

### Commit

A commit is a saved snapshot of your local changes. It should represent a logical unit of work.

Use a commit when:

- You have finished a small coherent change.
- Tests or checks pass, or you have documented what could not be verified.
- You are ready to push the change to GitHub.

Before committing, ask Codex to check scope:

```text
Run git status and git diff. Tell me which files changed and whether anything looks unrelated before staging.
```

### Pull Request

A pull request, often called a PR, is a request to merge one branch into another branch on GitHub. In most workflows, you push a feature branch and open a PR into `main`.

Think of a PR as the review and collaboration space for a code change. It usually contains:

- The code diff.
- A description of what changed and why.
- Review comments.
- CI check results.
- Discussion about whether the change is ready to merge.

Use a PR when:

- You want someone to review your code before merging.
- You want CI checks to run on your branch.
- You want a visible record of why a change was made.
- You want Codex to inspect comments, changed files, and test status.

Beginner tip: open a **draft PR** when you want feedback but are not ready to merge yet.

Prompt:

```text
Open a draft pull request for this branch. In the PR body, include what changed, why it changed, how it was tested, and what still needs review.
```

### Review Comments

Review comments are feedback left on a PR. They may ask for code changes, request clarification, or simply discuss design choices.

Not every comment requires a code change. Codex should separate:

- Actionable comments: require a change.
- Discussion-only comments: require a reply or decision.
- Resolved comments: already handled.
- Conflicting comments: need your judgment before editing.

Prompt:

```text
Use GitHub to inspect unresolved review comments on this PR. Separate actionable comments from discussion-only comments, then wait for my approval before editing.
```

### CI Checks And GitHub Actions

CI checks are automated tests or validations that run on GitHub. GitHub Actions is GitHub's built-in CI system.

Use CI checks when:

- You want to know whether the code builds.
- You want tests to pass before merging.
- You want Codex to debug a failing PR check.

The GitHub Plugin can inspect PR context, but Codex usually needs `gh` to read GitHub Actions logs.

Prompt:

```text
Inspect the failing GitHub Actions checks on this PR. Use GitHub for PR context and gh for logs. Summarize the failure and propose a fix before editing.
```

### How These Pieces Fit Together

A common GitHub workflow looks like this:

1. **Issue** records the task.
2. **Branch** isolates the work.
3. **Commits** save local progress.
4. **Push** uploads the branch to GitHub.
5. **Pull request** asks to merge the branch.
6. **Review comments** improve the change.
7. **CI checks** verify the change.
8. **Merge** brings the work into the main branch.

Codex and the GitHub Plugin help mostly with steps 1, 5, 6, and 7. Local `git` handles steps 2, 3, 4, and merge/rebase operations.

## Common Workflow 1: Understand A GitHub Issue Before Coding

Use this when an issue already describes the bug, feature, or task. The issue should explain what needs to be done; Codex helps translate it into an implementation plan.

Prompt:

```text
Use GitHub to inspect issue #12 in this repository. Explain the requested change in beginner-friendly language, identify likely files to edit, list assumptions, and propose a step-by-step implementation plan. Do not edit files yet.
```

What Codex should do:

1. Use the GitHub Plugin to read the issue.
2. Summarize the issue.
3. Inspect the local repo if needed.
4. Suggest likely files or areas.
5. Give you a plan before editing.

Good follow-up:

```text
Proceed with the implementation. Keep the change small, then run the relevant checks and summarize the diff.
```

## Common Workflow 2: Make A Local Code Change With GitHub Context

Use this when GitHub has the task context, usually in an issue or PR, but the actual code changes happen in your local checkout.

Prompt:

```text
Use GitHub issue #12 as the source of truth. Implement the requested change locally. After editing, run relevant tests if available, then show me the changed files and a short explanation of the diff.
```

What happens:

1. Codex reads GitHub issue context.
2. Codex edits local files.
3. Codex may run tests or linters.
4. Codex summarizes what changed.
5. You decide whether to commit and push.

Useful follow-up:

```text
Before committing, run git status and show me which files changed. Do not stage anything yet.
```

## Common Workflow 3: Review Local Changes Before Committing

Use this when you already changed files locally and want to check whether the diff is clean before making a commit.

Prompt:

```text
Review my current local diff before I commit. Use git status and git diff, then tell me whether the changes look scoped, whether there are risky edits, and what tests I should run.
```

What Codex should do:

1. Run `git status`.
2. Inspect the diff.
3. Point out risky or unrelated changes.
4. Recommend tests.
5. Avoid staging or committing unless you ask.

If the diff has unrelated files, ask:

```text
Only stage the files related to this feature. Leave unrelated local edits untouched.
```

## Common Workflow 4: Commit, Push, And Open A Draft Pull Request

Use this when your local change is ready to become a GitHub collaboration item. The commit saves your work locally, the push uploads your branch, and the draft PR creates a place for review and CI.

Prompt:

```text
Publish these local changes to GitHub. First run git status and inspect the diff. Confirm which files will be staged. If I am on main or master, create a new branch named codex/<short-description>. Then commit, push, and open a draft pull request.
```

What Codex should do:

1. Run `git status`.
2. Inspect the diff.
3. Confirm the files to stage if there are unrelated changes.
4. Create or use a branch.
5. Stage intended files.
6. Commit.
7. Push.
8. Use the GitHub Plugin to open a draft PR if possible.
9. Use `gh pr create` only if the plugin path is not enough.

Beginner tip: prefer draft PRs at first. A draft PR lets you upload work for review without saying it is finished.

Good follow-up:

```text
In the PR body, include: what changed, why it changed, how I tested it, and anything reviewers should pay attention to.
```

## Common Workflow 5: Review A Pull Request

Use this when a branch has already been pushed and a PR exists. The PR is the right place to review the diff, discussion, test status, and merge readiness.

Prompt:

```text
Use GitHub to inspect PR #34. Explain what changed, which files are important, whether the PR has tests, and what a reviewer should focus on.
```

If you want a stricter code review:

```text
Review PR #34 for bugs, regressions, missing tests, and risky implementation choices. Put the most important findings first and include file references when possible.
```

What Codex should do:

1. Use the GitHub Plugin to inspect PR metadata and patch context.
2. Summarize the PR.
3. Identify risky areas.
4. Mention missing tests or unclear behavior.
5. Suggest next steps.

## Common Workflow 6: Address Pull Request Review Comments

Use this when someone reviewed your PR and left feedback. Codex should first classify comments before editing, because some comments require code changes while others only require discussion.

Prompt:

```text
Use GitHub to inspect unresolved review comments on the current PR. Separate actionable change requests from discussion-only comments. Show me the list first, then wait for my approval before editing.
```

After you approve:

```text
Fix all unresolved actionable review comments. Keep each change small and explain which comment it addresses. Run relevant checks after editing.
```

Important: Codex should not post replies, resolve threads, or submit a review unless you explicitly ask it to do so.

If you want a drafted reply instead of code:

```text
Draft a polite GitHub reply explaining why this review comment does not require a code change. Do not post it yet.
```

## Common Workflow 7: Debug Failing GitHub Actions

Use this when your PR has failing checks. The PR tells you which checks failed; `gh` helps Codex inspect the detailed GitHub Actions logs.

Prompt:

```text
Inspect the failing GitHub Actions checks on the current PR. Use GitHub for PR context and gh for check logs. Summarize the failing job, the relevant error message, and a proposed fix plan. Do not edit files until I approve.
```

What Codex should do:

1. Identify the current PR.
2. Use the GitHub Plugin for PR context.
3. Use `gh` to inspect GitHub Actions checks and logs.
4. Summarize the root cause.
5. Propose a focused fix.
6. Wait for approval before editing.

After approval:

```text
Implement the CI fix you proposed, run the closest local check, and summarize what remains unverified.
```

Important: the GitHub Plugin is not enough for GitHub Actions logs. Codex usually needs `gh` for that.

## Full Example: From Issue To Draft PR

This example shows a complete beginner workflow.

### Step 1: Ask Codex to inspect the issue

```text
Use GitHub issue #12 as context. Summarize the requested change, identify likely files to edit, and propose a step-by-step plan. Do not edit files yet.
```

Expected result:

- Codex explains the issue.
- Codex lists likely files.
- Codex gives a plan.
- You approve or adjust the plan.

### Step 2: Ask Codex to implement locally

```text
Proceed with the implementation. Keep the change scoped to issue #12. After editing, run relevant tests if available and summarize the changed files.
```

Expected result:

- Codex edits local files.
- Codex runs tests or explains why it could not.
- Codex summarizes the diff.

### Step 3: Review local git status

```text
Run git status and show me exactly which files changed. Do not stage or commit yet.
```

Expected result:

- You see modified files.
- You can catch unrelated changes before committing.

### Step 4: Ask Codex to prepare the branch

```text
Create a new branch named codex/fix-issue-12 if I am currently on main or master. Otherwise stay on the current feature branch.
```

Expected result:

- Codex checks the current branch.
- Codex creates a branch only if appropriate.

### Step 5: Commit the intended files

```text
Stage only the files related to issue #12, then commit with the message "fix issue 12". Do not stage unrelated files.
```

Expected result:

- Codex stages intended files.
- Codex commits them.
- Codex leaves unrelated files alone.

### Step 6: Push the branch

```text
Push this branch to origin with upstream tracking.
```

Expected result:

- Codex runs the equivalent of `git push -u origin <branch>`.

### Step 7: Open a draft PR

```text
Open a draft pull request for this branch. Title it "[codex] fix issue 12". In the PR body, include what changed, why, how it was tested, and any remaining risks.
```

Expected result:

- Codex opens a draft PR using the GitHub Plugin when possible.
- Codex falls back to `gh pr create` if needed.
- Codex gives you the PR link.

### Step 8: Handle review comments

```text
Use GitHub to inspect review comments on this PR. List actionable comments separately from discussion-only comments. Do not edit yet.
```

Then:

```text
Address the actionable review comments, run relevant checks, and summarize which comments were handled.
```

### Step 9: Check CI

```text
Check the GitHub Actions status for this PR. If anything failed, summarize the failing job and propose a fix before editing.
```

Then:

```text
Implement the approved CI fix and run the closest local verification.
```

## Copyable Prompt Templates

### Inspect repository

```text
Use GitHub to summarize this repository: open PRs, open issues, recent activity, and what likely needs attention.
```

### Inspect issue

```text
Use GitHub to inspect issue #<number>. Summarize the request, acceptance criteria, likely files to edit, assumptions, and a proposed plan.
```

### Implement issue

```text
Use GitHub issue #<number> as context. Implement the requested change locally, keep the diff scoped, run relevant checks, and summarize the result.
```

### Review local diff

```text
Review my current local diff before committing. Identify unrelated files, risky changes, and recommended tests.
```

### Publish changes

```text
Publish these local changes. Confirm the diff scope first, create a codex branch if needed, commit, push, and open a draft PR.
```

### Inspect PR

```text
Use GitHub to inspect PR #<number>. Summarize the purpose, changed files, test coverage, risks, and next action.
```

### Address review comments

```text
Use GitHub to inspect unresolved review comments on PR #<number>. List actionable comments first, then wait for my approval before editing.
```

### Debug CI

```text
Inspect the failing GitHub Actions checks on PR #<number>. Summarize the failure evidence and propose a fix plan before editing files.
```

## What To Ask Codex To Confirm

Before any GitHub write action, ask Codex to confirm:

- Which repository it is using.
- Which branch it is on.
- Which files it will stage.
- Whether there are unrelated local changes.
- Which tests or checks were run.
- Whether the PR is draft or ready for review.

Useful prompt:

```text
Before pushing or opening a PR, summarize the repo, branch, staged files, commit message, target base branch, and validation status.
```

## Common Mistakes To Avoid

- Do not ask Codex to "just push everything" if your worktree has unrelated files.
- Do not open a ready-for-review PR if you only want feedback; use a draft PR.
- Do not assume the GitHub Plugin can read GitHub Actions logs by itself; use `gh`.
- Do not let Codex resolve review threads or post comments unless you explicitly ask.
- Do not skip `git status` before staging or committing.

## Recommended Beginner Workflow

For most tasks, use this sequence:

1. Ask Codex to inspect the GitHub issue or PR.
2. Ask for a small plan.
3. Approve the plan.
4. Let Codex edit locally.
5. Ask Codex to run checks.
6. Ask Codex to show `git status` and summarize the diff.
7. Ask Codex to stage only intended files.
8. Ask Codex to commit.
9. Ask Codex to push.
10. Ask Codex to open a draft PR.
11. Ask Codex to inspect review comments and CI results.
12. Repeat with follow-up fixes if needed.
