---

copyright:
  years: 2023
lastupdated: "2023-05-02"

keywords: 
content-type: tutorial
services: symphony, vpc 
account-plan: paid
completion-time: 1h
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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:table: .aria-labeledby="caption"}
{:step: data-tutorial-type='step'}

# Adding Spectrum multicluster (SMC) to Spectrum Symphony 
{: #smc-adding-to-symphony}
{: toc-content-type="tutorial"}
{: toc-completion-time="1h"}
{: toc-services="symphony, vpc"}

The Spectrum multicluster feature in {{site.data.keyword.symphony_full_notm}} Advanced Edition is used to connect multiple {{site.data.keyword.symphony_full_notm}} clusters into a federation cluster. Using this feature, you can:

*   Use workload placement to redirect {{site.data.keyword.symphony_full_notm}} sessions to nonbusy clusters. Redirecting sessions balances workloads so that tasks within one session can be dispatched to multiple clusters based on resource availability on each federation member cluster. 
*   Deploy and manage service packages from the multicluster management console across member clusters. 
*   Monitor workload and resources, as this feature collects data from {{site.data.keyword.symphony_full_notm}} clusters and sends it to the {{site.data.keyword.symphony_full_notm}} multicluster primary cluster for aggregation and display.
{: shortdesc}

SMC supports multiple Operating Systems, but the solution here uses RHEL8.6.

Adding SMC involves:

1.  Manually creating a {{site.data.keyword.bpshort}} workspace to define your Terraform environment.
2.  Manually entering and changing the SMC-related parameters.
3.  Generating a Terraform plan that verifies the parameter settings and shows the resources that the scripts create.
4.  Applying the generated plan to deploy the configured resources.

## Architecture diagram
{: #smc-arch-diagram}

![Architecture diagram](images/SMC-arch-diagram-Multi-Region.drawio.svg){: caption="Architecture diagram zones and regions" caption-side="bottom"}

## Before you begin
{: #smc-b4-begin}

Before you set up the SMC, you must:

Deploy 3 lone (symphony) clusters in different regions or zones. For more information about creating symphony clusters, see the [readme file](https://github.com/IBM-Cloud/hpc-cluster-symphony#readme).

When you provision lone symphony clusters, the cidr blocks and cluster_id must be unique for each Lone symphony cluster.
{: note}

SMC supports combination of single smc_zone as ["us-east-1"] or double smc_zone within the same or across regions as ["au-syd-3","eu-de-1"] or triple smc_zone within the same or across region as ["us-east-1","ca-tor-3","jp-tok-3"]
{: note}

## Creating a workspace for SMC manually
{: #smc-create-workspace-ui}
{: step}

1. Go to Schematics, the {{site.data.keyword.cloud_notm}} [deployment manager](https://cloud.ibm.com/schematics){: external}, select Workspaces, and then select Create workspace.
2. In the _Specify template_ section:
    * Provide the GitHub repository URL where your Terraform files are located. The SMC repository is provided in this public [GitHub repository](https://github.com/IBM-Cloud/hpc-cluster-symphony-smc){: external}.
    * Select a version of the Terraform engine that is greater than 1 to use in the {{site.data.keyword.bpshort}} workspace.
    * Click Next.
3. In the _Workspace details_ section:
    * Specify the name for your {{site.data.keyword.bpshort}} workspace.
    * Define any tags that you want to associate with the resources provisioned through the offering. The tags can later be used to query the resources in the {{site.data.keyword.Bluemix_notm}} console.
    * Select a resource group.
    * Select a location. The location determines where the workspace actions are run.
    * Provide a description (optional) of the {{site.data.keyword.bpshort}} workspace.
    * Click Next and then click Create. The {{site.data.keyword.bpshort}} workspace is created with the name that you specified. 

## Updating SMC parameters
{: #smc-update-parameters-ui}
{: step}

1. Go to the IBM Cloud Workspaces page and select the Workspace name that you created. The Schematics > Workspaces page opens on the Jobs tab that shows the job log.
2. Go to the Schematic Workspace Settings tab, and in the variable section, click "burger icons" to enter the required values and update default parameters.

    Parameters unique to your account and your deployment:
    
    | Name | Description | Type | Default |
    |------|-------------|-------|-------|
    | lone_vpc_name | The name of an existing Lone Symphony VPC, lone_vpc_name and lone_vpc_region must be in the same order. If no value is given, then you need to add existing_lone_vpc manually with SMC transit_gateway. Note: lone_vpc_name supports a maximum of 3 existing_lone_vpc_name. For more information, see [VPC](https://cloud.ibm.com/docs/vpc).| list(string) | null |
    |lone_vpc_region | The name of the IBM Cloud region where the existing Lone Symphony VPC, lone_vpc_name and lone_vpc_region must be in the same order (Examples: us-east, us-south, and so on). Note: lone_vpc_region support maximum of 3 existing_lone_vpc_region. For more information, see [Region and data center locations for resource deployment](https://cloud.ibm.com/docs/overview?topic=overview-locations).| list(string) | null |
    |ssh_key_name | Your {{site.data.keyword.Bluemix_notm}} SSH key name such as "smc-ssh-key" that is created in a specific region in {{site.data.keyword.Bluemix_notm}}. Mark it as sensitive to hide the SSH key in the {{site.data.keyword.Bluemix_notm}} console.|string | n/a |
    |api_key  | Your API key value. Mark it as sensitive to hide the API key in the {{site.data.keyword.Bluemix_notm}} console.| string | n/a |
    |sym_license_confirmation| Confirm your use of IBM Symphony Multi Cluster licenses. When you select 'true', you agreed to one of two conditions. 1. You are using the software in production and confirm you have sufficient licenses to cover your use under the International Program License Agreement (IPLA). 2. You are evaluating the software and agree to abide by the International License Agreement for Evaluation of Programs (ILAE). NOTE: Failure to comply with licenses for production use of software is a violation of the IBM International Program License Agreement. For more information, see[IBM International Program License Agreement](https://www.ibm.com/software/passportadvantage/programlicense.html).| string | n/a |
    |smc_zone |IBM Cloud zone name within the selected region where the Symphony Multi Cluster resources are deployed. SMC supports a combination of single smc_zone as ["us-east-1"] or double smc_zone as ["au-syd-3","eu-de-1"] or triple smc_zone as ["us-east-1","ca-tor-3","jp-tok-3"].| list(string) | n/a |
    |remote_allowed_ips |Comma-separated list of IP addresses that can access the Symphony Multi Cluster instance through an SSH interface. For security purposes, provide the public IP addresses assigned to the devices that are authorized to establish SSH connections (for example, "169.45.117.34"). To fetch the IP address of the device, use https://ipv4.icanhazip.com/. |list(string) | n/a |
    {: caption}

    Parameters that have default values that you might or might not need to update:

    | Name    | Description | Type  |  Default  |
    |----------|-------------|-----|--------|
    |cluster_prefix value| The specific cluster prefix for your Lone cluster.| string | "hpcc-smc" |
    |cluster_id| ID of the cluster used by Symphony Multi Cluster for configuration of resources. The ID must be up to 39 alphanumeric characters, including the underscore (\_), the hyphen (-), and the period (.). Other special characters and spaces are not allowed. Do not use the name of any host or user as the name of your cluster. You cannot change it after installation.|string | "HPCMultiCluster" |
    |bastion_host_instance_type| Specify the virtual server instance profile type name to be used to create the bastion node for the Symphony Multi Cluster. For more information, see [VPC Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles).| string  | bx2-2x8  |
    |cluster_prefix | Prefix that is used to name the Symphony Multi cluster and IBM Cloud resources that are provisioned to build the Symphony Multi cluster instance. You cannot create more than one instance of the Symphony Multi cluster with the same name. Make sure that the name is unique. Enter a prefix name, such as `my-hpcc`.|string | "hpcc-smc"|
    |dns_domain | IBM Cloud DNS Services domain name to be used for the Symphony Multi Cluster host.| string | "smc.ibmhpc.com" | 
    |login_cidr_block | IBM Cloud VPC address prefixes that are needed for VPC creation. For more information, see [Bring your own subnet](https://cloud.ibm.com/docs/vpc?topic=vpc-configuring-address-prefixes).|  list(string) | "10.10.4.0/28" | 
    |resource_group| Resource group name from your IBM Cloud account where the VPC resources are deployed. Resource group is populated when you create the workspace. For more information, see [Resource Groups](https://cloud.ibm.com/docs/account?topic=account-rgs).| string| "Default" |
    | primary_cidr_block | IBM Cloud VPC address prefixes that are needed for VPC creation. Provide a CIDR address prefix for Primary VPC creation. For more information, see [Bring your own subnet](https://cloud.ibm.com/docs/vpc?topic=vpc-configuring-address-prefixes).| string  | "10.10.0.0/24"  |
    | secondary_cidr_block| IBM Cloud VPC address prefixes that are needed for VPC creation. Provide a CIDR address prefix for Secondary VPC creation. For more information, see [Bring your own subnet](https://cloud.ibm.com/docs/vpc?topic=vpc-configuring-add|
    | secondary_candidate_cidr_block | IBM Cloud VPC address prefixes that are needed for VPC creation. Provide a CIDR address prefix for Secondary Candidate VPC creation. For more information, see [Bring your own subnet](https://cloud.ibm.com/docs/vpc?topic=vpc-configuring-address-prefixes). | string | "10.30.0.0/24"	|
    | smc_image_name | Name of the custom image that you want to use to create virtual server instances in your IBM Cloud account to deploy the IBM Symphony Multi Cluster. The default can or might not need to be changed, depending on your deployment. By default, the automation uses a base image with more software packages mentioned [here](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-getting-started-tutorial). To include your application-specific binary files, see [Planning for custom images](https://cloud.ibm.com/docs/vpc?topic=vpc-planning-custom-images) to create a custom image and use that image to build the IBM Symphony cluster through this offering. | string | "hpcc-symphony732-rhel86-smc-v1" |
    | smc_host_instance_type| Specify the virtual server instance profile type name to be used to create the Symphony Multi Cluster host. For more information, see [VPC profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles). | string | "bx2-4x16" |
    {: caption}

## Generate a plan
{: #smc-generate-plan-ui}
{: step}

After you create your {{site.data.keyword.bpshort}} workspace and updated the configuration parameters, you need to generate a plan to validate all the configuration properties.

1. Click Generate plan. 
   When you click Generate plan, a new log is generated that can be viewed in the Jobs tab by clicking Jobs. 
2. Review the log file for any errors, fix the properties, and regenerate the plan by clicking Generate plan again.

## Apply a plan
{: #smc-apply-plan-ui}
{: step}

When you apply a plan, the {{site.data.keyword.cloud}} SMC resources are deployed on your {{site.data.keyword.cloud_notm}} account with your specific choice of configuration properties. 

1. After you generate a plan in the {{site.data.keyword.cloud_notm}} console, click Apply plan. This action generates a new log that can be viewed in the Jobs tab.
2. Review the log file for any errors, fix the errors, and then click Apply plan again.
3. After you successfully apply a plan, you can review all the resources that are deployed under this workspace by clicking the _Resources_ tab. 

One of the last things shown in the log file after the Apply plan is an SSH key string:

    ```terraform
    2023/03/29 12:20:02 Terraform refresh | secondary_host_domain_name = "hpc-smc-secondary.smc.ibmhpc.com"
    2023/03/29 12:20:02 Terraform refresh | smc_web_console = "https://localhost:8443/platform"
    2023/03/29 12:20:02 Terraform refresh | ssh_command = "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8443:localhost:8443 -J ubuntu@169.63.102.28 root@10.10.0.5"
    2023/03/29 12:20:02 Command finished successfully.
    OK 
    ```

You use this string to configure SSH in the next step.

## Configure ssh
{: #smc-config-ssh-nodes}
{: step}

Copy the SSH command output from the Apply plan log to your laptop terminal. Use this ssh command to SSH to the primary node through the jump host public IP, and then to SSH into one of the nodes.

Use the public IP address of the jump host and modify the IP address of the target node to enable access to specific hosts through the jump host.

## Next steps
{: #next-smc-ui}

After you provision the SMC cluster, wait for 10 min for the host data to be copied (rsync) to the secondary system. The configuration uses the primary SMC host.

Continue with making a connection between Lone Symphony and SMC Cluster
