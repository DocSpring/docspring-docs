---
title: Error Reporting
description: Send DocSpring Enterprise's backend and frontend errors to Sentry.
sidebar:
  order: 3
---

We recommend sending DocSpring's error reports to [Sentry](https://sentry.io).
Configure it with two optional environment variables:

- `SENTRY_DSN`: backend errors (from Ruby on Rails).
- `DOCSPRING_SENTRY_JS_DSN`: frontend errors (from JavaScript in the browser).

Sentry is open source, so you can also run your own Sentry server: see
[Self-Hosted Sentry](https://develop.sentry.dev/self-hosted/).

## Create a Sentry project

Once you're signed in to Sentry, click **Create New Project**. The platform doesn't
matter much, but you can choose **Rails**:

![Sentry's "Create a new project" page, with platforms to choose from](@images/enterprise/sentry-create-project.png)

Give your project a name, then click **Create Project**:

![Sentry's project name and team fields, and the Create Project button](@images/enterprise/sentry-name-project.png)

Copy the DSN:

![Sentry's Rails SDK setup page, with the DSN in the configuration example highlighted](@images/enterprise/sentry-copy-dsn.png)

## Set the environment variables

- Set `SENTRY_DSN` to this DSN.
- For `DOCSPRING_SENTRY_JS_DSN`, you can:
  - Set it to the same DSN as `SENTRY_DSN`, to track backend and frontend errors in
    one Sentry project.
  - Create another Sentry project and use its DSN.
  - Leave it unset to ignore frontend errors.

DocSpring now sends its error reports to Sentry. If you need our help with an
error, send us the report, including the stack trace and any important details
about the request.

:::caution
Error reports can contain sensitive information. Remove it before you send us a
report.
:::
