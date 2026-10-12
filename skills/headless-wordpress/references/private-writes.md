# Private form submissions and authenticated writes

Read before implementing contact forms, mutations, or draft-authenticated access.

## Server boundary

Public form -> frontend Server Action or Route Handler -> fixed WordPress endpoint.

Keep the WordPress application password in a server-only module and secret storage. Application passwords authenticate as their WordPress user and inherit that user's capabilities; they are not automatically endpoint-scoped. Prefer a dedicated service user with narrowly defined capabilities for the target custom post type. Verify both the create and private-post capabilities actually required. Use HTTPS outside explicitly local development.

Use placeholder names in `.env.example`, for example `WP_URL`, `WP_USERNAME`, and `WP_APPLICATION_PASSWORD`. Public reads do not need the privileged credential. Ensure real local env files are ignored, deployment secrets are configured separately, and client component imports cannot include the privileged adapter. A `NEXT_PUBLIC_` prefix is never suitable for a secret.

## Input and abuse controls

Validate required fields, email format, lengths, body size, and accepted content on the server. Fix post type, author policy, and `status: private` on the server; callers cannot choose arbitrary endpoints, capabilities, or publication status. Match the actual ACF/meta schema for write payloads. Reject unexpected privileged fields.

Apply appropriate origin/CSRF controls for the selected handler/session model. CORS alone is not authentication or CSRF protection. Use rate limits that work across the hosting topology; add honeypot/timing or server-verified bot protection when warranted. A shared trivia answer, even stored in an environment variable, is guessable and replayable.

Handle duplicate clicks with pending UI and a defined idempotency strategy when needed. Do not blindly retry a non-idempotent POST after a timeout: WordPress may have saved it even though the response was lost. Return useful generic errors to visitors; record bounded server diagnostics without passwords, Authorization headers, raw submissions, or other sensitive fields.

## Storage and verification

Set private status explicitly and confirm the API response and saved record. Private visibility is governed by capabilities; do not promise only administrators can read it without verifying roles and custom controllers.

Check anonymous REST and, if installed, GraphQL access to both item and collection. Ensure contact entries cannot appear in public lists, search, sitemaps, feeds, cached responses, or frontend source. A custom endpoint or plugin can accidentally bypass core visibility protections.

Use synthetic data in the authorized test environment. Verify valid submission, invalid input, abuse rejection, WordPress denial, and ambiguous timeout behavior as relevant. Confirm the browser never receives the credential. Discuss retention and deletion only to the extent needed for the requested collection workflow; do not collect fields merely because the tutorial does.

Do not create credentials, widen roles, or submit test records to a live external system unless the user's task authorizes those actions. Prepare the implementation and state any remaining live-access dependency clearly.

## Server Actions and protected mutations

Check caller policy inside each action or route, even if the page, layout, or middleware checks access. Protected editor/account actions require authentication plus authorization for the target record. An anonymous contact form intentionally permits anonymous callers, but must constrain what they can create and apply validation/abuse controls there. Do not force visitor login merely because a server-side WordPress credential performs the bounded write.
