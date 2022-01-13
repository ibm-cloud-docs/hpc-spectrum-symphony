---

copyright:
  years: 2021, 2022
lastupdated: "2022-01-13"

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

# About IBM Spectrum Symphony
{: #about-spectrum-symphony}

IBM Spectrum Symphony allows you to deploy high-performance computing (HPC) clusters by using IBM Spectrum Symphony as HPC scheduling software. This offering uses open source Terraform-based automation to provision and configure {{site.data.keyword.cloud_notm}} resources. With simple steps to define configuration properties and use automated deployment, you can build your own HPC clusters in minutes. IBM Spectrum Symphony also enables configuration for auto-scaling, so IBM Spectrum Symphony clusters can automatically add and remove worker nodes based on workload specifications. This allows you to take full advantage of consumption-based pricing and pay for cloud resources only when they are needed. 
{: shortdesc}

IBM Spectrum Symphony offers the option of a public virtual machine, or virtual machines that are deployed on dedicated hosts, for static compute nodes only. The management nodes and dynamic compute nodes leverage public virtual machines only. The dedicated host option allows customers to have machines assigned just for their workloads and avoids issues like a noisy neighbor. The deployment properties allow customers to between 'pack' and 'spread' to either pack a dedicated host to full capacity before spilling to another instance or spread the VSIs evenly across all dedicated hosts. 

The offering supports the bring-your-own-license (BYOL) model for IBM Spectrum Symphony to deploy an HPC cluster on {{site.data.keyword.cloud_notm}}. Make sure that you have sufficient software licenses to deploy the required capacity on the {{site.data.keyword.cloud_notm}} cluster. For evaluation purposes, {{site.data.keyword.cloud_notm}} does enable limited access. Contact your {{site.data.keyword.cloud_notm}} sales or support team for evaluation licenses.

IBM Spectrum Symphony enables all three interfaces: UI, API and CLI. To use the API and CLI interfaces, the Terraform-based automation code is available in this [public git repository](https://github.com/IBM-Cloud/hpc-cluster-symphony){: external}.

The offering enables the initial Spectrum Symphony-based HPC cluster creation. Any updates that are needed post-deployment regarding Symphony configuration or setup should be performed by using Symphony tools and commands. If you use the {{site.data.keyword.bpshort}} interface to make changes to configuration properties and reapply those changes, you can cause disruptions to the running IBM Spectrum Symphony cluster. Restoring it back to a working state might not be easy.
{: important}

## Architecture diagram
{: #architecture-diagram}

![Architecture diagram](images/hpccluster-sym-schematics-architecture.png){:caption="Figure 1. Architecture diagram of an IBM Spectrum Symphony cluster on {{site.data.keyword.cloud_notm}}" caption-side="bottom"}

