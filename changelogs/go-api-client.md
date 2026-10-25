---
title: Go API Client
description: Changelog and version history for the DocSpring Go API client
editUrl: false
sidebar:
  order: 12
---

<!-- Generated from api_clients/changelogs/CHANGELOG.go.md in the DocSpring repo. Don't edit this page: change the changelog, then run scripts/generate_api_client_changelog_pages. -->

New versions of the DocSpring Go API client. To install it, see
[Install an API Client](/docs/api-guide/install-api-client/). You can also
[get these release notes by email](https://app.docspring.com/email/subscribe?list=api_client_go).

## Changelog

### 3.3.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Oct 18, 2026</div>

- Submissions and combined submissions can be in the new `insufficient_credits` state: a live PDF that your credit
  balance can't cover is saved and processed once you add credits. See
  https://docspring.com/docs/api-guide/credits-and-rate-limits/.
- generate_pdf, batch_generate_pdfs and combine_pdfs document the 402 response for insufficient credits. Their
  responses can include a `code`, which is `insufficient_credits` on a 402.
- generate_pdf, batch_generate_pdfs, create_pdf_template, create_pdf_template_from_upload, create_html_template and
  copy_template document a 403 response with a `code` of `upgrade_required` (the plan doesn't include it) or
  `account_suspended`.

### 3.2.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Oct 18, 2026</div>

- Models no longer return an error when a response has a field this version doesn't know, so new API fields don't
  break your code. Response models keep those fields in a new `AdditionalProperties` map. Versions 2.x to 3.1.0
  fail to decode any response with a field they don't know. Templates have one since October 18, 2026
  (`parse_dates_using_template_date_format`), so upgrade to keep template calls working.
- Because of the new map, `CustomFile`, `ErrorResponse`, `Folder`, `SubmissionDataRequestEvent`,
  `SubmissionDataRequestToken`, `SuccessErrorResponse` and `TemplateModelCodeResponse` can no longer be compared
  with `==`.
- Templates: new `parse_dates_using_template_date_format` setting, when you create or update a template. When it's
  true, date values sent as NN/NN/YYYY (with slashes, dashes or dots) are read in the order of the template's default
  date format: month first for `%m/%d/%Y`, day first for `%d/%m/%Y`. When it's false, they're read day first.
- A new README: installing, a quick start, every API method, errors, and how API changes are handled. The repository
  also has an AGENTS.md for coding agents.

### 3.1.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Oct 15, 2026</div>

- Submissions and data requests can now be in the `declined` state, when a signer declines to sign. Data
  requests have a new `declined_at` timestamp.
- New API methods for webhooks: create_webhook, list_webhooks, get_webhook, update_webhook and delete_webhook.
- New API method: get_template_model_code, which generates typed model code for a template's submission data, so
  your app can catch mistakes before it calls the API.
- Templates:
  - Field dependencies: `field_dependencies` when you create or update a template, and `dependencies` on the fields
    you add with add_fields_to_template.
  - add_fields_to_template accepts the `initials` field type and an `editable` setting for each field.
  - Embedded template editor settings: `embed_edit_allow_document_replacement`, `embed_edit_allow_settings`,
    `embed_edit_allow_versioning` and `embed_edit_token`.
  - New `template_schema_version` attribute.
  - A template's `path` is never null.
  - Individual template `defaults` can be null (for example `fontSize` after it's cleared in the template editor).
  - `last_changed_by_type` can be `external_user` (a change made in the embedded template editor), with
    `last_changed_by_external_id`, `last_changed_by_external_name` and `last_changed_by_external_email`.
  - Template versions say who published them with `published_by_type`, `published_by_user_id`,
    `published_by_name`, `published_by_email` and `published_by_external_id`. These replace `published_by_id`,
    which the API no longer returns.
  - The JSON schema from get_template_schema includes `$id`, `allOf` and `dependencies`.
- copy_template accepts a template `version`, and `parent_folder_id` is now optional. move_template_to_folder also
  accepts a `version`.
- Pass a null `parent_folder_id` to move_template_to_folder or move_folder_to_folder to move it to the root folder.
- Submissions and submission actions include an `error_message` when processing fails.
- combine_pdfs: `delete_custom_files` now deletes the custom files once the merged PDF is processed (it used to be
  ignored). `test` has no effect and is deprecated.
- get_submission, list_submissions and list_template_submissions no longer fail when `include_data` isn't set.
  `source`, `referrer` and `data` are optional on submissions, because the API only returns them with
  `include_data`. `Submission.Source` is now a `*string` (use `GetSource()`).
- `DOCSPRING_HOST` can end with `/api/v1/` or have spaces around it (a trailing slash after `/api/v1` used to
  repeat it).

### 3.0.1

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Dec 4, 2025</div>

- Fixed go.mod module path so the package can be indexed by the Go module proxy

### 3.0.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Sep 24, 2025</div>

- All our API clients are now 100% end-to-end tested against our real server.

### 2.1.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Mar 22, 2025</div>

- Added support for Template Versioning parameters:
  - Optional version parameter in submission requests (e.g., 1.2.3, draft, latest)
  - New API methods: publish_template_version, restore_template_version
  - Updated copy_template and delete_template methods to accept template version strings

### 2.0.1

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Feb 26, 2025</div>

- Fixed User-Agent header: docspring-go-x.x.x

### 2.0.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Feb 23, 2025</div>

- **BREAKING CHANGE**: Updated default host to our new synchronous API subdomain: sync.api.docspring.com. (EU customers should use sync.api-eu.docspring.com). Removed all custom polling code from library since this logic is now handled by the API service running on our sync subdomain

### 1.0.1

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Nov 28, 2021</div>

- Added 'password' parameter to Generate PDF and Combine PDF API endpoints. You can now encrypt each generated PDF with a unique password.

### 1.0.0

<div class="mt-2.5 text-sm text-gray-500 dark:text-gray-400">Released on Aug 28, 2021</div>

- Initial release of Go API client
