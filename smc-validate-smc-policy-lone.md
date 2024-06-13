---

copyright:
  years: 2023
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
                   
# Validating SMC policy on Lone(symphony)
{: #smc-validate-policy-lone}

1.  Log in to the lone(symphony) of primary as shown in lone(symphony) ssh_command output.
    ```shell
    # ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8443:localhost:8443 -J root@52.116.122.64 root@10.241.0.6
    ```
2.  Set smc_global_placement to enable jobs obtained from smc
    ```shell
    # export SMC_GLOBAL_PLACEMENT=enabled
    ```
3.  Set the master cluster url.
    ```shell
    # export SMC_MASTER_CLUSTER_URL="master_list://hpc-smc-primary.smc.ibmhpc.com:17870 hpc-smc-secondary.smc.ibmhpc.com:17870 hpc-smc-secondary-candidate.smc.ibmhpc.com:17870"
    ```
4.  Validate the SMC policy by running jobs. Scheduled jobs should follow the policy.
    ```shell
    # symping
    ```


