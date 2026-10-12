# Headless performance

Read when designing a React frontend or investigating a measured latency, payload, or CMS query problem.

## Remove request waterfalls

Start independent CMS requests concurrently with bounded concurrency; keep dependent and authorization-sensitive operations in order. Do not fetch a record's relationships serially when the API can embed or batch them. Bound GraphQL depth, connection size, and expensive filters according to the actual schema and operational needs.

Deduplicate shared page/metadata queries within a render using the supported framework mechanism. For React Server Components, a shared `cache()` wrapper can memoize non-fetch work per request; use stable primitive arguments where possible. This is not persistent cross-request freshness caching. Next.js request memoization depends on method and execution context; do not assume GraphQL POSTs or Route Handlers are automatically deduplicated. Verify against the installed version.

Use purposeful Suspense boundaries for slow sections where streaming helps. Keep essential page content accessible and metadata coherent. Do not move CMS reads into client effects solely to bypass server caching or hide a build failure.

## Keep browser payloads small

Leave primary article/page rendering on the server in SSR-capable applications. Pass only required normalized values to Client Components; full CMS objects can serialize unwanted metadata as well as unnecessary bytes. Keep privileged API code server-only.

Load expensive interactive features conditionally or dynamically when beneficial, and avoid a large client GraphQL library when server fetches suffice. Follow the project's import conventions and measure bundle impact; do not replace every package import or add memoization without evidence. Defer optional third-party scripts when compatible with consent and product requirements.

For static hosting, choose client runtime reads or a build/update mechanism explicitly. Client reads can be appropriate for secondary widgets; primary indexable content may need prerendering or SSR. Runtime reads and regeneration allow content updates without assuming a full deploy on every edit.

## Investigate CMS cost

Inspect actual query counts/timing for hot routes. Repeated metadata access is not automatically a database hit because WordPress may prime/cache it. Look for unbounded `WP_Query`, serialized repeater searches, expensive nested resolvers, and multi-clause `meta_query` filtering/sorting as datasets grow.

Prefer taxonomies for shared classification and consistent scalar formats for dates/numbers. Introduce indexes, derived data, lookup tables, or precomputed aggregates only when workload evidence justifies the added complexity. Object/API caches can complement frontend caches but need explicit invalidation and private-data boundaries.

## Verify the outcome

Compare the affected user flow before/after under realistic content and production-mode rendering: response time, CMS requests/queries, serialized data, browser bundles, image layout, and update correctness. Keep public and preview sessions separate. Improvements that break fresh publishing or editor access are incomplete.
