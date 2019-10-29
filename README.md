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

# Warning

Unless directed by IBM, only run commands that are also described in the Knowledge Center:

https://www.ibm.com/support/knowledgecenter/en/SSMNED_2018/com.ibm.apic.install.doc/capim_apicops_overview.html: 

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

## Setting the target namespace

The `default` namespace for the deployment will be targeted by default.  If your deployment makes use of an alternative namespace then you will need to set this in the relevant context of your kubeconfig file.
Your can either edit your kubeconfig file directly and add the namespace property with the desired value to the relevant context.
Alternatively you can set the namespace for the current context using the following command (where < namespace > is the value you want to set the namespace to):
```
kubectl config set "contexts."`kubectl config current-context`".namespace" < namespace >
```
You can view all contexts and their configured namespace with the command:
```
kubectl config get-contexts
```

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
* [`apicops catalogs:get CATALOG`](#apicops-catalogsget-catalog)
* [`apicops catalogs:list`](#apicops-catalogslist)
* [`apicops cron-jobs:recreate`](#apicops-cron-jobsrecreate)
* [`apicops custom:run SCRIPT [PARAMS]`](#apicops-customrun-script-params)
* [`apicops grace-period:get`](#apicops-grace-periodget)
* [`apicops grace-period:set PERIOD`](#apicops-grace-periodset-period)
* [`apicops help [COMMAND]`](#apicops-help-command)
* [`apicops locks:delete-expired`](#apicops-locksdelete-expired)
* [`apicops oauth-secret:fix URL`](#apicops-oauth-secretfix-url)
* [`apicops organisations:get ORG`](#apicops-organisationsget-org)
* [`apicops organisations:list`](#apicops-organisationslist)
* [`apicops services:get-configured-gateway GATEWAY`](#apicops-servicesget-configured-gateway-gateway)
* [`apicops services:get-configured-portal PORTAL`](#apicops-servicesget-configured-portal-portal)
* [`apicops services:get-gateway GATEWAY`](#apicops-servicesget-gateway-gateway)
* [`apicops services:identify-state`](#apicops-servicesidentify-state)
* [`apicops services:list-configured-gateway`](#apicops-serviceslist-configured-gateway)
* [`apicops services:list-configured-portal`](#apicops-serviceslist-configured-portal)
* [`apicops services:list-gateways`](#apicops-serviceslist-gateways)
* [`apicops snapshots:build URL`](#apicops-snapshotsbuild-url)
* [`apicops snapshots:check-invalid-products URL`](#apicops-snapshotscheck-invalid-products-url)
* [`apicops snapshots:check-invalid-products-gateway URL`](#apicops-snapshotscheck-invalid-products-gateway-url)
* [`apicops snapshots:fix-invalid-products CATALOG`](#apicops-snapshotsfix-invalid-products-catalog)
* [`apicops snapshots:fix-invalid-products-gateway CATALOG`](#apicops-snapshotsfix-invalid-products-gateway-catalog)
* [`apicops snapshots:send SERVICE`](#apicops-snapshotssend-service)
* [`apicops snapshots:validate CATALOG`](#apicops-snapshotsvalidate-catalog)
* [`apicops spaces:get SPACE`](#apicops-spacesget-space)
* [`apicops spaces:list`](#apicops-spaceslist)
* [`apicops subscriber-queues:clear`](#apicops-subscriber-queuesclear)
* [`apicops subscriber-queues:get-length SUBSCRIBER`](#apicops-subscriber-queuesget-length-subscriber)
* [`apicops tables:check-index`](#apicops-tablescheck-index)
* [`apicops tables:check-link`](#apicops-tablescheck-link)
* [`apicops task-queue:clear`](#apicops-task-queueclear)
* [`apicops tasks:create-gateway GATEWAY`](#apicops-taskscreate-gateway-gateway)
* [`apicops tasks:create-portal PORTAL`](#apicops-taskscreate-portal-portal)
* [`apicops tasks:get TASKID`](#apicops-tasksget-taskid)
* [`apicops tasks:renew TASKID`](#apicops-tasksrenew-taskid)
* [`apicops webhook-subscriptions:check-orphans`](#apicops-webhook-subscriptionscheck-orphans)
* [`apicops webhook-subscriptions:delete-orphans`](#apicops-webhook-subscriptionsdelete-orphans)
* [`apicops webhook-subscriptions:list`](#apicops-webhook-subscriptionslist)
* [`apicops webhook-subscriptions:mark-gateway URLORID`](#apicops-webhook-subscriptionsmark-gateway-urlorid)
* [`apicops webhook-subscriptions:mark-portal URLORID`](#apicops-webhook-subscriptionsmark-portal-urlorid)

## `apicops catalogs:get CATALOG`

(cat) Looks up a specific catalog based on uuid or name

```
USAGE
  $ apicops catalogs:get CATALOG

ARGUMENTS
  CATALOG  The id or name of the catalog

ALIASES
  $ apicops cat

EXAMPLES
  $ apicops cat 4eab42d5-6ba8-4e68-ac87-44ff229db677          # Get a catalog by UUID
  $ apicops cat cbd062ad-f04c-44cd-afae-dd6a9247309c:sandbox  # Get a catalog using the org UUID and catalog name
  $ apicops catalogs:get myuniqueorg/stuff                    # Get a catalog using the org name and catalog name
  $ apicops catalogs:get myuniquecat                          # Get a catalog using the unique catalog name
  $ apicops catalogs:get sandbox                              # Get all catalogs named sandbox
```

## `apicops catalogs:list`

(cats) Lists all catalogs

```
USAGE
  $ apicops catalogs:list

ALIASES
  $ apicops cats

EXAMPLES
  $ apicops catalogs:list  # List all catalogs
  $ apicops cats           # List all catalogs
```

## `apicops cron-jobs:recreate`

(cronjobs) Recreate the apim cron jobs

```
USAGE
  $ apicops cron-jobs:recreate

ALIASES
  $ apicops cronjobs

EXAMPLES
  $ apicops cron-jobs:recreate  # Recreate the apim cron jobs
  $ apicops cronjobs            # Recreate the apim cron jobs
```

## `apicops custom:run SCRIPT [PARAMS]`

(custom) Runs the provided nodejs script inside the apim pod

```
USAGE
  $ apicops custom:run SCRIPT [PARAMS]

ARGUMENTS
  SCRIPT  The path to the script to execute inside the apim pod

  PARAMS  Any parameters to pass to the script. Separate multiple parameters by a space and enclose all parameters with
          spaces in them with "s

ALIASES
  $ apicops runcustom

EXAMPLES
  $ apicops custom:run /tmp/myscript.js          # Run a script with no parameters
  $ apicops custom:run /tmp/myscript.js one two  # Run a script with 2 parameters
  $ apicops runcustom /tmp/s.js one "param two"  # Run a script with 2 parameters
```

## `apicops grace-period:get`

(getgrace) Gets the current value for the time between compaction of tombstones

```
USAGE
  $ apicops grace-period:get

ALIASES
  $ apicops getgrace

EXAMPLES
  $ apicops grace-period:get  # Gets the current grace period
  $ apicops getgrace          # Gets the current grace period
```

## `apicops grace-period:set PERIOD`

(setgrace) Sets the time between compaction of tombstones

```
USAGE
  $ apicops grace-period:set PERIOD

ARGUMENTS
  PERIOD  (short|long) Set to "short" to cleanup tombstones every 2 mins, or "long", to cleanup every 3 days

ALIASES
  $ apicops setgrace

EXAMPLES
  $ apicops grace-period:set long  # Sets the current grace period to 3 days
  $ apicops setgrace short         # Sets the current grace period to 2 minutes
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

## `apicops locks:delete-expired`

(deletelocks) Deletes any expired transaction locks

```
USAGE
  $ apicops locks:delete-expired

ALIASES
  $ apicops deletelocks

EXAMPLES
  $ apicops locks:delete-expired  # Deletes any expired transaction locks
  $ apicops deletelocks           # Deletes any expired transaction locks
```

## `apicops oauth-secret:fix URL`

(fixoauth) Fix the shared OAuth secret for the given catalog and gateway service

```
USAGE
  $ apicops oauth-secret:fix URL

ARGUMENTS
  URL  The catalog and gateway service, identified via name or UUID, in the form, <catalog>/<gateway service>, or
       <org>/<catalog>/<gateway service>

ALIASES
  $ apicops fixoauth

EXAMPLES
  $ apicops oauth-secret:fix unique-gateway-1              # Fixes the OAuth secret for the given gateway service
  $ apicops fixoauth 5cec22d4-3c93-44ac-a4c0-750b4a57756c  # Fixes the OAuth secret for the gateway identified by UUID
  $ apicops fixoauth myorg/mycatalog/gateway-1             # Fixes the OAuth secret for the given gateway service
```

## `apicops organisations:get ORG`

(org) Looks up a specific organisation based on uuid or name

```
USAGE
  $ apicops organisations:get ORG

ARGUMENTS
  ORG  The id or name of the org

ALIASES
  $ apicops org

EXAMPLES
  $ apicops org 4eab42d5-6ba8-4e68-ac87-44ff229db677  # Get an organisation by UUID
  $ apicops organisations:get myuniqueorg             # Get an organisation using the unique organisation name
  $ apicops organisations:get org1                    # Get all organisations named org1
```

## `apicops organisations:list`

(orgs) Lists all organisations

```
USAGE
  $ apicops organisations:list

ALIASES
  $ apicops orgs

EXAMPLES
  $ apicops organisations:list  # List all organisations
  $ apicops orgs                # List all organisations
```

## `apicops services:get-configured-gateway GATEWAY`

(configuredgateway) Looks up a specific configured gateway service based on uuid or name with an optional org/catalog/ in front of the name/uuid

```
USAGE
  $ apicops services:get-configured-gateway GATEWAY

ARGUMENTS
  GATEWAY  The id or name of the configured gateway service

ALIASES
  $ apicops configuredgateway

EXAMPLES
  $ apicops services:get-configured-gateway gateway-1                             # Gets all the configured gateways 
  with name gateway-1
  $ apicops configuredgateway myuniqueorg:cat:gateway-1                           # Gets all the configured gateways 
  with name gateway-1 in catalog cat in organisation myuniqueorg
  $ apicops configuredgateway cbd062ad-f04c-44cd-afae-dd6a9247309c/gateway-1      # Gets all the configured gateways 
  with name gateway-1 in catalog with the UUID specified
  $ apicops services:get-configured-gateway 740caa86-0c4e-4531-a460-3fb70890726e  # Gets the configured gateway with the 
  UUID specified
```

## `apicops services:get-configured-portal PORTAL`

(configuredportal) Looks up a specific configured portal service based on uuid or name with an optional org/catalog/ in front of the name/uuid

```
USAGE
  $ apicops services:get-configured-portal PORTAL

ARGUMENTS
  PORTAL  The id or name of the configured portal service

ALIASES
  $ apicops configuredportal

EXAMPLES
  $ apicops services:get-configured-portal portal-1                              # Gets all the configured portals with 
  name portal-1
  $ apicops configuredportal myuniqueorg:cat:portal-1                            # Gets all the configured portals with 
  name portal-1 in catalog cat in organisation myuniqueorg
  $ apicops configuredportal cbd062ad-f04c-44cd-afae-dd6a9247309c/portal-1       # Gets all the configured portals with 
  name portal-1 in catalog with the UUID specified
  $ apicops services:get-configured-portal 740caa86-0c4e-4531-a460-3fb70890726e  # Gets the configured portal with the 
  UUID specified
```

## `apicops services:get-gateway GATEWAY`

(gateway) Looks up a specific gateway service based on uuid or name

```
USAGE
  $ apicops services:get-gateway GATEWAY

ARGUMENTS
  GATEWAY  The id or name of the gateway service

ALIASES
  $ apicops gateway

EXAMPLES
  $ apicops services:get-gateway gateway-1                # Gets the gateway with name gateway-1
  $ apicops gateway 740caa86-0c4e-4531-a460-3fb70890726e  # Gets the gateway with the UUID specified
```

## `apicops services:identify-state`

(iss) Identifies the state of any gateway and portal services and returns any associated task ids that are incomplete. Can output compact or beautified and text or JSON

```
USAGE
  $ apicops services:identify-state

OPTIONS
  -e, --embellish  Output a table per service instead of single lines. In JSON mode beautify the JSON
  -j, --json       Output as raw JSON instead of lines/tables

ALIASES
  $ apicops iss
```

## `apicops services:list-configured-gateway`

(configuredgateways) Lists all configured gateway services

```
USAGE
  $ apicops services:list-configured-gateway

ALIASES
  $ apicops configuredgateways

EXAMPLES
  $ apicops services:list-configured-gateways  # Lists all the configured gateways
  $ apicops configuredgateways                 # Lists all the configured gateways
```

## `apicops services:list-configured-portal`

(configuredportals) Lists all configured portal services

```
USAGE
  $ apicops services:list-configured-portal

ALIASES
  $ apicops configuredportals

EXAMPLES
  $ apicops services:list-configured-portals  # Lists all the configured portals
  $ apicops configuredportals                 # Lists all the configured portals
```

## `apicops services:list-gateways`

(gateways) Lists all gateway services

```
USAGE
  $ apicops services:list-gateways

ALIASES
  $ apicops gateways

EXAMPLES
  $ apicops services:list-gateways  # Lists all the gateways
  $ apicops gateways                # Lists all the gateways
```

## `apicops snapshots:build URL`

(buildsnapshot) Compact the event queue into an up to date snapshot. If a catalog is given then the snapshot is build at the catalog level. If a configured gateway is given then the snapshot is build for that gateway

```
USAGE
  $ apicops snapshots:build URL

ARGUMENTS
  URL  The catalog or configured gateway service, identified via name in the form, <catalog>, or <org>/<catalog>, or
       <org>/<catalog>/<gateway service>, or by UUID in the form: <catalog>, or <gateway>. To list the catalogs run the
       catalogs command, to list the configured gateways, run the configuredgateways command

ALIASES
  $ apicops buildsnapshot

EXAMPLES
  $ apicops snapshots:build gateway-1                           # Builds a snapshot for the single configured gateway 
  with the name gateway-1
  $ apicops buildsnapshot 740caa86-0c4e-4531-a460-3fb70890726e  # Builds a snapshot for the catalog with the UUID 
  specified
  $ apicops buildsnapshot cbd062ad-f04c-44cd-afae-dd6a9247309c  # Builds a snapshot for the gateway with the UUID 
  specified
  $ apicops snapshots:build myuniqueorg/sandbox                 # Builds a snapshot for the sandbox catalog of the 
  myuniqueorg organisation
  $ apicops buildsnapshot myuniqueorg/sandbox/gateway-2         # Builds a snapshot for gateway-2 of the snapshot 
  catalog of the myuniqueorg organisation
```

## `apicops snapshots:check-invalid-products URL`

(checksnapshot) Identifies bad products in the snapshot payload that have references to invalid apis for the given org and catalog

```
USAGE
  $ apicops snapshots:check-invalid-products URL

ARGUMENTS
  URL  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

ALIASES
  $ apicops checksnapshot

EXAMPLES
  $ apicops snapshots:check-invalid-products mycatalog          # Checks if there are invalid products in catalog 
  mycatalog
  $ apicops checksnapshot aa592e2f-68b1-4380-ad7c-ac86c932f1e6  # Checks if there are invalid products in identified 
  catalog
  $ apicops checksnapshot myorg/mycatalog                       # Checks if there are invalid products in the catalog 
  mycatalog
```

## `apicops snapshots:check-invalid-products-gateway URL`

(checksnapshotgateway) Check bad references of the api in api_urls in the product payload of the snapshot for the first configured gateway service for the given catalog

```
USAGE
  $ apicops snapshots:check-invalid-products-gateway URL

ARGUMENTS
  URL  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

ALIASES
  $ apicops checksnapshotgateway

EXAMPLES
  $ apicops snapshots:check-invalid-products-gateway mycatalog         # Checks if there are invalid products in the 
  first gateway associated with catalog mycatalog
  $ apicops checksnapshotgateway aa592e2f-68b1-4380-ad7c-ac86c932f1e6  # Checks if there are invalid products in the 
  first gateway associated with identified catalog
  $ apicops checksnapshotgateway myorg/mycatalog                       # Checks if there are invalid products in the 
  first gateway associated with catalog mycatalog
```

## `apicops snapshots:fix-invalid-products CATALOG`

(fixsnapshot) Fixes the bad references of the api in api_urls in the product payload of the snapshot for the the given catalog

```
USAGE
  $ apicops snapshots:fix-invalid-products CATALOG

ARGUMENTS
  CATALOG  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

ALIASES
  $ apicops fixsnapshot

EXAMPLES
  $ apicops snapshots:fix-invalid-products mycatalog          # Fixes the invalid products in catalog mycatalog
  $ apicops fixsnapshot aa592e2f-68b1-4380-ad7c-ac86c932f1e6  # Fixes the invalid products in identified catalog
  $ apicops fixsnapshot myorg/mycatalog                       # Fixes the invalid products in the catalog mycatalog
```

## `apicops snapshots:fix-invalid-products-gateway CATALOG`

(fixsnapshotgateway) Fixes the bad references of the api in api_urls in the product payload of the snapshot for the first configured gateway service for the given catalog

```
USAGE
  $ apicops snapshots:fix-invalid-products-gateway CATALOG

ARGUMENTS
  CATALOG  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

ALIASES
  $ apicops fixsnapshotgateway

EXAMPLES
  $ apicops snapshots:fix-invalid-products-gateway mycatalog         # Fixes the invalid products in the first gateway 
  associated with catalog mycatalog
  $ apicops fixsnapshotgateway aa592e2f-68b1-4380-ad7c-ac86c932f1e6  # Fixes the invalid products in the first gateway 
  associated with the identified catalog
  $ apicops fixsnapshotgateway myorg/mycatalog                       # Fixes the invalid products in the first gateway 
  associated with catalog mycatalog
```

## `apicops snapshots:send SERVICE`

(sendsnapshot) Send a snapshot to the service identified by the UUID provided. Note that before sending the snapshot the entire task queue will be cleared, so if you have any tasks in 'new' or 'inprogress' state etc. then they should either be sorted out first, or you will have to run sendsnapshot for the catalog related to those tasks afterwards

```
USAGE
  $ apicops snapshots:send SERVICE

ARGUMENTS
  SERVICE  The portal or gateway service UUID

ALIASES
  $ apicops sendsnapshot

EXAMPLES
  $ apicops snapshots:send gateway-1                           # Gets all the configured gateways with name gateway-1
  $ apicops sendsnapshot 740caa86-0c4e-4531-a460-3fb70890726e  # Builds a snapshot for the catalog with the UUID 
  specified
  $ apicops sendsnapshot cbd062ad-f04c-44cd-afae-dd6a9247309c  # Builds a snapshot for the gateway with the UUID 
  specified
  $ apicops snapshots:send myuniqueorg/sandbox                 # Builds a snapshot for the sandbox catalog of the 
  myuniqueorg organisation
  $ apicops sendsnapshot myuniqueorg/sandbox/gateway-2         # Builds a snapshot for gateway-2 of the snapshot catalog 
  of the myuniqueorg organisation
```

## `apicops snapshots:validate CATALOG`

(validatesnapshot) Validate the snapshots for a catalog, for the portal or the gateway, or both

```
USAGE
  $ apicops snapshots:validate CATALOG

ARGUMENTS
  CATALOG  The catalog, identified via name or UUID, in the form, <catalog>, or <org>/<catalog>

OPTIONS
  -g, --gateway  Validate the gateway snapshots for the catalog
  -p, --portal   Validate the portal snapshots for the catalog

ALIASES
  $ apicops validatesnapshot

EXAMPLES
  $ apicops snapshots:validate -p myuniquecatalog                     # Validates the portal snapshot for the catalog 
  myuniquecatalog
  $ apicops snapshots:validate -pg myuniquecatalog                    # Validates the portal and gateway snapshots for 
  the catalog myuniquecatalog
  $ apicops snapshots:validate -g myuniquecatalog                     # Validates the gateway snapshot for the catalog 
  myuniquecatalog
  $ apicops validatesnapshot -g 740caa86-0c4e-4531-a460-3fb70890726e  # Validates the gateway snapshot for the catalog 
  identified by the UUID
```

## `apicops spaces:get SPACE`

(sp) Looks up a specific space based on the uuid of the space or name of the space in the format <org_id_or_name>/<catalog_id_or_name>/<space_id_or_name>

```
USAGE
  $ apicops spaces:get SPACE

ARGUMENTS
  SPACE  [default: ""] The id of the space, or, the name of the space in a particular org and catalog (format
         <org_id_or_name>/<catalog_id_or_name>/<space_id_or_name>)

OPTIONS
  -c, --catalogId=catalogId  [default: ""] The id or name of the catalog you want to list the spaces within

ALIASES
  $ apicops sp

EXAMPLES
  $ apicops sp 4eab42d5-6ba8-4e68-ac87-44ff229db677                      # Get a space by UUID
  $ apicops spaces:get myuniquespace                                     # Get a space using the unique space name
  $ apicops spaces:get org1/cat1/space-1                                 # Get a space by URL
  $ apicops spaces:get space-1                                           # Get all spaces named space-1
  $ apicops spaces:get -c cat1 space-1                                   # Get a space named space-1 in catalog cat-1
  $ apicops sp --catalogId 745c9bab-e0ba-4d34-a284-aa3a28f77a7b space-1  # Get a space named space-1 in the catalog 
  identified by the UUID
```

## `apicops spaces:list`

(sps) Lists all spaces

```
USAGE
  $ apicops spaces:list

OPTIONS
  -c, --catalogId=catalogId  [default: ""] The id or name of the catalog you want to list the spaces within

ALIASES
  $ apicops sps

EXAMPLES
  $ apicops sps                                                   # List all spaces
  $ apicops spaces:list                                           # List all spaces
  $ apicops spaces:list -c cat1                                   # List all spaces in catalog cat1
  $ apicops sps --catalogId 745c9bab-e0ba-4d34-a284-aa3a28f77a7b  # List all spaces in the catalog identified by the 
  UUID
```

## `apicops subscriber-queues:clear`

(clearsubqueue) Clears all subscriber queue

```
USAGE
  $ apicops subscriber-queues:clear

ALIASES
  $ apicops clearsubqueues

EXAMPLES
  $ apicops subscriber-queues:clear  # Clears all items from the subscriber queues
  $ apicops clearsubqueues           # Clears all items from the subscriber queues
```

## `apicops subscriber-queues:get-length SUBSCRIBER`

(subqueuelength) Shows the length of the subscriber queue specified

```
USAGE
  $ apicops subscriber-queues:get-length SUBSCRIBER

ARGUMENTS
  SUBSCRIBER  The id or name of the configured gateway service or portal service. If not unique you can prefix it with
              org/catalog or just catalog, where org and catalog can be names or uuids

ALIASES
  $ apicops subqueuelength

EXAMPLES
  $ apicops subscriber-queues:get-length myuniquecatalog/portal-1  # Gets the length of the portal-1 subscriber queue
  $ apicops subqueuelength aa592e2f-68b1-4380-ad7c-ac86c932f1e6    # Gets the length of the subscriber queue for the 
  portal service specified by the UUID
  $ apicops subqueuelength cbd062ad-f04c-44cd-afae-dd6a9247309c    # Gets the length of the subscriber queue for the 
  gateway service specified by the UUID
  $ apicops subscriber-queues:get-length unique-portal-1           # Gets the length of the unqiue-portal-1 subscriber 
  queue
```

## `apicops tables:check-index`

(checkdataindexes) Checks inconsistencies between the main and index tables (data available in main table and not present in index table. Similarly data present in index table but not in main table)

```
USAGE
  $ apicops tables:check-index

ALIASES
  $ apicops checktablesindex

EXAMPLES
  $ apicops tables:check-index  # Checks the index tables for inconsistencies
  $ apicops checktablesindex    # Checks the index tables for inconsistencies
```

## `apicops tables:check-link`

(checkdatalinks) Checks inconsistencies between the link tables and the corresponding index tables

```
USAGE
  $ apicops tables:check-link

ALIASES
  $ apicops checktableslink

EXAMPLES
  $ apicops tables:check-link  # Checks the link tables for inconsistencies
  $ apicops checktableslink    # Checks the link tables for inconsistencies
```

## `apicops task-queue:clear`

(cleartasks) Clear the task queue of all tasks

```
USAGE
  $ apicops task-queue:clear

ALIASES
  $ apicops cleartasks

EXAMPLES
  $ apicops task-queue:clear  # Clears all tasks from task queue
  $ apicops cleartasks        # Clears all tasks from task queue
```

## `apicops tasks:create-gateway GATEWAY`

(creategatewaytask) Creates a snapshot task for the specified gateway

```
USAGE
  $ apicops tasks:create-gateway GATEWAY

ARGUMENTS
  GATEWAY  The configured gateway service ID or URL

ALIASES
  $ apicops creategatewaytask

EXAMPLES
  $ apicops creategatewaytask 4eab42d5-6ba8-4e68-ac87-44ff229db677  # Creates a snapshot task for the gateway specified 
  by the UUID
  $ apicops tasks:create-gateway org/cat/gateway-1                  # Creates a snapshot task for the gateway specified 
  by the url
  $ apicops tasks:create-gateway unique-cat:gateway-1               # Creates a snapshot task for the gateway specified 
  by the url
```

## `apicops tasks:create-portal PORTAL`

(createportaltask) Creates a snapshot task for the specified portal

```
USAGE
  $ apicops tasks:create-portal PORTAL

ARGUMENTS
  PORTAL  The configured portal service ID or URL

ALIASES
  $ apicops createportaltask

EXAMPLES
  $ apicops createportaltask 4eab42d5-6ba8-4e68-ac87-44ff229db677  # Creates a snapshot task for the portal specified by 
  the UUID
  $ apicops tasks:create-portal org/cat/portal-1                   # Creates a snapshot task for the portal specified by 
  the url
  $ apicops tasks:create-portal unique-cat:portal-1                # Creates a snapshot task for the portal specified by 
  the url
```

## `apicops tasks:get TASKID`

(gettask) Dumps out the task identified by taskId

```
USAGE
  $ apicops tasks:get TASKID

ARGUMENTS
  TASKID  The id of the task to show

ALIASES
  $ apicops gettask

EXAMPLES
  $ apicops gettask 4eab42d5-6ba8-4e68-ac87-44ff229db677    # Get a task by UUID
  $ apicops tasks:get 4eab42d5-6ba8-4e68-ac87-44ff229db677  # Get a task by UUID
```

## `apicops tasks:renew TASKID`

(renewtask) Re-triggers the provided task id by setting its state to 'new' so that it gets picked up for processing again

```
USAGE
  $ apicops tasks:renew TASKID

ARGUMENTS
  TASKID  The id of the task to renew

ALIASES
  $ apicops renewtask

EXAMPLES
  $ apicops renewtask 4eab42d5-6ba8-4e68-ac87-44ff229db677    # Renews a task by UUID
  $ apicops tasks:renew 4eab42d5-6ba8-4e68-ac87-44ff229db677  # Renews a task by UUID
```

## `apicops webhook-subscriptions:check-orphans`

(checkorphans) Lists any orphaned webhook subscriptions

```
USAGE
  $ apicops webhook-subscriptions:check-orphans

ALIASES
  $ apicops checkorphans

EXAMPLES
  $ apicops webhook-subscriptions:check-orphans  # List any orphaned webhook subscriptions
  $ apicops checkorphans                         # List any orphaned webhook subscriptions
```

## `apicops webhook-subscriptions:delete-orphans`

(deleteorphans) Deletes any orphaned webhook subscriptions

```
USAGE
  $ apicops webhook-subscriptions:delete-orphans

ALIASES
  $ apicops deleteorphans

EXAMPLES
  $ apicops webhook-subscriptions:delete-orphans  # Deletes any orphaned webhook subscriptions
  $ apicops deleteorphans                         # Deletes any orphaned webhook subscriptions
```

## `apicops webhook-subscriptions:list`

(webhooksubscriptions) Lists all webhook subscriptions

```
USAGE
  $ apicops webhook-subscriptions:list

ALIASES
  $ apicops webhooksubs

EXAMPLES
  $ apicops webhook-subscriptions:list  # List all webhook subscriptions
  $ apicops webhooksubs                 # List all webhook subscriptions
```

## `apicops webhook-subscriptions:mark-gateway URLORID`

(markgatewaysub) Update the state of the given gateway webhook subscription record to online

```
USAGE
  $ apicops webhook-subscriptions:mark-gateway URLORID

ARGUMENTS
  URLORID  The "Webhook URL" of the gateway or just the uuid for the configured gateway service from "apicops iss"

ALIASES
  $ apicops markgatewaysub

EXAMPLES
  $ apicops markgatewaysub 843d3499-da6b-4acd-a265-3753d7a71668   # Update the subcsription to online for the gateway 
  identified by UUID
  $ apicops webhook-subscriptions:mark-gateway org/cat/gateway-1  # Update the subcsription to online for the gateway 
  identified by the URL
  $ apicops markgatewaysub myuniquecat:gateway-1                  # Update the subcsription to online for the gateway 
  identified by the URL
```

## `apicops webhook-subscriptions:mark-portal URLORID`

(markportalsub) Update the state of the given portal webhook subscription record to offline_configured

```
USAGE
  $ apicops webhook-subscriptions:mark-portal URLORID

ARGUMENTS
  URLORID  The "Webhook URL" of the portal or just the uuid for the configured portal service from "apicops iss"

ALIASES
  $ apicops markportalsub

EXAMPLES
  $ apicops markportalsub 843d3499-da6b-4acd-a265-3753d7a71668  # Update the subcsription to offline_configured for the 
  portal identified by UUID
  $ apicops webhook-subscriptions:mark-portal org/cat/portal-1  # Update the subcsription to offline_configured for the 
  portal identified by the URL
  $ apicops markportalsub myuniquecat:portal-1                  # Update the subcsription to offline_configured for the 
  portal identified by the URL
```
<!-- commandsstop -->
