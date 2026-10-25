---
title: Convox Console
description: What the hosted Convox Console offers, and why the DocSpring Enterprise install manages its Convox Rack with the CLI instead.
sidebar:
  order: 3
---

The Convox Console is a free hosted service that manages and backs up your Convox
Rack. It also offers a web interface, Slack notifications, workflows, a GitHub
integration, audit logs and paid support plans.

From [Convox Docs: Rack Management](https://docs.convox.com/management/console-rack-management/):

> When you install a Rack from your CLI, the Terraform state (and subsequently the
> ability to update it) is kept locally. If you want your teammates to be able to
> manage, interact and update the Rack with you, you should move the Rack to be
> owned by an organization within the Convox Console.

Convox also offers a self-hosted console that you can install and run inside your
own Rack. Contact the Convox sales team at
[sales@convox.com](mailto:sales@convox.com) to learn more.

## The DocSpring install doesn't use the Convox Console

:::caution
The DocSpring Enterprise install script sets up a self-hosted, open source Convox
Rack, which you manage with the Convox CLI. It also adds resources for Postgres,
Redis and an S3 bucket to your Terraform config.
:::

To move your rack to the Convox Console, first remove the RDS, ElastiCache and S3
resources that the install script added to your config. You can manage them in a
separate Terraform config. Once you're running an unmodified Convox Rack, you can
move it to the Convox Console with the `convox rack mv` command.

See the [Convox docs](https://docs.convox.com/management/console-rack-management/)
for more information.
