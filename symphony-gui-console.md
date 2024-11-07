---

copyright:
  years: 2021, 2022
lastupdated: "2022-11-17"

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

# Accessing the Symphony GUI 
{: #gui-console}

## Before you begin
{: #before-you-begin-accessing-symphony-console}

Before you begin accessing the {{site.data.keyword.symphony_short}} GUI, review the following considerations and requirements:

* The initial setup must be done from your local machine.
* The local machine must have one of the machine IP addresses that you specified when you created the cluster.

## Configuring an SSH tunnel to the cluster to allow access
{: #configuring-an-SSH-tunnel-to-the-cluster}

Before you can access the {{site.data.keyword.symphony_short}} cluster management console, you need to configure an SSH tunnel to the cluster to allow access to the management console through a local browser instance. You can do this by configuring port forwarding on your local browser host. 

1. Run the following command to create the SSH tunnel:

    ```sh
    ssh -L 8443:localhost:8443 -J root@{LOGIN_NODE_PUBLIC_IP} root@{MANAGEMENT_NODE_PRIVATE_IP}
    ```
    {: codeblock}

    where ``LOGIN_NODE_PUBLIC_IP`` needs to be replaced with the respective IP address of the login node and ``MANAGEMENT_NODE_PRIVATE_IP`` needs to be replaced with the IP address of the management node that is running the WEBGUI service.

2. From the shell prompt on the management node, run the following commands to identify which node the WEBGUI is running on (use the {{site.data.keyword.cloud_notm}} console to get the IP address of that node):

    ```sh
    egosh user logon -u Admin -x Admin
    egosh service list -ll | grep WEBGUI
    ```
    {: codeblock}

    **Example output**

    ```text
    [root@symphony-primary-0 ~]# egosh user logon -u Admin -x Admin
    Logged on successfully
    [root@symphony-primary-0 ~]# egosh service list -ll | grep WEBGUI
    "WEBGUI","","","","","","STARTED","8","/ManagementServices/EGOManagementServices","ManagementHosts","symphony-secondary-0.ibm.com","1","1","RUN","17"
    [root@symphony-primary-0 ~]#
    ```
    {: screen}

3.  Run the following command to close the tunnel:

    ```sh
    control-D
    ```
    {: codeblock}

4. Run the following command to reopen the tunnel:

    ```sh
    ssh -L 8443:localhost:8443 -J root@{LOGIN_NODE_PUBLIC_IP} root@{WEBGUI_NODE_PRIVATE_IP}
    ```
    {: codeblock}

    where `LOGIN_NODE_PUBLIC_IP` needs to be replaced with the respective IP address of the login node and `WEBGUI_NODE_PRIVATE_IP` needs to be replaced with the IP address of the node that is running the WEBGUI service.

5. Open a browser on the local machine, and run https://localhost:8443. You will get an SSL self-signed certificate warning with your browser the first time that you access this URL.

6. To access the GUI, enter the default login credentials: username is `Admin` and password is `Admin`.

For more information on how to use the Symphony GUI, see [Accessing the cluster management console](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=cluster-accessing-management-console){: external}.
