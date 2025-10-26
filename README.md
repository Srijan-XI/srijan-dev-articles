# srijan-dev-articles

A small collection of technical articles and how‑to guides maintained in Markdown.

This repository was created to automate article publishing: it stores articles (ready for publishing) and includes a GitHub Actions workflow that can publish new Markdown files to DEV.to automatically.

## Repository layout

```
/articles
	└─ *.md            # your article files (with YAML frontmatter)
.github/workflows
	└─ publish-to-dev.yml   # workflow that uploads new articles to DEV.to
```

## How publishing works

- The workflow `.github/workflows/publish-to-dev.yml` detects newly added `.md` files in a push and will POST them to the DEV.to API.
- To allow publishing, add your DEV.to API key to this repository's **Secrets** as `DEVTO_API_KEY` (Settings → Secrets → Actions).
- The workflow reads YAML frontmatter (between `---` markers) for metadata: `title`, `tags`, `series`, and `published` (boolean).

If you want to publish an article automatically, add a new Markdown file under `articles/` in a commit. The workflow uploads only newly added files (not edits) by default.

## Article format & conventions

Use a small YAML frontmatter block at the top of each article. Minimal example:

```yaml
---
title: Your article title here
published: true
tags: [devops, docker]
series: my-series-name
---
```

- `title` (string): the article title
- `published` (boolean): whether the article should be published (defaults to `true`)
- `tags` (array): up to 4 tags recommended for DEV.to
- `series` (string, optional): DEV.to series name

Filename guidance

- Keep file names descriptive and URL-friendly, e.g. `sora-2-openai-ai-video.md` or `n8n-docker-windows-11.md`.
- The workflow looks for files with `.md` extension anywhere in the repository (subject to the push diff).

## Adding a new article (quick steps)

1. Create a new Markdown file in `articles/` with frontmatter and body content.
2. Commit and push to `main` (or the branch your workflow listens to).
3. The workflow will detect the new file and attempt to publish it to DEV.to. Check Actions → latest run to see logs and responses.

## Preview locally

- Use your editor's Markdown preview (e.g., VS Code: `Ctrl+Shift+V`) to preview articles.
- Optionally use a static site generator or a Markdown viewer if you want richer previews.

## Troubleshooting publishing

- If the workflow fails, open the Actions tab and inspect the failed run. The job prints the DEV.to response body to help debug API errors.
- Common causes: missing `DEVTO_API_KEY`, malformed frontmatter, or the DEV.to API rejecting the payload (too many tags, missing title, etc.).

## Contributing

1. Fork the repo (or create a branch).
2. Add or update articles in `articles/` following the format above.
3. Open a PR; once merged to `main` the workflow will publish newly added files.

If you want edits to republish existing posts, we can adjust the workflow to detect changes and call the DEV.to update endpoint.

## License & contact

This repository contains user-created articles. Check each article for license or attribution requirements.

For questions or help with the publishing workflow, open an issue or ping me in the PR.

