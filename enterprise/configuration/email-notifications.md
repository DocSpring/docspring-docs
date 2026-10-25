---
title: Email Notifications
description: Configure an email provider for DocSpring Enterprise's user invitations, password resets and other emails.
sidebar:
  order: 4
---

Configure an email provider to send user invitations, password resets and other
emails.

DocSpring checks for these environment variables and uses the first provider it
finds, in this order:

| Provider | Environment variables                                                                           |
| -------- | ----------------------------------------------------------------------------------------------- |
| Postmark | `DOCSPRING_POSTMARK_API_TOKEN`                                                                  |
| SendGrid | `DOCSPRING_SENDGRID_API_KEY` (or `DOCSPRING_SENDGRID_USERNAME` + `DOCSPRING_SENDGRID_PASSWORD`) |
| Mailgun  | `DOCSPRING_MAILGUN_API_KEY`                                                                     |
| Gmail    | `DOCSPRING_GMAIL_USERNAME` + `DOCSPRING_GMAIL_PASSWORD`                                         |
| AWS SES  | `DOCSPRING_AWS_SES_ACCESS_KEY_ID` + `DOCSPRING_AWS_SES_SECRET_ACCESS_KEY`                       |
| SMTP     | `DOCSPRING_SMTP_SERVER` + `DOCSPRING_SMTP_USERNAME` + `DOCSPRING_SMTP_PASSWORD`                 |

For SMTP, you can also set `DOCSPRING_SMTP_PORT` (default `587`), `DOCSPRING_SMTP_AUTHENTICATION`
(default `plain`) and `DOCSPRING_SMTP_HELO_DOMAIN`.

Emails are sent from `DOCSPRING_FROM_EMAIL` (e.g. `DocSpring <notifications@example.com>`).
If it isn't set, they're sent from `support@` at `DOCSPRING_EMAIL_DOMAIN_NAME`, which
defaults to your `DOCSPRING_DOMAIN_NAME`.

If you don't need to send emails, set `DOCSPRING_DISABLE_EMAILS=true`. DocSpring then
doesn't log the "No email service has been configured" message when it starts.

See [Environment variables](/docs/enterprise/configuration/environment-variables/)
for every variable.
