# Amazon Nova (amazon-nova)
Amazon Nova is a new generation of state-of-the-art foundation models from Amazon that deliver a compelling combination of accuracy, speed, and cost efficiency. Amazon Nova models are accessible through Amazon Bedrock and support text, image, video, speech understanding and generation across a range of model types: Nova Premier (1M context), Nova Pro, Nova Lite, Nova Micro (text-only), Nova Canvas (image generation), Nova Reel (video generation), and Nova Sonic (speech).

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/amazon-nova/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AWS, Foundation Models, Generative AI, Image Generation, Machine Learning, Multimodal, Speech, Video Generation

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-19

## APIs

### Amazon Nova API
The Amazon Nova API provides programmatic access to Amazon Nova foundation models through Amazon Bedrock for text, image, and video generation, understanding, and reasoning tasks. Supports Nova Premier, Pro, Lite, Micro, Canvas, Reel, and Sonic models.

**Human URL:** [https://docs.aws.amazon.com/nova/latest/userguide/what-is-nova.html](https://docs.aws.amazon.com/nova/latest/userguide/what-is-nova.html)

#### Tags:

 - Foundation Models, Generative AI, Multimodal

#### Properties

- [Documentation](https://docs.aws.amazon.com/nova/latest/userguide/what-is-nova.html)
- [APIReference](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_runtime_InvokeModel.html)
- [GettingStarted](https://docs.aws.amazon.com/nova/latest/userguide/getting-started.html)
- [Pricing](https://aws.amazon.com/bedrock/pricing/)

## Common Properties

- [Portal](https://aws.amazon.com/ai/nova/)
- [Website](https://aws.amazon.com/ai/nova/)
- [Documentation](https://docs.aws.amazon.com/nova/)
- [Terms of Service](https://aws.amazon.com/service-terms/)
- [Privacy Policy](https://aws.amazon.com/privacy/)
- [Support](https://aws.amazon.com/premiumsupport/)
- [Blog](https://aws.amazon.com/blogs/machine-learning/tag/amazon-nova/)
- [GitHub Organization](https://github.com/aws)
- [Console](https://console.aws.amazon.com/bedrock/)
- [Sign Up](https://portal.aws.amazon.com/billing/signup)
- [Login](https://signin.aws.amazon.com/)
- [Status](https://health.aws.amazon.com/health/status)
- [Contact](https://aws.amazon.com/contact-us/)

## Features

| Name | Description |
|------|-------------|
| Multiple Model Types | Seven specialized models: Nova Premier (1M context), Nova Pro, Nova Lite, Nova Micro (text), Nova Canvas (image), Nova Reel (video), Nova Sonic (speech). |
| Multimodal Input | Supports text, images, video, documents (PDF, CSV, DOCX, XLS, HTML), and speech as input modalities. |
| Long Context Windows | Up to 1 million token context window in Nova Premier; 300k tokens in Nova Pro and Lite; 128k in Nova Micro. |
| Streaming Responses | All understanding models support streaming for real-time interactive applications. |
| Batch Inference | All understanding models support batch processing for high-volume offline workloads. |
| Fine-Tuning | Nova Pro, Lite, and Micro support fine-tuning for domain-specific customization. |
| Model Distillation | Nova Premier can serve as a teacher model for distillation into Pro, Lite, and Micro. |
| Bedrock Integration | Natively integrated with Amazon Bedrock Knowledge Bases, Agents, Guardrails, Evaluations, and Prompt Flows. |

## Use Cases

| Name | Description |
|------|-------------|
| Interactive Chat Interfaces | Build conversational AI applications with long context awareness using Nova Pro, Lite, or Micro. |
| Retrieval-Augmented Generation | Enhance knowledge retrieval accuracy by combining Nova models with Bedrock Knowledge Bases. |
| Agentic Applications | Build autonomous AI agents that reason and act using Nova models with Bedrock Agents. |
| Video and Document Analysis | Analyze video content and complex documents (PDF, DOCX, XLS) with Nova Pro and Premier. |
| Image Generation and Editing | Generate and edit high-quality images programmatically with Amazon Nova Canvas. |
| Video Generation | Create short video clips from text or image prompts using Amazon Nova Reel. |
| Voice Assistants | Build voice-enabled customer service and assistant applications with Nova Sonic speech model. |
| UI Workflow Automation | Automate UI interactions and screen navigation workflows using Nova vision capabilities. |

## Integrations

| Name | Description |
|------|-------------|
| Amazon Bedrock | Primary access method; all Nova models are served through the Bedrock InvokeModel and InvokeModelWithResponseStream APIs. |
| Bedrock Knowledge Bases | Connect Nova models to structured and unstructured data sources for RAG applications. |
| Bedrock Agents | Orchestrate multi-step agentic workflows with tool use and memory using Nova as the reasoning engine. |
| Bedrock Guardrails | Apply safety guardrails to Nova Premier, Pro, and Lite model outputs for content filtering. |
| Bedrock Prompt Flows | Build visual prompt chaining workflows connecting Nova models with other services. |
| Bedrock Evaluations | Evaluate Nova model performance on custom benchmarks and safety criteria. |
| Amazon S3 | Store and access training data, batch inference inputs/outputs, and generated media artifacts. |
| AWS IAM | Control access to Nova model invocations through fine-grained IAM policies and Bedrock model access settings. |

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
