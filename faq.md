---

copyright:
  years: 2021
lastupdated: "2021-12-15"

keywords: 

subcollection: hpc-spectrum-symphony

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# FAQs
{: #spectrum-symphony-faqs}

## What Spectrum Symphony version is used in cluster nodes deployed with this offering?

{: #my-faq-packages}
{: faq}

Cluster nodes deployed with this offering include IBM Spectrum Symphony 7.3.1 Advanced Edition. 

See the following for a summary of the features associated with each edition: [IBM Spectrum Symphony editions](/docs/en/spectrum-symphony/7.3.1?topic=foundations-spectrum-symphony-editions).

## What locations are available for deploying VPC resources?
{: #locations-vpc-resources}
{: faq}

Available regions and zones for deploying VPC resources, and a mapping of those to city locations and data centers can be found in [Locations for resource deployment](/docs/overview?topic=overview-locations){: external}.

## What permissions do I need in order to create a cluster using the offering?
{: #permissions-cluster-offering}
{: faq}

Instructions for setting the appropriate permissions for IBM Cloud services that are used by the offering to create a cluster can be found in [Granting user permissions for VPC resources, Managing user access for Schematics, Assigning access to Secrets Manager](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources&locale=en).


## How do I SSH among nodes?
{: #ssh-among-nodes}
{: faq}

All of the nodes in the HPC cluster have the same public key that you register at your cluster creation. You can use ssh-agent forwarding, which is a common technique to access remote nodes that have the same public key. It automates to securely forward private keys to remote nodes. Forwarded keys are deleted immediately after a session is closed.

To securely forward private keys to remote nodes, you need to do `ssh-add` and `ssh -A`.

```
[your local PC]~$ ssh-add {id_rsa for symphony cluster}
[your local PC]~# ssh -A -J root@jumpbox_fip root@master_private_ip
...
[root@master]~# ssh -A worker_private_ip
```
{: codeblock}

For Mac OS X, you can persist `ssh-add` by adding the following configuration to `.ssh/config`:

```
Host *
  UseKeychain yes
  AddKeysToAgent yes
```
{: codeblock}

You can even remove `-A` by adding "ForwardAgent yes" to `.ssh/config`.

## How many worker nodes can I deploy in my Spectrum Symphony cluster through this offering?
{: #worker-nodes}
{: faq}

Prior to deploying a cluster, it is important to ensure that the VPC resource quota settings are appropriate for the size of the cluster that you would like to create (see [Quotas and service limits](/docs/vpc?topic=vpc-quotas)).

The maximum number of worker nodes that are supported for the deployment value worker_node_max_count is 500 (see [Deployment values](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-deployment-values))). The `worker_node_min_count` variable specifies the number of worker nodes that are provisioned at the time that the cluster is created, which will exist throughout the life of the cluster. The delta between those two variables specifies the maximum number of worker nodes that can either be created or destroyed by the Symphony Host Factory auto-scaling feature.

When creating or deleting a cluster with a large number of worker nodes, you may encounter VPC resource provisioning or deletion failures. In those cases, running the Schematics apply or destroy operation again may result in the remaining resources being successfully provisioned or deleted. If you continue to see errors, see [Getting help and support](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-getting-help-and-support).

## Why are there two different resource group parameters that can be specified in the IBM Cloud catalog tile?
{: #resource-group-parameters}
{: faq}

The first resource group parameter entry in the **Configure your workspace** section in the IBM Cloud catalog applies to the resource group where the Schematics workspace is provisioned on your IBM Cloud account. The value for this parameter can be different than the one used for the second entry in the **Parameters with default values** section in the catalog. The second entry applies to the resource group where VPC resources are provisioned. As specified in the description for this second `resource_group` parameter, note that only the default resource group is supported for use of the Symphony Host Factory auto-scaling feature.