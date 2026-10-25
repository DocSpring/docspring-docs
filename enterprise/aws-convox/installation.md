---
title: Install DocSpring on AWS with Convox
description: Set up DocSpring Enterprise in your AWS account with Convox, including EKS, Postgres, Redis and an S3 bucket.
sidebar:
  label: Installation
  order: 1
---

:::note[Time required]
About 40 minutes: 10 minutes of setup, then about 30 minutes for the install script to run.
:::

These instructions install DocSpring Enterprise on AWS with Convox. They set up a
highly available, auto-scaling EKS (Kubernetes) cluster to run DocSpring. The
install script also sets up Postgres (RDS), Redis (ElastiCache) and an S3 bucket
for file storage. Convox writes Terraform config for all of these resources, so
once your Convox Rack is running you can manage your infrastructure with the
`terraform` CLI and keep the config in a git repository.

The install uses the free and open source [Convox Rack](https://github.com/convox/convox)
to set up your cluster and resources. Convox also offers a hosted Console that you
can sign up for at [convox.com](https://convox.com/). Your installation doesn't need
it: see [Convox Console](/docs/enterprise/aws-convox/convox-console/).

We also recommend reading the [Convox documentation](https://convox.com/docs).

## Requirements

- An AWS account with administrator permissions.
- The access key ID and secret for DocSpring's Docker registry, and your DocSpring
  license key. We send these to you by email.

## Install the Convox and AWS CLI tools

On macOS:

```bash
brew install convox awscli
convox --version    # Should be at least 3.18.6
aws --version       # Should be at least 2.15.54
```

For other platforms, see the
[Convox installation instructions](https://docs.convox.com/introduction/installation)
and the
[AWS CLI installation instructions](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html).

## Install Ruby and Bundler

The install script needs Ruby 2.5 or later (3.x is fine) and Bundler:

```bash
ruby --version        # Should be at least 2.5
bundler --version     # Should be at least 1.0.0
```

See the [Ruby installation instructions](https://www.ruby-lang.org/en/documentation/installation),
then run `gem install bundler`.

## Create a temporary AWS access key for an admin user

1. Sign in to your AWS account.
2. Go to [AWS Identity and Access Management (IAM)](https://console.aws.amazon.com/iam/home).
3. Click **Users**.
4. Click **Add User**.
5. Set the username to `docspring-enterprise`.
6. Click **Next**.
7. In the **Permissions options** section, select **Attach policies directly**.
8. In the **Permissions policies** section, type `AdministratorAccess` into the search input.
9. Check the box for the **AdministratorAccess** policy (only the first one).

   ![The AWS IAM "Set permissions" step with the AdministratorAccess policy checked](@images/enterprise/iam-attach-administrator-access.png)

10. Click **Next**.
11. Click **Create user**.
12. Click the `docspring-enterprise` user.
13. Click **Security credentials**.

    ![The docspring-enterprise IAM user's summary page, with the Security credentials tab highlighted](@images/enterprise/iam-user-security-credentials.png)

14. Scroll down to the **Access keys** section and click **Create access key**.
15. Click **Other**, then **Next**. (You can ignore the details on this page.)
16. Click **Create access key**.
17. Click **Show** to reveal the secret access key.
18. Copy the access key ID and secret. You'll need them for the install script.

    ![The AWS "Retrieve access keys" page, with the access key and secret access key redacted](@images/enterprise/iam-access-key.png)

## Clone the docspring_enterprise repository

This repository contains a Convox configuration file and the install script that
sets up your infrastructure on AWS. Go to the directory where you keep code (or
your home directory) and run:

```bash
git clone https://github.com/DocSpring/docspring_enterprise.git
cd docspring_enterprise
```

## Run the install script

The install script doesn't change any existing resources or services in your AWS
account. Convox can also remove everything it creates, if you need to uninstall
DocSpring or start again.

Run the install script:

```bash
./install.rb
```

The script asks for your AWS region (default: `us-east-1`), the access key ID and
secret of your AWS admin user, a name for your Convox installation, the access key
ID and secret for DocSpring's Docker registry, your DocSpring license key, and an
email address for the DocSpring admin user. It generates a random password for the
admin user. It then shows a summary and asks you to confirm:

![The install script's summary of the configuration, with credentials, the license key, the admin password and the bucket name redacted](@images/enterprise/install-script-summary.png)

Enter `yes` to continue.

The script now creates all the resources in your AWS account. It creates the EKS
cluster (about 10 to 15 minutes), then the Postgres and Redis resources (another
10 to 15 minutes) and the S3 bucket, and deploys DocSpring. The whole process
takes about 30 minutes.

:::tip
The install script is idempotent: it is safe to run more than once. If it stops
with an error, run it again. It checks the resources that already exist and
carries on where it left off. (Please let us know if it crashes.)
:::

When the script has finished, it prints your DocSpring installation's address and
the admin user's email and password. Sign in with them, then set up your templates
and start using the DocSpring API.

## Pin the Docker image version

The default `convox.yml` uses the latest Docker image. We recommend pinning a
version so that you decide when to upgrade. Add the version tag (e.g.
`:2724aeb4`) to the end of the image URL for every service in `convox.yml`
that has an `image:` line (`web`, `worker` and `command`). For example:

```yaml
image: <account_id>.dkr.ecr.us-east-1.amazonaws.com/docspring/enterprise:2724aeb4
```

The [DocSpring Enterprise releases](https://docspring.com/enterprise-releases)
page lists every version tag.

## Set up a custom domain and SSL certificate

Follow our guide to
[set up a custom domain and SSL certificate](/docs/enterprise/configuration/custom-domain-and-ssl/).

## Revoke the admin user's access key (optional)

You don't need the `docspring-enterprise` admin user once the installation has
finished: Convox creates its own IAM user with limited permissions. To protect your
AWS account, revoke access keys that you no longer use. In IAM, click the
`docspring-enterprise` user, go to the **Security credentials** tab, and click
**Make inactive** on the access key. You can also delete the user.

## Health checks

DocSpring has a health check endpoint at `/health`. When everything is working,
it responds with a 200 status code and the body `success`. It checks the database
connection, the Redis connection and the Rails cache.

`/health/site` is a minimal health check that only checks that the Rails
application has booted and can respond to HTTP requests.

## Back up your Terraform state

Convox creates a Terraform config directory in your home directory. On macOS, it
is at `~/Library/Preferences/convox/racks/<rack name>`.

You can `cd` to this directory to inspect the Terraform config and state, make
changes, and run `terraform plan` and `terraform apply`. See
[Convox CLI and Terraform](/docs/enterprise/aws-convox/convox-cli-and-terraform/).

We recommend initializing a git repository in this directory to back up your
Terraform config and state. Be careful with these files: they contain API tokens
and other sensitive information. If you store the repository on GitHub, encrypt
your `tfstate` file with [git-crypt](https://github.com/AGWA/git-crypt) or a
similar tool.

:::caution
Make sure you back up `terraform.tfstate`. If you lose it, you won't be able to
manage your Convox Rack. It is possible to recreate it by hand, but that takes
hours of work.
:::

## Configuration

The [environment variables](/docs/enterprise/configuration/environment-variables/)
page lists every way to configure DocSpring. You can also visit the `/admin/system`
page on your DocSpring installation to see the configuration variables and their
current values.

Set environment variables with `convox env set VAR=value`. For example:

```bash
convox env set DOCSPRING_POSTMARK_API_TOKEN=<your Postmark server token>
```

- [Custom domain and SSL certificate](/docs/enterprise/configuration/custom-domain-and-ssl/)
- [Environment variables](/docs/enterprise/configuration/environment-variables/)
- [Error reporting](/docs/enterprise/configuration/error-reporting/)
- [Email notifications](/docs/enterprise/configuration/email-notifications/)

You can also use third-party services to monitor your installation:

- Performance monitoring: [Skylight](https://www.skylight.io/) (set
  `SKYLIGHT_AUTHENTICATION`).
- Logs and infrastructure: services such as [New Relic](https://newrelic.com/),
  [Datadog](https://www.datadoghq.com/), [Loggly](https://www.loggly.com/) and
  [Papertrail](https://www.papertrail.com/).
- Uptime and scheduled jobs: services such as
  [UptimeRobot](https://uptimerobot.com/), [NodePing](https://nodeping.com/),
  [Cronitor](https://cronitor.io/) and [Cronhub](https://cronhub.io/). Uptime
  monitors can check the [health check endpoints](#health-checks).

## Further resources

- [Upgrade DocSpring to the latest version](/docs/enterprise/aws-convox/upgrade-docspring/)
  ([latest DocSpring release](https://docspring.com/enterprise-releases))
- [Upgrade Convox and Kubernetes](/docs/enterprise/aws-convox/upgrade-convox-rack/)
  ([latest Convox release](https://github.com/convox/convox/releases))
- [Work with the Convox CLI and your Terraform config](/docs/enterprise/aws-convox/convox-cli-and-terraform/)
- [Convox documentation](https://convox.com/docs)

Contact [support@docspring.com](mailto:support@docspring.com) if you need any help.
