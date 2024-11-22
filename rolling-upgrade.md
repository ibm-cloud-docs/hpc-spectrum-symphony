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

Symphony allows two different upgrade paths, parallel and rolling upgrade. Parallel requires the administrator to spin up a new cluster with the latest Symphony version and manually copy the configuration and working files to the new cluster. Meanwhile, rolling upgrade allows hosts and cluster upgrade. Other benefits of rolling upgrades are:

*  No additional or reduced resources during the upgrade process.
*  No impact on the existing workload and minimal impact on the new workload.
*  Automatic migration of configuration files.

This topic is about rolling the upgrade process. However, administrators can find more information about the parallel upgrade [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-by-using-rolling).

## Prerequisites before the upgrade
{: #pre-req-rol-upgrade}

Administrators need to be familiar with the version upgrade documentation before any upgrade. Symphony general software upgrade process are found here based on version level. Here is a high level of essential things to be aware of before upgrading:

*  Obtain the latest {{site.data.keyword.symphony_full_notm}} files.
*  Upgrade is only allowed for within product editions, for example, Developer to Developer, Advanced to Advanced, and so on.
*  Plan for two maintenance windows, one to upgrade your primary host and one to upgrade the cluster-wide configuration.
*  Perform the upgrade from the same user account used to install the previous version of {{site.data.keyword.symphony_full_notm}}.
*  Ensure ample storage to backup configuration files; size depends on the number of configuration files.
*  Back up any custom configuration files before the upgrade so administrators can restore them after the upgrade.
*  Do not upgrade the cluster by using the RPM option -U.
*  Identify and validate schema DB update requirements.

The administrator can find more detailed requirements [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-before-you-perform-rolling#before_you_upgrade).

## Performing a rolling upgrade on Linux with a shared file system
{: #perf-roll-upgrade}

Before starting, the administrator must have the {{site.data.keyword.symphony_full_notm}} installation package and the appropriate product edition entitlement file `(sym_*_entitlement.dat)`.

The administrator can use one shared location for all hosts. Alternatively, multiple shared locations are allowed (that is, a separate share for management and another for compute hosts). In this scenario, the host configuration and work are redirected to the appropriate shared location; the administrator manually creates an environment file so they can source the compute host environment separately.
{: note}

The administrator can find the procedure to perform the rolling upgrade [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=linux-upgrading-shared-file-system#taskrolling_upgrade_shared_fs__steps__1).

## Post upgrade
{: #post-upgrade}

After completion, the cluster will use the configuration from the original cluster. In addition, the following can be done:

*  If applicable, upgrade the host factory configuration.
*  Validate that the cluster is functioning correctly.
*  If applicable, update any {{site.data.keyword.symphony_full_notm}} applications to work with the new version.
*  If applicable, update log retrieval configuration.

The administrator can find additional information around post upgrade, [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-after-rolling).

## Additional Considerations
{: #add-considerations}

In some circumstances, a rollback is required. The administrator can find the procedure to perform a rollback for Linux [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-rolling-back-linux). {{site.data.keyword.symphony_full_notm}} troubleshooting guide can be found [here](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=upgrade-troubleshooting-your-rolling).
