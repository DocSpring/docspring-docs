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

### 4.4.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Nov 1, 2026</div>

- New: list_email_actions, create_email_action, get_email_action, update_email_action and delete_email_action manage a
  template's email actions (`EmailAction`), which email each processed submission's files as attachments or a download
  link. `to`, `cc` and `bcc` take addresses, and tags for an address in the submission's fields
  (`{{ data.client_email }}`) or metadata (`{{ metadata.agent_email }}`). `send_when` sends only for the submissions
  that meet its conditions. `file_name` names the attached files, and `file_formats` chooses them: a Word template's
  Word document (`docx`), OpenDocument file (`odt`) or PDF (`pdf`), or every file the submission made (`all`). Attached
  files over `max_attachment_mb` together (7 MB unless you raise it, up to 25) go as a link instead.
  send_email_action_test_email sends one to you to check how it looks (`EmailActionTestEmail`). Without a paid plan
  (Starter or higher), email actions only email your team: test emails, and test submissions' emails.
- New: list_account_email_actions, create_account_email_action, get_account_email_action, update_account_email_action
  and delete_account_email_action manage your account's email actions, which send for every template, or for the
  templates and folders they list (`template_ids`, `folder_ids`). send_account_email_action_test_email sends one to you
  for one of its templates.
- New: list_email_actions also lists your account's email actions that send for the template, with the template's
  changes in `customization` (`EmailActionCustomization`). update_email_action_customization changes one for that
  template only, or turns it off there (`enabled`), and delete_email_action_customization removes the template's
  changes.
- New: list_email_domains, create_email_domain, get_email_domain, verify_email_domain and delete_email_domain manage the
  domains your email actions send from (`EmailDomain`), with the DNS records to add to each one (`EmailDomainDnsRecord`).
- A submission's `actions` include the emails its email actions sent (its template's and your account's), with each
  recipient and what happened to their email, and a new `email_action_id`.
- New: a field's `string_condition_type` can be `not_equals`, `not_contains`, `not_starts_with`, `not_ends_with`,
  `not_regex`, `blank` or `not_blank`, and a `regex` condition uses RE2 syntax (no lookarounds or backreferences).
- create_submission: DocSpring now emails the signers of a submission's `signature_requests` a link to sign, any
  reminders, and the signed PDF once everyone has signed. Set `send_email` to false, for the submission or for one
  signature request, when you send the links yourself or show the signing form in your app. A signature request that
  DocSpring emails needs an `email`, and its `auth_type` is optional (`email_link`). Before anyone is emailed, the
  request can be refused with a 402 (`insufficient_credits_to_send`), a 403 or a 429 (`daily_email_limit_reached`), and
  nothing is saved.
- New create_submission properties for these emails: `signing_order` (`SigningOrder`) with each signature request's
  `signing_step`, `email_subject`, `email_message`, `cc`, `reply_to`, `notification_emails`, `notify_when_signed` and
  `reminders` (`SignatureRequestReminders`).
- A signature request has `send_email`, `signing_step`, `sent_at` and `email_status` (`SignatureRequestEmailStatus`),
  and a submission has `signing_order`.
- Templates have new settings for these emails: `signature_request_from_name`, `signature_request_from_address`,
  `signature_request_signing_url`, `signature_request_include_signing_token`, `signature_request_reply_to`,
  `signature_request_notification_emails` and `signature_request_email_attachments`
  (`SignatureRequestEmailAttachments`).
- New webhook event types: `signature_request.sent` and `signature_request.email_bounced`.
- New: page designs (`template_type` `page_designer`), PDFs designed in the Page Designer from a JSON layout, with no
  document to start from. create_template makes one from its `layout`, or with an empty page when `layout` is left
  out, and update_template replaces its layout. DocSpring adds a field for each value the layout reads. A template's
  `layout` is in the [Page Designer layout format](https://docspring.com/docs/page-designer/layout-format/) (its
  [JSON Schema](https://docspring.com/schemas/page-designer/layout.v1.json)), and is null for other templates.
- New: every method that creates, changes or deletes something takes an `Idempotency-Key` header, in its params'
  `idempotency_key` field. Send the same key when you send a request again after a timeout or a network error: within 24
  hours, DocSpring returns the first response instead of running the request twice. The params' builders need no
  change, but a params struct written as a struct literal needs `idempotency_key: None`.
- Changed: the request validator checks what API v2 now refuses: an ID, a query value or a template name that's blank or
  only whitespace, an update with nothing to change, a version with a leading zero (`01`), a `filename` with a slash, a
  signer `email` that isn't an email address, and an HTML template's `scale` outside 0.1 to 2.

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
