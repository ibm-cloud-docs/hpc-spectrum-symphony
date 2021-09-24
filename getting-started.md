---

copyright:
  years: 2021
lastupdated: "2021-09-24"

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
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Getting started with IBM Spectrum Symphony
{: #getting-started-tutorial}

IBM® Spectrum Symphony enables customers to deploy HPC clusters that use IBM Spectrum Symphony as a scheduling software. The deployment is performed by using Terraform and {{site.data.keyword.bplong_notm}} as automation frameworks. The following steps outline the high-level flow of events that are performed:

1. **Create a workspace** with the Terraform code from {{site.data.keyword.bplong_notm}}. This step defines the set of configuration properties that are used to perform the automation.
2. **Generate a plan** to confirm whether the configuration properties are valid, so that when you run the Terraform code, all of the resources are provisioned correctly. If the validation fails, fix the configuration properties and try again.
3. **Apply a plan** triggers the actual deployment of the {{site.data.keyword.cloud_notm}} resources to have an HPC cluster up and running by the time the deployment completes. If the deployment fails, identify the reason for failure, fix the problem, and try again. If a change is needed to the configuration properties, it might be better to generate a plan again.

If you decide to deploy your IBM Spectrum Symphony cluster through the {{site.data.keyword.cloud_notm}} catalog, when you click **Install**, the **Generate Plan** action is skipped, and the steps go from **Create Workspace** to **Apply Plan** directly. You need to enter values in the catalog that work for your permissions and {{site.data.keyword.cloud_notm}} account. If the deployment fails, the {{site.data.keyword.bpshort}} UI can be used to fix the errors, and you can retry the **Apply Plan** step.
{: note}

Before you can deploy your Spectrum Symphony cluster, you need to create or gather some information. To get started, complete the following steps.

## Generate API key
{: #generate-api-key}
{: step}

Generate an API key for your {{site.data.keyword.cloud_notm}} account where the Spectrum Symphony cluster will be deployed. For more information, see [Managing user API keys](/docs/account?topic=account-userapikey).

## Create SSH key
{: #create-ssh-key}
{: step}

Create an SHH key in your {{site.data.keyword.cloud_notm}} account. This is your SSH key that you will use to access the Symphony cluster. For more information, see [Managing SSH keys](/docs/vpc?topic=vpc-managing-ssh-keys).

## Create custom image
{: #create-custom-image}
{: step}

Create a custom image with your OS/Symphony and required application binary files. For more information, see [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images). {{site.data.keyword.cloud_notm}} provides a pre-built image with CentOS and RHEL to help you get started quickly. The image names are `hpcc-sym731-cent77-aug3121-v3` and `hpcc-sym731-rhel77-aug3121-v3`, respectively. In addition to the base operating system software, the images also include the nfs-utils software package. This package is required in order to mount the shared NFS storage used by the cluster nodes.

## Gather Symphony entitlement information
{: #gather-symphony-entitlement-information}
{: step}

The offering uses BYOL (Bring your own licenses) for Spectrum Symphony when you deploy an HPC cluster on {{site.data.keyword.cloud_notm}}. For production clusters, work with your business owners or license management team to make sure that your organization has procured enough licenses to deploy the HPC cluster by using IBM Spectrum Symphony. Failure to comply with licenses for production use of software is a violation of the [IBM International Program License Agreement](https://www.ibm.com/software/passportadvantage/programlicense.html){: external}.

## Next steps
{: #getting-started-next-steps}
{: step}

After you've gathered the necessary input values to define your cluster configuration, you are ready to deploy your IBM Spectrum Symphony cluster. The Spectrum Symphony cluster can be deployed on {{site.data.keyword.cloud_notm}} by using the {{site.data.keyword.cloud_notm}} catalog, {{site.data.keyword.bpshort}} CLI, or the {{site.data.keyword.bpshort}} APIs. If you want to deploy your cluster by using the CLI or API, review the prerequisites for your interface of choice:

* [Setting up the {{site.data.keyword.bplong_notm}} CLI](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-setting-up-cli)
* [Setting up the {{site.data.keyword.bplong_notm}} API](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-setting-up-api)

After you've created and gathered your information and reviewed any additional prerequisites for your interface of choice, you are ready to begin [Creating a workspace](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-creating-workspace).