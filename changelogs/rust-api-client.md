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
