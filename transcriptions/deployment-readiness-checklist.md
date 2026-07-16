# Deployment Readiness — Pre-Deploy Interrogation

**Source:** Instagram reel [DYSOYhQtDBX](https://www.instagram.com/reel/DYSOYhQtDBX/)
by **Mohamad Al Sayed** ([@sayed.developer](https://www.instagram.com/sayed.developer/)),
posted 2026-05-13, 43s. Caption: *"Comment „list" for the full repo🔥
#softwareengineering #computerscience #dev"*.
Captured & transcribed locally (yt-dlp + faster-whisper small/int8, no
third-party) on 2026-07-16 while building Pleiad (pleiad.io).

**Format:** a senior fires deployment-readiness questions; the dev answers. Each
Q maps to one gate in [`playbooks/pre-deploy-readiness.md`](../playbooks/pre-deploy-readiness.md).

**When to use:** the moment before you ship a web app to production.
**Skip:** internal tools with no untrusted users and no data worth protecting.

Language: en · Duration: 43s

---

**Transcript:**

> [00:00] I am finally deploying my website.
> [00:01] Wait, if I'm logged in, can I see your data?
> [00:03] No way — users are strictly locked into their own UUIDs.
> [00:06] Do your password reset links expire?
> [00:09] They're dead after 30 minutes.
> [00:10] Are you sanitizing every single field?
> [00:12] [Tender procedure] — SQL injection and XSS don't stand a chance here.
> [00:15] Is the API open to the whole world?
> [00:17] We only serve requests originating from our own domain.
> [00:20] Did you apply rate limiting?
> [00:21] Limits are active.
> [00:22] What about error handling?
> [00:24] We have custom error screens for every failure state.
> [00:27] Do you have indexes on your main queries?
> [00:28] Only on the high-traffic fields. We're keeping the traffic overhead low.
> [00:32] What about logging and monitoring?
> [00:33] Basic logs are flowing. I've got alerts set up for critical failures.
> [00:36] Last one — if this deployment crashes, can you roll back?
> [00:39] Easy — we're running a blue-green strategy for instant recovery.
> [00:42] Nice. Deploy it.

*[00:12] "Tender procedure" is an ASR mis-hear; from context the intended phrase
is about parameterized/sanitized inputs. Preserved verbatim above, corrected in
the playbook.*
