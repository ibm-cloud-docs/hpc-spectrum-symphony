---

copyright:
  years: 2023, 2024
lastupdated: "2024-11-15"

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
{:faq: data-hd-content-type='faq'}

# IBM Spectrum Symphony FAQs
{: #spectrum-symphony-faqs}

## Where are the Terraform files that are used by the offering located?
{: #terraform-location}
{: faq}

The Terraform-based templates can be found in this public [GitHub repository](https://github.com/IBM-Cloud/hpc-cluster-symphony){: external}.

## What Spectrum Symphony and Spectrum Scale versions are used in cluster nodes deployed with this offering?
{: #my-faq-packages}
{: faq}

Cluster nodes that are deployed with this offering include {{site.data.keyword.symphony_full_notm}} 7.3.2 Advanced Edition. See the following for a summary of the features associated with each edition: [{{site.data.keyword.symphony_full_notm}} editions](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=foundations-spectrum-symphony-editions){: external}.

If the cluster uses {{site.data.keyword.scale_short}} storage, the storage nodes include {{site.data.keyword.scale_full_notm}} 5.2.1.1 software. For more information, see the [{{site.data.keyword.scale_full_notm}}](https://www.ibm.com/docs/en/storage-scale/5.2.1){: external} product documentation.

## What locations are available for deploying VPC resources?
{: #locations-vpc-resources}
{: faq}

Available regions and zones for deploying VPC resources and mapping them to city locations and data centers can be found in [Locations for resource deployment](/docs/overview?topic=overview-locations).

## What permissions are required in order to create a cluster using the offering?
{: #permissions-cluster-offering}
{: faq}

Instructions for setting the appropriate permissions for {{site.data.keyword.cloud_notm}} services that are used by the offering to create a cluster can be found in [Granting user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources), [Managing user access for Schematics](/docs/schematics?topic=schematics-access), and [Assigning access to Secrets Manager](/docs/secrets-manager?topic=secrets-manager-assign-access).

## How to use SSH among the nodes?
{: #ssh-among-nodes}
{: faq}

All the nodes in the HPC cluster have the same public key that you register at your cluster creation. You can use ssh-agent forwarding, which is a common technique to access remote nodes that have the same public key. It automates to securely forward private keys to remote nodes. Forwarded keys are deleted immediately after a session is closed.

To securely forward private keys to remote nodes, you need to do `ssh-add` and `ssh -A`.

```sh
[your local PC]~$ ssh-add {id_rsa for symphony cluster}
[your local PC]~# ssh -A -J root@jumpbox_fip root@management_private_ip
...
[root@management]~# ssh -A worker_private_ip
```
{: codeblock}

For Mac OS X, you can persist `ssh-add` by adding the following configuration to `.ssh/config`:

```sh
Host *
  UseKeychain yes
  AddKeysToAgent yes
```
{: codeblock}

You can even remove `-A` by adding "ForwardAgent yes" to `.ssh/config`.

## How many worker nodes can be deployed in the Spectrum Symphony cluster through this offering?
{: #worker-nodes}
{: faq}

Before deploying a cluster, it is important to ensure that the VPC resource quota settings are appropriate for the size of the cluster that you would like to create (see [Quotas and service limits](/docs/vpc?topic=vpc-quotas)).

The maximum number of worker nodes that are supported for the deployment value `worker_node_max_count` is 500 (see [Deployment values](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-deployment-values)). The `worker_node_min_count` variable specifies the number of worker nodes that are provisioned at the time that the cluster is created, which will exist throughout the life of the cluster. The delta between those two variables specifies the maximum number of worker nodes that can either be created or destroyed by the Symphony Host Factory auto-scaling feature.

The {{site.data.keyword.symphony_short}} offering supports both bare metal worker nodes and {{site.data.keyword.scale_short}} storage nodes. The following combinations of values are supported:
* If `worker_node_type` is set as `baremetal`, a maximum of 16 bare metal nodes are supported.
* If `spectrum_scale_enabled` is set to `true` and `storage_type` is set as `persistent`, a maximum of 10 bare metal nodes are supported.

For more information, see [Deployment values](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-deployment-values).

When creating or deleting a cluster with many worker nodes, you might encounter VPC resource provisioning or deletion failures. In those cases, running the {{site.data.keyword.bpshort}} `apply` or `destroy` operation again might result in the remaining resources being successfully provisioned or deleted. If you continue to see errors, see [Getting help and support](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-getting-help-and-support).

## Why are there two different resource group parameters that can be specified in the IBM Cloud catalog tile?
{: #resource-group-parameters}
{: faq}

The first resource group parameter entry in the **Configure your workspace** section in the {{site.data.keyword.Bluemix_notm}} catalog applies to the resource group where the {{site.data.keyword.bpshort}} workspace is provisioned on your {{site.data.keyword.Bluemix_notm}} account. The value for this parameter can be different than the one used for the second entry in the **Parameters with default values** section in the catalog. The second entry applies to the resource group where VPC resources are provisioned. As specified in the description for this second `resource_group` parameter, only the default resource group is supported for use of the Symphony Host Factory auto-scaling feature.

## Can I use the Spectrum Symphony Host Factory feature for auto scaling on any cluster deployed with this offering?
{: #host-factory-auto-scaling}
{: faq}

No, the use of Host Factory to provision and delete compute nodes is not supported in the following cases:

* Provisioning and deleting compute nodes on dedicated hosts. Only static compute nodes can be deployed on dedicated hosts.
* When using {{site.data.keyword.scale_short}} for shared storage in the cluster.

## Where can you find the custom image name to image ID mappings for each cloud region?
{: #custom-image-name-mappings}
{: faq}

The mappings can be found in the `image-map.tf` file and the `scale-image-map.tf` file in this public [GitHub repository](https://github.com/IBM-Cloud/hpc-cluster-symphony){: external}.

## Why does the `egosh` command not work if tried immediately after Schematics provisions the cluster?
{: #egosh-command-not-working}
{: faq}

This is expected behavior. Even after the {{site.data.keyword.bpshort}} web console shows that the cluster is successfully provisioned, there are still some tasks that run in the background for several minutes. Allow a few minutes (typically 2 minutes is sufficient) after the cluster gets provisioned for `egosh` to be available.

## Why does cluster creation by using dedicated hosts fail sometimes with the error `status of dedicated host is failed`?
{: #cluster-creation-dedicated-hosts-fails}
{: faq}

In some regions, dedicated hosts have a limitation on the number of virtual server instances that can be placed on them at one time. You can try to provision the cluster with a smaller number of virtual server instances to overcome this.

## Why does Spectrum Scale not allow use of the default value of 0.0.0.0/0 for security group creation?
{: #security-group-default-value}
{: faq}

For security reasons, {{site.data.keyword.scale_short}} does not allow you to provide a default value that would allow network traffice from any external device. Instead, you can provide the address of your user system (for example, by using https://ipv4.icanhazip.com/) or a range of multiple IP addresses. 

## Does the Spectrum Symphony offering support multiple key pairs to establish SSH to all the nodes?
{: #multiple-key-pair-support}
{: faq}

Yes, the {{site.data.keyword.symphony_short}} offering supports multiple single key pairs that can be provided for access to all of the nodes that are part of the cluster. In addition, {{site.data.keyword.symphony_short}} has a feature where each node of the cluster can be accessed through passwordless SSH.

## What storage types are available through this offering?
{: #available-storage-types}
{: faq}

In the {{site.data.keyword.symphony_short}} offering, you can use {{site.data.keyword.scale_short}} scratch storage or persistent storage. A scratch storage configuration uses virtual server instances with instance storage. A persistent storage configuration uses bare metal servers with locally attached NVMe storage.

## Which Linux operating system is supported for worker nodes?
{: #supported-operating-system-worker-nodes}
{: faq}

The solution supports custom images based on RHEL 8.10 for virtual server instance worker nodes, and it supports the use of the stock RHEL 8.10 VPC images for bare metal worker nodes. At this time, custom images are not supported for use with VPC bare metal servers.

## Can I use a custom resolver that is already associated with a VPC?
{: #custom-resolver}
{: faq}

Yes, the solution supports the use of a custom resolver that is already associated to a VPC. If a VPC already has a custom resolver, the automation uses of it and the DNS service and associates the new DNS domain that is created from the solution for hostname resolution.

## Can I associate a single VPC with multiple DNS zones that have the same name?
{: #DNS-zone-names}
{: faq}

No, adding the same permitted network (for example, VPC) to two DNS zones of the same name is not allowed as mentioned [here](/docs/dns-svcs?topic=dns-svcs-managing-permitted-networks&interface=ui#adding-permitted-networks-ui). Therefore, when you select values for `vpc_scale_storage_dns_domain` and `vpc_worker_dns_domain`, ensure that they are unique and that there are no DNS zones that use either of those names that are already associated with the VPC that you might have specified in `vpc_name`.

## What file storage for {{site.data.keyword.cloud_notm}} Virtual Private Cloud (VPC) profiles are supported for the {{site.data.keyword.symphony_full_notm}} cluster shared storage?
{: #file-storages-for-vpc-profiles}
{: faq}

{{site.data.keyword.filestorage_vpc_full_notm}} is a zonal file storage offering that provides NFS-based file storage services. You create file share mounts from a subnet in an availability zone within a region. You can also share them with multiple virtual server instances within the same zone within a vpc. {{site.data.keyword.symphony_full_notm}} supports the use of [dp2 profiles](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#dp2-profile).

## Can you specify the total IOPS (input or output operations per second) for a file share when deploying an {{site.data.keyword.symphony_full_notm}} cluster?
{: #iops-cluster}
{: faq}

Yes, when you deploy an {{site.data.keyword.symphony_full_notm}} cluster, you can [choose the required IOPS value appropriate for your file share size](/docs/vpc?topic=vpc-file-storage-profiles&interface=ui#fs-tiers).

## How to share data, packages, or applications with {{site.data.keyword.symphony_full_notm}} compute nodes?
{: #shares}
{: faq}

{{site.data.keyword.filestorage_vpc_full_notm}} with two file shares (`/mnt/vpcstorage/tools` or `/mnt/vpcstorage/data`), and up to five file shares, is provisioned to be accessible by both Spectrum Symphony management and compute nodes. To copy to a file share, SSH to the Spectrum Symphony management node and use your file copy of choice (such as scp, rsync, or IBM Aspera) to the appropriate file share.
