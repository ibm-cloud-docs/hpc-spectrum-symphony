---

copyright:
  years: 2023, 2025
lastupdated: "2025-03-19"

keywords:

subcollection: hpc-spectrum-symphony

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Accessing the GUI
{: #accessing-gui}

After the cluster setup is done, you can monitor the resources and status of the service directly from the {{site.data.keyword.scale_full_notm}} GUI for both the compute and storage clusters. For more information about the GUI, see [{{site.data.keyword.scale_full_notm}} GUI](https://www.ibm.com/docs/en/storage-scale/5.2.2?topic=reference-storage-scale-gui){: external}.
{: shortdesc}

## Before you begin
{: #before-you-begin}

Before you begin accessing the {{site.data.keyword.scale_short}} GUI, review the following considerations and requirements:

* The initial setup must be done from your local system.
* Provide the SSH key path from the local system that is used to configure the compute and storage nodes.
* It is recommended to use the Safari browser to access the GUI.
* If you encounter slowness in loading or accessing the GUI, clear the browser's cache.
* You cannot open both the compute and storage GUIs on the same port 22443. Use a different port or close one of the GUIs so you can access the other GUI cluster.

## Gathering IP addresses
{: #gather-IP-addresses}

To access the GUI, you need to gather the IP addresses of both the storage and compute nodes and the floating IP address that is attached to the login node.

1. In the {{site.data.keyword.cloud}} console on the **Schematics > Workspaces** page, select your workspace.
2. Click Resources, and then in the list of Terraform resources, click the **primary** resource link, which opens a new window with a list of the virtual server instances that were provisioned.
3. On the _Virtual server instances for VPC_ page, locate the IP address for `<prefix>-scale-storage-0`, which is the storage IP address that you need. Then locate the IP address for `<prefix>-primary-0`, which is the compute IP address that you need.
4. In addition to these two IP addresses, pick up the floating IP address that is attached to the login node.

## Setting up access
{: #set-up-access}

You need to separately access both the storage and compute clusters.

This solution's automation always uses the same IP addresses, so there might be issues in the `~/.ssh/known_hosts` file. If you encounter problems with this file, remove the offending entries from that file and make sure to get a clean SSH in place, otherwise the GUI access won't work.
{: tip}

### Accessing the storage cluster
{: #access-storage-cluster}

1. Open a new command-line terminal.
2. Run the following command to access the storage cluster:

    ```shell
    ssh -L 22443:localhost:443 -J root@{FLOATING_IP_ADDRESS} root@{STORAGE_NODE_IP_ADDRESS}
    ```
    {: pre}

    where `FLOATING_IP_ADDRESS` needs to be replaced with the floating IP address that you identified, and `STORAGE_NODE_IP_ADDRESS` needs to be replaced with the storage IP address associated with `<prefix>-scale-storage-0`, which you gathered earlier.
3. Open a browser on the local system, and run https://localhost:22443. You get an SSL self-assigned certificate warning with your browser the first time that you access this URL.
4. Enter your login credentials that you set up when you created your workspace to access the Spectrum Scale GUI.

### Accessing the compute cluster
{: #access-compute-cluster}

1. Open a new command-line terminal.
2. Run the following command to access the compute cluster:

    ```shell
    ssh -L 21443:localhost:443 -J root@{FLOATING_IP_ADDRESS} root@{COMPUTE_NODE_IP_ADDRESS}
    ```
    {: pre}

    where `FLOATING_IP_ADDRESS` needs to be replaced with the floating IP address that you identified, and `COMPUTE_NODE_IP_ADDRESS` needs to be replaced with the storage IP address associated with `<prefix>-primary-0`, which you gathered earlier.
3. Open a browser on the local system, and run https://localhost:21443. You get an SSL self-assigned certificate warning with your browser the first time that you access this URL.
4. Enter your login credentials that you set up when you created your workspace to access the {{site.data.keyword.scale_short}} GUI.
