# Yanrix

**Continuous STRIDE threat modeling, built into GitHub.**

---

The threat model goes stale the moment it leaves the design meeting. Most teams either have no threat model or one that hasn't been touched since the last architecture review. Neither is useful when the codebase changes every day. Security reviews catch bugs in implementation. They don't catch threats in the architecture: the trust boundaries, the interaction patterns, the design decisions that no linter will ever flag.

Yanrix operates at the architectural reasoning layer. It applies design-phase thinking on every pull request, bringing threat modeling out of the design meeting and into the continuous development workflow. The output is a structured STRIDE threat model. Threat models, not bug lists.

## How it works

Yanrix runs as a GitHub Action on every pull request. It performs STRIDE analysis on the PR diff and posts findings as a PR comment. The threat model accumulates across the codebase. Each PR builds on the last.

## Example output

```
Yanrix Threat Model  v2.0.0
paypath-api · PR #47 · feat/stripe-webhook-v2
1 Critical · 2 High · 1 Medium · 0 Low

────────────────────────────────────────────

🔴 T01 · Critical · Spoofing
Webhook Secret Not Validated via Missing HMAC Verification on Stripe Webhook Endpoint
Confidence: Observed
Remediation: Validate every incoming webhook with stripe.webhooks.constructEvent(body, sig, secret)
before processing. Return HTTP 400 on signature failure.

🟠 T02 · High · Tampering
Client-Side Payment Amount Tampering via Unvalidated Request Field

🟠 T03 · High · Repudiation
Replay Attack via Missing Idempotency Key on /api/payments

🟡 T04 · Medium · Information Disclosure
Internal Payment Processor Details Leaked via Unhandled Error Responses
```

## Status

Yanrix is in closed beta. [Join the waitlist at yanrix.dev](https://yanrix.dev).

---

STRIDE-based · Runs on every PR · Built into GitHub · Free for open source · No diagrams required
