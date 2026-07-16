# Playbook — Pre-Deploy Readiness

Nine gates to clear before shipping a web app to production. Derived from
[`transcriptions/deployment-readiness-checklist.md`](../transcriptions/deployment-readiness-checklist.md)
(IG reel by [@sayed.developer](https://www.instagram.com/reel/DYSOYhQtDBX/)).

Each gate: the question to answer honestly, what "pass" means, and how to prove
it — don't take the code's word, exercise it. A gate you can't demonstrate is a
gate you haven't passed.

---

## 1. Tenant isolation
**Q:** If I sign in as user A, can I read user B's data?
**Pass:** every owner-scoped query filters by the *server-derived* user id, never
a client-supplied one. No endpoint trusts an id from the request body/query.
**Prove:** sign in as A, capture A's token, call a record-fetch endpoint with B's
record id. Must be 403/404, never B's data.

## 2. Password reset / auth links expire
**Q:** How long is a reset or magic link valid?
**Pass:** short TTL (≤30 min), single-use, invalidated after use.
**Prove:** request a link, wait past the TTL, try it → rejected. Use it twice →
second use rejected.

## 3. Input sanitization (SQL injection + XSS)
**Q:** Is every field that reaches a query or the DOM sanitized?
**Pass:** parameterized queries everywhere (no string-built SQL); output encoded
on render; a strict CSP as backstop.
**Prove:** submit `' OR 1=1 --` and `<script>alert(1)</script>` into free-text
fields. First must not alter the query; second must render inert, not execute.

## 4. API access scope (CORS / origin)
**Q:** Is the API callable from any origin?
**Pass:** CORS allows only your own origins; state-changing routes reject
cross-origin/unauthenticated calls.
**Prove:** `curl` a mutating endpoint with a foreign `Origin` header and no
session → rejected.

## 5. Rate limiting
**Q:** Can a client hammer sensitive endpoints unbounded?
**Pass:** limits on auth, reset, and write endpoints; excess returns 429.
**Prove:** loop-hit a limited endpoint; confirm it starts returning 429.

## 6. Error handling
**Q:** What does the user see on failure?
**Pass:** custom error states for 404/500/offline; no stack traces or internal
detail leaked to the client.
**Prove:** force a 500 and a 404; confirm friendly screens, and that responses
carry no stack trace or secret.

## 7. Database indexes on hot paths
**Q:** Are high-traffic queries indexed?
**Pass:** indexes on the columns behind frequent/filtering queries; no unindexed
full scans on hot paths. (Don't over-index — write cost.)
**Prove:** `EXPLAIN` the top queries; confirm index usage, not seq scans.

## 8. Logging & monitoring
**Q:** Will you know when it breaks?
**Pass:** structured logs flowing off-box; alerts on critical failures reach a
human channel.
**Prove:** trigger a critical error; confirm it lands in logs *and* fires an
alert.

## 9. Rollback
**Q:** If this deploy is bad, how fast can you undo it?
**Pass:** one-command/instant rollback — blue-green, canary, or pinned previous
release.
**Prove:** in staging, deploy then roll back; confirm the previous version is
live and the path is documented.

---

**Rule:** a gate is only green when the *Prove* step passed. "The code looks
right" is not a pass.
