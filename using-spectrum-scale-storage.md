---

copyright:
  years: 2022, 2025
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

# Using Spectrum Scale storage
{: #using-spectrum-scale-storage}

{{site.data.keyword.scale_full}} is a cluster file system that provides simultaneous access to a single file system from multiple compute nodes. In the {{site.data.keyword.symphony_full_notm}} offering, {{site.data.keyword.scale_short}} provides applications running in static compute cluster nodes with high-performance access to a shared data space. For more technical information, see the [{{site.data.keyword.scale_full_notm}}](https://www.ibm.com/docs/en/storage-scale/5.2.2){: external} product documentation.
{: shortdesc}

The {{site.data.keyword.symphony_full_notm}} offering uses [{{site.data.keyword.vpc_short}} virtual server instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui) provisioned with [instance storage](/docs/vpc?topic=vpc-instance-storage) for the {{site.data.keyword.scale_short}} storage nodes. By virtue of being on instance storage, this storage option can be used for scratch data use cases. When a storage node virtual server instance is rebooted, the data is preserved. However, when the instance is deleted, the instance storage data is lost. There is no automatic replication to persistent storage, for example, {{site.data.keyword.cos_full_notm}}, that is provided in the current implementation but you can add it on your own if required.

![Architecture diagram](images/hpccluster_sym_scale_architecture.svg){: caption="Architecture diagram of an {{site.data.keyword.symphony_full_notm}} cluster using {{site.data.keyword.scale_short}} storage on {{site.data.keyword.cloud_notm}}" caption-side="bottom"}

## Before you begin
{: #before-you-begin}

Before you begin, make sure to complete the steps for [Getting started with {{site.data.keyword.symphony_full_notm}}](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-getting-started-tutorial).

## Configuring Spectrum Scale storage
{: #configure-spectrum-scale-storage}

To configure and use Spectrum Scale storage, the `spectrum_scale_enabled` [deployment value](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-deployment-values) must be set to "true". The remaining {{site.data.keyword.scale_short}} storage parameters ("scale_xx") must be set to deploy the wanted storage cluster configuration.

## Checking the Spectrum Scale file system
{: #check-spectrum-scale-file-system}

Complete the following steps to check the {{site.data.keyword.scale_short}} file system:

1. Log in to a Scale storage node by using SSH. Details are available in the logs output with the following `spectrum_scale_storage_ssh_command`:

    ```sh
    ssh -J root@52.116.122.64 root@10.240.128.37
    ```
    {: codeblock}

2. Display the gpfs cluster setup on the Scale storage node:

    ```sh
    /usr/lpp/mmfs/bin/mmlscluster
    ```
    {: codeblock}

3. Display the file system size:

    ```sh
    /usr/lpp/mmfs/bin/mmlsfs all -i
    ```
    {: codeblock}

4. Display the file server details. This command can be used to validate file block size (node size in bytes):

    ```sh
    /usr/lpp/mmfs/bin/mmlsfs fs1
    ```
    {: codeblock}

5. Log in to the Symphony primary node by using SSH:

    ```sh
    ssh -J root@52.116.122.64 root@10.240.128.41
    ```
    {: codeblock}

6. Display the gpfs cluster setup on the compute nodes, which include the primary, secondary, management, and worker nodes:

    ```sh
    /usr/lpp/mmfs/bin/mmlscluster
    ```
    {: codeblock}

7. Create a file on the mount point path, for example, `/gpfs/fs1`, and verify on the other compute nodes that the file can be accessed.
