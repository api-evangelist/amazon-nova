---
name: Run a long-running Amazon Nova generation asynchronously
description: >-
  Start, poll and collect a long-running Amazon Nova generation (Nova Reel video) with
  StartAsyncInvoke and GetAsyncInvoke, using the one idempotency contract Amazon Nova offers.
api: openapi/amazon-nova-async-api-openapi.yml
operations: [startAsyncInvoke, getAsyncInvoke]
generated: '2026-09-01'
method: generated
source: openapi/amazon-nova-async-api-openapi.yml, conventions/amazon-nova-conventions.yml
---

# Run a long-running Amazon Nova generation asynchronously

Use this for work that will not finish inside a synchronous request — principally Amazon Nova Reel
video generation.

## 1. Start the job

`startAsyncInvoke` (`POST /async-invoke`).

- **Always set `clientRequestToken`.** It carries the `smithy.api#idempotencyToken` trait in
  Amazon's own service model and is the only idempotency guarantee anywhere in the Amazon Nova
  surface. Generate one deterministic token per logical job and reuse it on every retry. Reusing a
  token with *different* parameters returns `ConflictException` (400), which is the behaviour you
  want.
- Set `outputDataConfig.s3OutputDataConfig.s3Uri` to a bucket you own. The generated media is
  written there, not returned inline.
- The response is an `invocationArn`. Persist it before doing anything else.

## 2. Poll for completion

`getAsyncInvoke` (`GET /async-invoke/{invocationArn}`). Status moves `InProgress` →
`Completed` | `Failed`.

There is **no event** for async-invoke state change. Amazon Bedrock's EventBridge surface covers
batch inference and model customization jobs only (`asyncapi/amazon-nova-events.yml`), so polling
is the only option here. Poll on a backoff — `GetAsyncInvoke` is itself subject to
`ThrottlingException`.

## 3. Know what you cannot do

**There is no cancel and no stop operation.** Once `startAsyncInvoke` returns, the job runs to
completion and is billed. `getAsyncInvoke` and `listAsyncInvokes` only observe it. The only
protection available to you is the idempotency token in step 1, which prevents a *duplicate* job —
it does not undo one. Do not design a flow that assumes an abort path exists.

## 4. Concurrency limits

Concurrent `InvokeModel` requests are capped per model: Nova Reel 1.0 allows 10, Nova Reel 1.1
allows 3, Nova Sonic allows 20. Exceeding them returns `ThrottlingException` (429). See
`rate-limits/amazon-nova-rate-limits.yml`.

**Lifecycle warning:** both Nova Reel model ids (`amazon.nova-reel-v1:0`, `amazon.nova-reel-v1:1`)
are in the Legacy state with an EOL date of 2026-09-30.
