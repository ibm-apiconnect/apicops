apicops
===============

<!-- toc -->
* [Requirements](#requirements)
* [Usage](#usage)
* [Commands](#commands)
<!-- tocstop -->

# Requirements
In order to run `apicops` you need to have `kubectl` installed locally.
Then set the `KUBECONFIG` variable to point to your kubeconfig file and `apicops` will pick it up from there.

```sh-session
$ export KUBECONFIG=/home/user/my.kubeconfig
$ apicops
```

If running inside an API Connect OVA file then run `apicops` as root (`sudo su -`) and it will automatically pick up the kubeconfig.

# Usage
<!-- usage -->
```sh-session
$ npm install -g apicops
$ apicops COMMAND
running command...
$ apicops (-v|--version|version)
apicops/0.1.24 linux-x64 node-v10.16.3
$ apicops --help [COMMAND]
USAGE
  $ apicops COMMAND
...
```
<!-- usagestop -->
# Commands
<!-- commands -->
* [`apicops adjust_grace_period PERIOD`](#apicops-adjust_grace_period-period)
* [`apicops cleanup_locks`](#apicops-cleanup_locks)
* [`apicops fix_orphan_webhooks`](#apicops-fix_orphan_webhooks)
* [`apicops fix_snapshot_gw CATALOG`](#apicops-fix_snapshot_gw-catalog)
* [`apicops fix_snapshot_webhook CATALOG`](#apicops-fix_snapshot_webhook-catalog)
* [`apicops get_catalog [CATALOG]`](#apicops-get_catalog-catalog)
* [`apicops get_configured_gateway_service [GATEWAY]`](#apicops-get_configured_gateway_service-gateway)
* [`apicops get_org [ORG]`](#apicops-get_org-org)
* [`apicops get_snapshot_payload_gw_service URL`](#apicops-get_snapshot_payload_gw_service-url)
* [`apicops get_snapshot_webhook URL`](#apicops-get_snapshot_webhook-url)
* [`apicops help [COMMAND]`](#apicops-help-command)
* [`apicops identify_orphan_webhooks`](#apicops-identify_orphan_webhooks)
* [`apicops identify_services_state`](#apicops-identify_services_state)
* [`apicops mark_portal_webhook_subscription_state WEBHOOKSUBSCRIPTIONURL`](#apicops-mark_portal_webhook_subscription_state-webhooksubscriptionurl)
* [`apicops renew_task TASKID`](#apicops-renew_task-taskid)
* [`apicops runbook`](#apicops-runbook)
* [`apicops snapshot_builder URL`](#apicops-snapshot_builder-url)

## `apicops adjust_grace_period PERIOD`

(grace) Adjusts the time between compaction of tombstones.

```
USAGE
  $ apicops adjust_grace_period PERIOD

ARGUMENTS
  PERIOD  (short|long) Set to "short" to cleanup tombstones every 2 mins, or "long", to cleanup every 3 days.

ALIASES
  $ apicops grace
```

## `apicops cleanup_locks`

(locks) Clears out any invalid transaction locks.

```
USAGE
  $ apicops cleanup_locks

ALIASES
  $ apicops locks
```

## `apicops fix_orphan_webhooks`

(fixorphans) Fixes any orphaned webhooks.

```
USAGE
  $ apicops fix_orphan_webhooks

ALIASES
  $ apicops fixorphans
```

## `apicops fix_snapshot_gw CATALOG`

(fixsnapshotgateway) Fixes the bad references of the api in api_urls in the product payload of the snapshot for the first configured gateway service for the given catalog

```
USAGE
  $ apicops fix_snapshot_gw CATALOG

ARGUMENTS
  CATALOG  The catalog, specified as the name or UUID of the catalog with an optional preceeding org name or UUID,
           seperated by a / or :. e.g. mycatalog or myorg/mycatalog

ALIASES
  $ apicops fixsnapshotgateway
```

## `apicops fix_snapshot_webhook CATALOG`

(fixsnapshotwebhook) Fixes the bad references of the api in api_urls in the product payload of the snapshot, at webhook level, for the given org and catalog.

```
USAGE
  $ apicops fix_snapshot_webhook CATALOG

ARGUMENTS
  CATALOG  The catalog, specified as the name or UUID of the catalog with an optional preceeding org name or UUID,
           seperated by a / or :. e.g. mycatalog or myorg/mycatalog

ALIASES
  $ apicops fixsnapshotwebhook
```

## `apicops get_catalog [CATALOG]`

(catalog(s)) With no params lists all catalogs, or with a param looks up a specific catalog based on uuid or name

```
USAGE
  $ apicops get_catalog [CATALOG]

ARGUMENTS
  CATALOG  The id or name of the catalog

ALIASES
  $ apicops catalog
  $ apicops catalogs
```

## `apicops get_configured_gateway_service [GATEWAY]`

(gateway(s)) With no params lists all configured gateway services, or with a param looks up a specific configured gateway service based on uuid or name

```
USAGE
  $ apicops get_configured_gateway_service [GATEWAY]

ARGUMENTS
  GATEWAY  The id or name of the configured gateway service

ALIASES
  $ apicops gateway
  $ apicops gateways
```

## `apicops get_org [ORG]`

(org(s)) With no params lists all organisations, or with a param looks up a specific org based on uuid or name

```
USAGE
  $ apicops get_org [ORG]

ARGUMENTS
  ORG  The id or name of the org

ALIASES
  $ apicops org
  $ apicops orgs
```

## `apicops get_snapshot_payload_gw_service URL`

(checksnapshotgateway) Check for products with reference to missing APIs in gateway snapshots.

```
USAGE
  $ apicops get_snapshot_payload_gw_service URL

ARGUMENTS
  URL  The url of the org and catalog, or catalog of the form catalog or org/catalog. e.g.
       00000000-0000-0000-0000-000000000000/00000000-0000-0000-0000-000000000000 or 00000000-0000-0000-0000-000000000000

ALIASES
  $ apicops checksnapshotgateway
```

## `apicops get_snapshot_webhook URL`

(checksnapshotwebhook) Identifies bad products in the snapshot payload at webhook level that have references to invalid apis for the given org and catalog.

```
USAGE
  $ apicops get_snapshot_webhook URL

ARGUMENTS
  URL  The url of the org and catalog, or catalog of the form catalog or org/catalog. e.g.
       00000000-0000-0000-0000-000000000000/00000000-0000-0000-0000-000000000000 or 00000000-0000-0000-0000-000000000000

ALIASES
  $ apicops checksnapshotwebhook
```

## `apicops help [COMMAND]`

display help for apicops

```
USAGE
  $ apicops help [COMMAND]

ARGUMENTS
  COMMAND  command to show help for

OPTIONS
  --all  see all commands in CLI
```

_See code: [@oclif/plugin-help](https://github.com/oclif/plugin-help/blob/v2.2.0/src/commands/help.ts)_

## `apicops identify_orphan_webhooks`

(getorphans) Lists any orphaned webhooks.

```
USAGE
  $ apicops identify_orphan_webhooks

ALIASES
  $ apicops getorphans
```

## `apicops identify_services_state`

(iss) Identifies the state of any gateway and portal services and returns any associated task ids that are incomplete.  The option is given to carry out additional commands based on the output returned.

```
USAGE
  $ apicops identify_services_state

OPTIONS
  -c, --compact  Use a compact output.

ALIASES
  $ apicops iss
```

## `apicops mark_portal_webhook_subscription_state WEBHOOKSUBSCRIPTIONURL`

(markportalsub) Update the state of the given webhook subscription record to offline_configured.

```
USAGE
  $ apicops mark_portal_webhook_subscription_state WEBHOOKSUBSCRIPTIONURL

ARGUMENTS
  WEBHOOKSUBSCRIPTIONURL  The url of the webhook subscription record to update the state on

ALIASES
  $ apicops markportalsub
```

## `apicops renew_task TASKID`

(task) Re-triggers the provided task id by setting its state to 'new' so that it gets picked up for processing again.

```
USAGE
  $ apicops renew_task TASKID

ARGUMENTS
  TASKID  The id of the task to renew

ALIASES
  $ apicops task
```

## `apicops runbook`

This command allows for a particular runbook scenario to executed.  For scenarios where multiple scripts need to be run it will prompt for inputs / chain script execution

```
USAGE
  $ apicops runbook
```

## `apicops snapshot_builder URL`

(snapshot) Compact the event queue into an up to date snapshot.

```
USAGE
  $ apicops snapshot_builder URL

ARGUMENTS
  URL  The url of the org and catalog, or org, catalog and gateway service, in the form org/catalog or
       org/catalog/gatewayService. e.g. 00000000-0000-0000-0000-000000000000/00000000-0000-0000-0000-000000000000 or
       00000000-0000-0000-0000-000000000000/00000000-0000-0000-0000-000000000000/00000000-0000-0000-0000-000000000000

OPTIONS
  -c, --complete  Replay all events instead of just those that are older than the current time.

ALIASES
  $ apicops snapshot
```
<!-- commandsstop -->
