---

copyright:
  years: 2023, 2025
lastupdated: "2023-05-03"

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

# Accessing the SMC GUI
{: #smc-access-gui}

1.  Open a new command-line terminal.
2.  Log in to the primary host by using the ssh_command value as shown at the end of the log output.
    ```shell
    # ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8443:localhost:8443 -J ubuntu@52.116.122.64 root@10.10.0.5
    ```
3.  Open a browser on the local machine, and navigate to https://localhost:8443/platform. When you access this URL for the first time, your browser displays an SSL self-assigned certificate warning.
4.  Log in and access the SMC GUI for the cluster, use "Admin" as the login credentials for both the username and password.
