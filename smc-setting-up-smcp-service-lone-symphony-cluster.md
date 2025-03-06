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


# Setting up SMCP Service in Lone Symphony Cluster
{: #smc-setup-smcservice-lone-cluster}

1.  Log in to the primary host of lone symphony by using the ssh_command value as shown at the end of the Lone Symphony Cluster log output.

    ``` shell
    # ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 18443:localhost:8443 -J root@149.81.166.184 root@10.241.0.6
    ```
2.  Change to the `/tmp` directory.

    ```shell
    # cd /tmp
    ```
3.  Logon as an Admin.

    ```shell
    # egosh user logon -u Admin -x Admin
    ```
4.  Generate an SMCP xml file.

    ```shell
    # egosh service view SMCP -p
    ```

5.  Edit the SMCP xml file to set the proxy into listen mode so that the proxy expects an inbound connection.

    ```shell
    # vi SMCP.xml
    ```
6.  After the SMCP.xml file opens in the editor, replace MANUAL with AUTOMATIC in the sc:ControlPolicy block and add an environmental variable in the second block of sc:ActivityDescription as <ego:EnvironmentVariable name="SMC_PROXY_INBOUND_CONNECTION">Y</ego:EnvironmentVariable>.
    ```http
    <?xml version="1.0" encoding="UTF-8"?>
    <sc:ServiceDefinition xmlns:sc="http://www.platform.com/ego/2005/05/schema/sc" xmlns:ego="http://www.platform.com/ego/2005/05/schema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xsi:schemaLocation="http://www.platform.com/ego/2005/05/schema/sc ../sc.xsd http://www.platform.com/ego/2005/05/schema ../ego.xsd" ServiceName="SMCP">
        <sc:Version>1.2</sc:Version>
        <sc:Description>SMC: Proxy</sc:Description>
        <sc:MinInstances>1</sc:MinInstances>
        <sc:MaxInstances>1</sc:MaxInstances>
        <sc:Priority>10</sc:Priority>
        <sc:MaxInstancesPerSlot>1</sc:MaxInstancesPerSlot>
        <sc:NeedCredential>TRUE</sc:NeedCredential>
        <sc:ControlPolicy>
            <sc:StartType>AUTOMATIC</sc:StartType>
            <sc:MaxRestarts>10</sc:MaxRestarts>
            <sc:HostFailoverInterval>PT1S</sc:HostFailoverInterval>
        </sc:ControlPolicy>
        <sc:AllocationSpecification>
            <ego:ConsumerID>/ManagementServices/EGOManagementServices</ego:ConsumerID>
            
        <sc:ResourceSpecification ResourceType="http://www.platform.com/ego/2005/05/schema/ce">
            <ego:ResourceGroupName>ManagementHosts</ego:ResourceGroupName>
            </sc:ResourceSpecification>
        </sc:AllocationSpecification>
    <sc:ActivityDescription>
        <ego:Attribute name="hostType" type="xsd:string">LINUX86</ego:Attribute>
        <ego:ActivitySpecification>
            <ego:Command>${EGO_TOP}/soam/7.3.2/linux-x86/etc/smcproxy</ego:Command>
            <ego:ExecutionUser>egoadmin</ego:ExecutionUser>
            <ego:EnvironmentVariable name="SMC_HOME">${EGO_TOP}/soam</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="SOAM_HOME">${EGO_TOP}/soam</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="SMC_AGENT_TOP">${EGO_TOP}/soam/agent</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="SMC_KD_MASTER_LIST">@SMC_MASTER_LIST@</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="SMC_KD_PORT">@SMC_KD_PORT@</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="PATH">${EGO_TOP}/soam/7.3.2/linux-x86/lib;${EGO_LIBDIR}</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="SMC_PROXY_IDL_PORT">0</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="SMC_PLUGIN_REPORT_INTERVAL">60</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="WORKLOAD_SMC_PLUGIN_REPORT_INTERVAL">5</ego:EnvironmentVariable>
            <ego:EnvironmentVariable name="SMC_AGENT_MAX_LOG_PER_HOST">60</ego:EnvironmentVariable>
            <ego:WorkingDirectory>${EGO_CONFDIR}/../../soam/work</ego:WorkingDirectory>
            <ego:Umask>0022</ego:Umask>
            <ego:Rlimit name="NOFILE" type="soft">6400</ego:Rlimit>
        </ego:ActivitySpecification>
    </sc:ActivityDescription>
    <sc:ActivityDescription>
        <ego:Attribute name="hostType" type="xsd:string">X86_64</ego:Attribute>
        <ego:ActivitySpecification>
        <ego:Command>${EGO_TOP}/soam/7.3.2/linux-x86_64/etc/smcproxy</ego:Command>
        <ego:ExecutionUser>egoadmin</ego:ExecutionUser>
        <ego:EnvironmentVariable name="SMC_HOME">${EGO_TOP}/soam</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="SOAM_HOME">${EGO_TOP}/soam</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="SMC_AGENT_TOP">${EGO_TOP}/soam/agent</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="SMC_KD_MASTER_LIST">@SMC_MASTER_LIST@</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="SMC_KD_PORT">@SMC_KD_PORT@</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="PATH">${EGO_TOP}/soam/7.3.2/linux-x86_64/lib;${EGO_LIBDIR}</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="SMC_PROXY_IDL_PORT">0</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="SMC_PLUGIN_REPORT_INTERVAL">60</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="WORKLOAD_SMC_PLUGIN_REPORT_INTERVAL">5</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="SMC_AGENT_MAX_LOG_PER_HOST">60</ego:EnvironmentVariable>
        <ego:EnvironmentVariable name="SMC_PROXY_INBOUND_CONNECTION">Y</ego:EnvironmentVariable>
        <ego:WorkingDirectory>${EGO_CONFDIR}/../../soam/work</ego:WorkingDirectory>
        <ego:Umask>0022</ego:Umask>
        <ego:Rlimit name="NOFILE" type="soft">12800</ego:Rlimit>
        </ego:ActivitySpecification>
        </sc:ActivityDescription>
    </sc:ServiceDefinition>
    ```

7.  Modify the SMCP config file.

        ```shell
        # egosh service modify SMCP -f SMCP.xml
        ```

8.  Start the SMCP Service.

        ```shell
        # egosh service start SMCP
        ```

9.  Check that the SMCP Service started.

        ```shell
        # egosh service list -ll | grep SMCP
        ```

10. Check the status of SMCP Service, whether proxy inbound rule connection is active.

        ```shell
        # egosh service view | grep SMC
        ```
