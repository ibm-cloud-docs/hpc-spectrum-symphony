---

copyright:
  years: 2023
lastupdated: "2023-12-06"

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

# Symphony Rolling Upgrade
{: #symphony-rolling-upgrade}

Symphony allows two different upgrade paths, parallel and rolling upgrade. Parallel requires the administrator to spin up a new cluster with the latest Symphony version and manually copy the configuration and working files to the new cluster. Meanwhile, rolling upgrade allows you to upgrade the hosts and cluster. Other benefits of rolling upgrades are:

*  No additional or reduced resources during the upgrade process

*  There is no impact on the existing workload and minimal impact on the new workload

*  Automatic migration of configuration files

This topic covers the rolling upgrade process. However, administrators can find more information about the parallel upgrade [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-by-using-rolling).

## Prerequisites before the upgrade
{: #pre-req-rol-upgrade}

Administrators need to be familiar with the version upgrade documentation before any upgrade. Symphony general software upgrade process are found here based on version level. Here are high-level of essential things to be aware of before upgrading:

*  Obtain the latest IBM Spectrum Symphony files.

*  Upgrade is only allowed for within product editions, e.g., Developer to Developer, Advanced to Advanced, etc.

*  Plan for two maintenance windows, one to upgrade your primary host and one to upgrade the cluster-wide configuration.

*  Perform the upgrade from the same user account used to install the previous version of IBM Spectrum Symphony.

*  Ensure ample storage to backup configuration files; size depends on the number of configuration files.

*  Back up any custom configuration files before the upgrade so administrators can restore them after the upgrade.

*  Do not upgrade cluster using the RPM option -U.

*  Identify and validate schemat DB update requirements.

The administrator can find more detailed requirements [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-before-you-perform-rolling#before_you_upgrade).

## Performing a rolling upgrade on Linux with a shared file system
{: #perf-roll-upgrade}

Before starting, the administrator must have the IBM Spectrum Symphony installation package and the appropriate product edition entitlement file `(sym_*_entitlement.dat)`.

The administrator can use one shared location for all hosts. Alternatively, multiple shared locations are allowed (i.e., a separate share for management and another for compute hosts). In this scenario, the host configuration and work are redirected to the appropriate shared location; the administrator manually creates an environment file so they can source the compute host environment separately.
{: note}

The administrator can find the procedure to perform the rolling upgrade [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=linux-upgrading-shared-file-system#taskrolling_upgrade_shared_fs__steps__1).

## Post upgrade
{: #post-upgrade}

After completion, the cluster will use the configuration from the original cluster. In addition, the following can be done:

If applicable, upgrade host factory configuration.

Validate cluster is functioning correctly.

If applicable, update any IBM Spectrum Symphony applications to work with the new version.

If applicable, update log retrieval configuration.

The administrator can find additional information around post upgrade, here.

## Additional Considerations
{: #add-considerations}

In some circumstances, rollback may be required. The administrator can find the procedure to perform a rollback for Linux [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-rolling-back-linux). IBM Spectrum Symphony troubleshootting guide can be found [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-troubleshooting-your-rolling).