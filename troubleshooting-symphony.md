---

copyright:
  years: 2021, 2023
lastupdated: "2023-04-26"

keywords: 

subcollection: hpc-spectrum-symphony

---

{:support: data-reuse='support'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# IBM Spectrum Symphony Troubleshooting
{: #troubleshooting-spectrum-symphony}

## Why is IBM Cloud Schematics not able to clone the private GitHub repo?
{: #troubleshoot-topic-1}
{: troubleshoot}
{: support}

{{site.data.keyword.bpshort}} isn't able to clone the private GitHub repository, and you are seeing the following error message: `Failed to clone git repository, repository not found (check url, also check the scope 'repo' of the personal access token if SCHEMATICSGITTOKEN is used)`
{: tsSymptoms}

You didn't provide the correct GitHub token, or you didn't provide a GitHub token altogether.
{: tsCauses}

Provide a [GitHub token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token){: external} and check to see whether the correct GitHub token has been provided in the `github_token` parameter in the created workspace API.
{: tsResolve}

## Why is IBM Cloud Schematics not able to clone the public GitHub repo?
{: #troubleshoot-topic-2}
{: troubleshoot}
{: support}

{{site.data.keyword.bpshort}} isn't able to clone the public GitHub repository, and you are seeing one of the following error messages:

* `Fatal, could not download repo, Failed to clone git repository, authentication required (or the git url is incorrect). Problems found with the Repository. Please Rectify and Retry`
* `Template error: Failed to clone git repository, authentication required (or the git url is incorrect)`
{: tsSymptoms}

You didn't provide the correct GitHub URL, or you provided a GitHub token, which is not required to clone a public repo. A GitHub access token is only required to access a private repo.
{: tsCauses}

Do not provide a GitHub token, and check to see whether the GitHub token was provided in the `github_token` parameter while creating a workspace by using the public repo.
{: tsResolve}

## Why is IBM Cloud Schematics not able to create a workspace?
{: #troubleshoot-topic-3}
{: troubleshoot}
{: support}

{{site.data.keyword.bpshort}} is not able to create a workspace, and you are seeing the following error message: `You don't have the required to create a workspace in any resource groups. You must be assigned the manager role on the {{site.data.keyword.bpshort}} service in at least one resource group. Contact your account administrator for access.`
{: tsSymptoms}

You don't have the required access to create a workspace in any resource groups. You must be assigned the manager role on the {{site.data.keyword.bpshort}} service in at least one resource group.
{: tsCauses}

Contact your account administrator and get assigned with the manager role on the {{site.data.keyword.bpshort}} service in at least one resource group.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with an error message for the `symphony_license_confirmation` variable?
{: #troubleshoot-topic-4}
{: troubleshoot}
{: support}

{{site.data.keyword.bpshort}} isn't able to provision the cluster, and you are seeing the following error message: `Error: Invalid value for variable "symphony_license_confirmation"`
{: tsSymptoms}

You entered a value other than "true" for the property `symphony_license_confirmation`.
{: tsCauses}

The property `symphony_license_confirmation` only accepts "true" as a valid value. A "true" value indicates that you have agreed to one of the following two conditions:

1. If you are deploying a production cluster, you have confirmed with your business team that you have enough licenses to deploy the {{site.data.keyword.spectrum_full_notm}} on {{site.data.keyword.cloud_notm}} and that these licenses are covered for use under the International Program License Agreement (IPLA).
2. You are deploying an evaluation cluster with {{site.data.keyword.spectrum_full_notm}} on {{site.data.keyword.cloud_notm}} and agree to abide by the International License Agreement for Evaluation of Program (ILAE).

IBM terms of software use for both IPLA and ILAE can be found [here](https://www-03.ibm.com/software/sla/sladb.nsf/sla/bla){: external}.

After you agree to one of the two conditions, update the property value to "true" and try again.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with an authorization error?
{: #troubleshoot-topic-5}
{: troubleshoot}
{: support}

{{site.data.keyword.bpshort}} isn't able to provision the cluster, and you are seeing the following error message: `Request is not authorized. Check your user permissions and authorizations and try again.`
{: tsSymptoms}

You don't have the required access to get any VPC resources provisioned. 
{: tsCauses}

Contact your account administrator and get all of the required accesses. For more information, see [Required permissions](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls).
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with an error that the provided name is not unique? 
{: #troubleshoot-topic-6}
{: troubleshoot}
{: support}

{{site.data.keyword.bpshort}} isn't able to provision the cluster, and you are seeing the following example error message:
{: tsSymptoms}

```text
"code": "validation_unique_failed",
"message": "Provided Name (sample-symphony-vpc) is not unique",
"target": {
"name": "name",
"type": "field",
"value": "sample-symphony-vpc"
}
```
{: screen}

VPC resource names must be unique. If a resource exists with the same name, you might get a similar error.
{: tsCauses}

Deprovision the existing resource and try again.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster while using a custom image?
{: #troubleshoot-topic-7}
{: troubleshoot}
{: support}

While using a custom image, {{site.data.keyword.bpshort}} isn't able to provision the cluster, and you are seeing one of the following error messages:

* `The argument "image" is required, but no definition was found.`
* `Unknown variable. There is no variable named "image_id".`
{: tsSymptoms}

The custom image that is used for one of the virtual server instances isn't present in the target region and zone or it is not accessible by the account and API key that is used to provision the cluster.
{: tsCauses}

If you are using a custom image for any of your virtual server instances, ensure that the custom image is available in the target region and zone and is accessible by the account and API key that is used to provision the cluster.
{: tsResolve}

## Why am I receiving an error for my refresh token?
{: #troubleshoot-topic-8}
{: troubleshoot}
{: support}

You are receiving a refresh token error in the **generate a plan**, **apply a plan**, and **destroy resources** requests: `Error: The provided Refresh Token is invalid. Please provide a proper refresh token for Terraform to run the configuration. Code: 400`
{: tsSymptoms}

You didn't provide the correct refresh token, or you didn't provide a refresh token altogether.
{: tsCauses}

Check to see whether the refresh token that was generated by using the curl command is correct; otherwise, regenerate the refresh token.
{: tsResolve}

## Why am I receiving an error when I apply a change to my workspace?
{: #troubleshoot-topic-9}
{: troubleshoot}
{: support}

You are receiving the following error when you try to apply a change to your workspace: `Apply failed due to "Error: Error Deleting Volume : The volume is still attached to an instance."`
{: tsSymptoms}

After reconfiguring the volume profile, capacity, or IOPS, your workspace needs to be cleaned up before applying the change. 
{: tsCauses}

You need to destroy your existing resources and try applying the change again. Your data on the storage node will be deleted if you destroy your existing resources.
{: tsResolve}

## Why am I receiving an error with the provided ssh_key_name value?
{: #troubleshoot-topic-10}
{: troubleshoot}
{: support}

You are receiving the following error when you try to generate or apply a plan on {{site.data.keyword.bpshort}} workspace: `failed due to "Error: No SSH Key found with name <KEY_NAME>".`
{: tsSymptoms}

Terraform could not find the given SSH key names that are provided by you.
{: tsCauses}

1. Check whether the given SSH key is present in the current region where the cluster is being provisioned. If the given SSH key is not present, create the SSH key in the current region.
2. While configuring multiple SSH keys, ensure that there is no white space added before or after the SSH key names.
3. If you are using multiple SSH keys, check whether a comma (,) is used as a delimiter between the SSH keys and that there is no white space added before or after the SSH key.
{: tsResolve}

## Why am I getting an error when I try to run the Spectrum Symphony VaR simulation?
{: #troubleshoot-topic-11}
{: troubleshoot}
{: support}

You are getting a `Failed to Login` error when you try to run the [{{site.data.keyword.symphony_short}} Value at Risk (VaR) simulation](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=started-sample-value-risk-var-simulation){: external}.
{: tsSymptoms}

It is possible that you are hitting a limitation with the VaR simulation, which requires that the cluster prefix be no longer than 10 characters and that the Symphony primary host hostname is fewer than 20 characters.
{: tsCauses}

Ensure that your cluster prefix is no longer than 10 characters.
{: tsResolve}

## Worker nodes are released when workload is in progress
{: #troubleshoot-topic-12}
{: troubleshoot}
{: support}

The `symA` requester might release a compute node virtual machine while the workload is still in progress. This happens when the property `return_idle_only` is set to true and the immediate return policy `symA` is unable to get the allocation for this host, and therefore assumes that it has no allocation. This issue happens if there are only a few tasks remaining for the monitored applications. For more information, see [Updating idle time before worker nodes are removed](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-update-idle-time&interface=api).


## Incorrect provider configuration values do not result in an error
{: #troubleshoot-topic-13}
{: troubleshoot}
{: support}

When updating the {{site.data.keyword.Bluemix_notm}} provider configuration from the Symphony GUI, at **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) Resources > Cloud > Configuration**, the values set in the configuration are not validated. If there are invalid values in the configuration, the virtual machine provisioning fails. If a failure happens, check the host factory logs on the host running the HostFactory service in `/opt/ibm/spectrumcomputing/hostfactory/log for more information`.

## Limitation of available profiles for dedicated hosts
{: #troubleshoot-topic-14}
{: troubleshoot}
{: support}

The offering automatically selects instance profiles for dedicated hosts to be the same prefix (for example, bx2 and cx2) as ones for worker instances (`worker_node_instance_type`). However, available instance prefixes can be limited, depending on your target region. If you use dedicated hosts, check `ibmcloud target -r {region_name}` and `ibmcloud is dedicated-host-profiles` to see whether your `worker_node_instance_type` has the available prefix for your target region.

## Why do I see resource errors due to authentication or timeout issues?
{: #troubleshoot-topic-15}
{: troubleshoot}
{: support}

You are receiving the following error messages during the creation of any specific resource:

* `Error: An error occurred while performing the ‘authenticate’ step: Post “https://iam.cloud.ibm.com/identity/token”: context deadline exceeded (Client.Timeout exceeded while awaiting headers)`
* `Error: timeout while waiting for state to become 'done, ' (last state: 'provisioning', timeout: 10m0s)`
{: tsSymptoms}

While {{site.data.keyword.bpshort}} deploys the infrastructure resources, it authenticates with {{site.data.keyword.cloud_notm}} through API calls. If there are too many requests through the API to the cloud environment, {{site.data.keyword.bpshort}} will not be able to authenticate and might error out with the authentication error.
{: tsCauses}

To fix either issue (resource failing due to authentication error or the timeout error), destroy the resources from the {{site.data.keyword.bpshort}} workspace and retry deploying the resources. 
{: tsResolve}

## Why does cluster creation fail with SSH issues?
{: #troubleshoot-topic-16}
{: troubleshoot}
{: support}

You are receiving the following error messages when the Ansible provisioner tries to set up the {{site.data.keyword.scale_short}} function on the worker and storage node resources:

* `msg": "Failed to connect to the host via ssh, Connection closed by UNKNOWN port 65535", "unreachable": true}`
* `Error: Failed to connect to the host via ssh: Connection timed out during banner exchange", "unreachable`
{: tsSymptoms}

While {{site.data.keyword.bpshort}} deploys the infrastructure resources, the automation code is configured with a few Ansible playbooks, which are required to set up the {{site.data.keyword.scale_short}} function on the virtual server instance nodes with the help of the Ansible provisioner. When the Ansible provisioner tries to SSH to these nodes to se the {{site.data.keyword.scale_short}} feature, the nodes go to an `unreachable` state. 
{: tsCauses}

To fix the issue, you can:
1. Try to destroy the resources from the workspace and deploy again.
2. If this issue is observed on all of the deployments, raise a support issue with the {{site.data.keyword.cloud_notm}} support team to investigate if there is an infrastructure issue. 
3. If there are no issues with the infrastructure, report this issue to the automation team who can investigate further.
{: tsResolve}

## Why am I receiving an error that the image is not found?
{: #troubleshoot-topic-17}
{: troubleshoot}
{: support}

You are receiving the following error when you try to either generate or apply a plan to your workspace: `Apply failed due to "Error: [ERROR] No image found with name hpcc-symp731-scale5151-rhel84-v1-4".`
{: tsSymptoms}

Either during generating or applying a plan, Terraform tries to validate if the provided image name and its image ID is present in the `image_map.tf` file. If Terraform finds the correct image details, it provisions the instances, but if the correct image details can't be found, Terraform tries to fetch the image details from {{site.data.keyword.cloud_notm}} through `data_source`.

Even if the provided image is not present in the cloud from that specific region, you still might receive the error.
{: tsCauses}

You need to check whether the provided image name has any spaces and if that image is present in the region where you want to do the deployment.
{: tsResolve}

## Why am I receiving a `cannot_start_capacity` error?
{: #troubleshoot-topic-18}
{: troubleshoot}
{: support}

You are receiving the following error when you try to apply a plan to your workspace: `Apply failed due to "code : cannot_start_capacity : message : Can't start instance because resource capacity is unavailable.`
{: tsSymptoms}

During the apply plan process, Terraform initiates the virtual server instance provisioning or bare metal server process based on the selected deployment values. If there is a resource capacity issue or a quota issue from the region where you are trying to deploy, the resources will not provision as expected.
{: tsCauses}

You need to talk to the administrator of the account to increase the quota for the specific region, or you can try to clean up all of the unwanted resources that are associated with the cloud infrastructure. If you clean up unwanted resources, you might free up space for the deployment to process.
{: tsResolve}

## Why does the cluster fail with an IBM Customer Number error?
{: #troubleshoot-topic-19}
{: troubleshoot}
{: support}

You are receiving the following error when you try to apply a plan to your workspace: `Apply failed due to "ERROR - [CLOUD-DEPLOY] Provided IBM Customer Number is not entitled to use Spectrum Symphony on Cloud. Kindly contact IBM Support Team. Exiting!`
{: tsSymptoms}

During the apply plan process, the bootstrap node initiates provisioning the resources for storage and compute cluster creation. During the process, RPM- and GPFS-related packages and Symphony packages need to be decrypted through the BYOL concept. If the IBM Customer Number is valid, the deployment begins. If not, the automation errors out the deployment.
{: tsCauses}

You need to provide a valid IBM Customer Number that is entitled to {{site.data.keyword.symphony_short}} without any spaces in the number. If the value that you provided is valid and you still received this error, contact IBM support to clarify about the entitlement.
{: tsResolve}

## Why does my instance provisioning remain in `Starting` state?
{: #troubleshoot-topic-20}
{: troubleshoot}
{: support}

After you apply a plan, the workspace takes a long time to provision the virtual server instance, and you notice in the user interface that the virtual server instance remains in `Starting` state.
{: tsSymptoms}

During the apply plan process, Terraform initiates the provisioning process of the virtual server instance in the cloud infrastructure. If there is a capacity issue or any issue from the infrastructure side for that particular region and zone, you might see this issue.
{: tsCauses}

You can try to manually create an instance in the user interface with the same image that is used during the automation process to see whether you get the same issue, or you can try a different zone for the deployment. You can also raise a support request for this issue to see whether it's originating from the infrastructure side.
{: tsResolve}

## Why am I getting errors when I try to delete resources after a bare metal server fails to provision?
{: #troubleshoot-topic-21}
{: troubleshoot}
{: support}

After a bare metal server fails to provision and you try to delete resources from the bootstrap node, you are receiving the following error after you apply the `mmcloudworkflows cluster destroy` command: `[ERROR] Error deleting security group target binding while deleting security group : The specified network interface is not attached to any other security groups.`

If cluster provisioning fails, it's recommended to clean up all of the resources from the failed provision before you attempt to reprovision a cluster.
{: tsSymptoms}

During the destroy process, the bootstrap node tries to clean up all of the resources that were created during the provisioning phase. It's possible that if a bare metal server provisioning took longer than expected and then failed, then during a subsequent cleanup, the destroy process complains that the failed bare metal server is still attached to a security group that it is attempting to destroy.
{: tsCauses}

Copy the cluster prefix name and complete the following steps:
{: tsResolve}

1. Go to the security group and access the -storage-sg.
2. Go to the attached resources section for the security group.
3. Click the attached bare metal server and copy the ID of the server.
4. Run the following commands from the CLI to stop and delete the server:

    ```sh
    ibmcloud is bare-metal-server-stop $bare_metal_server_id
    ```
    {: pre}

    ```sh
    ibmcloud is bare-metal-server-delete $bare_metal_server_id
    ```
    {: pre}

Deletion of the bare metal server takes a few seconds. After the bare metal node is deleted, go to {{site.data.keyword.bpshort}} and apply for destroy resources.

## What is the `subnet_not_in_address_prefix` error or `invalid CIDR format` error during VPC and subnet creation?
{: #troubleshoot-topic-22}
{: troubleshoot}
{: support}

You are receiving the following error when you try to apply a plan to your workspace: `Apply failed due to Error: [ERROR] Error while creating subnet. The specified CIDR does not fit in any of the address prefixes in the specified VPC. Make sure the subnet's CIDR is a subset of the CIDR of one of the address prefixes.`
{: tsSymptoms}

During the apply plan process, the workspace tries to create the VPC and subnet with the specified range of the CIDR address prefix from the deployment value. If the address prefix range is out of scope or doesn't belong to the family of the IP address range of the VPC, then you get an error that the address is not in range.
{: tsCauses}

Validate if the address prefix range that's provided for subnet creation is from the same range of addresses that are used for the VPC. For example, if the VPC address prefix is 10.241.0.0/18, then the subnet should be in the 10.241.x.x range. If a different IP address range is used, then you need to divide the subnets and choose the IP address range that's required for subnet creation.
{: tsResolve}

## Why does the deployment fail with a "process exited with status 2" error message?
{: #troubleshoot-topic-23}
{: troubleshoot}
{: support}

You are receiving the following error when you try to apply a plan to your workspace: `Apply failed due to Error: Error: remote-exec provisioner error and error executing "/tmp/terraform_1756078506.sh": Process exited with status 2.`
{: tsSymptoms}

The solution is implemented with a logic to evaluate whether the provided license number is valid. This feature is designed on top of the custom image, so every time the deployment is done it evaluates and decrypts the required package for the product deployment to complete. If an image is used that doesn't have this functionality, the automation code errors out.
{: tsCauses}

Use the appropriate custom image that is provided by the solution team, which has the function of evaluating the license and decrypting the packages.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with an `Enabling the Custom resolver` error?
{: #troubleshoot-topic-24}
{: troubleshoot}
{: support}

While {{site.data.keyword.bpshort}} tries to create the VPC resources, it attempts to create a custom resolver but fails with the following error: `[ERROR] Error Enabling the Custom resolver : MaxTimeout`
{: tsSymptoms}

Terraform attempts to create the custom resolver environment and waits for the custom resolver state to reach an active state. During this process, if the custom resolver takes more time than expected, Terraform throws the error message.
{: tsCauses}

After a failed deployment, clean up all resources. During a subsequent attempt, use a new cluster prefix to avoid any name collisions with resources from the previous failed attempt. If the issue continues to occur, open an issue with {{site.data.keyword.cloud_notm}} Support.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with a passwordless SSH error?
{: #troubleshoot-topic-25}
{: troubleshoot}
{: support}

After {{site.data.keyword.bpshort}} creates all the resources when `spectrum_scale_enabled` is set as true, the solution triggers the Ansible code to configure the entire Scale configuration on storage bare metal servers. During the Ansible configuration, the following error occurs: `[ERROR] Check passwordless SSH on all scale inventory hosts (1 retries left)`
{: tsSymptoms}

After all the infrastructure-related resources are up and running, the Ansible code tries to perform the Scale configuration through a passwordless SSH method. During this process, on the storage bare metal server, if the SSH service is not in a running state, then Ansible can't SSH to that specific bare metal storage node and it fails with the error.
{: tsCauses}

After a failed deployment, clean up all resources. During a subsequent attempt, use a new cluster prefix to avoid any name collisions with resources from the previous failed attempt. If the issue continues to occur, open an issue with {{site.data.keyword.cloud_notm}} Support.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with an `nmmcrcluster` error?
{: #troubleshoot-topic-26}
{: troubleshoot}
{: support}

After {{site.data.keyword.bpshort}} creates all the resources when `spectrum_scale_enabled` is set as true, the solution triggers the Ansible code to configure the entire Scale configuration on storage bare metal servers. During the Ansible configuration, the following error occurs: `[ERROR] nmmcrcluster: Error found while checking node descriptor`
{: tsSymptoms}

After all the infrastructure-related resources are up and running, the Ansible code tries to perform the Scale configuration through a passwordless SSH method. During this process, on the storage bare metal server, if one of the bare metal configurations is not cleaned up properly from the previous `destroy` command, then the error could be seen.
{: tsCauses}

After a failed deployment, clean up all resources. During a subsequent attempt, use a new cluster prefix to avoid any name collisions with resources from the previous failed attempt. If the issue continues to occur, open an issue with {{site.data.keyword.cloud_notm}} Support.
{: tsResolve}
