---
title: Rust API Client
description: Changelog and version history for the DocSpring Rust API client
editUrl: false
sidebar:
  order: 18
---

<!-- Generated from api_clients/changelogs/CHANGELOG.rust.md in the DocSpring repo. Don't edit this page: change the changelog, then run scripts/generate_api_client_changelog_pages. -->

New versions of the DocSpring Rust API client. To install it, see
[Install an API Client](/docs/api-guide/install-api-client/). You can also
[get these release notes by email](https://app.docspring.com/email/subscribe?list=api_client_rust).

## Changelog

### 4.3.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Oct 31, 2026</div>

- New: Word templates (`template_type` `docx`). create_template makes one from a Word document (.docx, up to 50 MB)
  that you upload and pass as a `cache_id`, or pass as a `document_url` or `document_base64`; upload_template takes the
  file itself. update_template_document and upload_template_document replace its document the same ways. Each tag in
  the document, such as `{{ customer.name }}`, becomes a field. Each of them takes an OpenDocument text file (.odt)
  too, which DocSpring converts to a Word document.
- New: a Word template's `output_format`, what its submissions make unless they set `output_formats`: `docx` (a Word
  document), `odt` (an OpenDocument file) or `pdf`. create_template and upload_template require it, and
  update_template changes it. A Word template that encrypts its PDFs makes PDFs, because Word documents and
  OpenDocument files can't be password-protected.
- New: the `docx` and `odt` output formats, a Word template's filled-in Word document (`files.docx`) and OpenDocument
  file (`files.odt`, converted from the Word document). Add them to `output_formats` in create_submission or
  create_submission_batch to get more than one file, such as the Word document and the PDF.
- generate_submission_files takes `pdf`, the PDF of a Word template's submission that didn't make one (converted from
  its Word document), `docx`, the Word document of one that made only its PDF (made again from its data and the
  template version it used), and `odt`, its OpenDocument file. None of them uses credits again. Until the new file is
  ready, its `state` in `files` is `pending`, and when it couldn't be made, its `error_message` says why.
- A Word template's `document` is its Word document: `document.permanent_url` downloads it. Its `state` is `processed`
  once it's uploaded or downloaded (`downloading` or `download_failed` while a `document_url` downloads, `converting`
  or `conversion_failed` while an OpenDocument file is converted), and get_template_diff's `document_changed` says
  whether it was edited.
- get_template_page_images returns a 409 for a Word template, whose pages are only laid out when a document is made.
- create_template_preview previews Word templates too: images of the pages of the PDF that the template's Word
  document makes with your data. A DocSpring server that can't convert Word documents to PDF returns a 409.

### 4.2.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Oct 30, 2026</div>

- The first release of the Rust SDK, for API v2. `docspring::Client` has an async method for every operation (the
  `docspring::Api` trait), and each one takes its parameters from a builder, such as
  `docspring::CreateSubmissionParams::builder()`. Requires Rust 1.88 or later.
- `docspring::Client::builder()` takes the API token and the host or region, and `docspring::Client::from_env()` reads
  them from `DOCSPRING_TOKEN_ID`, `DOCSPRING_TOKEN_SECRET`, and `DOCSPRING_HOST` or `DOCSPRING_REGION` (`US` or `EU`).
- Every request is checked against the API's request schemas before it's sent: one the API would reject fails with
  `docspring::Error::RequestValidation`, which lists each problem. `validate_requests(false)` on the client builder
  turns this off.
- An error response is decoded into the operation's error type, by its status code.
- Enums are open: a value this version doesn't know yet, such as a new submission state, is read as `Unknown(String)`
  instead of failing the response.
- `docspring::verify_webhook_signature(payload, signature_header, secret)` checks a webhook request's
  `X-DocSpring-Signature` header. Pass it the request body exactly as it was received.
- The `mockall` feature adds `docspring::MockApi`, a mock of the `Api` trait for your tests. HTTPS uses rustls by
  default. For the system's TLS library, turn off the default features and turn on `native-tls`.
