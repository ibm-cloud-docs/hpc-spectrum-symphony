---

copyright:
  years: 2022

lastupdated: "2022-05-06"

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