---
title: "Add a Virtual Node"
description: "Add a Virtual Node (lightweight local VM) to Edgeworx Cloud"
weight: 450
---

Typically you want to push your apps to a "real edge node".
However, if you do not have a device to test on, you can simulate one using the Virtual Node feature.

Adding a Virtual Node sets up a new Ubuntu VM locally and registers it as a node to a Edgeworx Cloud project
via the edgectl CLI. MacOS and Windows users can create Virtual Nodes using Edgeworx Cloud Portal and
edgectl. Linux users can directly use the Register Node command on their terminal.

### Prerequisites

- [Edgeworx Cloud Account (Free)](https://cloud.edgeworx.io)
- Install [edgectl CLI]({{<ref "/docs/cloud/edgectl/">}})
- Install [Multipass](https://multipass.run): it is used by edgectl to manage VMs.

{{<warning>}} Virtual Nodes are not currently compatible with attached video devices.
You can still build locally using attached video and deploy to edge devices with attached video.
{{</warning>}}

## Creating a Virtual Node

### Add a Virtual Node using Edgeworx Cloud Portal

Log into [Edgeworx Cloud](https://cloud.edgeworx.io) and select the project to which you want to add the
node.

![Add Virtual Node](/images/project_view_empty_sandbox.png)

Click the `+ ADD NODE` button. This will bring up a modal dialog which shows the different types of
nodes you can add to your project.

![Portal: Select Virtual Node](/images/node_modal.png)

Choose `VIRTUAL NODE` to get the instructions for adding a virtual node.

![Virtual Node Script](/images/node_modal_vn_script.png)

Make sure you have the latest versions of [edgectl]({{<ref "/docs/cloud/edgectl/">}})
and [Multipass](https://multipass.run) installed. Click the `COPY` button to copy the command to
your clipboard. This command starts an Ubuntu VM which registers
itself with Edgeworx Cloud as a Virtual Node.

### Run the Virtual Node Registration Script

Paste the command line that you copied in the previous step into your local terminal. The entire install
process can take up to a few minutes (depending on the spec of your machine, your internet
connection speed, and other dependencies).

![Install Node](/images/virtual_node_script_terminal.png)

{{<info>}} If you would like to choose a specific name for your node, update
the `--name="your-choice-of-name"` in the _edgectl create virtual-node_ command as seen in the above
example. {{</info>}}

### View the Node in Your Edgeworx Cloud Project

Switch back to Edgeworx Cloud in your browser and if you have not done so yet, click the `DONE` button
in the modal dialog. You should see your new node `ONLINE` in your Nodes list. If you do not see
your node online, see troubleshooting (below) for more information.

![Node Added](/images/project_view_sandbox_vn.png)

You now have an edge node, let's start using it!

### Add a Virtual Node using edgectl

If you do not have edgectl, open the hamburger menu in the top right of the portal and click the `Get the CLI` link. A modal will appear with a script Linux, MacOS, and Windows.

Once you log into your account via the CLI, set your default Org and Project and run the below command to view any nodes in the project.

```bash
edgectl login 
edgectl set default org xxxxxx 
edgectl set default project xxxxxx
## Replace x's with your appropriate Org and Project name
```

You can then create a Virtual Node using the below command.

```bash
edgectl create virtual-node 
```

You can use Multipass' flags in edgectl to control the amount of memory, CPU cores, and storage the Virtual Node allocates.

### Delete a Virtual Node

We recommend using `edgectl delete virtual-node` command to delete the Virtual Node after use, so
that all the resources used are cleaned up properly i.e. Ubuntu VM.

{{<warning>}} Deleting Virtual Nodes in the Portal will require you delete the virtual machines manually where ever they are hosted. Using `edgectl delete virtual-node <nameofnode>` or `multipass delete <nameofvm>` on the host machine will delete the virtual machine.
{{</warning>}}

## Troubleshooting

<details>
  <summary>Unable to create Virtual Node with default values on Windows machine</summary>
    We can modify the default values based on our Windows machine spec. For example:

```bash
edgectl create virtual-node --name=Edgeworx-node --cpus 2
```

Below are the default values used to spin up a multipass VM.

```text
-d, --disk    string   Disk space to allocate. Positive integers, in bytes, or with K, M, G suffix. Minimum: 512M, default: 15G.
-c, --cpus    string   Number of CPUs to allocate. Minimum: 1, default: 2.
-m, --mem     string   Amount of memory to allocate. Positive integers, in  bytes, or with K, M, G suffix. Minimum: 128M, default: 1G.
    --network string   Add a network interface to the instance, where <spec> is in the "key=value,key=value" format, with the following keys available:
                       name: the network to connect to (required), use the networks command for a list of possible values,
                       or use 'bridged' to use the interface configured via "multipass set local.bridged-network".
                       mode: auto|manual (default: auto) mac: hardware address (default: random).
                     You can also use a shortcut of "<name>" to mean "name=<name>"
```

</details>
<details>
  <summary>Unable to view the output from a Virtual Node (incorrect IP).</summary>
<strong>Known Issue:</strong> Depending on the particular network setup, the Virtual Node IP address displayed in the portal may not be correct.
Use <code>multipass ls</code> to retrieve the correct IP.
</details>
<details>
  <summary>Use <code>edgectl delete virtual-node</code> in favor of <code>edgectl delete node</code>.</summary>
The <code>edgectl delete node</code> command deletes the node from Edgeworx Cloud, but does not delete the local VM.
Use <code>edgectl delete virtual-node</code>
to delete both the node and the local VM.
</details>
<details>
  <summary>Unable to SSH into the Virtual Node after the machine went into idle state.</summary>
<strong>Known Issue:</strong> There is an <a href="https://www.virtualbox.org/ticket/14374?cversion=2&cnum_hist=66">long-standing issue</a> with internet
sharing of virtual network when using multipass with Virtual Box driver.
</details>
<details>
  <summary>Unable to view attached video device in output</summary>
<strong>Known Issue:</strong> At the moment Virtual Node doesn't support mounting external devices, such as cameras, outside of the VM on the host machine.
</details>
