---
title: Upgrade Convox Rack
description: Upgrade your Convox Rack and Kubernetes version with the Convox CLI or by hand with Terraform.
sidebar:
  order: 5
---

You can update your Convox Rack with the Convox CLI, or by hand with the Terraform
CLI.

Every Convox Rack version requires a specific version of Kubernetes (k8s), so
upgrading Convox also keeps Kubernetes up to date. The Convox developers test each
new Kubernetes version carefully, so the latest version of Convox sometimes uses an
older version of Kubernetes.

:::danger
Never update Kubernetes by hand in your AWS console: it may break your Convox Rack.
**Kubernetes version upgrades can't be reverted.** If you update Kubernetes to a
version that Convox doesn't support, you may need to create a new Rack from scratch.
:::

## Prepare to upgrade Convox

First, make sure you have the latest version of the Convox CLI. `convox update`
updates the CLI (not the Rack):

```txt
$ convox --version
3.19.6
$ convox update
Updating to 3.19.7... OK
$ convox --version
3.19.7
```

Run `convox rack` to see the version your Rack is running:

```txt
$ convox rack
Name      docspring-enterprise
Provider  aws
Router    router.abcde12345.convox.cloud
Status    running
Version   3.19.6
```

Read the [Convox release notes on GitHub](https://github.com/convox/convox/releases)
before you upgrade:

![The Convox 3.19.7 release notes on GitHub](@images/enterprise/convox-release-notes.png)

The release notes include a lot of technical details. The important thing to look
for is a Kubernetes version upgrade. Convox is very good at zero-downtime upgrades,
even when it upgrades Kubernetes, but be extra careful when a Kubernetes upgrade is
involved.

## Upgrade the rack with the Convox CLI

Run `convox rack update` to update your rack to the latest version.

Then back up your new Terraform state and config (e.g. commit and push them to your
git repository).

## Upgrade the rack by hand with Terraform

If you need more control over the update, you can update your Terraform config by
hand and run `terraform apply`. This is what the Convox CLI does behind the scenes:
see [Convox CLI and Terraform](/docs/enterprise/aws-convox/convox-cli-and-terraform/).

Instead of running `convox rack update`, you can do the same steps yourself:

1. Update the rack version in your `vars.json` and `main.tf` files.
2. Run `terraform init -upgrade` to update the Convox module and any other
   dependencies.
3. Run `terraform plan` to see which resources will be added, changed and removed.
4. Run `terraform apply`, and type `yes` to approve the changes.

:::note
The Convox Terraform config often shows a lot of irrelevant changes, even when
nothing has changed. These can include updates to IAM roles and policies, and
setting the version to `$Latest` on EKS node groups. You can usually ignore them.
:::

```bash
# Typical output when nothing has changed:
$ terraform apply
...
Plan: 1 to add, 27 to change, 1 to destroy.
```

### How is this different from using the Convox CLI?

- You can skip upgrading all of your Terraform modules by running `terraform init`
  without the `-upgrade` flag. This installs the latest Convox module but doesn't
  change any of your other dependencies (e.g. for custom resources you've added).
- You can review the changes before you apply the update. The Convox CLI applies
  the update without asking for approval.

## Resources

- [Convox Docs: CLI Rack Management](https://docs.convox.com/management/cli-rack-management/)
