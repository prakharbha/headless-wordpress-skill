# Architecture review and migration intake

Read for inherited projects, headless audits, or migrations. Keep inspection within the requested surface.

## Map the running system

Identify WordPress custom plugins/mu-plugins, active theme, block registrations, ACF definitions, REST routes, WPGraphQL extensions, frontend apps, build scripts, and deploy configuration. Distinguish source, vendor code, and generated assets. Note Bedrock/Composer layouts, multisite, locale plugins, commerce, and page builders when present; they change routing, permissions, and deployment assumptions. A file existing in a repo does not prove that plugin or theme is active in production.

Record CMS/frontend origins, API choice, content ownership, credential boundary, preview entry point, update propagation, and relevant dependencies. Name missing runtime evidence explicitly. For create/connect/update hosting work, inspect existing installation/deployment records and reuse the intended site; avoid duplicate provisioning. Record non-secret site identifiers only when useful for subsequent work, using the project's existing convention.

## Choose the highest-value checks

- Verify anonymous/public versus editor/private data access, including custom resolvers and endpoints.
- Trace one content object from its model through API response, route, metadata, and cache dependencies.
- Trace one editorial update, preview, and implemented write through their permission and freshness paths.
- Inspect collection pagination, nested resolver cost, response size, and taxonomy versus metadata filtering.
- Check reproducibility of schema, plugin/frontend versions, generated artifacts, secret configuration, and deployment.

Prioritize demonstrated exposure, destructive behavior, broken publishing, missing content, and performance bottlenecks. Do not treat missing tests, uncommon structure, or an unfamiliar deployment alone as proof of a critical defect.

## Report actionable findings

Lead with the architecture and major outcome. For each actionable finding, give severity proportional to verified impact, affected file/line or runtime surface, trigger, evidence, consequence, and a concrete correction. Distinguish demonstrated failures from unverified concerns. If no actionable defects are found, say so and name meaningful coverage limits. Avoid manufacturing a required chain of companion skills.

## Migration-specific work

Inventory shortcodes, builder markup, theme/plugin scripts, interactive blocks, custom search, feeds, comments, forms, and commerce behavior used by real content. These do not transfer just because API reads work. Plan replacements only for required features.

For content model changes, stage additive backend support before frontend adoption where practical, preserve old consumers until migrated, and backfill data with counts/validation. Evaluate field and GraphQL contract changes together. For multisite or multilingual systems, carry site/locale context through queries, public routes, permissions, and cache keys. Keep commerce scope explicit; ordinary blog/contact guidance is not a checkout implementation.

Use a preview/staging environment and reversible rollout for substantial migrations. Keep a rollback target and validate URLs, data counts, and editor workflows before an authorized cutover. Do not perform database migrations or DNS cutover solely because an audit recommends them.
