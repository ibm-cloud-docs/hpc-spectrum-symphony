---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-09"

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:table: .aria-labeledby="caption"}

# Installing interim fixes from IBM Fix Central on Linux hosts
{: #installing-interim-fixes}

After you install or upgrade {{site.data.keyword.IBM}} {{site.data.keyword.symphony_short}} on Linux&reg;, use [IBM Fix Central](https://www.ibm.com/support/fixcentral/swg/selectFixes?parent=IBM%20Spectrum%20Computing&product=ibm/Other+software/IBM+Spectrum+Symphony&release=All&platform=All&function=all){: external} to download any interim fixes that are recommended for {{site.data.keyword.symphony_full_notm}} on Linux. You can install and manage interim fixes by using the ``egoinstallfixes`` and ``pversions`` commands.
{: shortdesc}

Interim fixes for {{site.data.keyword.symphony_full_notm}} are available on [IBM Fix Central](https://www.ibm.com/support/fixcentral/swg/selectFixes?parent=IBM%20Spectrum%20Computing&product=ibm/Other+software/IBM+Spectrum+Symphony&release=All&platform=All&function=all){: external} or by contacting your IBM Support representative. Typically, you download these interim fixes, then use the ``egoinstallfixes`` command to install and manage those fixes on Linux hosts in your cluster. The ``egoinstallfixes`` command enables you to complete the following tasks:

- Install fixes, which include:
    -  checking the system, backing up the current files
    
    -  deleting any files that the {{site.data.keyword.symphony_full_notm}} cluster no longer requires (these files might have been installed with {{site.data.keyword.symphony_full_notm}}, or applied during an {{site.data.keyword.symphony_full_notm}} fix)
    
    - installing the specified fix packages on your existing {{site.data.keyword.symphony_full_notm}} cluster

    - Also, if the command detects a pre-script or post-script file within the fix package, it runs the scripts as necessary. These scripts can contain custom actions to be completed before or after applying the fix.

- Check fix packages, without installing them. This preliminary check determines whether your existing cluster is compatible with the fix you want to install, verifies that the user account has appropriate permissions, and lists files that are overwritten by and added by the fix.

- Roll back the most recent fix (either by fix package name or by build number) so that your host returns to its original state before installing the fix.

Use the `egoinstallfixes` command only on Linux hosts; to manage fixes on Windows, refer to the readme file that is bundled with the fix you want to install.
{: note}

## Installing interim fixes
{: #install-interim-fixes}

1. Use the `egoinstallfixes` command to install or roll back interim fixes for your {{site.data.keyword.symphony_full_notm}} installation on a Linux host. For command syntax and option details, see [egoinstallfixes](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=commands-egoinstallfixes#reference_p3w_4dm_bdb){: external}.

    a. To run the `egoinstallfixes` command, your Linux host must include the ed Linux line-oriented text editor.

    b. To run the `egoinstallfixes` command remotely, note the following considerations:

    - Set the full path and file name to the environment file that properly defines your ``EGO_TOP`` and ``EGO_CONFDIR`` parameters by running the command with the ``-f env_file`` option.

    - Force ``pseudo-tty`` allocation to that remote host by prefacing the ``egoinstallfixes`` command with the ``ssh -t remote_hostname`` command.

    - Surround the command syntax with quotation marks ().

        Here is an example of using the ``egoinstallfixes`` command from your local host to apply the ``sym-version_x86_64_build123456.tar.gz`` package to a remote host called ``abc123``, and specifying the ``/home/egoadmin/cluster.env`` location for the ``-f`` option:

        ``ssh -t abc123 /opt/ibm/spectrumcomputing/3.9/install/egoinstallfixes -f /home/egoadmin/cluster.env sym-version_x86_64_build123456.tar.gz``

2. After you run the ``egoinstallfixes`` command to successfully install fixes on your local Linux host, use the ``pversions`` command to query whether your fixes are installed on that host. For command syntax and option details, see [pversions for Linux](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=commands-pversions-linux#pversions_linux){: external}.

3. To view the installed and active components on your entire cluster, use the **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) System & Services > Software Components > Installed and Active Software Components** page within the cluster management console. For more information, see [Determining installed and active software components](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=cluster-determining-installed-active-software-components#task_gjr_j5n_bbb){: external}.

For more background information, see [Installing and configuring IBM Spectrum Symphony](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=installation-configuration){: external}. 
{: note}
