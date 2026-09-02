---
name: update-github-info
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  pull-requests: read
engine:
  id: copilot
  model: gpt-4.1
tools:
  github:
    toolsets: [repos]
  bash:
    - "curl https://github.blog/*"
    - "curl https://awesome-copilot.github.com/*"
  edit:
  web-fetch:
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    max: 1
    draft: true
---

# Update GitHub Info

Refresh the website's GitHub information and propose the changes in a pull request for Mona to review.

## Sources and repository context

1. Use the GitHub repository API tools to read `notes/mona-notes.md` and `site/content/github-info.md`. Do not use terminal, CLI, or sandboxed shell commands to read repository guidance or reference files.
2. Use the `web_fetch` tool (the web-fetch frontmatter tool) to fetch https://github.blog/latest/.
3. Use the `web_fetch` tool to fetch https://github.blog/changelog/.
4. Use the `web_fetch` tool to fetch https://awesome-copilot.github.com/workflows/.
5. If `web_fetch` is unavailable, use the permitted `curl` tool only for those exact three allowlisted URLs. Do not report that web access is unavailable before trying both `web_fetch` and the permitted fallback commands.
6. Use the notes to guide the editorial style. Prefer short, practical developer-focused summaries and include the source for each Blog, Changelog, or Awesome Copilot update.

## Update and review

Update `site/content/github-info.md` with useful current items from the official GitHub Blog and Changelog. Preserve the existing Markdown structure and keep the content focused on practical GitHub guidance. Do not invent facts or include items that cannot be supported by the fetched sources.

Review the resulting diff for accuracy, concise writing, valid Markdown, and unrelated changes. When the update is complete, commit the changes locally and call the `create_pull_request` safe-output tool exactly once with a concise title and body explaining the sources reviewed and the content updated. Do not push directly to the default branch. The pull request is for Mona to review.