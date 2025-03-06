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

# Connecting Lone Symphony and SMC Cluster
{: #connect-lone-ans-smc-cluster}

After the Lone Symphony cluster is provisioned, add the SMC CIDR block to the Security Group of all previously created Lone symphony clusters to allow traffic between the SMC cluster and the Lone symphony cluster. When you created the SMC cluster by using the existing_lone_vpc and existing_lone_vpc_region and the Lone symphony VPC was added to the SMC Transit Gateway.

1.  On DNS Zones, add these forwarding rules:
    *   Lone symphony clusters to SMC using domain_name
    *   SMC to lone symphony using dns_server_ip

2.  Add a forwarding rule within each Lone symphony cluster to enable traffic between Lone symphony clusters.
3.  Verify the connection by pinging the host names of the SMC and Lone symphony clusters from each other.
