---
name: Call an Amazon Nova understanding model with the Converse API
description: >-
  Send a multimodal message turn to Amazon Nova Pro, Lite or Micro through the Amazon Bedrock
  Runtime Converse API, with the auth, quota and irreversibility rules an agent needs before it
  spends money.
api: openapi/amazon-nova-inference-api-openapi.yml
operations: [converse, converseStream]
generated: '2026-09-01'
method: generated
source: openapi/amazon-nova-inference-api-openapi.yml, conventions/amazon-nova-conventions.yml, errors/amazon-nova-problem-types.yml
---

# Call an Amazon Nova understanding model with the Converse API

## Before you call

1. **Pick a model that is still Active.** Nova Pro (`amazon.nova-pro-v1:0`), Nova Lite
   (`amazon.nova-lite-v1:0`) and Nova Micro (`amazon.nova-micro-v1:0`) are Active. Nova Premier,
   Sonic, Canvas and Reel v1 are **Legacy with EOL dates in September 2026** — see
   `lifecycle/amazon-nova-lifecycle.yml`. If you have any doubt, read `modelLifecycle` from the
   control plane first: `aws bedrock get-foundation-model --model-identifier <id>`.
2. **Authenticate.** Either SigV4-sign the request (signing name `bedrock`) or send an Amazon
   Bedrock API key as `Authorization: Bearer <token>`. See
   `authentication/amazon-nova-authentication.yml`.
3. **Confirm model access is enabled** for the account in the Amazon Bedrock console. IAM
   permission alone is not enough — a missing model-access grant returns
   `AccessDeniedException` (403).

## The call

Use `converse` (`POST /model/{modelId}/converse`) for a single response, or `converseStream`
(`POST /model/{modelId}/converse-stream`) when you need tokens as they are produced.

The body is a `messages[]` array; each message has a `role` and a `content[]` list of blocks
(`text`, `image`, `video`, `document`, `toolUse`, `toolResult`). Put system instructions in the
top-level `system` field, not in a message. Set `inferenceConfig.maxTokens` — you are billed on
output tokens.

## Rules that will bite you

- **There is no idempotency key on `converse`.** A retry is a second generation and a second
  charge. Do not blind-retry a timeout; check whether you actually got a response first. Only
  `startAsyncInvoke` has an idempotency contract (`clientRequestToken`).
- **There is no way to take a call back.** No cancel, no refund, no undo — see the
  `reversibility` block in `conventions/amazon-nova-conventions.yml`. Size the request before you
  send it with `POST /model/{modelId}/count-tokens`, which is a read-only operation.
- **You cannot see your remaining quota from a response.** Amazon Bedrock Runtime returns no
  `RateLimit-*`, no `X-RateLimit-*` and no `Retry-After`. Quotas are per account, per region, per
  model — Nova Pro is 250 requests/minute and 1,000,000 tokens/minute; Nova Lite and Micro are
  2,000 requests/minute and 4,000,000 tokens/minute in us-east-1 and eu-west-2, and a tenth of
  that elsewhere. See `rate-limits/amazon-nova-rate-limits.yml`.

## Handling errors

Errors are AWS restJson1: a JSON body with a `message` field, and the type in the
`x-amzn-ErrorType` response header. Full catalog in `errors/amazon-nova-problem-types.yml`.

| Status | Type | What to do |
|---|---|---|
| 400 | `ValidationException` | Fix the body or model id. Do not retry unchanged. |
| 403 | `AccessDeniedException` | Grant `bedrock:Converse` and enable model access. |
| 408 | `ModelTimeoutException` | Shorten the prompt or output; do not blind-retry. |
| 429 | `ThrottlingException` | Back off exponentially with jitter. No `Retry-After` is sent. |
| 429 | `ModelNotReadyException` | The one error Amazon marks retryable. Retry with backoff. |
| 503 | `ServiceUnavailableException` | Retry with backoff. |

Log `x-amzn-RequestId` from every response — it is the only correlation handle AWS Support and
CloudTrail share with you.
