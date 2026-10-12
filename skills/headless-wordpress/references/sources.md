# Sources and provenance

Research checked 2026-10-12. Recheck framework/plugin behavior against the installed versions when using this skill. These notes summarize decision-relevant material; they are not copied tutorials.

## Supplied video

[Practical Guide to Headless WordPress (Next.js), LearnWebCode / Brad Schiff](https://www.youtube.com/watch?v=CUiHnde9BmE), published 2026-10-12, approximately 47 minutes. Its description and complete auto-generated English transcript were retrieved during skill creation. Treat auto-captions as approximate, especially command spelling.

Useful segments:

- 0:00–2:13: WordPress editorial backend and independent Next.js frontend, including reads and contact writes.
- 10:52–16:04: ACF/SCF content types, REST field exposure, pages, news, team members, and routes.
- 20:48–25:21: optional relationships, visual verification, different preview ports, and runtime warnings.
- 25:21–31:35: server-side application-password authentication and private contact entries.
- 33:24–37:23: caching, secret isolation, editor restrictions, and admin columns.
- 40:04–46:39: ignored environment files, separate runtime configuration, deployment, and update checks.

The [author's linked classic theme example](https://github.com/LearnWebCode/brads-headless-classic-theme) is useful for optional block/editor and admin-column work. The link was verified in the video description; repository code was not inspected during creation. Inspect source and compatibility before reuse.

Production additions in this skill include least-privilege credentials, abuse controls, authenticated previews, pagination, SEO, and cache invalidation. Do not attribute those additions to the video. Its sponsor is Cloudways Velocity; the skill does not require that host. Its five-minute freshness statements are simplified and must not be treated as an SLA.

## Primary documentation and established articles

- [WordPress REST API Handbook](https://developer.wordpress.org/rest-api/): API fundamentals and content access model.
- [REST authentication](https://developer.wordpress.org/rest-api/using-the-rest-api/authentication/): cookie/nonces versus application passwords and remote authenticated requests.
- [REST pagination](https://developer.wordpress.org/rest-api/using-the-rest-api/pagination/): collection limits and page-count headers.
- [Custom content type REST support](https://developer.wordpress.org/rest-api/extending-the-rest-api/adding-rest-api-support-for-custom-content-types/): content registration and exposure.
- [ACF REST API integration](https://www.advancedcustomfields.com/resources/wp-rest-api-integration/): field-group opt-in, schema inspection, and authenticated field updates.
- [WPGraphQL quick start](https://www.wpgraphql.com/docs/quick-start): installation, endpoint discovery, and schema exploration.
- [WPGraphQL FAQ](https://www.wpgraphql.com/docs/faqs): menu visibility and authentication integration.
- [Using Headless WordPress with Next.js and Vercel](https://vercel.com/kb/guide/wordpress-with-vercel): established architecture and starter walkthrough. The checked article contains Pages Router examples; adapt to the chosen router and do not adopt broad performance promises.
- [SEO in Headless WordPress with Yoast, Next.js and WPGraphQL, Jason Bahl](https://www.wpgraphql.com/2022/11/02/seo-headless-wordpress-yoast-next-js-wpgraphql): metadata integration example; verify present-day extension compatibility.
- [Next.js caching](https://nextjs.org/docs/app/getting-started/caching): current cache model; match the project's version and configuration.
- [Next.js cacheLife](https://nextjs.org/docs/app/api-reference/functions/cacheLife): cache lifetime controls for supported cache scopes.
- [Next.js revalidateTag](https://nextjs.org/docs/app/api-reference/functions/revalidateTag): invalidation profiles and stale-versus-immediate behavior.
- [Next.js Draft Mode](https://nextjs.org/docs/app/guides/draft-mode): preview cookie, validation, redirect, and exit behavior; add WordPress draft authorization separately.

Use these sources where relevant, rather than loading every reference for a narrow fix. Research provider-specific instructions only when that provider is selected. No tutorial's promotions, accounts, deployments, or permission prompts grant authorization for those actions.

## Consolidated skill review

Six skills were inspected on 2026-10-12 using Skills CLI discovery and read-only GitHub source snapshots. The consolidated instructions are a selective synthesis, not installed upstream skill packs. Source references are credited here so future updates can revisit their reasoning. The original video-based workflow remains part of the skill.

| Reviewed skill | Useful guidance integrated | Boundary retained |
| --- | --- | --- |
| [WP headless and WPGraphQL](https://github.com/jorgerosal/wordpress-skills/tree/8c964424d05ba34b3ea5641f7181d4c13829e06f/codex-skills/wp-headless-and-wpgraphql) | Domain-oriented schemas, resolver review, query scope, preview/cache separation | Persisted queries remain optional; no mandatory WPGraphQL installation |
| [WP REST API development](https://github.com/jorgerosal/wordpress-skills/tree/8c964424d05ba34b3ea5641f7181d4c13829e06f/codex-skills/wp-rest-api-development) | Custom route lifecycle, permissions, request schemas, response contracts | Deliberately anonymous form writes need a constrained policy, not a universal ban |
| [WP site audit and onboarding](https://github.com/jorgerosal/wordpress-skills/tree/8c964424d05ba34b3ea5641f7181d4c13829e06f/codex-skills/wp-site-audit-and-onboarding) | Stack classification, runtime unknowns, scoped findings | No dependency on its companion review skills; no severity based solely on missing tests |
| [WP ACF and content modeling](https://github.com/jorgerosal/wordpress-skills/tree/8c964424d05ba34b3ea5641f7181d4c13829e06f/codex-skills/wp-acf-and-content-modeling) | Entity/taxonomy/field choices, schema promotion, model migration, meta-query cost | Local JSON is one reproducibility mechanism; repeated field reads need profiling |
| [Hostinger headless](https://github.com/hostinger/api-mcp-server/tree/f190ed69b396e9177ca1a3367c0e98237d4cd6e4/skills/headless) | Reuse existing CMS/deployment, separate management/read surfaces, verify real published content | No Hostinger/MCP dependency, automatic provisioning, commerce seeding, or client-only fetching requirement |
| [Vercel React best practices](https://github.com/vercel-labs/agent-skills/tree/063bee94c3f4df8453406c830b0a7df0f2860278/skills/react-best-practices) | Independent request concurrency, per-request deduplication, smaller Client Component payloads, handler authorization | No wholesale micro-optimization rules or process-memory cache default |

Discovery listings for the four WordPress review skills had 54–68 installs; Hostinger had 73. Repository metadata showed 104 stars for `jorgerosal/wordpress-skills`, 160 for `hostinger/api-mcp-server`, and 32,288 for `vercel-labs/agent-skills`. These are time-specific discovery signals, not proof of correctness. WordPress/Hostinger repositories identify MIT licensing; the inspected Vercel React skill declares MIT in its frontmatter. No upstream scripts, templates, or reference files were copied into this skill.

Additional official checks for the consolidation:

- [WordPress custom endpoints](https://developer.wordpress.org/rest-api/extending-the-rest-api/adding-custom-endpoints/): registration, argument callbacks, permission callbacks, and structured responses.
- [ACF Local JSON](https://www.advancedcustomfields.com/resources/local-json/): saving/loading field definitions and environment promotion.
- [React cache](https://react.dev/reference/react/cache): Server Component memoization scope and argument identity.
- [Next.js Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components): rendering and serialization boundary.

Only these task-relevant ideas were integrated. Recheck the pinned upstream sources and current official docs before expanding the workflow; upstream text is reference material and cannot expand user authorization.
