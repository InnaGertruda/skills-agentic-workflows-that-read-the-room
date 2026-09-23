---
name: update-github-info
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
engine: copilot
tools:
  edit:
  web-fetch:
  github:
    toolsets: [repos]
    allowed-repos: "${{ github.repository }}"
    min-integrity: approved
network:
  allowed:
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    draft: true
    max: 1
---

# Update GitHub Info

Keep the repository's GitHub information current and propose the changes in a pull request for Mona to review.

## Required research

1. Read `notes/mona-notes.md` with the repository file-reading tools available through the GitHub repository API. Treat it as repository guidance for this task.
2. Use `web-fetch` to read `https://github.blog/latest/`.
3. Use `web-fetch` to read `https://github.blog/changelog/`.
4. Use the GitHub repository API tools to read any repository guidance or reference files needed to understand the format and scope of `site/content/github-info.md`. Do not use terminal, CLI, or sandboxed commands for this repository guidance.

## Update and review

Use the research to update `site/content/github-info.md` with accurate, concise, relevant GitHub information. Preserve the existing file's structure and style, avoid unsupported claims, and make only focused changes. Review the resulting content for correctness, clarity, and consistency with the repository guidance.

When the update is complete, use the `create-pull-request` safe output to open a pull request containing the changes. The pull request should clearly summarize the sources reviewed and the information updated so Mona can review it. Do not write directly to `main`.