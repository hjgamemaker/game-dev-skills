# Contracts, services, and E2E

Read this reference only when the risk crosses an API, persistence, service, or real-runtime boundary.

## Contract tests

Exercise public request/response mapping, event envelopes, versions, and stable error categories.
Check agreement between the producer and consumer rather than only validating a hand-written fixture.
Keep persistence models behind the public API boundary.
Treat a breaking field or semantic change as an explicit contract change.

For client actions, assert intent payloads. Do not let a client assert facts such as a victory or credited reward.
Check authorization and resource ownership at the authoritative boundary.
Use negative cases for another owner's resource, expired sessions, malformed input, and stable downstream error mapping when relevant.
Verify that responses and logs do not expose raw internal exceptions, tokens, or private request bodies.

## Notification and authoritative refresh

Where notifications invalidate cached state, simulate notification receipt and assert the authoritative fetch.
Apply the fetched response as truth. Do not declare success merely because a notification arrived.
Test reconnect or missed-notification recovery when delivery is not guaranteed.
Do not reinterpret this as requiring HTTP polling in a protocol that explicitly has a different authority model.

## Idempotency and persistence

For a retryable mutation, test the documented idempotency contract:

- Submit the same key twice. Assert one authoritative effect and the documented duplicate response.
- Check reuse of a key with a different payload when the contract distinguishes it.
- When concurrency is part of the risk, exercise competing requests through the real transactional boundary.
- Simulate a lost response after commitment. Retrying must not repeat the effect.

A timeout does not prove the write failed.
Mock repositories can test orchestration, but cannot prove database atomicity, isolation, or uniqueness.
Use explicit infrastructure integration tests for those claims.

If the system uses an outbox, test atomic recording of the state change and outbound event.
For competing relays, test exclusive claim behavior and recovery of abandoned claims under the actual contract.
Do not assume every local game needs these mechanisms.
For stream-based adapters, verify that a blocking consumer cannot prevent publishing on the same connection when that is the failure risk.

## Test environment

Keep the default suite self-contained: no manually started database, fixed service port, or undeclared network dependency.
Run infrastructure tests through explicit project commands.
Do not silently add database or browser-stack requirements to the default test entrypoint.

Before a mutating infrastructure test:

1. Inspect its command, environment resolution, and reset behavior.
2. Confirm it targets a dedicated test database/account/environment.
3. Use isolated data and guaranteed cleanup according to the project's lifecycle.

Missing test configuration is a blocker for those tests. Do not fall back to development credentials or a production endpoint.
Do not run destructive resets against a user-owned manual review profile.
This skill does not authorize account creation or external writes beyond the user's task.

## Runtime readiness and diagnostics

Check actual dependency readiness before migrations or requests, not merely an open port.
Probe the critical cross-service route and retain per-service logs for an authorized runtime suite.
Validate configured base URLs, including browser/runtime restrictions, when changing endpoints.
Classify connection refusal, invalid environment, migration failure, and authentication failure as setup failures when appropriate.
They do not establish whether the game behavior passed.

For SPA navigation, test direct route loading or refresh if hosting is the risk.
A successful client-side transition does not prove the host serves that route correctly.
Keep supported route declarations aligned with the host's authoritative route source.

## E2E scope

Use a small set of critical journeys: session recovery, starting play, a key game outcome, a transaction, or notification-driven refresh.
Assert the destination route and authoritative outcome, not a single intermediate “ready” indicator.
Do not verify every rule by clicking through the UI.

Use the configured browser and viewport policy. Do not introduce a specific browser channel or resolution without a relevant risk.
Reuse authorized test authentication for non-authentication scenarios.
Authentication-specific tests exercise the actual login/session flow.
Keep reusable manual review profiles separate from isolated automated test accounts and storage.

Do not start services, browsers, or local page inspection unless the requested verification authorizes them.
Report which parts used real adapters and which used doubles.
Preserve diagnostic evidence without exposing credentials or private data.
