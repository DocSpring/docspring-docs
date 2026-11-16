---
title: Data Retention
description: The settings and scheduled task that let DocSpring Enterprise delete old data, and what it keeps until you turn them on.
sidebar:
  order: 6
---

DocSpring Enterprise deletes nothing by itself until you turn it on. Each kind
of deletion has its own environment variable, and one scheduled task runs them
all. Until a setting is on, its part of the task only logs how many records it
would delete, so you can check the numbers first.

## What needs no setup

These work as soon as you upgrade:

- **Expiry settings.** Each account's **Data retention** tab has expiry settings
  for submissions, signed documents and combined PDFs, and a template can
  override them. They start off for every account. When someone turns one on or
  shortens it, older submissions are deleted too, starting 24 hours after an
  email to the account's owner.
- **Trash.** Deleted templates go to Trash, where they can be restored.
- **Delete Account.** It suspends the account and rejects its API requests. A
  sign-in by any of its users cancels the deletion.
- **Signing records.** When a signature request is completed, DocSpring writes
  a record of the hashes of the signed PDF and its audit trail to your database.
  Nothing deletes these records.

Your nightly timer (`update-submissions` in the `convox.yml` our installer made)
runs the expiry, and restarts any deletion or signing record whose background
job was lost. Keep it scheduled.

## Add the data retention timer

Add this timer to `convox.yml` and deploy:

```yaml
timers:
  data-retention:
    schedule: 30 7 * * ?
    command: run_command deploy DATABASE_URL rake timers:data_retention
    service: command
```

With no settings on, it changes nothing: each run logs what it would delete.
Read the log with `convox logs`.

## Settings

Set each one to `true` to turn it on.

| Setting                                        | What it deletes                                                                                                                                                                                               |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `DOCSPRING_TEST_SUBMISSION_EXPIRY_ENABLED`     | The data and files of test submissions over 30 days old.                                                                                                                                                      |
| `DOCSPRING_TEMPLATE_TRASH_PURGE_ENABLED`       | Templates 30 days after they were moved to Trash. The Trash page then shows each template's date.                                                                                                             |
| `DOCSPRING_REMOVED_USER_PURGE_ENABLED`         | Users 30 days after they were removed from their account.                                                                                                                                                     |
| `DOCSPRING_EXPORT_ARCHIVE_EXPIRY_ENABLED`      | The files of account exports and imports over 6 months old.                                                                                                                                                   |
| `DOCSPRING_SECURITY_HISTORY_RETENTION_ENABLED` | Audit rows, sign-in records, visits, events and records of sent emails over 3 years old.                                                                                                                      |
| `DOCSPRING_SIGNING_RECORD_PURGE_ENABLED`       | A signed document's signers, events, signature images and audit trail, when the document expires. See below before you turn it on.                                                                            |
| `DOCSPRING_REQUESTED_ACCOUNT_PURGES_ENABLED`   | The data of accounts deleted with **Delete Account**, 14 days later.                                                                                                                                          |
| `DOCSPRING_ACCOUNT_DELETION_NOTICES_ENABLED`   | Nothing. It emails the users of accounts that have been suspended and unused for over a year that their data will be deleted in 30 days. It sends nothing until `DOCSPRING_ACCOUNT_PURGES_ENABLED` is on too. |
| `DOCSPRING_ACCOUNT_PURGES_ENABLED`             | The data of those accounts, once the 30 days of their notice are over.                                                                                                                                        |

Every setting is described on the
[Environment Variables](/docs/enterprise/configuration/environment-variables/#data-retention)
page.

## Signed documents

Until `DOCSPRING_SIGNING_RECORD_PURGE_ENABLED` is on, a signed document that
expires loses its data and PDF but keeps its signature requests, signature
images and audit trail, as it did before. Accounts with signed documents are
also never purged.

Once it's on, expiry deletes all of a signed document's data, and only the
record of its hashes stays in your database. That record holds no personal
data. Documents signed before you upgraded have no record yet, so write them
first:

1. Set `DOCSPRING_SIGNING_RECORD_BACKFILL_ENABLED=true`.
2. Run `convox run command run_command deploy DATABASE_URL rake signing_records:backfill_all`.
   It can be stopped and run again, and it carries on from what's missing.
3. Run `rake signing_records:reconcile` the same way. It prints how many
   completed signings have a record. Carry on when it shows zero missing.
4. Set `DOCSPRING_SIGNING_RECORD_PURGE_ENABLED=true`.
5. Signed documents that had already expired still hold their signing data. Run
   `rake signing_records:purge_expired` to count them, and again with
   `APPLY=true` to delete it.

DocSpring Enterprise has no public page for checking a PDF against these
records. They are in the `signing_records` table of your database.

## Deleting accounts

With `DOCSPRING_REQUESTED_ACCOUNT_PURGES_ENABLED` and
`DOCSPRING_SIGNING_RECORD_PURGE_ENABLED` both on, the **Delete Account** dialog
and its email give the date the account's data will be deleted, 14 days later.
Otherwise they say that the data stays until an administrator removes it, and
you delete the account in the admin dashboard.

The emails that tell someone their account was suspended mention the deletion of
unused accounts only when `DOCSPRING_ACCOUNT_PURGES_ENABLED`,
`DOCSPRING_ACCOUNT_DELETION_NOTICES_ENABLED` and
`DOCSPRING_SIGNING_RECORD_PURGE_ENABLED` are all on.

To keep one account's data whatever the settings say, turn on the
`hold_data_purges` feature for it at `/flipper` on your admin domain (as an admin
with the owner or developer role).

## Data that older versions kept

Before this release, a submission that expired kept some of its details in the
database: its metadata, the submitter's IP address and location, and the copies
of its data in audit rows and in webhook and API logs. Expiry now clears those
too. To clear them from submissions that expired before you upgraded, run
`rake submissions:clear_expired_data`, which counts them, and then again with
`APPLY=true`.
