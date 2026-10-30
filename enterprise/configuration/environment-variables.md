---
title: Environment Variables
description: Every environment variable you can set to configure DocSpring Enterprise.
editUrl: false
sidebar:
  order: 2
---

<!-- Generated from config/environment_variables.yml in the DocSpring repo. Don't edit this page: change the YAML. -->

Configure DocSpring Enterprise with these environment variables. Only a few are required; everything else
has a sensible default.

- **On/off settings** accept `true`, `1`, `yes` or `on` to turn them on, and `false`, `0`, `no`, `off` or
  an empty value to turn them off.
- **Older names still work.** Settings that were renamed list their old names ("Formerly"). If you set
  both, the new name wins.
- DocSpring logs a warning at startup for any `DOCSPRING_` variable it doesn't recognize, so a typo
  doesn't go unnoticed.

## Domains and URLs

### DOCSPRING_ADMIN_DOMAIN_NAME

The host name for the admin dashboard (`/admin`). Leave it unset to serve the admin dashboard on the app's host name.

- Type: text
- Default: the value of [`DOCSPRING_APP_DOMAIN_NAME`](#docspring_app_domain_name)
- Example: `admin.docspring.example.com`
- Formerly: `ADMIN_DOMAIN_NAME`

### DOCSPRING_ALLOW_ALL_HOSTS

Answers requests for any host name. By default DocSpring only answers requests for its own host names (the domain, app, API, admin and asset host names), which protects against DNS rebinding attacks, so leave this off unless you can't list every host name.

- Type: on/off
- Formerly: `ALLOW_ALL_HOSTS`

### DOCSPRING_API_DOMAIN_NAME

The host name for API requests (`/api/v1`) and the API URLs in the app. The API also answers on the app's host name. It can't include a port: put the port in `DOCSPRING_DOMAIN_NAME`.

- Type: text
- Default: the value of [`DOCSPRING_DOMAIN_NAME`](#docspring_domain_name)
- Example: `api.docspring.example.com`
- Formerly: `API_DOMAIN_NAME`

### DOCSPRING_APP_DOMAIN_NAME

The host name for the web app (sign-in, templates, submissions) and the links in emails. It can't include a port: put the port in `DOCSPRING_DOMAIN_NAME`.

- Type: text
- Default: the value of [`DOCSPRING_DOMAIN_NAME`](#docspring_domain_name)
- Example: `app.docspring.example.com`
- Formerly: `APP_DOMAIN_NAME`

### DOCSPRING_ASSET_HOST

A host name, such as a CDN in front of DocSpring, that serves the compiled JavaScript, CSS, fonts and images. Pages load their assets from it and allow it in their Content Security Policy. Leave it unset to serve assets from the app.

- Type: text
- Example: `assets.docspring.example.com`
- Formerly: `ASSET_HOST`

### DOCSPRING_DOMAIN_NAME

The host name (or IP address) people use to reach DocSpring. The app, API and admin host names default to it. You can add a port, e.g. `docspring.example.com:8080`; the port is then used in every URL DocSpring generates.

- **Required**
- Type: text
- Example: `docspring.example.com`
- Formerly: `DOMAIN_NAME`

### DOCSPRING_PUBLIC_PORT

The port people connect to (your load balancer or proxy), used in every URL DocSpring generates. A port in `DOCSPRING_DOMAIN_NAME` takes precedence. Leave it unset for the standard ports (80 and 443).

- Type: whole number
- Example: `8080`
- Formerly: `PUBLIC_PORT`, `APP_PORT`

## Security and sign-in

### DOCSPRING_CSP_REPORT_ONLY

Sends the Content Security Policy in report-only mode: browsers report violations to `DOCSPRING_CSP_REPORT_URI` without blocking anything. Only used when `DOCSPRING_CSP_REPORT_URI` is set.

- Type: on/off
- Formerly: `CSP_REPORT_ONLY`

### DOCSPRING_CSP_REPORT_URI

Where Content Security Policy violation reports go. Needs `DOCSPRING_ENABLE_CSP_HEADERS`. Browsers send them to the app's `/csp_reports`, and a background job forwards each one to this URL without the query strings, fragments and tokens in its URLs, and without reports caused by browser extensions or Google Translate. A path instead of a URL is used as the policy's `report-uri` as it is.

- Type: text
- Example: `https://csp-reports.example.com/report`
- Formerly: `CSP_REPORT_URI`

### DOCSPRING_DISABLE_BECOME_USER

Turns off the admin dashboard's "Become User" feature, which lets an admin sign in as another user.

- Type: on/off
- Formerly: `DISABLE_BECOME_USER`

### DOCSPRING_DISABLE_PWNED_PASSWORD_CHECK

Turns off the check that rejects new passwords found in known data breaches ([Pwned Passwords](https://haveibeenpwned.com/Passwords)). It allows weak passwords, so only turn it off on a test server or an install without internet access.

- Type: on/off
- Formerly: `DISABLE_PWNED_PASSWORD_CHECK`

### DOCSPRING_DISABLE_SIGN_UP

Hides the sign-up link on the sign-in page and leaves sign-up out of the "please sign in" message. It doesn't turn off the sign-up page itself.

- Type: on/off
- Formerly: `DISABLE_SIGN_UP`

### DOCSPRING_ENABLE_CSP_HEADERS

Sends a Content Security Policy header with every HTML page.

- Type: on/off
- Formerly: `ENABLE_CSP_HEADERS`

### DOCSPRING_FORCE_SSL

Generates `https` URLs, and redirects the app and admin host names to HTTPS with an HSTS header that covers subdomains. Turn it on when DocSpring is behind a load balancer or proxy that handles TLS. When it's off, every URL DocSpring generates uses `http`.

- Type: on/off
- Formerly: `FORCE_SSL`

### DOCSPRING_GITHUB_OAUTH_CLIENT_ID

The client ID of a GitHub OAuth app, for "Sign in with GitHub". It's turned on when this and `DOCSPRING_GITHUB_OAUTH_CLIENT_SECRET` are both set.

- Type: text
- Formerly: `GITHUB_OAUTH_CLIENT_ID`

### DOCSPRING_GITHUB_OAUTH_CLIENT_SECRET

The client secret of the GitHub OAuth app in `DOCSPRING_GITHUB_OAUTH_CLIENT_ID`.

- Type: text
- Formerly: `GITHUB_OAUTH_CLIENT_SECRET`

### DOCSPRING_GOOGLE_OAUTH_CLIENT_ID

The client ID of a Google OAuth client, for "Sign in with Google". It's turned on when this and `DOCSPRING_GOOGLE_OAUTH_CLIENT_SECRET` are both set.

- Type: text
- Formerly: `GOOGLE_OAUTH_CLIENT_ID`

### DOCSPRING_GOOGLE_OAUTH_CLIENT_SECRET

The client secret of the Google OAuth client in `DOCSPRING_GOOGLE_OAUTH_CLIENT_ID`.

- Type: text
- Formerly: `GOOGLE_OAUTH_CLIENT_SECRET`

### DOCSPRING_MANDATORY_OTP

Makes every user set up two-factor authentication before they can use the app.

- Type: on/off
- Formerly: `MANDATORY_OTP`

### DOCSPRING_OPENID_AUTOJOIN_ACCOUNT_ID

An account ID. People who sign in with OpenID Connect for the first time get a user in this account instead of a new account of their own.

- Type: text
- Example: `acc_1234567890abcdef`
- Formerly: `OPENID_AUTOJOIN_ACCOUNT_ID`

### DOCSPRING_OPENID_AUTOLINK_ACCOUNTS

When someone signs in with OpenID Connect and a user already has their verified email address, links the OpenID Connect account to that user and signs them in. Users with two-factor authentication still enter their code.

- Type: on/off
- Formerly: `OPENID_AUTOLINK_ACCOUNTS`

### DOCSPRING_OPENID_CLIENT_AUTH_METHOD

How DocSpring sends its client ID and secret to your provider's token endpoint: `basic` uses an HTTP basic auth header and `jwt_bearer` a signed JWT. `query` (the default), or any other value, sends them as request parameters.

- Type: text
- Default: `query`
- Formerly: `OPENID_CLIENT_AUTH_METHOD`

### DOCSPRING_OPENID_CLIENT_ID

The client ID DocSpring uses with your OpenID Connect provider. OpenID Connect sign-in is turned on when this and `DOCSPRING_OPENID_CLIENT_SECRET` are both set.

- Type: text
- Formerly: `OAUTH_CLIENT_ID`, `OPENID_CLIENT_ID`

### DOCSPRING_OPENID_CLIENT_SECRET

The client secret for `DOCSPRING_OPENID_CLIENT_ID`.

- Type: text
- Formerly: `OAUTH_CLIENT_SECRET`, `OPENID_CLIENT_SECRET`

### DOCSPRING_OPENID_DISCOVERY

Reads your provider's endpoints and signing keys from its discovery document (at `DOCSPRING_OPENID_ISSUER`). On by default. Turn it off for a provider without discovery: DocSpring then uses `DOCSPRING_OPENID_HOST`, `DOCSPRING_OPENID_PORT` and `DOCSPRING_OPENID_SCHEME` with the standard endpoint paths.

- Type: on/off
- Formerly: `OPENID_DISCOVERY`

### DOCSPRING_OPENID_HOST

Your OpenID Connect provider's host name.

- Type: text
- Example: `login.example.com`
- Formerly: `OPENID_HOST`

### DOCSPRING_OPENID_ICON_URL

An image URL for the OpenID Connect sign-in button and the user's linked accounts.

- Type: text
- Example: `https://login.example.com/logo.png`
- Formerly: `OPENID_ICON_URL`

### DOCSPRING_OPENID_ISSUER

Your OpenID Connect provider's issuer URL.

- Type: text
- Example: `https://login.example.com`
- Formerly: `OPENID_ISSUER`

### DOCSPRING_OPENID_PORT

Your OpenID Connect provider's port. Leave it unset for the scheme's standard port.

- Type: whole number
- Example: `443`
- Formerly: `OPENID_PORT`

### DOCSPRING_OPENID_PROVIDER_NAME

The provider name on the OpenID Connect sign-in button and in the user's linked accounts.

- Type: text
- Default: `OpenID`
- Example: `Okta`
- Formerly: `OPENID_PROVIDER_NAME`

### DOCSPRING_OPENID_REDIRECT_URI

The callback URL you registered with your OpenID Connect provider: `/users/auth/openid_connect/callback` on the app's host name.

- Type: text
- Example: `https://app.docspring.example.com/users/auth/openid_connect/callback`
- Formerly: `OPENID_REDIRECT_URI`

### DOCSPRING_OPENID_SCHEME

Your OpenID Connect provider's URL scheme: `https` or `http`.

- Type: text
- Example: `https`
- Formerly: `OPENID_SCHEME`

### DOCSPRING_OPENID_UID_FIELD

The claim in your provider's user info that identifies a person.

- Type: text
- Default: `sub`
- Example: `preferred_username`
- Formerly: `OPENID_UID_FIELD`

### DOCSPRING_RATE_LIMIT_SIGNUPS

Allows one sign-up per IP address per hour.

- Type: on/off
- Formerly: `RATE_LIMIT_SIGNUPS`

### DOCSPRING_RECAPTCHA_SECRET_KEY

The secret key for Google reCAPTCHA on the sign-up page. Sign-ups are checked when it's set; set `DOCSPRING_RECAPTCHA_SITE_KEY` too, to show the reCAPTCHA.

- Type: text
- Formerly: `RECAPTCHA_SECRET_KEY`

### DOCSPRING_RECAPTCHA_SITE_KEY

The site key for Google reCAPTCHA. The sign-up page shows a reCAPTCHA when it's set.

- Type: text
- Formerly: `RECAPTCHA_SITE_KEY`

### DOCSPRING_REQUIRE_RAILS_CONSOLE_AUTH

Asks for an admin user's email address, password and two-factor code before a Rails console opens. Only admins with two-factor authentication turned on can open one.

- Type: on/off
- Formerly: `REQUIRE_RAILS_CONSOLE_AUTH`

### DOCSPRING_SUBMISSION_DATA_ENCRYPTION_KEY

The key that encrypts submission data and other sensitive values in the database: 64 hex characters (32 bytes), e.g. from `openssl rand -hex 32`. DocSpring doesn't start without it. Never change it once there's data: values encrypted with the old key can't be read with a new one, so keep a copy somewhere safe.

- **Required**
- Type: text
- Formerly: `SUBMISSION_DATA_ENCRYPTION_KEY`

### SECRET_KEY_BASE

The secret Rails signs and encrypts sessions, cookies and tokens with. Generate it with `openssl rand -hex 64`. Changing it signs everyone out.

- **Required**
- Type: text

## Database and Redis

### DATABASE_URL

The PostgreSQL connection URL, with the user name, password, host, port and database name: `postgres://USER:PASSWORD@HOST:5432/DATABASE`. PostgreSQL 14 or later is required. Values in the URL take precedence over the `DOCSPRING_DATABASE_*` settings.

- Type: text

### DOCSPRING_DATABASE_HOST

The PostgreSQL host, when `DATABASE_URL` doesn't set one.

- Type: text
- Default: `localhost`
- Formerly: `DATABASE_HOST`

### DOCSPRING_DATABASE_NAME

The database name, when `DATABASE_URL` doesn't set one. Defaults to `docspring_production` in production.

- Type: text
- Formerly: `DATABASE_NAME`

### DOCSPRING_DATABASE_PASSWORD

The PostgreSQL password, when `DATABASE_URL` doesn't set one.

- Type: text
- Formerly: `DATABASE_PASSWORD`

### DOCSPRING_DATABASE_PORT

The PostgreSQL port, when `DATABASE_URL` doesn't set one.

- Type: whole number
- Default: `5432`
- Formerly: `DATABASE_PORT`

### DOCSPRING_DATABASE_USERNAME

The PostgreSQL user name, when `DATABASE_URL` doesn't set one.

- Type: text
- Default: `docspring`
- Formerly: `DATABASE_USERNAME`

### REDIS_URL

The Redis connection URL, e.g. `redis://redis:6379`. Redis 6.2 or later is required. Without it, DocSpring connects to Redis on `localhost`, port 6379.

- Type: text

## File storage

### DOCSPRING_AWS_ACCESS_KEY_ID

The AWS access key ID for S3 file storage, with `DOCSPRING_AWS_ACCESS_KEY_SECRET`. Leave both unset to use the AWS SDK's default credentials, such as an EKS Pod Identity or an instance role.

- Type: text
- Formerly: `AWS_ACCESS_KEY_ID`

### DOCSPRING_AWS_ACCESS_KEY_SECRET

The AWS secret access key for `DOCSPRING_AWS_ACCESS_KEY_ID`.

- Type: text
- Formerly: `AWS_ACCESS_KEY_SECRET`

### DOCSPRING_AWS_S3_ROLE_ACCESS_KEY_ID

The AWS access key ID DocSpring uses to assume the IAM roles set up for the AWS S3 integration (uploading PDFs to your users' own buckets), with `DOCSPRING_AWS_S3_ROLE_ACCESS_KEY_SECRET`.

- Type: text
- Default: the value of [`DOCSPRING_AWS_ACCESS_KEY_ID`](#docspring_aws_access_key_id)
- Formerly: `AWS_S3_ROLE_ACCESS_KEY_ID`

### DOCSPRING_AWS_S3_ROLE_ACCESS_KEY_SECRET

The AWS secret access key for `DOCSPRING_AWS_S3_ROLE_ACCESS_KEY_ID`.

- Type: text
- Default: the value of [`DOCSPRING_AWS_ACCESS_KEY_SECRET`](#docspring_aws_access_key_secret)
- Formerly: `AWS_S3_ROLE_ACCESS_KEY_SECRET`

### DOCSPRING_AWS_UPLOADS_S3_BUCKET

The S3 bucket for templates, generated PDFs and other files. Setting it turns on S3 storage. Without S3 or Google Cloud Storage, DocSpring stores files on the local disk (`public/uploads`).

- Type: text
- Example: `docspring-uploads`
- Formerly: `AWS_UPLOADS_S3_BUCKET`

### DOCSPRING_AWS_UPLOADS_S3_REGION

The S3 bucket's region. When it isn't set, DocSpring asks the AWS SDK, which reads `AWS_REGION` or the instance's region.

- Type: text
- Example: `us-east-1`
- Formerly: `AWS_UPLOADS_S3_REGION`

### DOCSPRING_GCS_ACCESS_KEY_ID

A Google Cloud Storage HMAC access key (from the bucket settings' Interoperability tab). Google Cloud Storage is turned on when this, `DOCSPRING_GCS_ACCESS_KEY_SECRET` and `DOCSPRING_GCS_UPLOADS_BUCKET` are set, and S3 isn't.

- Type: text
- Formerly: `GCS_ACCESS_KEY_ID`

### DOCSPRING_GCS_ACCESS_KEY_SECRET

The secret for `DOCSPRING_GCS_ACCESS_KEY_ID`.

- Type: text
- Formerly: `GCS_ACCESS_KEY_SECRET`

### DOCSPRING_GCS_UPLOADS_BUCKET

The Google Cloud Storage bucket for templates, generated PDFs and other files.

- Type: text
- Example: `docspring-uploads`
- Formerly: `GCS_UPLOADS_BUCKET`

### DOCSPRING_S3_ALLOW_PRIVATE_ENDPOINTS

Lets S3 integrations upload to S3-compatible endpoints on private or local networks, over http too, such as your own MinIO. Off by default, so an integration can't reach your internal network.

- Type: on/off
- Formerly: `S3_ALLOW_PRIVATE_ENDPOINTS`

## Email

### DOCSPRING_AWS_SES_ACCESS_KEY_ID

Sends email through Amazon SES in `us-east-1` with this access key ID and `DOCSPRING_AWS_SES_SECRET_ACCESS_KEY`.

- Type: text
- Formerly: `AWS_SES_ACCESS_KEY_ID`

### DOCSPRING_AWS_SES_SECRET_ACCESS_KEY

The AWS secret access key for `DOCSPRING_AWS_SES_ACCESS_KEY_ID`.

- Type: text
- Formerly: `AWS_SES_SECRET_ACCESS_KEY`

### DOCSPRING_DISABLE_EMAILS

Turns email off when no email service is configured, e.g. while you set up an install. It has no effect once an email service is configured.

- Type: on/off
- Formerly: `DISABLE_EMAILS`

### DOCSPRING_EMAIL_DOMAIN_NAME

The domain of DocSpring's default sender addresses (`support@` and `notifications@` this domain) and of the support address in emails and pages.

- Type: text
- Default: the value of [`DOCSPRING_DOMAIN_NAME`](#docspring_domain_name)
- Example: `example.com`
- Formerly: `EMAIL_DOMAIN_NAME`

### DOCSPRING_FROM_EMAIL

The sender of DocSpring's notification and support emails, as a name and address. When it isn't set, they come from `support@` or `notifications@` at `DOCSPRING_EMAIL_DOMAIN_NAME`.

- Type: text
- Example: `Acme Forms <forms@example.com>`
- Formerly: `FROM_EMAIL`

### DOCSPRING_GMAIL_DOMAIN

The domain DocSpring announces to Gmail's SMTP server (HELO), such as your Google Workspace domain.

- Type: text
- Example: `example.com`
- Formerly: `GMAIL_DOMAIN`

### DOCSPRING_GMAIL_PASSWORD

The password (or app password) for `DOCSPRING_GMAIL_USERNAME`.

- Type: text
- Formerly: `GMAIL_PASSWORD`

### DOCSPRING_GMAIL_USERNAME

Sends email through Gmail's SMTP server as this Gmail or Google Workspace user, with `DOCSPRING_GMAIL_PASSWORD`.

- Type: text
- Example: `forms@example.com`
- Formerly: `GMAIL_USERNAME`

### DOCSPRING_MAILGUN_API_KEY

Sends email through Mailgun with this API key.

- Type: text
- Formerly: `MAILGUN_API_KEY`

### DOCSPRING_MAILGUN_DOMAIN

The Mailgun sending domain.

- Type: text
- Default: the value of [`DOCSPRING_DOMAIN_NAME`](#docspring_domain_name)
- Example: `mg.example.com`
- Formerly: `MAILGUN_DOMAIN`

### DOCSPRING_POSTMARK_API_TOKEN

Sends email through Postmark with this server API token. DocSpring uses the first email service that's configured, in this order: Postmark, SendGrid, Mailgun, Gmail, Amazon SES, SMTP.

- Type: text
- Formerly: `POSTMARK_API_TOKEN`

### DOCSPRING_SENDGRID_API_KEY

Sends email through SendGrid's SMTP server with this API key.

- Type: text
- Formerly: `SENDGRID_API_KEY`

### DOCSPRING_SENDGRID_DOMAIN

The domain DocSpring announces to SendGrid's SMTP server (HELO).

- Type: text
- Default: the value of [`DOCSPRING_DOMAIN_NAME`](#docspring_domain_name)
- Formerly: `SENDGRID_DOMAIN`

### DOCSPRING_SENDGRID_PASSWORD

An older way to sign in to SendGrid, with `DOCSPRING_SENDGRID_USERNAME`. Use `DOCSPRING_SENDGRID_API_KEY` instead.

- Type: text
- Formerly: `SENDGRID_PASSWORD`

### DOCSPRING_SENDGRID_USERNAME

An older way to sign in to SendGrid, with `DOCSPRING_SENDGRID_PASSWORD`. Use `DOCSPRING_SENDGRID_API_KEY` instead.

- Type: text
- Formerly: `SENDGRID_USERNAME`

### DOCSPRING_SMTP_AUTHENTICATION

How DocSpring signs in to your SMTP server: `plain`, `login` or `cram_md5`.

- Type: text
- Default: `plain`
- Formerly: `SMTP_AUTHENTICATION`

### DOCSPRING_SMTP_HELO_DOMAIN

The domain DocSpring announces to your SMTP server (HELO).

- Type: text
- Example: `example.com`
- Formerly: `SMTP_HELO_DOMAIN`

### DOCSPRING_SMTP_PASSWORD

The password for `DOCSPRING_SMTP_USERNAME`.

- Type: text
- Formerly: `SMTP_PASSWORD`

### DOCSPRING_SMTP_PORT

Your SMTP server's port.

- Type: whole number
- Default: `587`
- Formerly: `SMTP_PORT`

### DOCSPRING_SMTP_SERVER

Your SMTP server's host name. DocSpring sends email through it when this, `DOCSPRING_SMTP_USERNAME` and `DOCSPRING_SMTP_PASSWORD` are set.

- Type: text
- Example: `smtp.example.com`
- Formerly: `SMTP_SERVER`

### DOCSPRING_SMTP_STARTTLS

Upgrades the connection to your SMTP server with STARTTLS when the server supports it. On by default.

- Type: on/off
- Formerly: `SMTP_STARTTLS`

### DOCSPRING_SMTP_USERNAME

The user name for your SMTP server.

- Type: text
- Formerly: `SMTP_USERNAME`

## PDF generation

### DOCSPRING_DOCUMENT_TOOLS_LAMBDA_FUNCTION

The AWS Lambda function (a name or ARN) that runs the tools that read customers' PDFs and images: PDFBox, qpdf, pdftk, ImageMagick and librsvg. When it's set, DocSpring sends each file to the function as a presigned URL instead of running the tools in its own workers. When it isn't, the workers run them (the Docker image has them). DocSpring's hosted service always uses the function: contact us to set one up.

- Type: text
- Example: `arn:aws:lambda:us-east-1:123456789012:function:document-tools:live`

### DOCSPRING_HTML_PDF_GENERATION_TIMEOUT

How long, in seconds, an HTML template can take to render to a PDF: from 30 to 120.

- Type: whole number
- Default: `45`
- Formerly: `HTML_PDF_GENERATION_TIMEOUT`

### DOCSPRING_HTML_TO_PDF_RENDERER

How HTML templates are rendered to PDFs. `inline` uses the Chromium in the Docker image. `lambda` sends them to DocSpring's HTML-to-PDF AWS Lambda function, which needs that function and its queues in your AWS account and `DOCSPRING_SQS_HTML_TO_PDF_QUEUE_URL`: contact us to set it up.

- Type: text
- Default: `inline`
- Values: `inline`, `lambda`
- Formerly: `HTML_TO_PDF_RENDERER`

### DOCSPRING_PDF_URL_EXPIRATION_TIME

How long, in seconds, PDF download URLs stay valid: from 60 to 604800 (7 days). DocSpring uses the nearest limit for a value outside that range.

- Type: whole number
- Default: `86400`
- Formerly: `PDF_URL_EXPIRATION_TIME`

### DOCSPRING_SQS_HTML_TO_PDF_QUEUE_URL

The SQS queue URL for the `lambda` HTML-to-PDF renderer.

- Type: text
- Formerly: `SQS_HTML_TO_PDF_QUEUE_URL`

## API

### DOCSPRING_ADMIN_API_TOKEN

A token for the Admin API (exporting and importing templates and accounts, among other things), e.g. from `openssl rand -hex 32`. Authenticate with HTTP basic auth, using `ADMIN_API_TOKEN` as the user name and this token as the password. It acts as the first admin user and works for the Admin API and the API, but not the web app.

- Type: text
- Formerly: `ADMIN_API_TOKEN`

### DOCSPRING_FAILED_API_AUTH_ID_IGNORELIST

API token IDs whose failed API authentications aren't recorded, e.g. an old integration that keeps retrying with a deleted token.

- Type: comma-separated list
- Formerly: `FAILED_API_AUTH_ID_IGNORELIST`

### DOCSPRING_STRICT_PARAM_SCHEMA_VALIDATION

Rejects API requests whose parameters don't match the endpoint's schema, with a 422 error. When it's off, endpoints that don't enforce their schema yet log the mismatch and carry on.

- Type: on/off
- Formerly: `STRICT_PARAM_SCHEMA_VALIDATION`

### DOCSPRING_SYNCHRONOUS_API

Makes API requests that generate PDFs wait until the PDFs are ready before responding. Add `?wait=false` to a request to get a pending response straight away. Each waiting request holds a web server thread, so this suits moderate volumes.

- Type: on/off
- Formerly: `SYNCHRONOUS_API`

### DOCSPRING_SYNCHRONOUS_TIMEOUT_LIMIT

How long, in seconds, a request waits for its PDF when `DOCSPRING_SYNCHRONOUS_API` is on. `REQUEST_TIMEOUT` also limits every web request, so raise that as well.

- Type: whole number
- Default: `300`
- Formerly: `SYNCHRONOUS_TIMEOUT_LIMIT`

## Branding and customization

### DOCSPRING_AFTER_SIGN_IN_PATH

The page people see after they sign in, as a path, unless they were on their way to another page.

- Type: text
- Example: `/templates`
- Formerly: `AFTER_SIGN_IN_PATH`

### DOCSPRING_FAVICON_URL

A URL for the app's favicon.

- Type: text
- Example: `https://example.com/favicon.ico`
- Formerly: `FAVICON_URL`

### DOCSPRING_PAGE_TITLE

Replaces the title of every app page (the text in the browser tab).

- Type: text
- Example: `Acme Forms`
- Formerly: `PAGE_TITLE`

### DOCSPRING_SESSIONS_APP_TITLE

Text shown in place of the DocSpring logo on the sign-in, sign-up and password pages.

- Type: text
- Example: `Acme Forms`
- Formerly: `SESSIONS_APP_TITLE`

### DOCSPRING_SESSIONS_REMOVE_FORGOT_PASSWORD

Hides the "Forgot password?" link on the sign-in page.

- Type: on/off
- Formerly: `SESSIONS_REMOVE_FORGOT_PASSWORD`

### DOCSPRING_SUPPORT_URL

The page the template editor's help button opens. Defaults to DocSpring's template editor docs.

- Type: text
- Example: `https://example.com/help`
- Formerly: `SUPPORT_URL`

## Templates and forms

### COMMONFORMS_LITE_MODELS_DIR

Where the AI field detection model files are. The `-ai-field-detection` Enterprise image sets it, and DocSpring passes it on to the detector.

- Type: text

### DOCSPRING_AI_FIELD_DETECTION_ENABLED

Runs AI field detection in DocSpring's own workers. The `-ai-field-detection` Enterprise image turns it on and includes the models. Detection needs about 1 GB of worker memory for each page it's detecting.

- Type: on/off
- Formerly: `AI_FIELD_DETECTION_ENABLED`

### DOCSPRING_AI_FIELD_NAMING_API_KEY

The API key for AI field naming: a Claude API key for `anthropic`, an OpenAI API key for `openai` (both required), or an optional Amazon Bedrock API key for `bedrock`.

- Type: text
- Formerly: `AI_FIELD_NAMING_API_KEY`

### DOCSPRING_AI_FIELD_NAMING_AWS_ACCESS_KEY_ID

An AWS access key for the `bedrock` provider, with `DOCSPRING_AI_FIELD_NAMING_AWS_SECRET_ACCESS_KEY`. Without a key pair or a Bedrock API key, DocSpring uses the default AWS credentials (an instance or pod role).

- Type: text
- Formerly: `AI_FIELD_NAMING_AWS_ACCESS_KEY_ID`

### DOCSPRING_AI_FIELD_NAMING_AWS_REGION

The AWS region for the `bedrock` provider. Defaults to `AWS_REGION`.

- Type: text
- Default: the value of [`AWS_REGION`](#aws_region)
- Example: `us-east-1`
- Formerly: `AI_FIELD_NAMING_AWS_REGION`

### DOCSPRING_AI_FIELD_NAMING_AWS_SECRET_ACCESS_KEY

The secret for `DOCSPRING_AI_FIELD_NAMING_AWS_ACCESS_KEY_ID`.

- Type: text
- Formerly: `AI_FIELD_NAMING_AWS_SECRET_ACCESS_KEY`

### DOCSPRING_AI_FIELD_NAMING_BATCH_SIZE

How many fields AI field naming sends to the model in each request.

- Type: whole number
- Default: `30`
- Formerly: `AI_FIELD_NAMING_BATCH_SIZE`

### DOCSPRING_AI_FIELD_NAMING_MODEL

The model that names AI-detected fields. Defaults to `claude-opus-5-5`, or `gpt-5.5` for the `openai` provider. For `bedrock` in a region outside the US and EU, set a full Bedrock inference profile ID, such as `au.anthropic.claude-opus-5-5`.

- Type: text
- Default: `claude-opus-5-5`
- Formerly: `AI_FIELD_NAMING_MODEL`

### DOCSPRING_AI_FIELD_NAMING_PROVIDER

Turns on AI field naming, which names, types and describes the fields AI field setup detects, with one of: `bedrock` (Claude on Amazon Bedrock), `anthropic` (the Claude API) or `openai` (the OpenAI API). `anthropic` and `openai` need `DOCSPRING_AI_FIELD_NAMING_API_KEY`, and `bedrock` needs a region. When it isn't set, AI field naming is off. DocSpring won't start with an unknown provider or a missing key or region.

- Type: text
- Example: `anthropic`
- Formerly: `AI_FIELD_NAMING_PROVIDER`

### DOCSPRING_AI_FIELD_SETUP_CONCURRENCY

How many AI field setup jobs each worker runs at once. Field detection in DocSpring's workers needs about 1 GB of memory per page, so lower it on small workers.

- Type: whole number
- Default: `4`
- Formerly: `AI_FIELD_SETUP_CONCURRENCY`

### DOCSPRING_DISABLE_NEW_HTML_TEMPLATES

Hides the option to create an HTML template on the new template page, so people create PDF templates.

- Type: on/off
- Formerly: `DISABLE_NEW_HTML_TEMPLATES`

### DOCSPRING_FIELD_DEFAULTS_ALIGNMENT

The horizontal text alignment of new fields in the template editor. When it isn't set, new fields use the template's default alignment (left unless changed).

- Type: text
- Values: `left`, `center`, `right`
- Formerly: `FIELD_DEFAULTS_ALIGNMENT`

### DOCSPRING_FIELD_DEFAULTS_MULTILINE

Makes new text fields multiline in the template editor.

- Type: on/off
- Formerly: `FIELD_DEFAULTS_MULTILINE`

### DOCSPRING_FIELD_DEFAULTS_REQUIRED

Makes new fields required in the template editor. On by default: turn it off to make new fields optional.

- Type: on/off
- Formerly: `FIELD_DEFAULTS_REQUIRED`

### DOCSPRING_FIELD_DEFAULTS_VALIGNMENT

The vertical text alignment of new fields in the template editor.

- Type: text
- Default: `bottom`
- Values: `top`, `center`, `bottom`
- Formerly: `FIELD_DEFAULTS_VALIGNMENT`

### DOCSPRING_FIELD_NAME_DEBOUNCE_INTERVAL

How long, in milliseconds, the template editor waits after you stop typing a field's name before it applies it. Raise it if renaming fields is slow on large templates.

- Type: whole number
- Default: `350`
- Formerly: `FIELD_NAME_DEBOUNCE_INTERVAL`

### DOCSPRING_GOOGLE_PLACES_PUBLIC_API_KEY

A Google Maps JavaScript API key with the Places library, for address suggestions in web forms. Browsers receive it, so restrict it to your domains in the Google Cloud console.

- Type: text
- Formerly: `GOOGLE_PLACES_PUBLIC_API_KEY`

### DOCSPRING_TEMPLATE_DEFAULTS_ALLOW_ADDITIONAL_PROPERTIES

Turns on "Allow additional properties" for new templates, so they accept data for fields they don't have.

- Type: on/off
- Formerly: `TEMPLATE_DEFAULTS_ALLOW_ADDITIONAL_PROPERTIES`

### DOCSPRING_TEMPLATE_DEFAULT_FONT_SIZE

The default font size for new templates, unless the account has its own template defaults. When it isn't set, it's 11.

- Type: whole number
- Example: `10`
- Formerly: `TEMPLATE_DEFAULT_FONT_SIZE`

### DOCSPRING_TEMPLATE_DEFAULT_TEXT_COLOR

The default text color for new templates, as a hex color without `#`, unless the account has its own template defaults. When it isn't set, it's `444444`.

- Type: text
- Example: `000000`
- Formerly: `TEMPLATE_DEFAULT_TEXT_COLOR`

### DOCSPRING_TEMPLATE_DEFAULT_TYPEFACE

The default font for new templates (a name from the template editor's font list), unless the account has its own template defaults. When it isn't set, it's Helvetica.

- Type: text
- Example: `Courier`
- Formerly: `TEMPLATE_DEFAULT_TYPEFACE`

## Monitoring and error reporting

### DD_TRACE_AGENT_PORT

The Datadog agent's trace port, for `DOCSPRING_ENABLE_DATADOG`.

- Type: whole number
- Default: `8126`

### DOCSPRING_AWS_CLOUDWATCH_ACCESS_KEY_ID

The access key ID of an IAM user with the `CloudWatchAgentServerPolicy` policy, for CloudWatch metrics, with `DOCSPRING_AWS_CLOUDWATCH_ACCESS_KEY_SECRET`. Not needed with an EKS Pod Identity or IAM Roles for Service Accounts.

- Type: text
- Formerly: `AWS_CLOUDWATCH_ACCESS_KEY_ID`

### DOCSPRING_AWS_CLOUDWATCH_ACCESS_KEY_SECRET

The secret access key for `DOCSPRING_AWS_CLOUDWATCH_ACCESS_KEY_ID`.

- Type: text
- Formerly: `AWS_CLOUDWATCH_ACCESS_KEY_SECRET`

### DOCSPRING_AWS_CLOUDWATCH_METRIC_NAMESPACE

The CloudWatch namespace for the app's metrics, such as memory use.

- Type: text
- Default: `DocSpringRails`
- Formerly: `AWS_CLOUDWATCH_METRIC_NAMESPACE`

### DOCSPRING_AWS_CLOUDWATCH_REGION

The CloudWatch region. DocSpring sends Sidekiq queue metrics and app metrics to CloudWatch when this is set, along with the CloudWatch access keys or an AWS workload identity (EKS Pod Identity or IAM Roles for Service Accounts).

- Type: text
- Example: `us-east-1`
- Formerly: `AWS_CLOUDWATCH_REGION`

### DOCSPRING_AWS_CLOUDWATCH_SIDEKIQ_NAMESPACE

The CloudWatch namespace for Sidekiq's queue metrics.

- Type: text
- Default: `Sidekiq`
- Formerly: `AWS_CLOUDWATCH_SIDEKIQ_NAMESPACE`

### DOCSPRING_ENABLE_DATADOG

Sends request traces to a Datadog agent at the container's default gateway (the host), on `DD_TRACE_AGENT_PORT`.

- Type: on/off
- Formerly: `ENABLE_DATADOG`

### DOCSPRING_ENABLE_STATSD

Sends counts and timings of PDF and webhook jobs to the StatsD server at `DOCSPRING_STATSD_ADDR`.

- Type: on/off
- Formerly: `ENABLE_STATSD`

### DOCSPRING_PUMA_LOG_REQUESTS

Logs every request in Puma's own format too, alongside DocSpring's request logs.

- Type: on/off
- Formerly: `PUMA_LOG_REQUESTS`

### DOCSPRING_REPORT_MEMORY_GROWTH_METRIC

Reports each web request's memory use and growth to CloudWatch (see `DOCSPRING_AWS_CLOUDWATCH_REGION`).

- Type: on/off
- Formerly: `REPORT_MEMORY_GROWTH_METRIC`

### DOCSPRING_REPORT_MEMORY_GROWTH_THRESHOLD

The smallest memory growth, in MB, that `DOCSPRING_REPORT_MEMORY_GROWTH_METRIC` reports. `0` reports every change.

- Type: whole number
- Default: `10`
- Formerly: `REPORT_MEMORY_GROWTH_THRESHOLD`

### DOCSPRING_SENTRY_JS_DSN

The Sentry DSN for browser errors. It's included in every page. Leave it unset to turn browser error reporting off.

- Type: text
- Formerly: `SENTRY_JS_DSN`

### DOCSPRING_STATSD_ADDR

The StatsD server for `DOCSPRING_ENABLE_STATSD`, as `host:port`.

- Type: text
- Example: `statsd.example.com:8125`
- Formerly: `STATSD_ADDR`

### PUMA_CLOUDWATCH_AWS_ACCESS_KEY_ID

An AWS access key ID for sending Puma's stats to CloudWatch, with `PUMA_CLOUDWATCH_AWS_SECRET_ACCESS_KEY`. Leave both unset to use the AWS SDK's default credentials.

- Type: text

### PUMA_CLOUDWATCH_AWS_REGION

The CloudWatch region for Puma's stats. Defaults to the AWS SDK's region.

- Type: text
- Example: `us-east-1`

### PUMA_CLOUDWATCH_AWS_SECRET_ACCESS_KEY

The AWS secret access key for `PUMA_CLOUDWATCH_AWS_ACCESS_KEY_ID`.

- Type: text

### PUMA_CLOUDWATCH_DIMENSION_NAME

The CloudWatch dimension name for Puma's stats.

- Type: text
- Default: `App`

### PUMA_CLOUDWATCH_DIMENSION_VALUE

The CloudWatch dimension value for Puma's stats, e.g. your app's name.

- Type: text
- Default: `puma`
- Example: `docspring-web-puma`

### PUMA_CLOUDWATCH_ENABLED

Sends Puma's thread pool stats (backlog, running threads, pool capacity) to CloudWatch.

- Type: on/off

### PUMA_CLOUDWATCH_FREQUENCY

How often, in seconds, Puma's stats are sent to CloudWatch.

- Type: whole number
- Default: `60`

### PUMA_CLOUDWATCH_NAMESPACE

The CloudWatch namespace for Puma's stats.

- Type: text
- Default: `WebServer`

### SENTRY_DSN

The Sentry DSN for server errors. Leave it unset to turn error reporting off.

- Type: text

## Enterprise license

### DOCSPRING_LICENSE

Your DocSpring Enterprise license key. DocSpring Enterprise needs it to run in production.

- **Required**
- Type: text
- Formerly: `FORMAPI_LICENSE`

## Platform

### APP

Set by Convox: the app's name. DocSpring logs it at startup.

- Type: text

### AWS_CONTAINER_CREDENTIALS_FULL_URI

Set by EKS Pod Identity. When it's set, DocSpring uses the pod's IAM role for AWS services that don't have their own access keys.

- Type: text

### AWS_REGION

The default AWS region for the AWS SDK. Required for the `lambda` HTML-to-PDF renderer and the document tools and outbound HTTP Lambda functions.

- Type: text
- Example: `us-east-1`

### AWS_ROLE_ARN

Set by IAM Roles for Service Accounts (IRSA), with `AWS_WEB_IDENTITY_TOKEN_FILE`. When both are set, DocSpring uses the pod's IAM role for AWS services that don't have their own access keys.

- Type: text

### AWS_WEB_IDENTITY_TOKEN_FILE

Set by IAM Roles for Service Accounts (IRSA), with `AWS_ROLE_ARN`.

- Type: text

### BUILD

Set by Convox: the build ID. DocSpring logs it at startup.

- Type: text

### BUILD_DESCRIPTION

Set by Convox: the build's description. DocSpring logs it at startup.

- Type: text

### DOCSPRING_ADMIN_EMAIL

The email address of the first admin user, which `rake db:seed` creates. Required by `rake db:seed`.

- Type: text
- Example: `admin@example.com`
- Formerly: `ADMIN_EMAIL`

### DOCSPRING_ADMIN_NAME

The name of the first admin user, which `rake db:seed` creates.

- Type: text
- Example: `Admin`
- Formerly: `ADMIN_NAME`

### DOCSPRING_ADMIN_PASSWORD

The password of the first admin user, which `rake db:seed` creates. Required by `rake db:seed`. Use a unique password, e.g. from `openssl rand -hex 16`.

- Type: text
- Formerly: `ADMIN_PASSWORD`

### DOCSPRING_ENABLE_GC_COMPACT_AFTER_HEALTHCHECK_REQUESTS

Compacts Ruby's memory (`GC.compact`) after every `DOCSPRING_RUN_GC_COMPACT_AFTER_REQUESTS` full health checks, to keep memory use steady.

- Type: on/off
- Formerly: `ENABLE_GC_COMPACT_AFTER_HEALTHCHECK_REQUESTS`

### DOCSPRING_ENTERPRISE

Runs DocSpring in Enterprise mode. The Enterprise Docker image always runs in Enterprise mode, so you don't need to set this.

- Type: on/off
- Formerly: `ENTERPRISE`

### DOCSPRING_GOOGLE_MAPS_GEOCODING_API_KEY

A Google Maps Geocoding API key, for address lookups. You don't need it to locate IP addresses (for audit trails and submissions): DocSpring uses the GeoLite2 database in its Docker image for those.

- Type: text
- Formerly: `GOOGLE_MAPS_GEOCODING_API_KEY`

### DOCSPRING_IPINFO_IO_API_KEY

An ipinfo.io API key, for locating IP addresses when the GeoLite2 database isn't available. Ignored when `DOCSPRING_GOOGLE_MAPS_GEOCODING_API_KEY` is set.

- Type: text
- Formerly: `IPINFO_IO_API_KEY`

### DOCSPRING_OUTBOUND_HTTP_LAMBDA_FUNCTION

The AWS Lambda function (a name or ARN) that sends requests to URLs customers give DocSpring: webhooks, Slack messages, and the images and PDFs that submissions, merges and templates download. When it's set, the requests leave from the function, outside your network. When it isn't, DocSpring's workers send them.

- Type: text
- Example: `arn:aws:lambda:us-east-1:123456789012:function:outbound-http:live`

### DOCSPRING_RESTRICTED_PATH

The `PATH` that app processes switch to once they've booted, so they can only run the commands DocSpring needs. The Docker image sets it. Used with `DOCSPRING_SHOULD_RESTRICT_PATH`.

- Type: text
- Formerly: `RESTRICTED_PATH`

### DOCSPRING_RUN_GC_COMPACT_AFTER_REQUESTS

How many full health checks pass between memory compactions, with `DOCSPRING_ENABLE_GC_COMPACT_AFTER_HEALTHCHECK_REQUESTS`.

- Type: whole number
- Default: `50`
- Formerly: `RUN_GC_COMPACT_AFTER_REQUESTS`

### DOCSPRING_SHOULD_RESTRICT_PATH

Switches app processes to `DOCSPRING_RESTRICTED_PATH` once they've booted, in production.

- Type: on/off
- Formerly: `SHOULD_RESTRICT_PATH`

### DOCSPRING_SIDEKIQ_CONCURRENCY

How many jobs each Sidekiq worker process runs at once. Each job holds a database connection, so keep it within the database pool.

- Type: whole number
- Default: `3`

### DOCSPRING_TIMEZONE

The app's time zone, as a [Rails time zone name](https://api.rubyonrails.org/classes/ActiveSupport/TimeZone.html), e.g. `UTC` or `Pacific Time (US & Canada)`.

- Type: text
- Default: `Eastern Time (US & Canada)`
- Example: `UTC`
- Formerly: `TIMEZONE`

### GOOD_JOB_MAX_THREADS

Threads each GoodJob worker process runs jobs on in production, such as account exports. Set it to 1 to run exports one at a time on an install with little memory.

- Type: whole number
- Default: `3`

### LANG

The locale for the commands DocSpring runs (PDF tools, Chromium). The Docker image sets `C.UTF-8`.

- Type: text

### NODE_PATH

Where the Node scripts DocSpring runs find their packages. The Docker image sets it. Defaults to `node_tools/node_modules` in the app's directory.

- Type: text

### PATH

Where the commands DocSpring runs are found. In production, app processes switch it to `DOCSPRING_RESTRICTED_PATH` once they've booted.

- Type: text

### PORT

The port the web server listens on. The Docker image sets `4001`.

- Type: whole number
- Default: `3000`

### RACK

Set by Convox: the rack's name. DocSpring logs it at startup.

- Type: text

### RAILS_ENV

The Rails environment. The Docker image sets `production`.

- Type: text
- Default: `development`
- Values: `production`, `development`, `test`

### RAILS_LOG_TO_STDOUT

Writes the Rails log to standard output. The Docker image turns it on.

- Type: on/off

### RAILS_MAX_THREADS

Threads per web server process.

- Type: whole number
- Default: `5`

### RAILS_SERVE_STATIC_FILES

Serves the compiled assets in `public/` from the app, compressed. The Docker image turns it on; turn it off only if a web server or CDN serves them.

- Type: on/off

### RELEASE

Set by Convox: the release ID. DocSpring logs it at startup.

- Type: text

### REQUEST_TIMEOUT

How long, in seconds, a web request can run before it's stopped with a 503 error (in production).

- Type: whole number
- Default: `15`

### SERVICE

Set by Convox: the service's name (web, worker). DocSpring logs it at startup.

- Type: text

### WEB_CONCURRENCY

The number of web server processes, in production.

- Type: whole number
- Default: `2`
