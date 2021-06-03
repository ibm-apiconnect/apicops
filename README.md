apicops
===============

<!-- toc -->
* [About](#about)
* [Latest v2018 release](#latest-v2018-release)
* [Latest v10 release](#latest-v10-release)
* [Warning](#warning)
* [Installing](#installing)
* [Requirements](#requirements)
* [Usage](#usage)
* [Commands](#commands)
<!-- tocstop -->

# About
`apicops` is a command line interface to IBM API Connect v2018 specifically targetted at Operations teams. It contains commands to check the healthy running of the system as well as some commands to remedy specific problems if encountered.

It is in active development and new versions will be posted here regularly. Please always use the latest version, as it will contain the latest improvements, commands, and any bug fixes.

Please note this has only been tested against IBM API Connect v2018.4.1.6+.

# Latest v2018 release

https://github.com/ibm-apiconnect/apicops/releases/tag/v0.2.202

# Latest v10 release

https://github.com/ibm-apiconnect/apicops/releases/tag/v0.10.47


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
Alternatively, you can pass an argument -n <your namespace here> to all apicops commands to specify the namespace for your deployment.  

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

## Commonly used commands:
1. `apicops iss`
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

1. `apicops task-queue:list-stuck-tasks` and `apicops task-queue:fix-stuck-tasks`
'list-stuck-tasks' will list the task ids of tasks that have not been updated with a specified timeframe (default is 15 minutes).
'fix-stuck-tasks' will renew the tasks that 'list-stuck-tasks' identified.
```
USAGE
  $ apicops task-queue:list-stuck-tasks

OPTIONS
  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may have set)
  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in your kubeconfig)
  -s, --staleTime=staleTime    [default: 900] Time (seconds) to look for stuck tasks
  --kinds=kinds                Comma separated list of kinds to look for stuck tasks

ALIASES
  $ apicops liststucktasks

EXAMPLES
  $ apicops task-queue:list-stuck-tasks  # Lists all stuck task in task queue
  $ apicops liststucktasks        # Lists all stuck task in task queue
```

```
USAGE
  $ apicops task-queue:fix-stuck-tasks

OPTIONS
  -i, --ids=ids                Include an option comma separated list of task ids
  -k, --kubeconfig=kubeconfig  The KUBECONFIG to use (this will override any KUBECONFIG environment variable you may have set)
  -n, --namespace=namespace    The kubernetes namespace to target (this will override any namespace you may have set in your kubeconfig)
  -s, --staleTime=staleTime    [default: 900] Time (seconds) to look for stuck tasks

ALIASES
  $ apicops fixstucktasks

EXAMPLES
  $ apicops task-queue:fix-stuck-tasks  # Fixes stuck task in task queue
  $ 
```


# Commands
Starting with v0.2.94, please refer to the README.md file for a list of the available commands available for a given release.
