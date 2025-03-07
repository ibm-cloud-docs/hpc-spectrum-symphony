---

copyright:
  years: 2022, 2025

lastupdated: "2025-03-07"

keywords: IBM Spectrum Symphony release notes

subcollection: hpc-spectrum-symphony

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}
{:external: target="_blank" .external}
{:release-note: data-hd-content-type='release-note'}

# Release notes
{: #release-notes}

The following new features and changes to {{site.data.keyword.symphony_full}} are available.
{: shortdesc}

## March 2025
{: #hpc-spectrum-symphony-mar25}

### 31 March 2025
{: #hpc-spectrum-symphony-mar3125}
{: release-note}

IBM Cloud VPC Flow Logs
:   IBM Cloud VPC Flow Logs enable the collection, storage, and presentation of information about the Internet Protocol (IP) traffic that is going to and from the network interfaces within your Virtual Private Cloud (VPC).

Key Management Service (KMS)
:   The KMS in IBM Cloud Private helps to keep the data secure. It integrates with user-owned Hardware Security Modules (HSM). A `root` key is used for envelope encryption to secure the data encryption keys used inside your applications.

Support for Symphony 7.3.2 fix patches
:   The {{site.data.keyword.symphony_full_notm}} version 7.3.2 offering is available for 64-bit Linux x86 and Windows platforms. The patches installed are Build602225, Build602237, Build602244, Build602259, Build602274, Build602300, Build602294, Build602288 and so on.

Updated the Spectrum Scale version from 5.2.1.1 to 5.2.2.1
:   The Storage Scale product version was updated from 5.2.1.1 to 5.2.2.1

## November 2024
{: #hpc-spectrum-symphony-nov24}

### 22 November 2024
{: #hpc-spectrum-symphony-nov2224}
{: release-note}

File Storage for VPC support
:   {{site.data.keyword.cloud}} File Storage for VPC is a zonal file storage offering that provides NFS-based file storage services. You can create file shares in an availability zone within a region. You can share them with multiple Symphony cluster nodes within the same zone in your region.

File share support for Windows worker nodes using Cloud Object Storage service
:   Windows worker nodes using Cloud Object Storage (COS) provides a compatible file storage solution that integrates efficiently within the VPC environment, enabling Windows instances to access file shares effectively.

Support for Symphony 7.3.2 fix patches
:   The {{site.data.keyword.symphony_full_notm}} version 7.3.2 offering is available for 64-bit Linux x86 and Windows platforms. The patches installed that are: Build601706, Build602082, Build602143, Build602148, Build602149, Build602158, Build602161, Build602162, Build602163, Build602185, Build602225.

Updated the Spectrum Scale version from 5.2.0.1 to 5.2.1.1
:   The Storage Scale product version was updated from 5.2.0.1 to 5.2.1.1

Updated Spectrum Symphony and Storage Scale custom images
:   Spectrum Symphony and Storage Scale custom images were updated to use RHEL 8.10 instead of RHEL 8.8.

Updated Spectrum Symphony Windows worker image
:   Spectrum Symphony Windows worker image is updated with Build602163, Build602210 fix patches.

Supports Windows operating system versions 2016 and 2022
:   Spectrum Symphony Windows worker image supports Windows 2016 and Windows 2022 versions.

## September 2024
{: #hpc-spectrum-symphony-sep24}

### 6 September 2024
{: #hpc-spectrum-symphony-sep0624}
{: release-note}

Support for Symphony 7.3.2 fix patches
:   The {{site.data.keyword.symphony_full_notm}} version 7.3.2 offering is available for 64-bit Linux x86 and Windows platforms. The patches installed that are: Build602125, Build602100, Build602094, Build602071, Build602061, Build602068, Build602039.

Updated the Spectrum Scale version from 5.1.9.3 to 5.2.0.1
:   The Storage Scale product version was updated from 5.1.9.3 to 5.2.0.1.

Updated Spectrum Symphony Windows worker image
:   Spectrum Symphony Windows worker image is updated with Build602052 fix patch.

## June 2024
{: #hpc-spectrum-symphony-june24}

### 21 June 2024
{: #hpc-spectrum-symphony-june2124}
{: release-note}

Support for Symphony 7.3.2 build601860 fix patch
:   The {{site.data.keyword.symphony_full_notm}} 7.3.2 Fix 601860 offering is available for 64-bit Linux x86 and Windows platforms. This update addresses security vulnerabilities in {{site.data.keyword.symphony_full_notm}} and should be installed on top of your existing version 7.3.2 Fix 601711 installation. While it is not mandatory, but applying Fix 601860 is recommended for optimal security. You can also apply other fixes on top of {{site.data.keyword.symphony_full_notm}} 7.3.2 Fix 601711 without applying Fix 601860.

Along with build601860 patch, 16 more patches are updated and they are available in the [change log](https://github.com/IBM-Cloud/hpc-cluster-symphony/blob/master/CHANGELOG.md) md file.

Updated the Spectrum Scale version from 5.1.9.0 to 5.1.9.3
:   The Storage Scale product version was updated from 5.1.9.0 to 5.1.9.3.

Updated Spectrum Symphony and Storage Scale custom images
:   Spectrum Symphony and Storage Scale custom images are RHEL 8.8.

Updated Spectrum Symphony Windows worker image
:   Spectrum Symphony Windows worker image was updated to Symphony 7.3.2 and also has Build601860 fix patch.

## December 2023
{: #hpc-spectrum-symphony-dec23}

### 7 December 2023
{: #hpc-spectrum-symphony-dec0723}
{: release-note}

Support for Symphony 7.3.2 Build601711 fix patch
:   The {{site.data.keyword.symphony_full_notm}} 7.3.2 Fix 601711 offering available for 64-bit Linux x86 is a mandatory fix that contains various fixes and enhancements, and includes Fix 601349 released in November 2022.

Spectrum Scale version has been updated from 5.1.7.0 to 5.1.9.0
:   The Storage Scale product version was updated from 5.1.7.0 to 5.1.9.0.

Updated Spectrum Symphony and Storage Scale custom images
:   Spectrum Symphony and Storage Scale custom images were updated to use RHEL 8.8 instead of RHEL 8.6.

Update Spectrum Symphony windows worker image
:   Spectrum Symphony windows worker image to Symphony 7.3.2 and also have Build601711 fix patch.

## May 2023
{: #hpc-spectrum-symphony-may23}

### 31 May 2023
{: #hpc-spectrum-symphony-may3123}
{: release-note}

Spectrum multicluster (SMC) support
:   The initial release of Spectrum multicluster (SMC), which connects multiple {{site.data.keyword.symphony_full_notm}} clusters into a federation cluster, is now available. You can use SMC to redirect {{site.data.keyword.symphony_full_notm}} sessions to nonbusy clusters, deploy and manage service packages, and monitor workload and resources.

Documentation enhancement: Integrating OpenLDAP with {{site.data.keyword.symphony_full_notm}}
:   The newly added [Integrating OpenLDAP with {{site.data.keyword.symphony_full_notm}}](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-integrate-openldap-spectrum-symphony) tutorial walks you through the steps that are involved in configuring {{site.data.keyword.symphony_full_notm}} to use OpenLDAP as the primary directory service for user authentication.

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
:   When you choose to use an existing VPC, the automation associates the public gateway with all the subnets if it is already part of the VPC. If the public gateway is not part of the VPC, then it creates a new public gateway and associates it with all subnets to allow internet access from the cluster nodes.

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
:   Updated to use RHEL 8.6 stock image instead of an RHEL 8.2 stock image for the login node and NFS storage node.

## August 2022
{: #hpc-spectrum-symphony-aug22}

### 11 August 2022
{: #hpc-spectrum-symphony-aug1122}
{: release-note}

Bug fix
:   Code fixes were applied related to an Ansible version 2.10 upgrade.

## June 2022
{: #hpc-spectrum-symphony-jun22}

### 30 June 2022
{: #hpc-spectrum-symphony-jun3022}
{: release-note}

Security update and enhancements
:   This release provides fixes for the Polkit Local Privilege Escalation Vulnerability (CVE-2021-4034) and for the Spring Framework Vulnerability (CVE-2022-22965) within the RHEL 8.4 based custom image accessible by the solution. This release also removes the earlier Polkit mitigation that is implemented through changes to the post-provisioning scripts. The RHEL 8.4 custom image replaces the RHEL 8.2 image that was provided in the previous release. In addition, changes in the code now allow creation of compute nodes in the cluster by Host Factory on VPC resource groups other than the default group. And, security group rules have been added to provide more secure access to the cluster by allowing SSH from only specific nodes.

## January 2022
{: #hpc-spectrum-symphony-jan22}

### 28 January 2022
{: #hpc-spectrum-symphony-jan2822}
{: release-note}

{{site.data.keyword.symphony_full_notm}} enhancements
:   You now have the option to use {{site.data.keyword.scale_short}} for shared storage that's accessible by the {{site.data.keyword.symphony_short}} compute nodes. For more information, see [Using {{site.data.keyword.scale_short}} storage](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-using-spectrum-scale-storage). With the latest changes, RHEL 8.2 custom images take the place of RHEL 7.7 and CentOS 7.7 custom images with included optimizations for NFS. Also, the security vulnerability for Polkit Local Privilege Escalation Vulnerability (CVE-2021-4034) has been mitigated with changes to the post-provisioning scripts.

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
:   You now have the option to use dedicated hosts for static worker nodes in addition to having an enhanced `ssh_command` output and hyperthreading by default. For more information, see [Using dedicated hosts](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-using-dedicated-hosts). Further improvements include an update of the stock images that are used for login and storage nodes to RHEL 8.2 and clean up of entitlement and location input properties. Parallelism is now supported in {{site.data.keyword.bpshort}} `destroy` operations to help overall performance, and due to a bug fix, you can now avoid the Terraform warning regarding the interpolation.

## October 2021
{: #hpc-spectrum-symphony-oct21}

### 18 October 2021
{: #hpc-spectrum-symphony-oct1821}
{: release-note}

Introducing {{site.data.keyword.symphony_full_notm}} for HPC workloads
:   You can now use {{site.data.keyword.symphony_full_notm}} as a workload manager with automated deployment of HPC clusters. The clusters are configured for auto scaling and use the cluster compute node configuration that you select.
