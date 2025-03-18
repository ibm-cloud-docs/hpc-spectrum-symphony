---

copyright:
  years: 2021, 2025
lastupdated: "2025-03-18"

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

# Getting started with IBM Spectrum Symphony
{: #getting-started-tutorial}

{{site.data.keyword.symphony_full}} enables customers to deploy HPC clusters that use {{site.data.keyword.symphony_full_notm}} as a scheduling software. The deployment is performed by using Terraform and {{site.data.keyword.bplong_notm}} as automation frameworks. The following steps outline the high-level flow of events that are performed:

1. Create a workspace - with the Terraform code from {{site.data.keyword.bplong_notm}}. This step defines the set of configuration properties that are used to perform the automation.
2. Generate a plan - to confirm whether the configuration properties are valid, so that when you run the Terraform code, all the resources are provisioned correctly. If the validation fails, fix the configuration properties and try again.
3. Apply a plan - triggers the actual deployment of the {{site.data.keyword.cloud_notm}} resources to have an {{site.data.keyword.symphony_short}} cluster up and running by the time the deployment completes. If the deployment fails, identify the reason for failure, fix the problem, and try again. If a change is needed to the configuration properties, it might be better to generate a plan again.

If you decide to deploy your {{site.data.keyword.symphony_full_notm}} cluster through the {{site.data.keyword.cloud_notm}} catalog, when you click Install, the Generate Plan action is skipped, and the steps go from **Create Workspace** to **Apply Plan** directly. You need to enter values in the catalog that work for your permissions and {{site.data.keyword.cloud_notm}} account. If the deployment fails, the {{site.data.keyword.bpshort}} UI can be used to fix the errors, and you can retry the Apply Plan step.
{: note}

Before you can deploy your {{site.data.keyword.symphony_short}} cluster, you need to create or gather some information. To get started, complete the following steps:

## Generate API key
{: #generate-api-key}
{: step}

Generate an API key for your {{site.data.keyword.cloud_notm}} account where the {{site.data.keyword.symphony_short}} cluster will be deployed. For more information, see [Managing user API keys](/docs/account?topic=account-userapikey).

## Create SSH key
{: #create-ssh-key}
{: step}

Create an SHH key in your {{site.data.keyword.cloud_notm}} account. This is your SSH key that you use to access the Symphony cluster. For more information, see [Managing SSH keys](/docs/vpc?topic=vpc-managing-ssh-keys).

## Choose between IBM-managed or customer-managed encryption
{: #cust-encryption}
{: step}

By default, VPC volumes and file shares are encrypted with IBM-managed encryption. However, you can opt for customer-managed encryption per your security requirements. Customer-managed encryption uses your root key, which gives you complete control over your data. You can provision or import existing encrypted keys by using {{site.data.keyword.keymanagementservicefull_notm}}.

If you decide to use customer-managed encryption, complete the following steps before you deploy your {{site.data.keyword.symphony_short}} architecture:

1. [Provision an instance of Key Protect](/docs/key-protect?topic=key-protect-provision#provision-gui)
2. [Create or import key](/docs/key-protect?topic=key-protect-getting-started-tutorial#get-started-keys)
3. [Authorize access between](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-volumes-prereqs):
    * Cloud Block Storage and the key management service
    * Cloud Block Storage and the file share service
4. Gather information for the following boot volume encryption deployment values (you provide this information when you deploy your {{site.data.keyword.symphony_short}} architecture):
    * `enable_customer_managed_encryption`: Gives you toggling options.
    * `kms_instance_id`: Instance ID of the Key Protect instance that you create.
    * `kms_key_name`: Name of the KMS key that you create

Customer-managed encryption applies only to the login, management, and static compute nodes. The dynamic compute nodes are still IBM-managed.
{: note}

## Create custom images
{: #create-custom-image}
{: step}

The offering provides a default set of images that you can use for the nodes within your Spectrum Symphony cluster. However, if you prefer to use your own custom images, documentation and scripts are provided to help you create them.

Worker Image: The default image that is specified in `image_name` acts as the worker image. This image is used to create the Symphony cluster nodes: management, management-candidates, and workers.

If you prefer to create the worker custom image with your own settings and configurations, follow the instructions that are provided [here](https://github.com/IBM-Cloud/hpc-cluster-symphony/blob/master/custom_image/worker/README.md){: external}.

Storage Image: If you intend to use {{site.data.keyword.scale_short}} as shared storage for the cluster, the default image that is specified in `scale_storage_image_name` acts as the storage image. This image is used to create the {{site.data.keyword.scale_short}} storage cluster nodes. In addition to the base operating system, the image also includes the required {{site.data.keyword.scale_short}} software packages.

If you prefer to create the storage custom image with your own settings and configurations, follow the instructions that are provided [here](https://github.com/IBM-Cloud/hpc-cluster-symphony/blob/master/custom_image/storage/README.md){: external}.

## Gather Spectrum software entitlement information
{: #gather-spectrum-entitlement-information}
{: step}

The offering uses the Bring Your Own License (BYOL) model for Spectrum software when you deploy an Spectrum Symphony cluster on {{site.data.keyword.cloud_notm}}. This applies to {{site.data.keyword.symphony_full_notm}} in all cases and to IBM Spectrum Scale if you decide to use that for the HPC cluster storage instead of just NFS. Work with your business owners or license management team to make sure that your organization has procured enough licenses to deploy the HPC cluster using the Spectrum software. Failure to comply with licenses for production use of software is a violation of the [IBM International Program License Agreement](https://www.ibm.com/software/passportadvantage/programlicense.html){: external}.

The current solution no longer requires `ibm_customer_number`(ICN) for entitlement check before deploying the solution for non-production use. The solution is now available for use without ICN validation. Users can provision up to a maximum of 10 static worker nodes for evaluation or non-production use cases. If the number of worker nodes exceeds 10, it becomes the user responsibility to obtain the necessary entitlement check and licensing for those additional nodes in the production environment. For production use or for evaluating greater than 10 worker nodes, the user must purchase the necessary Symphony licenses. To purchase the license, go to [Purchasing licenses](https://www.ibm.com/docs/en/devops-test-embedded/9.0.0?topic=licenses-purchasing).
{: important}

## Next steps
{: #getting-started-next-steps}
{: step}

After you gather the necessary input values to define your cluster configuration, you are ready to deploy your {{site.data.keyword.symphony_full_notm}} cluster. The {{site.data.keyword.symphony_short}} cluster can be deployed on {{site.data.keyword.cloud_notm}} by using the {{site.data.keyword.cloud_notm}} catalog, {{site.data.keyword.bpshort}} CLI, or the {{site.data.keyword.bpshort}} APIs. If you want to deploy your cluster by using the CLI or API, review the prerequisites for your interface of choice:

* [Setting up the {{site.data.keyword.bplong_notm}} CLI](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-setting-up-cli)
* [Setting up the {{site.data.keyword.bplong_notm}} API](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-setting-up-api)

After you create and gather your information and review for any additional prerequisites for your interface of choice, you are ready to begin [Creating a workspace](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-creating-workspace).
