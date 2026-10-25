---
title: Uninstall Convox and DocSpring
description: Remove DocSpring Enterprise and every AWS resource that Convox created from your AWS account.
sidebar:
  label: Uninstall
  order: 6
---

You can remove Convox and DocSpring from your AWS account with a few commands.

:::note
It is safe to run the uninstall command in an AWS account that has other
resources. It only deletes the resources that Convox created.
:::

To uninstall DocSpring Enterprise, follow these steps in order:

1. Turn off **Termination Protection** on any resources where you turned it on.
2. Delete every file in the S3 bucket that holds your template PDFs and
   submissions. The bucket's name is in the `DOCSPRING_AWS_UPLOADS_S3_BUCKET`
   environment variable, or `AWS_UPLOADS_S3_BUCKET` on older installations
   (`convox env | grep S3_BUCKET`). You need an AWS access key
   with admin permissions:

   ```bash
   export AWS_ACCESS_KEY_ID=<your admin access key ID>
   export AWS_SECRET_ACCESS_KEY=<your admin secret access key>
   aws s3 rm "s3://<your bucket name>" --recursive
   ```

3. Delete the `docspring` Convox app:

   ```bash
   convox apps delete docspring
   ```

4. Uninstall Convox. This deletes every AWS resource that Terraform and Convox
   manage:

   ```bash
   convox rack uninstall aws <your rack name>
   ```
