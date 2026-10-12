---
name: headless-wordpress
license: MIT
description: Build, migrate, audit, and troubleshoot headless WordPress sites with a separate frontend, especially Next.js using the REST API or WPGraphQL. Use for CMS data modeling, frontend integration, previews, caching, private form submissions, and headless SEO; exclude ordinary PHP theme edits without a decoupled frontend.
---

# Headless WordPress

Keep WordPress as the editorial backend and build a separate frontend around its actual content contract. Deliver working editor and visitor workflows, not just a page that fetches posts.

## Select the working mode

For new builds, establish the content contract and implement one working slice. For migrations, inventory existing URLs and editor/plugin behavior before replacing presentation. For audits, read [references/architecture-review.md](references/architecture-review.md) and inspect the requested surface before changing code. For troubleshooting, reproduce the failing request or editor action and trace it through the CMS, API, frontend, and cache layers. Keep narrow fixes scoped to the demonstrated fault.

These references form one self-contained skill; companion skills or provider connectors are optional, not prerequisites.

## Establish the contract

Inspect the repository, package versions, router, hosting configuration, and available WordPress schema before choosing an implementation. Preserve existing stack choices. For a new Next.js frontend, prefer App Router, server-rendered content, and TypeScript unless the user chooses otherwise. Match framework APIs to the installed version and cache configuration; verify current official documentation before copying tutorial code.

Identify the CMS and public origins, content types, route hierarchy, custom fields, media, menus, editorial preview needs, freshness requirements, and any writes. Ask only for missing information that blocks the requested work. Public content reads usually need no credential. Do not ask users to paste secrets into chat.

Choose the smallest API surface that fits:

- REST is the starting point for the supplied LearnWebCode tutorial and straightforward posts, pages, custom types, and forms. Inspect `/wp-json/`, type definitions, and sample responses; never infer an endpoint from an admin label.
- Keep WPGraphQL when already installed or when its schema and nested queries suit the project. Inspect its actual schema before authoring queries. Do not install it merely because another tutorial does.
- Preserve another frontend framework when requested; adapt routing, preview, and cache semantics instead of imposing Next.js.

Read [references/content-api.md](references/content-api.md) for API setup, custom fields, Gutenberg rendering, relationships, and routing. Use its modeling guidance when designing or changing CPTs, taxonomies, ACF fields, or GraphQL schemas.

## Build a complete vertical slice

Start with one real list and detail page or one representative CMS page. Separate the API adapter from presentation and normalize API responses into stable frontend models. Extend only after confirming the slice works.

Use server-side fetching for primary content. Keep interactive components small. Handle timeouts, unsuccessful HTTP responses, GraphQL errors inside successful HTTP responses, absent fields, and missing records explicitly. A CMS outage must not masquerade as an empty successful result or a permanent 404. Paginate collections; bound relationship lookups and avoid N+1 queries.

Map WordPress paths to public frontend routes deliberately, including hierarchical pages, archives, pagination, and slug collisions. For Next.js internal navigation use `Link`; use ordinary anchors where appropriate for external destinations or downloads. Missing published content returns a real 404.

Choose rendered HTML or structured block rendering based on the editor's needs. Verify representative content, theme-dependent blocks, styles, embeds, image spacing, and links. WordPress theme/plugin frontend behavior does not automatically move to the new frontend. Define a sanitization and allowed-embed policy before rendering HTML. Preserve useful content rather than stripping it indiscriminately.

Images need configured remote hosts, meaningful alt text, correct dimensions or aspect ratio, responsive sizing, and missing-image handling. Verify the real browser output at the actual running URL and port, including mobile layout and keyboard navigation.

For performance work or a new React frontend, read [references/performance.md](references/performance.md). Prioritize request waterfalls, resolver cost, and client payload size before small rendering optimizations.

## Preserve editorial behavior and freshness

Define public cache behavior explicitly and keep drafts and personalized data out of shared caches. Use framework-native caching compatible with the installed router and options. A five-minute revalidation value is a freshness policy, not a promise that every reader sees changes within five minutes.

Read [references/nextjs-editorial.md](references/nextjs-editorial.md) when implementing caching, revalidation, previews, SEO, migration, or deployment. Implement authenticated previews when requested, with private responses, valid CMS permissions, a safe local redirect, a preview indicator, and an exit flow. Draft Mode alone does not authorize reading WordPress drafts.

For CMS-triggered invalidation, authenticate webhooks, derive affected paths/tags from validated content identifiers, and include changed archives, old slugs, deletions, unpublishing, and shared navigation where relevant. Decide whether stale-while-revalidate is acceptable; private-content removal can require immediate expiry across cache layers.

## Handle writes only when needed

Read [references/private-writes.md](references/private-writes.md) before implementing contact submissions or another authenticated write. Keep credentials in server-only modules and ignored local environment files or deployment secret storage. Never expose application passwords in public environment variables, browser bundles, URLs, logs, or returned error objects.

Use a dedicated WordPress account with only the capabilities required. For contacts stored as posts, fix the target type and `private` status on the server and verify anonymous readers cannot retrieve them. A private post is capability-protected, not necessarily limited to administrators.

Validate input and prevent abuse on the server. Do not treat the tutorial's shared-answer question as production bot protection. Public read routes must remain independent of privileged write credentials. Custom WordPress endpoints also need explicit permission callbacks and request/response schemas; see [references/content-api.md](references/content-api.md). A server function is an exposed entry point, so enforce its policy inside the handler.

## Verify and hand over

Scale validation to the requested change. For a new integration, exercise real pagination, one detail route, missing media/custom fields, a missing record, and a CMS failure. When implemented, verify editor previews, content updates, invalid webhook requests, and private form submissions. Use synthetic contact data and the appropriate authorized test environment.

Run the repository's applicable type, lint, test, and production build checks. Use behavioral tests for pagination, access boundaries, and invalidation where they materially reduce risk. A successful build does not establish that live WordPress permissions or content updates work.

For deployment work, respect the user's chosen host and authorization. Keep WordPress and frontend deployables distinct; configure runtime secrets separately. Confirm deployed public URLs, editor behavior, and cache operation before reporting success. Explain what changed, what was verified, remaining blockers, and how an editor publishes or previews. Do not inherit any unrelated repository's publishing or promotion workflow.

## Source use

Read [references/sources.md](references/sources.md) for the supplied video, its verified takeaways, authoritative supplemental articles, and the reviewed community skills with their consolidation decisions. Treat tutorials as examples with dates and version context. Prefer current official documentation for implementation decisions and never represent sponsorship or popularity as technical evidence.
