# Next.js editorial integration

Read for cache behavior, preview, metadata, migration, and deployment work.

## Version-aware caching

Inspect the installed Next.js version, App/Pages Router, `cacheComponents`, runtime, and host. Read the matching official documentation. Do not mix Pages Router `getStaticProps` examples with App Router code, or combine legacy route options with Cache Components without checking compatibility.

For existing fetch-based caching, set cache/revalidation/tag behavior explicitly where supported. For Cache Components, use compatible `use cache`, `cacheLife`, and `cacheTag` boundaries and keep request-specific input out of public cached functions. Resolve request data and place uncached work under suitable Suspense boundaries according to the installed version. Do not hide framework warnings without addressing their cause.

Agree on freshness requirements before choosing TTL or webhooks. Distinguish server data/render caches, CDN responses, client navigation caches, and image caches. Timed revalidation may happen after a request and may serve stale content during refresh; it is not a strict publication deadline. Newly created routes and failures also need defined behavior.

Authenticated preview, drafts, contact data, and personalized requests must not enter public caches. Confirm this at the CDN and application levels. An outage fallback should serve previously safe public content only where supported; do not invent successful empty data and cache it.

## CMS webhook invalidation

Accept authenticated, bounded requests from the configured CMS. Prefer signed payloads with replay controls or a suitable secret mechanism; keep secrets out of query strings and routine logs. Validate the body and map content IDs to known tags/paths. Do not expose an arbitrary public path/tag purge service.

Cover publish/update, slug changes, trash/delete, public-to-private/draft transitions, taxonomy changes, and shared menus when relevant. Invalidate detail pages, old paths, relevant lists, sitemap data, and metadata that derive from the changed record. Skip autosave/revision noise where appropriate. Ensure scheduled publications can deliver their event reliably.

Check current `revalidateTag` semantics for the installed version. The `max` profile provides stale-while-revalidate behavior; immediate expiry is a different choice. Current docs show `{ expire: 0 }` for immediate expiry from a webhook and `updateTag` for supported Server Action use. Do not assume an invalidation call eagerly rebuilds every page or purges an external CDN.

Never automatically retry an ambiguous write/event in a way that duplicates side effects. Record delivery outcomes and use a bounded retry policy where implemented.

## Preview

Separate enabling frontend preview from WordPress authorization to fetch drafts or revisions. Validate a short-lived token or authenticated editor flow before granting preview access. A permanently reusable URL containing a shared secret is a basic integration pattern with exposure risks, not a complete editor identity system.

Resolve the requested record using authorized server-side CMS access and redirect only to a validated same-origin local path. Reject external/protocol-relative redirects. Support unpublished content that is absent from public slug queries. Keep credentials and draft data private, avoid indexing, display a preview indicator, and provide an explicit exit action. Check cookie behavior across CMS/frontend domains.

Test latest saved draft, revision/autosave requirements if requested, published visitor behavior in a separate session, invalid/expired tokens, and preview exit. Use the installed version's async `draftMode`, params, and request APIs as applicable; do not copy obsolete signatures.

## Headless SEO and migration

The frontend owns emitted metadata. WordPress SEO plugins can supply data only when an appropriate API exposes it. Inspect Yoast/Rank Math integration and actual fields rather than inventing `yoast_head_json` or GraphQL SEO fields.

Map title, description, canonical, robots, social images, and appropriate structured data into one frontend metadata strategy. Use public-domain canonicals; avoid duplicate metadata and incorrect CMS-origin URLs. Preview and staging responses should not be indexable. Generate sitemaps from all public content with pagination and real timestamps; exclude private and preview routes. Return valid 404s and redirects, not soft 404s.

When migrating, inventory routes, trailing slashes, locale paths, media, archive pagination, feeds, search, comments, forms, and redirects as applicable. Preserve high-value URLs and editor workflows. Do not redirect the entire WordPress origin blindly: admin, login, APIs, uploads, and required preview/auth callbacks still need to function.

## Deployment and troubleshooting

Use a runtime capable of the features selected. Static export alone cannot provide runtime authenticated form writes, Draft Mode, or on-demand regeneration; provide an explicit external runtime design if static hosting is chosen.

Verify WordPress reachability from the deployed frontend, HTTPS, environment values, image host rules, and authentication header forwarding. Configure CMS and frontend separately. Keep real `.env` files ignored and commit only placeholder examples. Self-hosted multi-instance deployments may need a shared cache/invalidation strategy; do not assume process memory is globally shared.

For stale styling, first confirm URL/port, process, changed files, CSS import and Tailwind scanning, then inspect browser errors and production output. The video's agent and browser viewed different ports; validate the actual site instead of repeatedly rebuilding without a diagnosis.

For missing API data, distinguish wrong route, exposure configuration, pagination, authentication/capability failures, and schema mismatch. For update delays, inspect cache policy and webhook delivery. For 401/403 writes, inspect the credential and capability path without printing secrets; do not retry indefinitely or fall back to a more privileged account.
