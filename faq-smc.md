---

copyright:
  years: 2023, 2025
lastupdated: "2023-05-05"

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
{:faq: data-hd-content-type='faq'}

# Symphony Multicluster (SMC) FAQs
{: #smc-spectrum-symphony-faqs}

## Is it possible to submit jobs during a failover in-progress?
{: #submit-failover}
{: faq}

Yes, if SMC is not available for any reason, submitted jobs are scheduled in the respective Lone symphony cluster.

## How to check the SMC host logs?
{: #check-host-logs}
{: faq}

1. Change the directory to `/data/<cluster_id>/sym731/multicluster/smc/logs`.
2. Run command `tail -f smc-<host-file-name>.log`.

## Does SMC allow there to be more than three zones in a region selected for deployment (smc_zone parameter to have more than 3 zones)?
{: #smc-zones-more-than-3-zones}
{: faq}

No, the smc-zone parameter supports a maximum of 3 zones, entered as a list, for example, ["us-south-1","eu-gb-3","jp-tok-2"].

## Does SMC allow there to be more than three existing VPCs or more than three existing regions for VPCs?
{: #lone-vpc-name-region-more-than-3}
{: faq}

No SMC supports a maximum of 3 existing VPCs in `lone_vpc_name` and 3 existing regions for VPCs in `lone_vpc_region`. The VPC names and VPC regions are provided in a list and must be in the same order. For example, there are three VPCs in three regions, "vpc-east" in "us-east". "vpc-south" in "us-south", and "vpc-tor" in "ca-tor". For this example, lone_vpc_name = ["vpc-east", "vpc-south" "vpc-tor"] and lone_vpc_region = [ "us-east","us-south","ca-tor"].

## When one of the joined lone symphony clusters with SMC fails to participate in workload placement after execution of symping command, what should I do?
{: #check-host-logs}
{: faq}

Run a few more iterations of workload placement by running jobs, the lone symphony cluster will join back automatically.
