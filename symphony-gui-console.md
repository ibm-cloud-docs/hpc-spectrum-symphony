---

copyright:
  years: 2021, 2022
lastupdated: "2022-10-26"

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

# Symphony GUI console
{: #gui-console}

## Configuring an SSH tunnel to the cluster to allow access
{: #configuring-an-SSH-tunnel-to-the-cluster}

Before you can access the {{site.data.keyword.symphony_short}} cluster management console, you need to configure an SSH tunnel to the cluster to allow access to the management console through a local browser instance. You can do this by configuring port forwarding on your local browser host. 

1. Run the following command to create the SSH tunnel:

    ```
    ssh -L 8443:localhost:8443 -J root@{LOGIN_NODE_PUBLIC_IP} root@{MANAGEMENT_NODE_PRIVATE_IP}
    ```
    {: codeblock}

    where ``LOGIN_NODE_PUBLIC_IP`` needs to be replaced with the respective IP address of the login node and ``MANAGEMENT_NODE_PRIVATE_IP`` needs to be replaced with the IP address of the management node that is running the **WEBGUI** service.

2. After starting the SSH session, when you open the designated localhost site on your local web browser, it will connect to the Symphony management node.

## Further considerations
{: #further-considerations}

1. You get an SSL self-signed certificate warning with your browser the first time that you access this URL. 

2. This SSH command will log you into the primary management host and initialize a {{site.data.keyword.symphony_short}} command-line environment.

Further information on how to use Symphony UI can be found at [Accessing the cluster management console](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=cluster-accessing-management-console#accessing_PMC){: external}.

