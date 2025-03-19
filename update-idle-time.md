---

copyright:
  years: 2021, 2025
lastupdated: "2025-03-19"

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

# Updating idle time before worker nodes are removed
{: #update-idle-time}

The idle time behavior for Symphony host factory is controlled by three properties that are defined in the `symAreq_policy_configuration.json` file:

- ***billing_interval*** has a default value of 60, defines what the interval is in minutes where cloud resources are metered. For {{site.data.keyword.Bluemix_notm}}, VPC hourly virtual server instances are used.

- ***return_interval*** has a default value of 10, defines the time before it presses the next counter of billing interval where the worker can be released from cluster. The Symphony `symA` requester constantly monitors the cluster capacity and workload requirements, and if it detects that unnecessary cores are present in the cluster it can release workers from the cluster and adjust the workloads to optimally use the existing compute resources.

- ***return_idle_only*** has a default value of false, and determines that the systems must be returned from the cluster only if they are idle and not busy processing workload.

These three properties can be edited to guarantee the wanted behavior on idle time for worker nodes.

For more information on this file configuration, see [symAreq_policy_config.json reference](https://www.ibm.com/docs/en/spectrum-symphony/7.3.2?topic=factory-symareq-policy-configjson){: external}.
{: note}

The file can be found in: `/data/<CLUSTER>/sym731/hostfactory/conf/requestors/symAinst/symAinstreq_policy_config.json`. Replace `<CLUSTER>` value with the name of your cluster. The default value is `HPCCluster`. 

You can change the configuration file and restart the Symphony Host Factory service by using the following commands:

```sh
egosh service stop HostFactory
```
{: pre}

```sh
egosh service start HostFactory
```
{: pre}
