## Agent skills

### Issue tracker

Issues live as local markdown files under `.scratch/<feature>/`. See `docs/agents/issue-tracker.md`.

### Domain docs

Single-context: one `GLOSSARY.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.

## Design

`DESIGN.md` is the authoritative design; `docs/reference/yinger-style.md` is borrowed source material only. Mockups live in `mockups/` and are throwaway. Preview them with the `mockups` server in `.claude/launch.json`.

The live site is `site/`, deployed to GitHub Pages by `.github/workflows/pages.yml` on push to `main`. Add a Project by editing `site/projects.js`. Preview it with the `site` server.
