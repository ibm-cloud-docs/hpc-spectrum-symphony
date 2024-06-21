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


# Validating failover on SMC
{: #smc-validate-failover}

1.  Log in to the primary host by using the ssh_command value as shown at the end of the log output.
    ```shell
    # ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8443:localhost:8443 -J ubuntu@169.63.102.28 root@10.10.0.5
    ```
2.  Logon as Admin
    ```shell
    # egosh user logon -u Admin -x Admin
    ```

3.  Check the primary host
    ```shell
    # egosh resource list -m
    ```
4.  Stop EGO at primary
    ```shell
    # systemctl stop ego
    ```
5.  Log in to the secondary and wait for a while.
    ```shell
    # ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 8443:localhost:8443 -J ubuntu@52.116.122.64 root@10.20.0.    
    ```

6.  Check the master hostname and make sure it targets to secondary host as master.
    ```shell
    # egosh ego info
    # egosh resource list -m
    ```
7.  Log in to the primary host to initiate a restart of EGO.
    ```shell
    # systemctl start ego
    ```
8.  Check the master hostname and make sure it targets to primary host as master.
    ```shell
    # egosh ego info
    # egosh resource list -m
    ```
