apicops
===============

<!-- toc -->
* [About](#about)
* [Installing](#installing)
* [Requirements](#requirements)
* [Usage](#usage)
* [Commands](#commands)
<!-- tocstop -->

# About
`apicops` is a command line interface to IBM API Connect v2018 specifically targetted at Operations teams. It contains commands to check the healthy running of the system as well as some commands to remedy specific problems if encountered.

It is in active development and new versions will be posted here regularly. All suggestions, feedback and bug reports are welcome - please feel free to raise an issue in this repository. Please limit the issues to those specific to `apicops` itself - we cannot accept ones about APIC, they have to be raised via the normal IBM Support route.

Please note this has only been tested against IBM API Connect v2018.4.1.6+.

# Installing
Download the latest binary for your operating system from the Releases tab and rename it to be `apicops`. Note that Linux and Mac will require you to run chmod +x on the downloaded file before you can execute it.

# Requirements
In order to run `apicops` you need to have `kubectl` or something that implements the same CLI as `kubectl`, such as `oc` installed locally. If you aren't using `kubectl` then set the environment variable APICOPS_K8SCLIENT to the name of the Kubernetes client binary, such as `oc`.

If using the openshift client (`oc`) then v4.1.x or greater is required (even if using v3.x openshift cluster).

Then set the `KUBECONFIG` environment variable to point to your kubeconfig file and `apicops` will pick it up from there.

```sh-session
$ export KUBECONFIG=/home/user/my.kubeconfig
$ apicops
```

If running inside an API Connect OVA file then run `apicops` as root (`sudo -i`) and it will automatically pick up the kubeconfig.

# Usage
```sh-session
$ apicops COMMAND
running command...
$ apicops (-v|--version|version)
apicops/0.1.46 linux-x64 node-v10.16.3
$ apicops --help [COMMAND]
USAGE
  $ apicops COMMAND
...
```
# Commands
<!-- commands -->
* [`apicops adjust_grace_period PERIOD`](#apicops-adjust_grace_period-period)
* [`apicops check_data_integrity_all_index_tables`](#apicops-check_data_integrity_all_index_tables)
* [`apicops check_data_integrity_all_link_tables`](#apicops-check_data_integrity_all_link_tables)
* [`apicops clean_cron_jobs`](#apicops-clean_cron_jobs)
* [`apicops clean_oauth_shared_secret URL`](#apicops-clean_oauth_shared_secret-url)
* [`apicops cleanup_locks`](#apicops-cleanup_locks)
* [`apicops cleanup_task_queue`](#apicops-cleanup_task_queue)
* [`apicops clear_subscriber_queues`](#apicops-clear_subscriber_queues)
* [`apicops create_gateway_task GATEWAY`](#apicops-create_gateway_task-gateway)
* [`apicops create_portal_task PORTAL`](#apicops-create_portal_task-portal)
* [`apicops custom_script SCRIPT [PARAMS]`](#apicops-custom_script-script-params)
* [`apicops fix_orphan_webhooks`](#apicops-fix_orphan_webhooks)
* [`apicops fix_snapshot_gw CATALOG`](#apicops-fix_snapshot_gw-catalog)
* [`apicops fix_snapshot_webhook CATALOG`](#apicops-fix_snapshot_webhook-catalog)
* [`apicops get_all_webhooks`](#apicops-get_all_webhooks)
* [`apicops get_catalog [CATALOG]`](#apicops-get_catalog-catalog)
* [`apicops get_configured_gateway_service [GATEWAY]`](#apicops-get_configured_gateway_service-gateway)
* [`apicops get_configured_portal_service [PORTAL]`](#apicops-get_configured_portal_service-portal)
* [`apicops get_gateway_service [GATEWAY]`](#apicops-get_gateway_service-gateway)
* [`apicops get_org [ORG]`](#apicops-get_org-org)
* [`apicops get_snapshot_payload_gw_service URL`](#apicops-get_snapshot_payload_gw_service-url)
* [`apicops get_snapshot_webhook URL`](#apicops-get_snapshot_webhook-url)
* [`apicops get_subscriber_queue_length SUBSCRIBER`](#apicops-get_subscriber_queue_length-subscriber)
* [`apicops get_task TASKID`](#apicops-get_task-taskid)
* [`apicops help [COMMAND]`](#apicops-help-command)
* [`apicops identify_orphan_webhooks`](#apicops-identify_orphan_webhooks)
* [`apicops identify_services_state`](#apicops-identify_services_state)
* [`apicops mark_gateway_catalog_online URLORID`](#apicops-mark_gateway_catalog_online-urlorid)
* [`apicops mark_portal_webhook_subscription_state URLORID`](#apicops-mark_portal_webhook_subscription_state-urlorid)
* [`apicops renew_task TASKID`](#apicops-renew_task-taskid)
* [`apicops send_snapshot SERVICE`](#apicops-send_snapshot-service)
* [`apicops snapshot_builder URL`](#apicops-snapshot_builder-url)
* [`apicops snapshot_validator2 CATALOG`](#apicops-snapshot_validator2-catalog)

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

## `apicops check_data_integrity_all_index_tables`

(checkdataindexes) Checks inconsistencies between the main and index tables (data available in main table and not present in index table. Similarly data present in index table but not in main table).

```
USAGE
  $ apicops check_data_integrity_all_index_tables

ALIASES
  $ apicops checkdataindexes
```

## `apicops check_data_integrity_all_link_tables`

(checkdatalinks) Checks inconsistencies between the link tables and the corresponding index tables.

```
USAGE
  $ apicops check_data_integrity_all_link_tables

ALIASES
  $ apicops checkdatalinks
```

## `apicops clean_cron_jobs`

(cronjobs) Recreate the apim cron jobs.

```
USAGE
  $ apicops clean_cron_jobs

ALIASES
  $ apicops cronjobs
```

## `apicops clean_oauth_shared_secret URL`

(cleanoauth) Clean the shared OAuth secret for the given catalog and gateway service.

```
USAGE
  $ apicops clean_oauth_shared_secret URL

ARGUMENTS
  URL  The catalog and gateway service, identified via name or UUID, in the form, <catalog>/<gateway service>, or
       <org>/<catalog>/<gateway service>

ALIASES
  $ apicops cleanoauth
```

## `apicops cleanup_locks`

(cleanlocks) Clears out any invalid transaction locks.

```
USAGE
  $ apicops cleanup_locks

ALIASES
  $ apicops cleanlocks
```

## `apicops cleanup_task_queue`

(cleantasks) Clear the task queue and then recreate the apim cron jobs.

```
USAGE
  $ apicops cleanup_task_queue

ALIASES
  $ apicops cleantasks
```

## `apicops clear_subscriber_queues`

(clearsubqueues) Clears out all subscriber queues.

```
USAGE
  $ apicops clear_subscriber_queues

ALIASES
  $ apicops clearsubqueues
```

## `apicops create_gateway_task GATEWAY`

(creategatewaytask) Creates a snapshot task for the specified gateway.

```
USAGE
  $ apicops create_gateway_task GATEWAY

ARGUMENTS
  GATEWAY  The configured gateway service ID or URL.

ALIASES
  $ apicops creategatewaytask
```

## `apicops create_portal_task PORTAL`

(createportaltask) Creates a snapshot task for the specified portal.

```
USAGE
  $ apicops create_portal_task PORTAL

ARGUMENTS
  PORTAL  The configured portal service ID or URL.

ALIASES
  $ apicops createportaltask
```

## `apicops custom_script SCRIPT [PARAMS]`

(custom) Runs the provided nodejs script inside the apim pod.

```
USAGE
  $ apicops custom_script SCRIPT [PARAMS]

ARGUMENTS
  SCRIPT  The path to the script to execute inside the apim pod.
  PARAMS  Any parameters to pass to the script, seperate multiple parameters by a space and enclose all paramaters in "

ALIASES
  $ apicops custom
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
  CATALOG  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

ALIASES
  $ apicops fixsnapshotgateway
```

## `apicops fix_snapshot_webhook CATALOG`

(fixsnapshotwebhook) Fixes the bad references of the api in api_urls in the product payload of the snapshot, at webhook level, for the the given catalog.

```
USAGE
  $ apicops fix_snapshot_webhook CATALOG

ARGUMENTS
  CATALOG  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

ALIASES
  $ apicops fixsnapshotwebhook
```

## `apicops get_all_webhooks`

(getwebhooks) Lists all webhooks currently in the database.

```
USAGE
  $ apicops get_all_webhooks

ALIASES
  $ apicops getwebhooks
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

(configuredgateway(s)) With no params lists all configured gateway services, or with a param looks up a specific configured gateway service based on uuid or name with an optional org/catalog/ in front of the name/uuid

```
USAGE
  $ apicops get_configured_gateway_service [GATEWAY]

ARGUMENTS
  GATEWAY  The id or name of the configured gateway service

ALIASES
  $ apicops configuredgateway
  $ apicops configuredgateways
```

## `apicops get_configured_portal_service [PORTAL]`

(configuredportal(s)) With no params lists all configured portal services, or with a param looks up a specific configured portal service based on uuid or name with an optional org/catalog/ in front of the name/uuid

```
USAGE
  $ apicops get_configured_portal_service [PORTAL]

ARGUMENTS
  PORTAL  The id or name of the configured portal service

ALIASES
  $ apicops configuredportal
  $ apicops configuredportals
```

## `apicops get_gateway_service [GATEWAY]`

(gateway(s)) With no params lists all gateway services, or with a param looks up a specific gateway service based on uuid or name

```
USAGE
  $ apicops get_gateway_service [GATEWAY]

ARGUMENTS
  GATEWAY  The id or name of the gateway service

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
  URL  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

ALIASES
  $ apicops checksnapshotwebhook
```

## `apicops get_subscriber_queue_length SUBSCRIBER`

(subqueuelength) Shows the length of the specified subscriber queue.

```
USAGE
  $ apicops get_subscriber_queue_length SUBSCRIBER

ARGUMENTS
  SUBSCRIBER  The id or name of the configured gateway service or portal service. If not unique you can prefix it with
              org/catalog or just catalog, where org and catalog can be names or uuids.

ALIASES
  $ apicops subqueuelength
```

## `apicops get_task TASKID`

(gettask) Dumps out the task identified by taskId.

```
USAGE
  $ apicops get_task TASKID

ARGUMENTS
  TASKID  The id of the task to show.

ALIASES
  $ apicops gettask
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

(checkorphans) Lists any orphaned webhooks.

```
USAGE
  $ apicops identify_orphan_webhooks

ALIASES
  $ apicops checkorphans
```

## `apicops identify_services_state`

(iss) Identifies the state of any gateway and portal services and returns any associated task ids that are incomplete. Can output compact or beautified and text or JSON.

```
USAGE
  $ apicops identify_services_state

OPTIONS
  -e, --embellish  Output a table per service instead of single lines. In JSON mode beautify the JSON.
  -j, --json       Output as raw JSON instead of lines/tables.

ALIASES
  $ apicops iss
```

## `apicops mark_gateway_catalog_online URLORID`

(markgatewaysub) Update the state of the given gateway webhook subscription record to online.

```
USAGE
  $ apicops mark_gateway_catalog_online URLORID

ARGUMENTS
  URLORID  The "Webhook URL" of the gateway or just the uuid for the configured gateway service from "apicops iss -c"

ALIASES
  $ apicops markgatewaysub
```

## `apicops mark_portal_webhook_subscription_state URLORID`

(markportalsub) Update the state of the given portal webhook subscription record to offline_configured.

```
USAGE
  $ apicops mark_portal_webhook_subscription_state URLORID

ARGUMENTS
  URLORID  The "Webhook URL" of the portal or just the uuid for the configured portal service from "apicops iss -c"

ALIASES
  $ apicops markportalsub
```

## `apicops renew_task TASKID`

(renewtask) Re-triggers the provided task id by setting its state to 'new' so that it gets picked up for processing again.

```
USAGE
  $ apicops renew_task TASKID

ARGUMENTS
  TASKID  The id of the task to renew

ALIASES
  $ apicops renewtask
```

## `apicops send_snapshot SERVICE`

(sendsnapshot) Send a snapshot to the service identified by the UUID provided. Note that before sending the snapshot the entire task queue will be cleared, so if you have any tasks in 'new' or 'inprogress' state etc. then they should either be sorted out first, or you will have to run sendsnapshot for the catalog related to those tasks afterwards.

```
USAGE
  $ apicops send_snapshot SERVICE

ARGUMENTS
  SERVICE  The portal or gateway service UUID

ALIASES
  $ apicops sendsnapshot
```

## `apicops snapshot_builder URL`

(buildsnapshot) Compact the event queue into an up to date snapshot.

```
USAGE
  $ apicops snapshot_builder URL

ARGUMENTS
  URL  The catalog or gateway service, identified via name in the form, <catalog>, or <org>/<catalog>, or
       <org>/<catalog>/<gateway service>, or by UUID in the form: <catalog>, or <gateway>. To list the catalogs run the
       catalogs command, to list the configured gateways, run the configuredgateways command.

OPTIONS
  -c, --complete  Replay all events instead of just those that are older than the current time.

ALIASES
  $ apicops buildsnapshot
```

## `apicops snapshot_validator2 CATALOG`

(validatesnapshot) Validate the snapshots for a catalog, for the portal or the gateway, or both.

```
USAGE
  $ apicops snapshot_validator2 CATALOG

ARGUMENTS
  CATALOG  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

OPTIONS
  -g, --gateway  Validate the gateway snapshots for the catalog.
  -p, --portal   Validate the portal snapshots for the catalog.

ALIASES
  $ apicops validatesnapshot
```
<!-- commandsstop -->
