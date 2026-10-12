# Headless WordPress Skill

A reusable agent skill for building, migrating, auditing, and troubleshooting WordPress-backed frontends. Works with Codex and Claude Code through the standard `SKILL.md` format.

WordPress remains the editorial backend. The skill guides frontend implementation with Next.js, the WordPress REST API, or WPGraphQL, while preserving existing framework and hosting choices.

## Install

Install for both Codex and Claude Code:

```bash
npx skills add prakharbha/headless-wordpress-skill --skill headless-wordpress -g -a codex -a claude-code
```

For one agent, use `-a codex` or `-a claude-code`. Omit `-g` for a project installation. You can inspect the available skill without installing:

```bash
npx skills add prakharbha/headless-wordpress-skill --list
```

## Use

In Codex, invoke `$headless-wordpress`. In Claude Code, invoke `/headless-wordpress` or describe a matching task. Examples:

- “Use headless-wordpress to connect this Next.js app to my existing WordPress REST API.”
- “Audit this WPGraphQL integration for draft exposure, pagination, and resolver performance.”
- “Add authenticated editor previews and content-triggered cache invalidation.”
- “Migrate the frontend while preserving existing URLs, custom fields, and editor workflows.”
- “Implement a contact form that stores private submissions in WordPress.”

Provide the project and non-secret CMS details. Configure credentials through local environment files or the deployment's secret storage, rather than pasting them into prompts.

## Coverage

- REST and WPGraphQL discovery, pagination, permissions, and custom endpoints
- CPTs, taxonomies, ACF field models, relationships, and schema promotion
- Gutenberg/rendered HTML, media, routing, and existing plugin behavior
- Editor previews, drafts, cache boundaries, webhooks, and content freshness
- Private form submissions and constrained authenticated writes
- Headless metadata, canonicals, sitemaps, and URL migration
- Architecture audits and measured React/CMS performance improvements
- Deployment verification with the selected hosting provider

This is an instruction skill, not a WordPress plugin or application starter. It does not install a CMS, buy hosting, or grant access to live systems by itself. It does not cover ordinary PHP theme work without a separate frontend.

## Structure

```text
skills/headless-wordpress/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── architecture-review.md
    ├── content-api.md
    ├── nextjs-editorial.md
    ├── performance.md
    ├── private-writes.md
    └── sources.md
```

The entry point loads focused references as needed. `agents/openai.yaml` supplies Codex UI metadata; the shared instructions do not depend on it.

## Sources and maintenance

Developed from LearnWebCode / Brad Schiff's [Practical Guide to Headless WordPress (Next.js)](https://www.youtube.com/watch?v=CUiHnde9BmE), current primary documentation, and a review of six relevant community skills. See [source credits and consolidation decisions](skills/headless-wordpress/references/sources.md) for pinned upstream revisions and the guidance retained or adapted.

The skill preserves the tutorial's REST-based starting point and adds production guidance for access boundaries, editor previews, content contracts, and caching. Framework and plugin behavior should always be checked against the project's installed versions.

Skill structure and local reference links have been validated. Cross-agent behavioral testing has not yet been completed; format compatibility does not imply identical results across models or agents.

## License

[MIT](LICENSE). Linked upstream projects and documentation retain their respective licenses. The instructions are a selective synthesis; upstream code, scripts, and templates are not bundled.
