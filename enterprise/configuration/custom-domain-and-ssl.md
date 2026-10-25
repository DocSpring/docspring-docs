---
title: Custom Domain and SSL Certificate
description: Serve DocSpring Enterprise on Convox from your own domain, with a free Let's Encrypt SSL certificate.
sidebar:
  label: Custom Domain and SSL
  order: 1
---

Convox uses Let's Encrypt to issue SSL certificates, by running
[cert-manager](https://cert-manager.io/docs/) in your Kubernetes cluster. To verify
your domain, your DocSpring installation must be reachable from the public
internet, and your domain needs a CNAME DNS record that points to your rack's
`convox.cloud` hostname.

## Configure the DNS record

Run `convox rack` to find your rack's `convox.cloud` router hostname:

![The output of convox rack, with the router hostname highlighted](@images/enterprise/convox-rack-router.png)

Create a CNAME record with your router hostname as the target. For example, in
Cloudflare:

![A Cloudflare CNAME record named docspring that targets the rack's router hostname, with proxying turned off](@images/enterprise/cloudflare-cname-record.png)

## Configure the domain in convox.yml and deploy

1. Go to your `docspring_enterprise` directory and open `convox.yml` in your text
   editor.
2. Change the `# domain: docspring.example.com` line to `domain: <your domain>`
   (remove the `#` at the start of the line to uncomment it).
3. Run:

   ```bash
   convox env set DOCSPRING_DOMAIN_NAME=<your domain>
   ./update.sh
   ```

Once the change has been deployed, you can visit your DocSpring installation at
your custom domain.

If it doesn't work straight away, you may need to wait for your DNS record to
propagate so that Let's Encrypt can verify your domain. This usually takes 15 to
20 minutes, but can take up to 24 hours.

:::tip
Debugging Kubernetes and cert-manager issues can be challenging, so please contact
[support@docspring.com](mailto:support@docspring.com) if you get stuck or have any
questions. We can also help you configure cert-manager to verify your domain with
DNS, using a Cloudflare API key.
:::
