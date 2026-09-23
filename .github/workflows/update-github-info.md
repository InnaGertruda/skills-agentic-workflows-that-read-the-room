---
name: update-github-info
description: Refresh Mona's GitHub info site content from official GitHub sources and propose a reviewable PR.
on:
  workflow_dispatch:
  schedule:
    - cron: '17 9 * * *'
tools:
  edit:
  web-fetch:
  github:
    toolsets: [repos]
    allowed-repos: all
    min-integrity: approved
network:
  allowed:
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    draft: true
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Keep the repository's GitHub information current and propose the changes in a pull request for Mona to review.

## Required research

Perform research sequentially, one request at a time. Reuse each response when preparing the update.

Your first three research actions must be direct `web-fetch` calls in exactly this order:

1. Read `https://github.blog/latest/`.
2. Read `https://github.blog/changelog/`.
3. Read `https://awesome-copilot.github.com/workflows/`.

Do not perform any other research action before these three requests are complete.

After completing the three web requests:

4. Read `notes/mona-notes.md` using the repository file-reading tools available through the GitHub repository API. Treat it as repository guidance for this task.

5. Use the GitHub repository API tools to read `site/content/github-info.md` and any additional repository guidance or reference files needed to understand its existing format and scope.

Do not use shell at any point for this task.

Do not use `curl`, temporary files, terminal commands, sandboxed commands, or other tools as substitutes for `web-fetch` or the GitHub repository API tools.

If `web-fetch` is unavailable or denied, stop the task and use the `noop` safe output with a short explanation. Do not attempt a shell fallback.

If any `web-fetch` request or GitHub repository API request returns HTTP `423` (Locked) or HTTP `429` (Too Many Requests), stop the affected research immediately.

Do not retry the failed request.

Do not switch to another tool or workaround that repeats the same request.

Use the `noop` safe output with a short reason and end the workflow without making repository changes.

## Update GitHub information

Use the collected research to update `site/content/github-info.md` with accurate, concise, and relevant GitHub information.

Preserve the existing file's structure, formatting, and writing style.

Make only focused changes supported by the sources reviewed.

Do not add unsupported claims, assumptions, or speculative information.

Do not modify any file other than:

`site/content/github-info.md`

If the research does not reveal anything meaningful that should be updated, do not make a change simply to create a pull request. Use the `noop` safe output with a short explanation instead.

## Review

Before proposing the update, review the resulting content for:

* factual correctness;
* consistency with `notes/mona-notes.md`;
* consistency with the existing structure and style of `site/content/github-info.md`;
* relevance to the purpose of the file;
* unsupported or speculative claims.

Remove or correct anything that is not supported by the reviewed sources.

## Pull request

When the update is complete, use the `create-pull-request` safe output to create a draft pull request containing the changes.

The pull request should clearly summarize:

* which sources were reviewed;
* what GitHub information was updated;
* why the changes are relevant;
* any existing information intentionally left unchanged because the research did not support changing it.

Do not write directly to `main`.

Do not modify files other than `site/content/github-info.md`.
