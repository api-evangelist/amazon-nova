---
name: Generate an image with Amazon Nova Canvas
description: >-
  Call Amazon Nova Canvas through the Bedrock Runtime InvokeModel operation for text-to-image and
  colour-guided image generation, including the deprecation you need to plan around.
api: openapi/amazon-nova-inference-api-openapi.yml
operations: [invokeModel]
generated: '2026-09-01'
method: generated
source: openapi/amazon-nova-inference-api-openapi.yml, mcp/amazon-nova-tool-crosswalk.yml
---

# Generate an image with Amazon Nova Canvas

## Read this first

`amazon.nova-canvas-v1:0` is in the **Legacy** state with an EOL date of **2026-09-30**. New
customers cannot start using a Legacy model, and existing customers may lose access after 15 days
of inactivity. Verify with `aws bedrock get-foundation-model --model-identifier
amazon.nova-canvas-v1:0 --query modelDetails.modelLifecycle` before building on it.

## The call

`invokeModel` (`POST /model/{modelId}/invoke`) with `modelId=amazon.nova-canvas-v1:0`. Canvas is
**not** a Converse model — it takes a task-shaped JSON body, not `messages[]`.

- `taskType: TEXT_IMAGE` with a `textToImageParams.text` prompt for plain generation.
- `taskType: COLOR_GUIDED_GENERATION` with a `colors[]` palette (up to 10 hex values) to steer
  style and mood.
- `imageGenerationConfig` carries dimensions, `numberOfImages`, `quality` (standard | premium),
  `cfgScale` and `seed`.

Images come back base64-encoded in the response body.

## Cost

Billed per image, not per token: 0.04 USD for a standard 1024px image and 0.06 USD for premium;
2048px is 0.06 / 0.08 USD. Full table in `plans/amazon-nova-plans-pricing.yml`. `numberOfImages`
multiplies the charge — an agent looping generation without a cap is the expensive failure mode
here, and **there is no refund path**.

## Quota

100 requests/minute per account per region for Nova Canvas. No rate-limit headers are returned;
exhaustion is a 429 `ThrottlingException`.

## If you are wiring this through MCP

The AWS Labs `awslabs.nova-canvas-mcp-server` exposes `generate_image` and
`generate_image_with_colors`, both of which are thin wrappers over this same `invokeModel`
operation. It is **deprecated** and has shipped no release since 2026-03-24 — see
`mcp/amazon-nova-mcp.yml`. Calling `invokeModel` directly is the supported path.
