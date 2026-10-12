# Content API and rendering

Read for REST/GraphQL setup, content modeling, or route/rendering work.

## REST contract

Discover the namespace, routes, supported query arguments, and custom post type `rest_base` from the API or configuration. For a public custom type, verify `show_in_rest` and its controller. Do not turn a private type public to fix an access error. WordPress admin uses REST; disabling it globally can break editing.

For ACF, opt in to REST visibility only for field groups intended for API consumers. Check installed ACF/SCF versions and actual responses independently; the tutorial presents alternatives, not a guarantee of identical APIs. Custom registered metadata can need an explicit schema, `show_in_rest`, capabilities, and post-type support. Use OPTIONS/schema inspection when available.

Inspect relationships and media fields instead of assuming they return expanded objects. Normalize ID, array, and object shapes at the adapter boundary. Handle empty relationships and unpublished/deleted targets. Batch lookups or use embedding where supported. A featured image can be absent; preserve alt text and aspect ratio when it exists.

WordPress REST collections commonly default to ten items. For core endpoints, `per_page` is capped at 100; use `page` and `X-WP-TotalPages`/`X-WP-Total` rather than assuming one request contains all content. Keep user-facing lists paginated, and use bounded traversal for sitemaps and migration inventories. Custom controllers may have different limits. Stable ordering matters if content changes during traversal.

Use `_fields` to reduce payload where appropriate and `_embed` to retrieve supported linked resources; ensure field projection retains required embedding fields. Slug lookup can return an array; distinguish zero matches, ambiguous matches, and API failures. Hierarchical pages need more context than the leaf slug.

## WPGraphQL contract

Check plugin and extension versions, endpoint, schema visibility, and permissions. Common non-plain permalinks expose `/graphql`; inspect the configured endpoint instead of hardcoding it universally. Use GraphiQL or schema introspection where available.

Use variables for dynamic values, bounded connections, `pageInfo`, and cursors. Inspect `errors` even for HTTP 200. ACF requires compatible GraphQL integration and explicit exposure; REST exposure does not imply GraphQL exposure. Menus may need assignment to a menu location to be available publicly. Verify installed authentication support rather than assuming a JWT endpoint exists.

Resolve content by URI when hierarchy matters. Preserve database IDs and GraphQL IDs distinctly. Do not assume REST and GraphQL responses can share raw types.

## Content model and editor

Keep durable custom types and business rules in a site plugin or mu-plugin when appropriate so they survive theme replacement. Keep presentation-specific editor restrictions with the relevant presentation configuration. The video's small classic theme is a reference example, not a required architecture.

Choose allowed blocks with the editor and existing content in mind. Inventory current block types before restricting them. Do not globally remove patterns or blocks merely to simplify frontend rendering.

For rendered HTML, add deliberate typography, list, table, figure, caption, and responsive embed styles. Review the trust boundary: trusted WordPress authors may have capabilities that allow markup unsuitable for the separate frontend. Use maintained sanitization with an explicit policy; do not implement an HTML sanitizer with regex. Validate URL schemes and allowed embed origins.

For structured rendering, map supported blocks to frontend components and specify a visible fallback or safe supported HTML fallback for unknown blocks. Include reusable/synced patterns, dynamic blocks, nested blocks, and plugin blocks in the inventory when present. Do not assume the REST API supplies parsed blocks in the desired shape.

Rewrite CMS-origin internal links through a route mapper while preserving external links, media URLs, query parameters, and fragments. Avoid broad string replacement. Handle root page, nested pages, taxonomies, pagination, and locale prefixes as needed. Preserve existing URLs during migration or provide explicit redirects.

## Model boundaries and schema promotion

Use a CPT for an entity with its own publishing lifecycle, reuse, or canonical route; use taxonomies for shared classification/filtering; use fields for properties attached to an entity. Keep truly global settings distinct from per-page or locale-specific data. Repeaters suit short parent-owned lists; independently searchable or reusable records usually deserve entities. Keep flexible-content layouts bounded to supported frontend components.

Inventory existing field names, keys, formats, defaults, and location rules before changing them. Promote ACF definitions through the project's reproducible mechanism: Local JSON, PHP registration, or a documented export/import path. Verify configured save/load paths and sync behavior. Do not label a database-managed model unsafe solely because it lacks Local JSON, but identify environment drift and whether a fresh install can recreate it. Preserve stable keys and names when importing; plan compatibility, backfill, and rollout order for renames or removals. Verify staging and production expose the same contract before frontend rollout.

## Custom WordPress endpoints

Register routes on `rest_api_init` under a vendor/version namespace. Define allowed methods, argument types, required fields, validation, sanitization, response shape, and pagination. Read parameters from `WP_REST_Request` rather than unchecked globals. Callback signatures must match WordPress's invocation; wrap PHP built-ins when extra callback arguments would be invalid.

Supply a `permission_callback` for every endpoint. For privileged operations, check the relevant capability and resource ownership/site context; merely being logged in is insufficient. An explicitly public read may use a permissive callback. An intentionally anonymous form endpoint needs its own constrained write policy and abuse controls; do not confuse it with unrestricted content mutation. Return structured `WP_Error`/`WP_REST_Response` results with appropriate status codes instead of printing JSON or leaking exception details.

Preserve response compatibility for existing consumers. Introduce a migration path or versioned contract for breaking changes and verify denial paths alongside successful requests.

## GraphQL schema extensions

Expose stable domain concepts, with deliberate nullability and explicit variants, rather than temporary storage keys. Prefer paginated connections where relationships can grow. Reuse query fragments when they help keep route contracts consistent. Inspect custom resolvers for capability-aware visibility and field-level exposure; the frontend hiding a field provides no protection.

Profile nested resolvers and batch/preload only where needed. WordPress metadata caching may already remove repeated database reads, so verify query counts before claiming an N+1 database problem. Persisted/allowlisted queries can help bound query shapes and cache keys when supported, but do not replace permissions, pagination, or resolver optimization. Keep draft and public query execution/cache scopes separate.
