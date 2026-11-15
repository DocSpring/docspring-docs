---
title: Differences from the Hosted Service
description: Where DocSpring Enterprise behaves differently from DocSpring's hosted service, and where the rest of the docs don't apply to your install.
---

The rest of the docs describe DocSpring's hosted service. DocSpring Enterprise
runs the same application, so almost all of it applies to your install too. This
page lists what differs.

## Submissions made while the license isn't valid

A submission that wasn't processed because your license wasn't valid keeps its
data until you reprocess it from the submission's menu, once the license is
valid again. When the new submission has made its PDF, the old one expires like
any other failed submission.

On the hosted service, a submission that fails is kept for at least 48 hours and
then expires with the template's or account's
[expiry settings](/docs/template-editor/settings/#expire-submissions).

## Data retention

The hosted service deletes old data on a schedule: test submissions, templates
in Trash, removed users, deleted and unused accounts, and old security history.
Your install deletes none of these until you turn each one on. See
[Data Retention](/docs/enterprise/configuration/data-retention/).

## Signed documents

The hosted service keeps a record of each signed document's hashes for at least
10 years, and anyone can check a PDF against it on a public page. Your install
keeps these records in its own database, with no fixed period and no public
page. See
[Signed documents](/docs/enterprise/configuration/data-retention/#signed-documents).
