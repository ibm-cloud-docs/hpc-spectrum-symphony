---

copyright:
  years: 2021
lastupdated: "2021-11-30"

keywords: 

subcollection: hpc-spectrum-symphony

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note .note}
{:important: .important}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:table: .aria-labeledby="caption"}

# Symphony GUI console
{: #gui-console}

## Configure an SSH tunnel to the cluster to allow access
{: #configuring-an-SSH-tunnel-to-the-cluster}

Before you can access the Spectrum Symphony cluster management console, you need to configure an SSH tunnel to the cluster to allow access to the management console through a local browser instance. You can do this by configuring port forwarding on your local browser host. 

1. Execute the following command to create the SSH tunnel:

    ``$ ssh -L 8443:localhost:8443 -J root@{LOGIN_NODE_PUBLIC_IP} root@{MANAGEMENT_NODE_PRIVATE_IP}``

    where ``LOGIN_NODE_PUBLIC_IP`` needs to be replaced with the respective IP address of the Login Node and ``MANAGE_NODE_PRIVATE_IP`` needs to be replaced with the IP address of the management node that is running the **WEBGUI** service.

2. After starting the above SSH session, when you open the site https://localhost:8443 on your local web browser, it will connect to the Symphony management node.

## Further considerations
{: further-considerations}

1. You get an SSL self-signed certificate warning with your browser the first time that you access this URL. 
{:note .note}

2. This ssh command will log you into the primary management host and initialize a Spectrum Symphony command-line environment.
{:note .note}

Further information on how to use Symphony UI can be found at [Accessing the cluster management console](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=cluster-accessing-management-console#accessing_PMC).

