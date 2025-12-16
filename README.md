apicops
===============

<!-- toc -->
* [About](#about)
* [Warning](#warning)
* [Installing](#installing)
* [Requirements](#requirements)
* [Usage](#usage)
* [Commands](#commands)
<!-- tocstop -->

# About
`apicops` is a command line interface to IBM API Connect v10 specifically targeted at Operations teams. It contains commands to check the healthy running of the system as well as some commands to remedy specific problems if encountered.

It is in active development and new versions will be posted here regularly. Please always use the latest version, as it will contain the latest improvements, commands, and any bug fixes.

Please note this has only been tested against IBM API Connect v10.

# Warning

Unless directed by IBM, only run commands that are also described in the Knowledge Center:

https://www.ibm.com/support/knowledgecenter/en/SSMNED_2018/com.ibm.apic.install.doc/capim_apicops_overview.html:


# Installing
Download the latest binary for your operating system from the Releases tab and rename it to be `apicops`. Note that Linux and Mac will require you to run chmod +x on the downloaded file before you can execute it.
Windows OS is NOT supported.

# Requirements
In order to run `apicops` you need to have `kubectl` or something that implements the same CLI as `kubectl`, such as `oc` installed locally. If you aren't using `kubectl` then set the environment variable APICOPS_K8SCLIENT to the name of the Kubernetes client binary, such as `oc`.

If using the openshift client (`oc`) then v4.1.x or greater is required (even if using v3.x openshift cluster).

Then set the `KUBECONFIG` environment variable to point to your kubeconfig file and `apicops` will pick it up from there.

```sh-session
$ export KUBECONFIG=/home/user/my.kubeconfig
$ apicops
```

If running inside an API Connect OVA file then run `apicops` as root (`sudo -i`) and it will automatically pick up the kubeconfig.

Some commands require the toolkit credentials to be set inorder to obtain an access token for the admin user. Download the credentials from the Cloud Manager and set them using the toolkit command `apic client-creds:set /PATH/TO/credentials.json`

## Setting the target namespace

The `default` namespace for the deployment will be targeted by default. If your deployment makes use of an alternative namespace then you will need to set this in the relevant context of your kubeconfig file.
Your can either edit your kubeconfig file directly and add the namespace property with the desired value to the relevant context.
Alternatively you can set the namespace for the current context using the following command (where < namespace > is the value you want to set the namespace to):
```
kubectl config set "contexts."`kubectl config current-context`".namespace" < namespace >
```
You can view all contexts and their configured namespace with the command:
```
kubectl config get-contexts
```

Alternatively, you can pass an argument `-n <your namespace here>` to all `apicops` commands to specify the namespace for your deployment.

# Usage
```sh-session
$ apicops-v12 COMMAND
running command...
$ apicops-v12 (-v|--version|version)
apicops/0.10.1 linux-x64 node-v10.16.3
$ apicops --help [COMMAND]
USAGE
  $ apicops COMMAND
...
```

## Commonly used commands:
1. `apicops-v12 iss`
API Connect uses tasks to do actions such as synchronizing content between API Manager and the Gateways and Portals. When you are diagnosing some problems, it can be useful to determine what the state is of those tasks.

Determining the state of the task can be done by using the apicops services:identify-state command, which identifies the state of any gateway and portal services and returns any associated task IDs that are incomplete.
```
USAGE
  $ apicops services:identify-state

OPTIONS
  -e, --embellish  Output a table per service instead of single lines. In JSON mode beautify the JSON
  -j, --json       Output as raw JSON instead of lines/tables

ALIASES
  $ apicops iss
```

# Commands
Below are the commands that are applicable for the latest version of `apicops` released.  They may not be applicable to older versions of the tool.  We strongly encourage all to use the latest apicops tool at all times.
<!-- commands -->
* [`apicops-v12 appliance-checks:appliance-pre-upgrade`](#apicops-v12-appliance-checksappliance-pre-upgrade)
* [`apicops-v12 catalogs:get CATALOG`](#apicops-v12-catalogsget-catalog)
* [`apicops-v12 catalogs:list`](#apicops-v12-catalogslist)
* [`apicops-v12 certs:renew-ingress-ca-certs`](#apicops-v12-certsrenew-ingress-ca-certs)
* [`apicops-v12 custom:run SCRIPT [PARAMS]`](#apicops-v12-customrun-script-params)
* [`apicops-v12 debug:info`](#apicops-v12-debuginfo)
* [`apicops-v12 help [COMMAND]`](#apicops-v12-help-command)
* [`apicops-v12 logs:analyse`](#apicops-v12-logsanalyse)
* [`apicops-v12 logs:change-log-spec`](#apicops-v12-logschange-log-spec)
* [`apicops-v12 logs:get-log-spec`](#apicops-v12-logsget-log-spec)
* [`apicops-v12 logs:search`](#apicops-v12-logssearch)
* [`apicops-v12 logs:unique-logs`](#apicops-v12-logsunique-logs)
* [`apicops-v12 organisations:get ORG`](#apicops-v12-organisationsget-org)
* [`apicops-v12 organisations:list`](#apicops-v12-organisationslist)
* [`apicops-v12 platform:check-appliance-disk`](#apicops-v12-platformcheck-appliance-disk)
* [`apicops-v12 platform:check-mgmt-disk`](#apicops-v12-platformcheck-mgmt-disk)
* [`apicops-v12 platform:check-portal-disk`](#apicops-v12-platformcheck-portal-disk)
* [`apicops-v12 portal:exec`](#apicops-v12-portalexec)
* [`apicops-v12 postgres:pg_dump`](#apicops-v12-postgrespg_dump)
* [`apicops-v12 postgres:pg_dump_all`](#apicops-v12-postgrespg_dump_all)
* [`apicops-v12 postgres:pg_restore RESTORE_FILE`](#apicops-v12-postgrespg_restore-restore_file)
* [`apicops-v12 services:get-configured-gateway GATEWAY`](#apicops-v12-servicesget-configured-gateway-gateway)
* [`apicops-v12 services:get-configured-portal PORTAL`](#apicops-v12-servicesget-configured-portal-portal)
* [`apicops-v12 services:get-gateway GATEWAY`](#apicops-v12-servicesget-gateway-gateway)
* [`apicops-v12 services:identify-state`](#apicops-v12-servicesidentify-state)
* [`apicops-v12 services:list-configured-gateway`](#apicops-v12-serviceslist-configured-gateway)
* [`apicops-v12 services:list-configured-portal`](#apicops-v12-serviceslist-configured-portal)
* [`apicops-v12 services:list-gateways`](#apicops-v12-serviceslist-gateways)
* [`apicops-v12 snapshots:get WEBHOOK_ID`](#apicops-v12-snapshotsget-webhook_id)
* [`apicops-v12 snapshots:check-subscriptions WEBHOOK_ID`](#apicops-v12-checksubscriptions-webhook_id)
* [`apicops-v12 spaces:list`](#apicops-v12-spaceslist)
* [`apicops-v12 tables:get-contents [TABLE]`](#apicops-v12-tablesget-contents-table)
* [`apicops-v12 tables:list-sizes [KEYSPACE]`](#apicops-v12-tableslist-sizes-keyspace)
* [`apicops-v12 tables:topology`](#apicops-v12-tablestopology)
* [`apicops-v12 tasks:get TASKID`](#apicops-v12-tasksget-taskid)
* [`apicops-v12 tasks:list`](#apicops-v12-taskslist)
* [`apicops-v12 upgrade:check-pvc`](#apicops-v12-upgradecheck-pvc)
* [`apicops-v12 upgrade:check-subsystem-status [SUBSYSTEM]`](#apicops-v12-upgradecheck-subsystem-status-subsystem)
* [`apicops-v12 upgrade:pg-health-check`](#apicops-v12-upgradepg-health-check)
* [`apicops-v12 upgrade:stale-certs`](#apicops-v12-uogradestale-certs)
* [`apicops-v12 version:check-install`](#apicops-v12-versioncheck-install)
* [`apicops-v12 version:delete-external-events`](#apicops-v12-versiondelete-external-events)
* [`apicops-v12 version:post-upgrade`](#apicops-v12-versionpost-upgrade)
* [`apicops-v12 version:pre-upgrade`](#apicops-v12-versionpre-upgrade)
* [`apicops-v12 version:version`](#apicops-v12-versionversion)
* [`apicops-v12 webhook-subscriptions:list`](#apicops-v12-webhook-subscriptionslist)
* [`apicops-v12 webhook-subscriptions:update`](#apicops-v12-webhook-subscriptionsupdate)

## `apicops-v12 appliance-checks:appliance-pre-upgrade`

(appliance-pre-upgrade) Appliance specific script to run before upgrade to do some checks

```
USAGE
  $ apicops-v12 appliance-checks:appliance-pre-upgrade

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 appliance-pre-upgrade

EXAMPLE
  $ apicops appliance-checks:appliance-pre-upgrade                                 # Run appliance-pre-upgrade script
```

_See code: [src/commands/appliance-checks/appliance-pre-upgrade.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/appliance-checks/appliance-pre-upgrade.js)_

## `apicops-v12 catalogs:get CATALOG`

(cat) Looks up a specific catalog based on UUID or name

```
USAGE
  $ apicops-v12 catalogs:get CATALOG

ARGUMENTS
  CATALOG  The ID or name of the catalog

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 cat

EXAMPLES
  $ apicops-v12 cat 4eab42d5-6ba8-4e68-ac87-44ff229db677          # Get a catalog by UUID
  $ apicops-v12 cat cbd062ad-f04c-44cd-afae-dd6a9247309c:sandbox  # Get a catalog using the org UUID and catalog name
  $ apicops-v12 catalogs:get myuniqueorg/stuff                    # Get a catalog using the org name and catalog name
  $ apicops-v12 catalogs:get myuniquecat                          # Get a catalog using the unique catalog name
  $ apicops-v12 catalogs:get sandbox                              # Get all catalogs named sandbox
```

_See code: [src/commands/catalogs/get.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/catalogs/get.js)_

## `apicops-v12 catalogs:list`

(cats) Lists all catalogs

```
USAGE
  $ apicops-v12 catalogs:list

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 cats

EXAMPLES
  $ apicops-v12 catalogs:list  # List all catalogs
  $ apicops-v12 cats           # List all catalogs
```

_See code: [src/commands/catalogs/list.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/catalogs/list.js)_

## `apicops-v12 certs:renew-ingress-ca-certs`

(renew-ingress-ca-certs) Renew all the ingress ca certs after updating the ingress-ca expiry and restart the effected deployments.

```
USAGE
  $ apicops-v12 certs:renew-ingress-ca-certs

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 renew-ingress-ca-certs

EXAMPLES
  $ apicops-v12 certs:renew-ingress-ca-certs                  # Renews the ingress ca certs based on the updated expirey
   then restarts the effected deployments.
  $ apicops-v12 certs:renew-ingress-ca-certs --namespace <ns>
```

_See code: [src/commands/certs/renew-ingress-ca-certs.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/certs/renew-ingress-ca-certs.js)_

## `apicops-v12 custom:run SCRIPT [PARAMS]`

(runcustom) Runs the provided nodejs script inside the apim pod

```
USAGE
  $ apicops-v12 custom:run SCRIPT [PARAMS]

ARGUMENTS
  SCRIPT  The path to the script to execute inside the apim pod

  PARAMS  Any parameters to pass to the script. Separate multiple parameters by a space and enclose all parameters with
          spaces in them with "s

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 runcustom

EXAMPLES
  $ apicops-v12 custom:run /tmp/myscript.js          # Run a script with no parameters
  $ apicops-v12 custom:run /tmp/myscript.js one two  # Run a script with 2 parameters
  $ apicops-v12 runcustom /tmp/s.js one "param two"  # Run a script with 2 parameters
```

_See code: [src/commands/custom/run.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/custom/run.js)_

## `apicops-v12 debug:info`

(debug-info) Output results of several apicops-v12 commands

```
USAGE
  $ apicops-v12 debug:info

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 debug-info

EXAMPLES
  $ apicops-v12 debug:info       # Get output from following apicops-v12 commands: iss -a, webhook-subscriptions:list,
  tasks:list, catalogs:list
  $ apicops-v12 debug-info       # Get output from following apicops-v12 commands: iss -a, webhook-subscriptions:list,
  tasks:list, catalogs:list
```

_See code: [src/commands/debug/info.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/debug/info.js)_

## `apicops-v12 help [COMMAND]`

display help for apicops-v12

```
USAGE
  $ apicops-v12 help [COMMAND]

ARGUMENTS
  COMMAND  command to show help for

OPTIONS
  --all  see all commands in CLI
```

_See code: [@oclif/plugin-help](https://github.com/oclif/plugin-help/blob/v3.2.18/src/commands/help.ts)_

## `apicops-v12 logs:analyse`

(analyse) Get frequency and count of provided search terms in pod logs

```
USAGE
  $ apicops-v12 logs:analyse

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -S, --search=search          (required) Search logs for specific criteria. Can cascade searches using '*' between
                               search terms.

  -c, --container=container    Retrieve logs from container.

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -p, --pod=pod                (required) Retrieve pod logs from pod (regex).

  -s, --since=since            Retrieve K8s pod logs since (e.g. last 2 minutes would be '-s 2m').

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 analyse

EXAMPLES
  $ apicops-v12 logs:analyse -p apim -s 2m                          # Retrieve frequency and count logs from pods that
  match regex 'apim' since 2 minutes ago (-s 2m)
  $ apicops-v12 analyse -p postgres-79cf6 -c database -S "PATCH"    # Retrieve frequency and count of logs from pods
  that match regex 'postgres-79cf6' and container 'database' that contain the term 'PATCH'
  $ apicops-v12 analyse -p apim -S "PATCH*catalog"                  # Retrieve frequency and count of logs from pods
  that match regex 'apim' and contain the term 'PATCH' and the term 'catalog'. This is equivalent to piping output
  through '| grep PATCH | grep catalog'
```

_See code: [src/commands/logs/analyse.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/logs/analyse.js)_

## `apicops-v12 logs:change-log-spec`

(changelogspec) Change logging level specification and/or show large objects for apim pod logging. Script requires the username (defaults to 'admin') and password (prompted) for the Cloud Manager.

```
USAGE
  $ apicops-v12 logs:change-log-spec

OPTIONS
  -P, --podName=podName              Specify Pod to use

  -k, --kubeconfig=kubeconfig        The KUBECONFIG to use (this will override any KUBECONFIG environment variable you
                                     may have set)

  -l, --large_object=large_object    [default: not_specified] Optional: Show content of large objects (true) or not
                                     (false)

  -n, --namespace=namespace          The kubernetes namespace to target (this will override any namespace you may have
                                     set in your kubeconfig)

  -s, --specification=specification  [default: not_specified] Optional: Specification of logging level to be applied to
                                     apim pods

  -u, --username=username            [default: admin] Cloud Manager admin user name

  --logfile=logfile                  Log file

ALIASES
  $ apicops-v12 changelogspec

EXAMPLES
  $ apicops-v12 logs:change-log-spec -s audit,bhendi:error,apim:server,apim:error        # Change logging level to -s
  specification, users default Cloud Manager username of 'admin' and uses the default of 'false' for show large objects
  in logs
  $ apicops-v12 changelogspec -l true -u cloudmanagerAdminUser  # Change logging level to show large objects, and
  specify 'cloudmanagerAdminUser' as name of admin user for Cloud Manager.
```

_See code: [src/commands/logs/change-log-spec.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/logs/change-log-spec.js)_

## `apicops-v12 logs:get-log-spec`

(getlogspec) Get the log specification. Script requires the username (defaults to 'admin') and password (prompted) for the Cloud Manager.

```
USAGE
  $ apicops-v12 logs:get-log-spec

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -u, --username=username      [default: admin] Cloud Manager admin user name

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 getlogspec

EXAMPLES
  $ apicops-v12 logs:get-log-spec                         # Get Log Spec using the default Cloud Manager username of
  'admin'
  $ apicops-v12 getlogspec  -u cloudmanagerAdminUser      # Get Log Spec using 'cloudmanagerAdminUser' as name of admin
  user for Cloud Manager.
```

_See code: [src/commands/logs/get-log-spec.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/logs/get-log-spec.js)_

## `apicops-v12 logs:search`

(search) Get logs based on pod name. Can filter retrieved logs by search terms

```
USAGE
  $ apicops-v12 logs:search

OPTIONS
  -A, --after=after            Retrieve specified number of lines after a search match
  -B, --before=before          Retrieve specified number of lines before a search match
  -P, --podName=podName        Specify Pod to use
  -S, --search=search          Search logs for specific criteria. Can cascade searches using '*' between search terms.
  -c, --container=container    Retrieve logs from container.

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -o, --ordered                Order/Sort all logs by timestamp

  -p, --pod=pod                (required) Retrieve pod logs from pod (regex).

  -s, --since=since            Retrieve K8s pod logs since (e.g. last 2 minutes would be '-s 2m').

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 search

EXAMPLES
  $ apicops-v12 logs:search -p apim -s 2m                          # Retrieve logs from pods that match regex 'apim'
  since 2 minutes ago (-s 2m)
  $ apicops-v12 search -p postgres-79cf6 -c database -S "PATCH"    # Retrieve logs from pods that match regex
  'postgres-79cf6' and container 'database' that contain the term 'PATCH'
  $ apicops-v12 search -p apim -S "PATCH*catalog"                  # Retrieve logs from pods that match regex 'apim' and
   contain the term 'PATCH' and the term 'catalog'. This is equivalent to piping output through '| grep PATCH | grep
  catalog'
  $ apicops-v12 search -p apim -S "PATCH*catalog" -A 1 -B 1        # Retrieve logs from pods that match regex 'apim' and
   contain the term 'PATCH' and the term 'catalog'.  Also include 1 line before and 1 line after. This is equivalent to
  piping output through '| grep -A 1 -B 1 PATCH | grep -A 1 -B 1 catalog'
```

_See code: [src/commands/logs/search.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/logs/search.js)_

## `apicops-v12 logs:unique-logs`

(uniqueLogs) Get frequency and count each unique line in pod logs

```
USAGE
  $ apicops-v12 logs:unique-logs

OPTIONS
  -P, --podName=podName                Specify Pod to use

  -S, --search=search                  [default: :error] Search logs for specific criteria. Can cascade searches using
                                       '*' between search terms.

  -c, --container=container            Retrieve logs from container.

  -k, --kubeconfig=kubeconfig          The KUBECONFIG to use (this will override any KUBECONFIG environment variable you
                                       may have set)

  -l, --limit_outputs=limit_outputs    [default: ALL] Limit the output the the first few most common logs.

  -m, --min_occurences=min_occurences  [default: 0] Minimum number of occurences of term to search for.

  -n, --namespace=namespace            The kubernetes namespace to target (this will override any namespace you may have
                                       set in your kubeconfig)

  -p, --pod=pod                        [default: apim] Retrieve pod logs from pod (regex).

  -s, --since=since                    Retrieve K8s pod logs since (e.g. last 2 minutes would be '-s 2m').

  --logfile=logfile                    Log file

ALIASES
  $ apicops-v12 uniqueLogs

EXAMPLES
  $ apicops logs:unique-logs -p apim -s 2m                          # Retrieve frequency and count of unique logs from
  pods that match regex 'apim' since 2 minutes ago (-s 2m)
  $ apicops uniqueLogs -p apim -S "PATCH*catalog"                   # Retrieve frequency and count of unique logs from
  pods that match regex 'apim' and contain the term 'PATCH' and the term 'catalog'. This is equivalent to piping output
  through '| grep PATCH | grep catalog'
  $ apicops uniqueLogs -p apim -l 9                                 # Retrieve frequency and count of unique logs from
  pods that match regex 'apim' only showing the 9 most frequent logs (-l 9)
  $ apicops uniqueLogs -p apim -m 5                                 # Retrieve frequency and count of unique logs from
  pods that match regex 'apim' only showing logs with at least 5 occurrences (-m 5)
```

_See code: [src/commands/logs/unique-logs.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/logs/unique-logs.js)_

## `apicops-v12 organisations:get ORG`

(org) Looks up a specific organisation based on UUID or name

```
USAGE
  $ apicops-v12 organisations:get ORG

ARGUMENTS
  ORG  The ID or name of the org

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 org

EXAMPLES
  $ apicops-v12 org 4eab42d5-6ba8-4e68-ac87-44ff229db677  # Get an organisation by UUID
  $ apicops-v12 organisations:get myuniqueorg             # Get an organisation using the unique organisation name
  $ apicops-v12 organisations:get org1                    # Get all organisations named org1
```

_See code: [src/commands/organisations/get.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/organisations/get.js)_

## `apicops-v12 organisations:list`

(orgs) Lists all organisations

```
USAGE
  $ apicops-v12 organisations:list

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 orgs

EXAMPLES
  $ apicops-v12 organisations:list  # List all organisations
  $ apicops-v12 orgs                # List all organisations
```

_See code: [src/commands/organisations/list.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/organisations/list.js)_

## `apicops-v12 platform:check-appliance-disk`

(check-appliance-disk) Checks appliance disk space. (local-storage only)

```
USAGE
  $ apicops-v12 platform:check-appliance-disk

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 check-appliance-disk

EXAMPLE
  $ apicops-v12 check-appliance-disk              # Check appliance disk space. (local-storage only)
```

_See code: [src/commands/platform/check-appliance-disk.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/platform/check-appliance-disk.js)_

## `apicops-v12 platform:check-mgmt-disk`

(check-mgmt-disk) checks disk space in postgres pods

```
USAGE
  $ apicops-v12 platform:check-mgmt-disk

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 check-mgmt-disk

EXAMPLES
  $ apicops-v12 check-mgmt-disk              # Check disk space in postgres pods.
  $ apicops-v12 check-mgmt-disk -n apic      # Check disk space in postgres pods in a namespace.
```

_See code: [src/commands/platform/check-mgmt-disk.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/platform/check-mgmt-disk.js)_

## `apicops-v12 platform:check-portal-disk`

(check-portal-disk) runs check_disk script against Portal pods

```
USAGE
  $ apicops-v12 platform:check-portal-disk

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -v, --verbose                Specifies verbose mode for check_disk script on Portal pods.

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 check-portal-disk

EXAMPLES
  $ apicops-v12 check_disk              # Get output from check_disk script on Portal pods.
  $ apicops-v12 check_disk -v           # Get verbose output from check_disk script on Portal pods.
  $ apicops-v12 check_disk -n apic      # Get output from check_disk script on Portal pods in a namespace.
  $ apicops-v12 check_disk -n apic -v   # Get verbose output from check_disk script on Portal pods in a namespace.
```

_See code: [src/commands/platform/check-portal-disk.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/platform/check-portal-disk.js)_

## `apicops-v12 portal:exec`

Enable users to run command and scripts inside the portal pods.

```
USAGE
  $ apicops-v12 portal:exec

OPTIONS
  -C, --command=command        (required) The command to be executed inside a container in a pod. eg clear_site_cache
                               site-uuid

  -c, --container=container    [default: admin] Container name. The admin container is chosen as default

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -p, --podName=podName        [default: portal-www] Pod name (regex), Portal-www is chosen as a default pod.

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 exec

EXAMPLE
  $ apicops-v12 portal:exec -p portal-www -c admin -C "list_sites"        # Listing all sites
```

_See code: [src/commands/portal/exec.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/portal/exec.js)_

## `apicops-v12 postgres:pg_dump`

(pg_dump) Run 'pg_dump' against postgres pod.

```
USAGE
  $ apicops-v12 postgres:pg_dump

OPTIONS
  -F, --format=p|t|c               Format to output. 'p' = plain-text SQL (not compatible with pg_restore), 't' = tar
                                   archive suitable for 'pg_restore', 'c' = custom archive suitable for input into
                                   'pg_restore'.

  -P, --podName=podName            Specify Pod to use

  -T, --excludeTable=excludeTable  Do not dump any tables matching the table pattern. The pattern is interpreted
                                   according to the same rules as for -t.

  -a, --dataOnly                   Dump only the data, not the schema (data definitions)

  -b, --blobs                      Include large objects in the dump. This is the default behavior except when --table
                                   is specified, so the -b switch is only useful to add large objects to selective
                                   dumps.

  -d, --database=database          [default: apim] Specify the database to use.

  -f, --file=file                  Send output to the specified file. If this is omitted, the standard output is used..

  -k, --kubeconfig=kubeconfig      The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                                   have set)

  -n, --namespace=namespace        The kubernetes namespace to target (this will override any namespace you may have set
                                   in your kubeconfig)

  -t, --table=table                Dump only tables (or views or sequences or foreign tables) matching table. The table
                                   parameter is interpreted as a pattern according to the same rules used by psql's \d
                                   commands

  -v, --verbose                    Specifies verbose mode. This will cause pg_dump to output detailed object comments
                                   and start/stop times to the dump file, and progress messages to standard error.

  --logfile=logfile                Log file

ALIASES
  $ apicops-v12 pg_dump

EXAMPLES
  $ apicops-v12 pg_dump -F t -f <db_file>  # Dump all data from default dB (apim) to Format t (tar) into file <db_file>
  that can be used with `pg_restore`
  $ apicops-v12 pg_dump -d lur -F t -f <db_file>  # Dump all data from default dB lur to Format t (tar) into file
  <db_file> that can be used with `pg_restore`
  $ apicops-v12 postgres:pg_dump           # Dump all table content for the given database to terminal
  $ apicops-v12 pg_dump -a -b              # Dump only the data (-a), and blob content (-b) of all table for the given
  database
  $ apicops-v12 pg_dump -a -t task_queue   # Dump only the data (-a) for the specified table that matches 'task_queue'
  for the given database
  $ apicops-v12 pg_dump -t db_user -d lur  # Dump all table content for table that matches 'db_user' for the database
  'lur'
  $ apicops-v12 pg_dump -v -T task_queue   # Verbosely dump all content from given database except the tables that match
   pattern 'task_queue'
```

_See code: [src/commands/postgres/pg_dump.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/postgres/pg_dump.js)_

## `apicops-v12 postgres:pg_dump_all`

(pg_dump_all) does a 'pg_dump' of both databases, lur and apim. Two .sql files (lur.sql, apim.sql) are created in your local directory.

```
USAGE
  $ apicops-v12 postgres:pg_dump_all

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 pg_dump_all

EXAMPLES
  $ apicops-v12 pg_dump_all              # Dump all data from apim and lur DBs into 2 local files, lur.sql and apim.sql.
  $ apicops-v12 pg_dump_all -n apic      # Dump all data from apim and lur DBs from the namespace 'apic' into 2 local
  files, lur.sql and apim.sql.
```

_See code: [src/commands/postgres/pg_dump_all.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/postgres/pg_dump_all.js)_

## `apicops-v12 postgres:pg_restore RESTORE_FILE`

(pg_restore) Run 'pg_restore' against postgres pod.

```
USAGE
  $ apicops-v12 postgres:pg_restore RESTORE_FILE

ARGUMENTS
  RESTORE_FILE  The local filename to restore to dB

OPTIONS
  -C, --create                 Create the database before restoring into it. (When this option is used, the database
                               named with -d is used only to issue the initial CREATE DATABASE command. All data is
                               restored into the database name that appears in the archive.)

  -P, --podName=podName        Specify Pod to use

  -a, --dataOnly               Restore only the data, not the schema (data definitions)

  -c, --clean                  Clean (drop) database objects before recreating them.

  -d, --database=database      [default: apim] Specify the database to use. Connect to this database and restore
                               directly into the database

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -p, --patch                  Patch pg_restore file to add 'public' before certain functions

  -t, --table=table            Restore definition and/or data of named table only.

  -v, --verbose                Specifies verbose mode.

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 pg_restore

EXAMPLES
  Note: all mentions of <dBfile> in examples below refer a file that was generated using the tar (-F t) or custom (-F c)
   format using pg_dump
  $ apicops-v12 postgres:pg_restore <dBfile>  # Restore <dBfile> to default db (apim).
  $ apicops-v12 pg_restore -d lur <dBfile>    # Restore <dBfile> to lur dB.
  $ apicops-v12 pg_restore -p <dBfile> -v     # Restore <dBfile>, and perform 'public' patch on backup to be restored.
  Verbose output
```

_See code: [src/commands/postgres/pg_restore.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/postgres/pg_restore.js)_

## `apicops-v12 services:get-configured-gateway GATEWAY`

(configuredgateway) Looks up a specific configured gateway service based on UUID or name with an optional org/catalog/ in front of the name/UUID

```
USAGE
  $ apicops-v12 services:get-configured-gateway GATEWAY

ARGUMENTS
  GATEWAY  The ID or name of the configured gateway service

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 configuredgateway

EXAMPLES
  $ apicops-v12 services:get-configured-gateway gateway-1                             # Gets all the configured gateways
   with name gateway-1
  $ apicops-v12 configuredgateway myuniqueorg:cat:gateway-1                           # Gets all the configured gateways
   with name gateway-1 in catalog cat in organisation myuniqueorg
  $ apicops-v12 configuredgateway cbd062ad-f04c-44cd-afae-dd6a9247309c/gateway-1      # Gets all the configured gateways
   with name gateway-1 in catalog with the UUID specified
  $ apicops-v12 services:get-configured-gateway 740caa86-0c4e-4531-a460-3fb70890726e  # Gets the configured gateway with
   the UUID specified
```

_See code: [src/commands/services/get-configured-gateway.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/services/get-configured-gateway.js)_

## `apicops-v12 services:get-configured-portal PORTAL`

(configuredportal) Looks up a specific configured portal service based on UUID or name with an optional org/catalog/ in front of the name/UUID

```
USAGE
  $ apicops-v12 services:get-configured-portal PORTAL

ARGUMENTS
  PORTAL  The ID or name of the configured portal service

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 configuredportal

EXAMPLES
  $ apicops-v12 services:get-configured-portal portal-1                              # Gets all the configured portals
  with name portal-1
  $ apicops-v12 configuredportal myuniqueorg:cat:portal-1                            # Gets all the configured portals
  with name portal-1 in catalog cat in organisation myuniqueorg
  $ apicops-v12 configuredportal cbd062ad-f04c-44cd-afae-dd6a9247309c/portal-1       # Gets all the configured portals
  with name portal-1 in catalog with the UUID specified
  $ apicops-v12 services:get-configured-portal 740caa86-0c4e-4531-a460-3fb70890726e  # Gets the configured portal with
  the UUID specified
```

_See code: [src/commands/services/get-configured-portal.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/services/get-configured-portal.js)_

## `apicops-v12 services:get-gateway GATEWAY`

(gateway) Looks up a specific gateway service based on UUID or name

```
USAGE
  $ apicops-v12 services:get-gateway GATEWAY

ARGUMENTS
  GATEWAY  The ID or name of the gateway service

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 gateway

EXAMPLES
  $ apicops-v12 services:get-gateway gateway-1                # Gets the gateway with name gateway-1
  $ apicops-v12 gateway 740caa86-0c4e-4531-a460-3fb70890726e  # Gets the gateway with the UUID specified
```

_See code: [src/commands/services/get-gateway.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/services/get-gateway.js)_

## `apicops-v12 services:identify-state`

(iss) Identifies the state of any gateway and portal services and returns any associated task IDs that are incomplete. Can output compact or beautified and text or JSON

```
USAGE
  $ apicops-v12 services:identify-state

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -a, --allWebhooks            Identify the state of gateway and portal services and display data for all identified
                               webhook ids (-e -j -c have no effect when used with this)

  -e, --embellish              Output a table per service instead of single lines. In JSON mode beautify the JSON

  -j, --json                   Output as raw JSON instead of lines/tables

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -w, --webhookId=webhookId    The id of the webhook subscription record for the service you want to display information
                               about (-e -j -c have no effect when used with this)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 iss

EXAMPLES
  $ apicops-v12 services:identify-state  # Identify the state of gateway and portal services
  $ apicops-v12 iss        # Identify the state of gateway and portal services
  $ apicops-v12 iss -w <webhook id>     # Display data for provided webhook id
  $ apicops-v12 iss -a                  # Identify the state of gateway and portal services and display data for all
  identified webhook ids
```

_See code: [src/commands/services/identify-state.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/services/identify-state.js)_

## `apicops-v12 services:list-configured-gateway`

(configuredgateways) Lists all configured gateway services

```
USAGE
  $ apicops-v12 services:list-configured-gateway

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 configuredgateways

EXAMPLES
  $ apicops-v12 services:list-configured-gateways  # Lists all the configured gateways
  $ apicops-v12 configuredgateways                 # Lists all the configured gateways
```

_See code: [src/commands/services/list-configured-gateway.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/services/list-configured-gateway.js)_

## `apicops-v12 services:list-configured-portal`

(configuredportals) Lists all configured portal services

```
USAGE
  $ apicops-v12 services:list-configured-portal

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 configuredportals

EXAMPLES
  $ apicops-v12 services:list-configured-portals  # Lists all the configured portals
  $ apicops-v12 configuredportals                 # Lists all the configured portals
```

_See code: [src/commands/services/list-configured-portal.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/services/list-configured-portal.js)_

## `apicops-v12 services:list-gateways`

(gateways) Lists all gateway services

```
USAGE
  $ apicops-v12 services:list-gateways

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 gateways

EXAMPLES
  $ apicops-v12 services:list-gateways  # Lists all the gateways
  $ apicops-v12 gateways                # Lists all the gateways
```

_See code: [src/commands/services/list-gateways.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/services/list-gateways.js)_

## `apicops-v12 snapshots:get WEBHOOK_ID`

(getsnapshot) Get the snapshot for a given webhook UUID

```
USAGE
  $ apicops-v12 snapshots:get WEBHOOK_ID

ARGUMENTS
  WEBHOOK_ID  The Webhook id (UUID format) that you want to get the snapshot of

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -t, --timeout=timeout        [default: 1] Timeout (seconds) to allow all blob results to be printed

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 getsnapshot

EXAMPLES
  $ apicops-v12 snapshots:get 740caa86-0c4e-4531-a460-3fb70890726e  # Get a snapshot for the webhook id with the UUID
  specified
  $ apicops-v12 getsnapshot 740caa86-0c4e-4531-a460-3fb70890726e    # Get a snapshot for the webhook id with the UUID
  specified
```

_See code: [src/commands/snapshots/get.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/snapshots/get.js)_

## `apicops-v12 snapshots:check-subscriptions WEBHOOK_ID`

(checksubscriptions) Get the snapshot for a given webhook UUID

```
USAGE
  $ apicops-v12 snapshots:check-subscriptions WEBHOOK_ID

ARGUMENTS
  WEBHOOK_ID  The Webhook id (UUID format) that you want to check the snapshot of

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -m, --mode=mode              Mode of the script which can be fix

  -t, --timeout=timeout        [default: 1] Timeout (seconds) to allow all blob results to be printed

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 checksubscriptions

EXAMPLES
$ apicops-v12 snapshots:check-subscriptions 740caa86-0c4e-4531-a460-3fb70890726e  # Check the snapshot subscriptions for the webhook UUID specified
$ apicops-v12 checksubscriptions 740caa86-0c4e-4531-a460-3fb70890726e -m fix   # Check and create the missing subscriptions in snapshots for the webhook UUID specified
```

_See code: [src/commands/snapshots/check-subscriptions.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/snapshots/check-subscriptions.js)_

## `apicops-v12 spaces:list`

(sps) Lists all spaces

```
USAGE
  $ apicops-v12 spaces:list

OPTIONS
  -P, --podName=podName        Specify Pod to use
  -c, --catalogId=catalogId    [default: ""] The ID or name of the catalog you want to list the spaces within

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 sps

EXAMPLES
  $ apicops-v12 sps                                                   # List all spaces
  $ apicops-v12 spaces:list                                           # List all spaces
  $ apicops-v12 spaces:list -c cat1                                   # List all spaces in catalog cat1
  $ apicops-v12 sps --catalogId 745c9bab-e0ba-4d34-a284-aa3a28f77a7b  # List all spaces in the catalog identified by the
   UUID
```

_See code: [src/commands/spaces/list.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/spaces/list.js)_

## `apicops-v12 tables:get-contents [TABLE]`

(tablecontents) Dump out the contents in JSON format for the table specified. If the table is not provided you will be prompted from a list of available tables.

```
USAGE
  $ apicops-v12 tables:get-contents [TABLE]

ARGUMENTS
  TABLE  The table to list the contents of

OPTIONS
  -P, --podName=podName        Specify Pod to use
  -b, --blob                   (required) Do not inflate the blob attribute content
  -e, --embellish              Beautify the JSON

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -s, --skip=skip              Skip tables matching the regex provided

  -t, --timeout=timeout        [default: 1] Timeout (seconds) to allow all results to be printed

  --logfile=logfile            Log file

  --separate                   Used with 'get-contents all' to produce separate json files per table bundled in
                               'pgdata.tgz'.

ALIASES
  $ apicops-v12 tablecontents

EXAMPLES
  $ apicops-v12 tables:get-contents apim.webhook    # Get the contents of the apim.webhook table
  $ apicops-v12 tables:get-contents all             # Get the contents of every table written to a file 'pgdata.out'
  $ apicops-v12 tables:get-contents all --separate  # Get the contents of every table written to a file 'pgdata.out' &
  separate json files per table in 'pgdata.tgz'.
  $ apicops-v12 tablecontents lur.db_user           # Get the contents of the lur.db_user table
  $ apicops-v12 tablecontents -e lur.db_user        # Get the contents of the lur.db_user table and beautify the JSON
  $ apicops-v12 tablecontents primary_event -b      # Get the contents of the primary_event table, in default dB 'apim',
   and do not inflate the blob attribute
```

_See code: [src/commands/tables/get-contents.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/tables/get-contents.js)_

## `apicops-v12 tables:list-sizes [KEYSPACE]`

(tablesizes) List the sizes of all tables in the key space given. If no key space is provided you will be prompted from a list of available key spaces.

```
USAGE
  $ apicops-v12 tables:list-sizes [KEYSPACE]

ARGUMENTS
  KEYSPACE  (apim|lur) The keyspace in which to list the table sizes

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 tablesizes

EXAMPLES
  $ apicops tables:list-sizes apim # List the sizes of all tables in the apim key space
  $ apicops tablesizes lur         # List the sizes of all tables in the lur key space
```

_See code: [src/commands/tables/list-sizes.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/tables/list-sizes.js)_

## `apicops-v12 tables:topology`

(topology) Get the Topology for the deployment

```
USAGE
  $ apicops-v12 tables:topology

OPTIONS
  -P, --podName=podName        Specify Pod to use
  -d, --debug                  Output debug

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -u, --username=username      (required) [default: admin] Cloud Manager admin user name

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 topology

EXAMPLES
  $ apicops tables:topology          # Get the Topology for the deployment using default Cloud Admin user 'admin'.
  $ apicops topology -u newadmin     # Get the Topology for the deployment using Cloud Admin user 'newadmin'
```

_See code: [src/commands/tables/topology.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/tables/topology.js)_

## `apicops-v12 tasks:get TASKID`

(gettask) Dumps out the task identified by taskId

```
USAGE
  $ apicops-v12 tasks:get TASKID

ARGUMENTS
  TASKID  The ID of the task to show

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 gettask

EXAMPLES
  $ apicops-v12 gettask 4eab42d5-6ba8-4e68-ac87-44ff229db677    # Get a task by UUID
  $ apicops-v12 tasks:get 4eab42d5-6ba8-4e68-ac87-44ff229db677  # Get a task by UUID
```

_See code: [src/commands/tasks/get.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/tasks/get.js)_

## `apicops-v12 tasks:list`

( Lists all tasks )

```
USAGE
  $ apicops-v12 tasks:list

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

EXAMPLE
  $ apicops-v12 tasks:list  # List all tasks
```

_See code: [src/commands/tasks/list.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/tasks/list.js)_

## `apicops-v12 upgrade:check-pvc`

(check-pvc) Run Check PVC to validate all the PVCs are mounted.

```
USAGE
  $ apicops-v12 upgrade:check-pvc

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 check-pvc

EXAMPLES
  $ apicops-v12 upgrade:check-pvc                 # Run Check PVC to validate all the PVCs are mounted
  $ apicops-v12 upgrade:check-pvc -n dev          # Run Check PVC against any namespace
```

_See code: [src/commands/upgrade/check-pvc.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/upgrade/check-pvc.js)_

## `apicops-v12 upgrade:check-subsystem-status [SUBSYSTEM]`

(check-subsystem-status) checks each subsystem status is 'Running'

```
USAGE
  $ apicops-v12 upgrade:check-subsystem-status [SUBSYSTEM]

ARGUMENTS
  SUBSYSTEM  The name of a subsystem. E.g. Management, Portal, Analytics, Gateway

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 check-subsystem-status

EXAMPLES
  $ apicops-v12 upgrade:check-subsystem-status                     # Run subsystem-status.
  $ apicops-v12 upgrade:check-subsystem-status -n apic             # Run subsystem-status with namespace.
  $ apicops-v12 upgrade:check-subsystem-status Management          # Run subsystem-status to check a specific subsystem.
   E.g Management
  $ apicops-v12 upgrade:check-subsystem-status a7s                 # Run subsystem-status to check a specific subsystem.
   E.g Analytics
  $ apicops-v12 upgrade:check-subsystem-status -n apic Gateway     # Run subsystem-status with namespace and a specific
  subsystem.
  $ apicops-v12 check-subsystems-status                      # Run using alias.
```

_See code: [src/commands/upgrade/check-subsystem-status.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/upgrade/check-subsystem-status.js)_

## `apicops-v12 upgrade:pg-health-check`

(pg-health-check) Run 'pg-health-check' against postgres pod.

```
USAGE
  $ apicops-v12 upgrade:pg-health-check

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 pg-health-check

EXAMPLES
  $ apicops-v12 upgrade:pg-health-check                     # Run Postgres health check
  $ apicops-v12 upgrade:pg-health-check -n apic             # Run Postgres health check with namespace.
```

_See code: [src/commands/upgrade/pg-health-check.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/upgrade/pg-health-check.js)_

## `apicops-v12 upgrade:stale-certs`

(stale-certs) Run stale-certs to identify which secrets are stale.

```
USAGE
  $ apicops-v12 upgrade:stale-certs
OPTIONS
  -P, --podName=podName        Specify Pod to use
  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)
  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)
  --logfile=logfile            Log file
ALIASES
  $ apicops-v12 stale-certs
EXAMPLES
  $ apicops-v12 upgrade:stale-certs                     # Run Detect stale certs to identify the certs
  $ apicops-v12 upgrade:stale-certs -n apic             # Run Run Detect stale certs against any namespace.
```

_See code: [src/commands/upgrade/stale-certs.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/upgrade/stale-certs.js)_

## `apicops-v12 version:check-install`

(check-install) Script can be run at any time, will check the APIC install.

```
USAGE
  $ apicops-v12 version:check-install

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -t, --[no-]topology          Get Topology info

  -u, --username=username      [default: admin] Cloud Manager admin user name

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 check-install

EXAMPLE
  $ apicops version:check-install                                 # Run check-install script
```

_See code: [src/commands/version/check-install.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/version/check-install.js)_

## `apicops-v12 version:delete-external-events`

(delete-external-events) Remove external_events table.

```
USAGE
  $ apicops-v12 version:delete-external-events

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 delete-external-events

EXAMPLE
  $ apicops-v12 version:delete-external-events                 # Remove external_events table
```

_See code: [src/commands/version/delete-external-events.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/version/delete-external-events.js)_

## `apicops-v12 version:post-upgrade`

(postupgrade) Script to run after upgrade to do some checks

```
USAGE
  $ apicops-v12 version:post-upgrade

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -t, --topology               Get Topology info

  -u, --username=username      [default: admin] Cloud Manager admin user name

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 postupgrade

EXAMPLE
  $ apicops version:post-upgrade -l output                          # Run post-upgrade script and log output to file
  'output.log'
```

_See code: [src/commands/version/post-upgrade.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/version/post-upgrade.js)_

## `apicops-v12 version:pre-upgrade`

(preupgrade) Script to run before upgrade to do some checks

```
USAGE
  $ apicops-v12 version:pre-upgrade

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -t, --[no-]topology          Get Topology info

  -u, --username=username      [default: admin] Cloud Manager admin user name

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 preupgrade

EXAMPLE
  $ apicops version:pre-upgrade                                 # Run pre-upgrade script
```

_See code: [src/commands/version/pre-upgrade.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/version/pre-upgrade.js)_

## `apicops-v12 version:version`

```
USAGE
  $ apicops-v12 version:version
```

_See code: [src/commands/version/version.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/version/version.js)_

## `apicops-v12 webhook-subscriptions:list`

(webhooksubs) Lists all webhook subscriptions

```
USAGE
  $ apicops-v12 webhook-subscriptions:list

OPTIONS
  -P, --podName=podName        Specify Pod to use

  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may
                               have set)

  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in
                               your kubeconfig)

  -t, --timeout=timeout        [default: 1] Timeout (seconds) to allow all results to be printed

  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 webhooksubs

EXAMPLES
  $ apicops-v12 webhook-subscriptions:list  # List all webhook subscriptions
  $ apicops-v12 webhooksubs                 # List all webhook subscriptions
```

_See code: [src/commands/webhook-subscriptions/list.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/webhook-subscriptions/list.js)_

## `apicops-v12 webhook-subscriptions:update`

(updatewebhook) Update the state of a webhook subscription record

```
USAGE
  $ apicops-v12 webhook-subscriptions:update

OPTIONS
  -P, --podName=podName                  Specify Pod to use
  -d, --debug                            Output debug

  -k, --kubeconfig=kubeconfig            The KUBECONFIG to use (this will override any KUBECONFIG environment variable
                                         you may have set)

  -n, --namespace=namespace              The kubernetes namespace to target (this will override any namespace you may
                                         have set in your kubeconfig)

  -s, --state=online|offline_configured  (required) State you wish to transition too

  -u, --username=username                (required) [default: admin] Cloud Manager admin user name

  -w, --webhookid=webhookid              (required) Webhook Subscription ID you wish to change the state for. Get this
                                         from the output of `apicops-v12 iss'

  --logfile=logfile                      Log file

ALIASES
  $ apicops-v12 updatewebhook

EXAMPLES
  $ apicops-v12 updatewebhook -w 843d3499-da6b-4acd-a265-3753d7a71668 -s offline_configured  # Update the subscription
  to offline_configured for the gateway identified by webhook UUID
  $ apicops-v12 webhook-subscriptions:update -w 843d3499-da6b-4acd-a265-3753d7a71668 -s offline_configured  # Update the
   subscription to offline_configured for the gateway identified by webhook UUID
  where, '-w <Webhook Subscription ID>' can be obtained from the output of 'apicops-v12 iss'
```

_See code: [src/commands/webhook-subscriptions/update.js](https://github.com/velox/apicops/blob/v0.10.62/src/commands/webhook-subscriptions/update.js)_

## `apicops-v12 certs:gather`

(gather) Gather all certificates without their key. Useful for debugging certificate issues.

```
USAGE
  $ apicops-v12 certs:gather

OPTIONS
  -P, --podName=podName        Specify Pod to use
  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may have set)
  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in your kubeconfig)
  --logfile=logfile            Log file

ALIASES
  $ apicops-v12 gather

EXAMPLES
  $ apicops-v12 certs:gather             # Gather certificates
  $ apicops-v12 certs:gather -n apic     # Gather certificates on the namespace called apic
```
_See code: [src/commands/certs/gather.js](https://github.ibm.com/velox/apicops/blob/v0.10.62/src/commands/certs/gather.js)_

## `apicops-v12 certs:validate`

(validate) Run validate to identify which certificates and/or secrets are invalid.

```
USAGE
  $ apicops-v12 certs:validate

OPTIONS
  -P, --podName=podName              Specify Pod to use
  -c, --compare=compare              Compare against the provided tgz (useful when comparing multi-namespace or two data center deployments)
  -i, --issuerSubject=issuerSubject  ingress issuer name used on the deployment (default = ingress-ca)
  -k, --kubeconfig=kubeconfig        The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may have set)
  -m, --multiSiteMode=multiSiteMode  Set for a two data center deployment (one of [active, passive])
  -n, --namespace=namespace          The kubernetes namespace to target (this will override any namespace you may have set in your kubeconfig)
  -s, --subsystem=subsystem          (required) Subsystem to validate (one or more of: [management, portal, gateway, analytics, all])
  --logfile=logfile                  Log file

ALIASES
  $ apicops-v12 validate

EXAMPLES
  $ apicops-v12 certs:validate -s management                        # Validate management certificates
  $ apicops-v12 certs:validate -s all                               # Validate certificates for all subsystems including cross-subsystem certificates
  $ apicops-v12 certs:validate -s portal,management                 # Validate portal and management certificates including their cross-subsystem certificates
  $ apicops-v12 certs:validate -n apic -s analytics                 # Validate analytics certificates on the given namespace
  $ apicops-v12 certs:validate -i "O=ACME, CN=Acme CA" -s all       # Validate all certificates using a custom ingress issuer name
  $ apicops-v12 certs:validate -s all -c passive-dc-certs.tgz       # Validate all certificates and compare with any subsystems found within the provided tgz
```

_See code: [src/commands/certs/validate.js](https://github.ibm.com/velox/apicops/blob/v0.10.62/src/commands/certs/validate.js)_
<!-- commandsstop -->
