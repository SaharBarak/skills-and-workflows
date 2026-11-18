# PostHog analytics contract

The rules every site follows when it sends data to PostHog. Each repo implements them in its own analytics layer and records its event table in its own `docs/analytics.md`; this file is the part they share.

## Context

Several independent sites report to PostHog Cloud US. The free plan allows one project per organization, so the studio sites share one project and are told apart by a property. Beneath has its own organization and project.

| Site slug | Repo | Domain | Class | PostHog project |
|---|---|---|---|---|
| `saharbarak-dev` | portfolio | saharbarak.dev | site | shared (281417) |
| `pleiad` | omnis | pleiad.io | app | shared (281417) |
| `wessley` | wessley-web | wessley.ai | app | shared (281417) |
| `shaya` | shaya-web | shaya-art.com | site | shared (281417) |
| `twocircles` | twocirclestudio-web | twocirclestudios.com | site | shared (281417) |
| `karen` | karen-cli-web | karencli.dev | site | shared (281417) |
| `beneath` | beneath | usebeneath.com | app | own (579127) |

A slug never changes once events carry it. A new site adds a row here before it sends anything.

## Tenancy inside the shared project

One project is one namespace for events, persons, flags, replay allowance and settings. `site` separates analysis; it does not isolate. The rules below keep the sites from colliding.

- **Reserved properties.** Every event carries `site` (the slug above) and `env` (`production`, `preview` or `development`). The client registers them as super properties at init; the server attaches them to every capture. The wrapper sets them last, so an event's own properties cannot overwrite them, and the event types do not accept those keys.
- **`env` is decided at build or deploy time**, never from the hostname at runtime. Order: `NEXT_PUBLIC_POSTHOG_ENV` (or the framework's equivalent) if set; else the host's own variable (`NEXT_PUBLIC_VERCEL_ENV`); else `development` when the build is a dev build, `production` otherwise. Cloudflare preview deploys set the variable explicitly.
- **`$host` stays untouched.** Web analytics filters on it; `site` is additional.
- **Identified ids are namespaced:** `<site>:user:<auth provider id>`. One function per repo produces it and both client and server call that function. Email, name and plan are person properties, never the id.
- **Flag keys are prefixed by site** (`pleiad-new-checkout`) and every flag has a `site` condition.
- **Shared event names mean the same thing everywhere.** The names in "Common events" below keep the same meaning and property types on every site. Everything else is site-specific and listed in that repo's spec.
- **Project-level settings are shared.** Replay allowance, test-account filters and autocapture settings affect every site; change them here first, then in PostHog.

## Site classes

| | `site` | `app` |
|---|---|---|
| Who | Marketing, portfolio and content sites where analytics does not depend on login | Products with accounts |
| PostHog mode | `cookieless_mode: 'always'`, `person_profiles: 'never'` | `cookieless_mode: 'on_reject'`, `person_profiles: 'identified_only'` |
| Storage | None | Cookies only after acceptance |
| Banner | None for PostHog | Ask-first in EEA, UK and CH; GPC or DNT counts as refusal |
| `identify()` | Never | After login, with the namespaced id; `reset()` on logout |
| Session replay | Off (unavailable without storage) | On after acceptance; inputs masked; sensitive regions carry `ph-no-capture` |

On an `app`, the consent decision governs every non-essential tracker on that site, not only PostHog. A banner that controls one tracker while others load regardless is misleading. Beneath's `consent.ts` is the reference behaviour. PostHog's `on_reject` already holds capture while the visitor is undecided, so a repo adds its own queue only for business events that must survive the decision.

Server events on an `app` follow the same decision as the browser:

- The consent module writes the visitor's answer, never the region default, into a first-party preference cookie: host-only, `Path=/`, `SameSite=Lax` (it has to reach the OAuth callback), and `Secure` on https. The server re-runs the browser's own gate on that answer plus the request's country and GPC or DNT headers, so a moved visitor or a newly sent signal is never stale.
- A request without an answer carries no identity.
- An event with no visitor request, such as a payment webhook, takes the answer recorded when the visitor started the action, or the account's stored answer. Wessley carries it in the Stripe Checkout Session's metadata; Beneath stores it on the account; pleiad has neither yet, so its billing webhook events are anonymous.
- Allowed: identified, as before. Refused or unknown: on pleiad and wessley, sent anonymously, as posthog-js counts a refusal. Beneath withholds the event when its stored refusal (`analytics_refused`, per workspace or report) says so. That means a one-off id, `$process_person_profile: false`, no session or URL, and only properties on a per-event allowlist. A provider event id that only deduplicates retries is kept.

## Configuration

Client (`posthog-js`):

- Initialise once, in the place the framework's PostHog docs name (`instrumentation-client.ts` on Next.js 15.3+).
- `defaults: '2026-08-30'`. Pageviews, including SPA navigations, come from the defaults; a repo does not also fire `$pageview` by hand.
- `capture_exceptions: true`. Framework error boundaries call `captureException` once; nothing else reports the same error.
- Autocapture stays on. Typed events cover the funnel and money actions; autocapture covers the long tail.
- A `before_send` hook redacts tokens and secrets from URLs and properties where a site puts them in paths or query strings.

Missing configuration:

- The key and host are optional. A build and a boot succeed with neither set.
- Without a key the layer is a no-op in production. In development the browser raises, and server code logs, this message: `<VAR> variable required by PostHog is missing or un-configured, this causes events to be silently missed. This error stops appearing once <VAR> is configured`.
- Keys live in env files and the host's secret store. They are never written into source, docs or reports.

Variable names: a repo that already has `NEXT_PUBLIC_POSTHOG_KEY` and `NEXT_PUBLIC_POSTHOG_HOST` keeps them, because its CI and hosting already set them. A new integration uses the names in PostHog's docs (`NEXT_PUBLIC_POSTHOG_PROJECT_TOKEN`, `NEXT_PUBLIC_POSTHOG_HOST`; `VITE_` prefix on Vite).

Server:

- One client per process, reused.
- The handler does not return before the event is sent. On Vercel Node functions: `await client.flush()`. On Cloudflare Workers: `ctx.waitUntil(client.captureImmediate(...))`.
- Analytics never fails the business action. A send error is caught and logged; a webhook that already did its work still returns success.
- Events that come from a provider webhook carry a deterministic `uuid` derived from the provider's event id, so a retried webhook does not count twice.
- Server events carry the namespaced id, `site` and `env`. Where the request has them, they also carry the browser's session id and current URL so they join the web session.

## The layer every repo owns

```ts
track(event, properties)          // event must be a key of the repo's EventMap
identify(userId, traits?)         // app class only; applies the namespace
reset()
captureException(error, context?)
```

- Application code imports the layer, never `posthog-js` or `posthog-node` directly. A lint rule (`no-restricted-imports`) or one CI grep enforces it.
- The `EventMap` type is the source of the spec's event table. An undeclared event does not compile.
- Existing event names are kept. New names are `snake_case`, `object_verb`, past tense (`share_created`). Property names are `snake_case`; booleans start `is_` or `has_`. Values are never interpolated into names.
- An existing abstraction is extended, not replaced: Beneath's `lib/analytics`, omnis's `lib/analytics/posthog.ts`, shaya-web's `js/lib/pixel.js`.

## Common events

| Event | Meaning | Sent from | Required properties |
|---|---|---|---|
| `user_signed_up` | An account was created | Server | `method` |
| `user_logged_in` | A session started for an existing account | Server or client | `method` |
| `newsletter_subscribed` | An address joined a list | Server where there is one | `placement` |
| `lead_submitted` | A contact, booking or WhatsApp handoff | Client | `channel`, `placement` |
| `checkout_started` | Checkout opened | Client | `plan` or `product_id` |
| `checkout_completed` | Payment confirmed by the provider | Server only | `plan` or `product_id`, `value`, `currency` |

Signup and payment are counted from the server. `identify` is not a proxy for signup.

## Reverse proxy

Sites send directly to `https://us.i.posthog.com` first, so a failure is attributable. A proxy is added per site afterwards, in this order of preference:

1. Rewrites the repo already has (`/ingest` in portfolio and wessley-web) stay.
2. Cloudflare-hosted sites use a Worker route or Pages Function: no egress cost.
3. PostHog's managed proxy for Vercel-hosted sites, within the plan's two-domain limit; Next rewrites bill replay traffic as data transfer.

A proxy path avoids the words `analytics`, `tracking`, `telemetry`, `posthog` and `ph`. With Next rewrites, `skipTrailingSlashRedirect: true` is set and any middleware matcher excludes the proxy path.

## Project settings (shared project)

- Cookieless tracking enabled, or cookieless events are dropped.
- Exception autocapture enabled.
- Test-account filter, on by default for new insights: host is not localhost, `127.0.0.1`, a `*.vercel.app` or a `*.workers.dev` address, and `env` is neither `development` nor `preview`. A site is reached in production only under its own domain, so a deployment address is always a preview or a smoke test, whatever `env` the build carries.
- One dashboard, "Sites overview", whose tiles break down by `site`: pageviews, daily visitors, the common conversion events and errors. A site-specific funnel is a tile on it, filtered to that site. A site gets its own dashboard only when it outgrows a few tiles.

## Acceptance per repo

Checked on a deployed preview, then one production smoke event, using PostHog's live events and the browser's network panel.

1. With no key: build and boot succeed; development shows the one warning.
2. Exactly one SDK initialisation.
3. A pageview arrives on first load and on an in-app navigation, once each.
4. A named business event arrives once, with the right `site`, `env`, `$host` and URL.
5. A preview event is found when queried and is absent from production dashboards.
6. `app` class: nothing is sent while consent is pending; acceptance enables storage; refusal yields cookieless events with no persistent id.
7. `app` class: login produces the namespaced id; logout resets and the next event has a new anonymous id.
8. One server event arrives before its handler ends.
9. One deliberate client exception is grouped once and carries no secret.
10. `app` class: a replay appears under the right site with inputs masked.

## Sequencing

1. Shared-project settings and this contract.
2. omnis (pilot for the shared project and Supabase identity).
3. portfolio.
4. wessley-web (repoint to the shared project, logout reset, server delivery).
5. shaya-web.
6. twocirclestudio-web, karen-cli-web.
7. Beneath: `defaults`, error tracking and proxy only; its layer is not reshaped.
8. Proxies, per deployment class.
9. Source-map upload for error tracking, per repo.

## Open questions

- Replay on `site`-class sites needs storage and therefore a banner. shaya-web runs paid traffic and may want it.
- GA4, Meta Pixel and the first-party beacon load without a consent decision on the `site`-class sites today.
- karen-cli-web has working sign-in but no reachable backend and no payment that can complete. It moves to the `app` class when the backend is reachable and a Stripe payment can complete and be recorded.

## Second opinions

| Reviewer | Scope | Date | Status |
|---|---|---|---|
| codex-cli 0.154.0 | Tenancy, package vs per-repo layer, consent model, proxy, sequencing, acceptance | 2026-11-17 | Incorporated |
