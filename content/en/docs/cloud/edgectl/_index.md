---
title: "edgectl"
description: "edgectl is the CLI for Edgeworx Cloud."
linkTitle: "edgectl CLI"
weight: 300
identifier: edgectl
aliases:
- /edgeworx/edgeworx-cloud/get-started-edgectl
- /docs/cloud/edgectl
- /docs/cloud/get-started-edgectl
---


_edgectl_ is [Edgeworx Cloud's](/docs/cloud/start-portal) command line interface (CLI). It can be used to manage Edgeworx
Cloud
[accounts, organizations,](/docs/more/terminology/#account--org) [projects](/docs/more/terminology#project), [nodes](/docs/cloud/adding-nodes),
and [applications](/docs/more/terminology#application).

In this section we will show you how to use _edgectl_ to get started with your first project and
deploy some live [microservices](/docs/apps/microservices) to the edge.

## Create a Edgeworx Cloud Account

Before installing and using _edgectl_, we must first create an account
via [Edgeworx Cloud Portal](https://cloud.Edgeworx.io) (see [docs](/docs/cloud/start-portal)).

Navigate to [Cloud Portal](https://cloud.Edgeworx.io) and click the `Create Account` button in the top
right.

Enter your unique username and hit `NEXT`.

On the "Welcome" page, choose an auth provider or provide your own email and password.

## Install edgectl

_edgectl_ currently supports Mac, Linux & Windows.

{{<tabs name="platform" >}}

{{%tab name="macOS" %}}
Use [brew](https://brew.sh) to install _edgectl_:

```bash
brew install edgeworx/edgectl/edgectl
```

{{% /tab %}}

{{%tab name="Windows" %}}
On Windows, use [scoop](https://scoop.sh) to install _edgectl_:

```shell
scoop bucket add edgectl https://github.com/edgeworx/edgectl
scoop install edgectl
```

{{% /tab %}}

{{%tab name="Linux" %}}
On Linux distros, use `get-edgectl.bash`:

```bash
curl -s https://cloud.edgeworx.io/get-edgectl.bash | sudo bash
```

{{% /tab %}}
{{</tabs>}}

### Enable edgectl tab completion

It is highly recommended that you enable tab completion, so that `edgectl` can help you complete
commands and arguments. The installation process is shell-dependent: `bash`, `zsh`, `fish`,
and `powershell` are supported. Execute `edgectl completion --help` for detailed instructions for
your shell.

## Use edgectl

Now we are ready to use _edgectl_ to login and start managing our Edgeworx Cloud resources.

### Login

_edgectl_ requires an [Access Token](/docs/cloud/access-tokens/). You can get one
via: `edgectl login`, which will open a web browser on `cloud.Edgeworx.io`. After authentication,
_edgectl_ will receive the account's master _Personal Access Token_, and will be logged in.

If a web browser is not available (e.g. SSH'd into a box), you can also login by providing either
a _Personal Access Token_ or _Project Access Token_ from `cloud.Edgeworx.io`. For _Personal Access
Token_, click `Access Tokens` in the upper-right account menu. For _Project Access Token_, click the
settings (gear) icon on the project page. Once you have the _Access Token_, you can execute:

```bash
edgectl login --token xyz
```

## Command Overview

Let's get familiar with _edgectl_. We can observe the main use cases by running the top-level help
command:

```bash
edgectl --help
```

This produces help output similar to:

```text
Available Commands:
  completion  Generate completion script
  create      Create a resource
  delete      Delete a resource
  deploy      Deploy resources
  describe    Show details of a resource
  get         Display one or many resources
  help        Help about any command
  login       Login to Edgeworx Cloud API
  logout      Logout from Edgeworx Cloud API
  logs        Display microservice logs
  migrate     Migrate a resource
  patch       Update fields of a resource
  ping        Ping Edgeworx Cloud API
  rename      Rename a resource
  restart     Restart a resource
  rotate      Rotate a resource
  set         Set values or features on a resource
  ssh         SSH into a node.
  start       Start a resource
  stop        Stop a resource
  version     Print version info
```

For each of the commands, you can execute `edgectl CMD --help` for help on that command or to list
subcommands. For example, if you run `edgectl get --help`:

```text
$ edgectl get --help
Display one or many resources.

Usage:
  edgectl get [command]

Available Commands:
  account              Get account
  app                  Get details of an app
  apps                 List apps in project
  default              Get an edgectl default value
  defaults             List edgectl defaults
  heartbeat            Get heartbeat details
  heartbeats           List node heartbeats
  invites              List invites
  members              List members of org
  microservice         Get details of an app microservice
  microservices        List app microservices
  node                 Get node details
  node-register-script Get the node registration script for a project
  nodes                List nodes for project
  org                  Get org details
  project              Get project details
  projects             List projects in org
  registries           List container registries for project
  registry             Get details of container registry
  token                Get access token
  tokens               List access tokens
```

## Configure Defaults

After your first `edgectl login`, an initial org and project likely exist. We can use
the `edgectl set default` command to set a default org and project. This allows us to use many
commands without having to provide the `--org` and `--project` flags.

{{<info>}}Note that _edgectl_ defaults are local to your environment (typically saved
in `$HOME/.config/edgeworx/edgectl/edgectl.yml`).{{</info>}}

Let's start by viewing the current defaults:

```text
edgectl get defaults
```

On first login, the default org is set to your account's personal org. You typically don't want to
change the default org, but you probably want to change the default project. First, let's list the
available projects in your org:

```text
edgectl get projects
```

And then set the default project (note that there is tab-completion available for the project name).

```text
edgectl set default project alice/edge-project-1
```

Similarly, you can change other defaults, e.g.

```text
edgectl set default format json
```
