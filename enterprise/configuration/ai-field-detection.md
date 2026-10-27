---
title: AI Field Detection
description: Set up AI field detection and AI field naming on DocSpring Enterprise, with the ai-field-detection image and Claude or OpenAI.
sidebar:
  label: AI Field Detection
  order: 5
---

DocSpring can set up a PDF template's fields for you: it detects the fields on
each page, names them with AI, and sets up conditional logic and required
fields. [Automatic field detection](/docs/template-editor/automatic-field-detection/)
describes how it works for your users. This page covers turning it on for a
DocSpring Enterprise install.

It has two parts, and you can turn on either or both:

- **AI field detection** finds the fields on a flat PDF's pages. It runs in
  DocSpring's own workers, with models built into the `-ai-field-detection`
  variant of the Enterprise image. Nothing leaves your install.
- **AI field naming** sends page images to an AI provider to name and type the
  fields, and to set up conditional logic and required fields. It uses Claude
  (through the Claude API or Amazon Bedrock) or OpenAI, with your own account.

| What you turn on     | What happens                                                                                                                                                                      |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Neither              | The upload page doesn't show the AI options. Fillable PDFs import their own form fields.                                                                                          |
| Detection only       | Fields are detected on flat PDFs and keep plain names, such as `detected_text_box_page_1_1`. There's no conditional logic.                                                        |
| Naming only          | AI names a fillable PDF's own form fields and sets up conditional logic. A flat PDF gets no fields: the template editor says they couldn't be detected, and you add them by hand. |
| Detection and naming | Everything described in [Automatic field detection](/docs/template-editor/automatic-field-detection/).                                                                            |

## Turn on AI field detection

1. Switch the **web** and **worker** services to the `-ai-field-detection`
   variant of the image. Each release has one: its tag is the release's version
   tag followed by `-ai-field-detection` (for example
   `<version>-ai-field-detection`), and `latest-ai-field-detection` follows the
   newest release. Both architectures (AMD64 and ARM64) are included. The
   variant turns detection on for you.
2. Give the worker more memory. Detection needs about 1 GB of memory for each
   page it's detecting, and each worker process detects up to
   `DOCSPRING_AI_FIELD_SETUP_CONCURRENCY` pages at once (default `4`). Add about
   4 GB to the worker's memory limit, or lower
   `DOCSPRING_AI_FIELD_SETUP_CONCURRENCY` on a smaller worker.

The models are built into the image, so detection works without internet access
and doesn't download anything when it starts. The
[DocSpring Enterprise releases](https://docspring.com/enterprise-releases) page
lists the version tags.

## Turn on AI field naming

Set `DOCSPRING_AI_FIELD_NAMING_PROVIDER` on the **web** and **worker** services,
with the settings for your provider:

| `DOCSPRING_AI_FIELD_NAMING_PROVIDER` | Also set                                               | Default model     |
| ------------------------------------ | ------------------------------------------------------ | ----------------- |
| `anthropic` (the Claude API)         | `DOCSPRING_AI_FIELD_NAMING_API_KEY`: a Claude API key  | `claude-opus-5-5` |
| `bedrock` (Claude on Amazon Bedrock) | An AWS region (see below)                              | `claude-opus-5-5` |
| `openai` (the OpenAI API)            | `DOCSPRING_AI_FIELD_NAMING_API_KEY`: an OpenAI API key | `gpt-5.5`         |

To use another model, set `DOCSPRING_AI_FIELD_NAMING_MODEL`.

DocSpring doesn't start if these settings conflict: an unknown provider,
`anthropic` or `openai` without an API key, `bedrock` without a region, half of
an access key pair, or both a Bedrock API key and an access key pair.

### Amazon Bedrock

Bedrock uses `DOCSPRING_AI_FIELD_NAMING_AWS_REGION`, or `AWS_REGION` when that
isn't set. Requests go to Bedrock's US or EU geographic inference profile for
that region, so page images stay in the US or the EU. In a region outside the
US and EU, set `DOCSPRING_AI_FIELD_NAMING_MODEL` to a full Bedrock inference
profile ID, such as `au.anthropic.claude-opus-5-5`.

For credentials, set one of these, or neither:

- A Bedrock API key in `DOCSPRING_AI_FIELD_NAMING_API_KEY`.
- An access key pair in `DOCSPRING_AI_FIELD_NAMING_AWS_ACCESS_KEY_ID` and
  `DOCSPRING_AI_FIELD_NAMING_AWS_SECRET_ACCESS_KEY`.

With neither, DocSpring uses the default AWS credentials, such as an instance
role, an ECS task role or EKS Pod Identity.

The credentials need `bedrock:InvokeModel` on the inference profile and its
foundation models, and your AWS account must have accepted the model's terms in
the Bedrock console.

## Settings and limits

- Your users choose the field name language and whether AI sets up conditional
  logic on the **Template defaults** page, and when they run Set Up Fields in
  the template editor.
- AI field setup can run 5 times an hour on each template.
- `DOCSPRING_AI_FIELD_NAMING_BATCH_SIZE` sets how many fields go to the model in
  each request (default `30`).

See [Environment variables](/docs/enterprise/configuration/environment-variables/)
for every setting.

## Check that it works

Run these in a one-off container from your image, with the same environment
variables as your services.

```bash
# Detects fields on a generated test page, the same way the workers do: checks
# that the models load, fit in memory and find fields
bundle exec rails checks:field_detection

# Sends one small request to your AI provider: checks the credentials and the
# model
bundle exec rails checks:ai_field_naming
```
