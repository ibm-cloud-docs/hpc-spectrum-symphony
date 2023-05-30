---

copyright:
  years: 2022, 2023

lastupdated: "2023-05-30"

keywords: IBM Spectrum Symphony release notes

subcollection: hpc-spectrum-symphony

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}
{:external: target="_blank" .external}
{:release-note: data-hd-content-type='release-note'}

# Release notes for IBM Spectrum Symphony
{: #release-notes}

Use these release notes to learn about the latest updates to {{site.data.keyword.symphony_full}} that are grouped by date.
{: shortdesc}

## May 2023
{: #hpc-spectrum-symphony-may23}

## 30 May 2023
{: #hpc-spectrum-symphony-may3023}
{: release-note}

Persistent storage type support when using {{site.data.keyword.scale_full_notm}} storage
:   You now have the option to use bare metal servers to deploy persistent storage for {{site.data.keyword.scale_short}} storage nodes.

Updated {{site.data.keyword.symphony_short}} and {{site.data.keyword.scale_short}} custom images
:   {{site.data.keyword.symphony_short}} and {{site.data.keyword.scale_short}} custom images were updated to use RHEL 8.6 instead of RHEL 8.4.

Custom image support
:   Documentation and scripts to make it easier for users to create and use their own custom images for {{site.data.keyword.symphony_short}} worker and {{site.data.keyword.scale_short}} storage nodes are now included in the solution.

{{site.data.keyword.symphony_short}} version update
:   {{site.data.keyword.symphony_short}} product version was updated from 7.3.1 to 7.3.2.

{{site.data.keyword.scale_short}} version update
:   {{site.data.keyword.scale_short}} product version was updated from 5.1.5.1 to 5.1.7.0.

Support automated public gateway creation when one is not associated with the subnets of an existing VPC
:   When you choose to use an existing VPC, the automation associates the public gateway with all of the subnets if it is already part of the VPC. If the public gateway is not part of the VPC, then it creates a new public gateway and associates it with all subnets to allow internet access from the cluster nodes.

DNS functionality support
:   Introduced use of DNS custom resolver for name resolution in place of static hostname to IP address mapping.

## February 2023
{: #hpc-spectrum-symphony-feb23}

### 16 February 2023
{: #hpc-spectrum-symphony-feb1623}
{: release-note}

Bug fix
:   Code fixes were applied to address an issue when creating new subnets with custom CIDR in an existing VPC range.

## January 2023
{: #hpc-spectrum-symphony-jan23}

### 27 January 2023
{: #hpc-spectrum-symphonyjan2723}
{: release-note}

Bare metal server support for worker nodes
:   You now have the option to use bare metal servers instead of virtual server instances for your cluster worker nodes.

Updated RHEL 8.4 custom images
:   New RHEL 8.4 custom images that are used for deploying the compute nodes and {{site.data.keyword.scale_short}} storage nodes replace images that are provided in the previous release. The new images include several miscellaneous fixes.

Allow CIDR block specification
:   You now have the option to either accept the default CIDR block or specify the CIDR block for the compute and storage cluster private subnet. This is helpful to avoid overlapping IP addresses, for example, if you are using a hybrid cloud environment.

Added valid {{site.data.keyword.symphony_short}} license check
:   The solution now checks for a valid license that is associated with `ibm_customer_number`, which has been added as a required variable.

## October 2022
{: #hpc-spectrum-symphony-oct22}

### 27 October 2022
{: #hpc-spectrum-symphony-oct2722}
{: release-note}

Windows&reg; support
:   You can now deploy Windows&reg; worker nodes in your {{site.data.keyword.symphony_full_notm}} cluster.

## September 2022
{: #hpc-spectrum-symphony-sept22}

### 23 September 2022
{: #hpc-spectrum-symphony-sept2322}
{: release-note}

Bug fix
:   Code fixes were applied to address an issue that was seen in regions where instance storage profiles are not available even though {{site.data.keyword.scale_short}} is not selected as the storage option.

Updated RHEL stock image
:   Made an update to use a RHEL 8.6 stock image instead of a RHEL 8.2 stock image for the login node and the NFS storage node.

## August 2022
{: #hpc-spectrum-symphony-aug22}

### 11 August 2022
{: #hpc-spectrum-symphony-aug1122}
{: release-note}

Bug fix
:   Code fixes were applied related to an Ansible version 2.10 upgrade.

## June 2022
{: #hpc-spectrum-symphony-jun22}

## 30 June 2022
{: #hpc-spectrum-symphony-jun3022}

Security update and enhancements
:   This release provides fixes for the Polkit Local Privilege Escalation Vulnerability (CVE-2021-4034) and for the Spring Framework Vulnerability (CVE-2022-22965) within the RHEL 8.4 based custom image accessible by the solution. This release also removes the earlier Polkit mitigation implemented through changes to the post-provisioning scripts. The RHEL 8.4 custom image replaces the RHEL 8.2 image that was provided in the previous release. In addition, changes in the code now allow creation of compute nodes in the cluster by Host Factory on VPC resource groups other than the default group. And, security group rules have been added to provide more secure access to the cluster by allowing SSH from only specific nodes.

## January 2022
{: #hpc-spectrum-symphony-jan22}

### 28 January 2022
{: #hpc-spectrum-symphony-jan2822}
{: release-note}

{{site.data.keyword.symphony_full_notm}} enhancements
:   You now have the option to use {{site.data.keyword.scale_short}} for shared storage that's accessible by the {{site.data.keyword.symphony_short}} compute nodes. For more information, see [Using {{site.data.keyword.scale_short}} storage](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-using-spectrum-scale-storage). With the latest changes, RHEL 8.2 custom images take the place of RHEL 7.7 and CentOS 7.7 custom images with included optimizations for NFS. Additionally, the security vulnerability for Polkit Local Privilege Escalation Vulnerability (CVE-2021-4034) has been mitigated with changes to the post-provisioning scripts.

### 13 January 2022
{: #hpc-spectrum-symphony-jan1322}
{: release-note}

Security update
:   A new custom image with an upgraded log4j version (2.17) to mitigate Log4Shell vulnerability (CVE-2021-44228) is included in this security update.

## December 2021
{: #hpc-spectrum-symphony-dec21}

### 23 December 2021
{: #hpc-spectrum-symphony-dec2321}
{: release-note}

Security update
:   A security update is included in this release that addresses the Log4Shell vulnerability (CVE-2021-44228).

### 14 December 2021
{: #hpc-spectrum-symphony-dec1421}
{: release-note}

Dedicated hosts and additional improvements
:   You now have the option to use dedicated hosts for static worker nodes in addition to having an enhanced `ssh_command` output and hyperthreading by default. For more information, see [Using dedicated hosts](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-using-dedicated-hosts). Further improvements include an update of the stock images that are used for login and storage nodes to RHEL 8.2 and clean up of entitlement and location input properties. Parallelism is now supported in {{site.data.keyword.bpshort}} `destroy` operations to help overall performance, and due to a bug fix, you can now avoid the Terraform warning regarding interpolation.

## October 2021
{: #hpc-spectrum-symphony-oct21}

### 18 October 2021
{: #hpc-spectrum-symphony-oct1821}
{: release-note}

Introducing {{site.data.keyword.symphony_full_notm}} for HPC workloads
:   You can now use {{site.data.keyword.symphony_full_notm}} as a workload manager with automated deployment of HPC clusters. The clusters are configured for auto scaling and use the cluster compute node configuration that you select.