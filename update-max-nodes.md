---

copyright:
  years: 2021
lastupdated: "2021-09-13"

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

# Updating maximum number of worker nodes
{: #update-max-worker-nodes-count}

## Making updates to the Symphony configuration using the GUI
{: #update-max-worker-nodes-gui}

You can make updates to Symphony configuration using the GUI console or through CLI.

### Configuration updates using Symphony GUI console
{: #update-max-worker-nodes-symphony-gui-console}

Please reference the topic [Symphony GUI console](/docs/hpc-spectrum-symphony?topic=hpc-spectrum-symphony-gui-console&interface=ui) for more information on how to use Symphony GUI console. 

Once you login to console please follow these steps to update the maximum number of worker nodes:

1. From the cluster management console, click **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) Resources > Cloud > Configuration.**

2. In the Cloud Configuration page, update the HostFactory service configuration, your cloud provider's host template, or both. If your updates do not include optional parameters, the parameters default values are used.

3. To update the cloud provider's host template definition, select ``ibmcloudgen2inst`` from the Provider list and edit your existing template definition in the Templates text box.

4. Modify the values for following:

    a. ***maxNumber*** changes the maximum number of worker nodes. 
  
    b. ***imageId*** changes the ID of the custom image.

    c. ***vmType*** changes the instance type. If you modify the instance type, you will also need to modify the corresponding values for ***ncpus*** and ***nram*** in the **attributes** section. For example, if you choose ``bx2-2x8``, this instance type has 2 VCPUs and 8GB of RAM. Since VCPUs includes hyper-threads, use VCPUs divided by 2, to get ***ncpus***. Set ncpus to 1 and nram to 8192.

    d. ***postProvisionFile*** changes the location  of the post provision script.

    e. ***hostPrefix*** changes the prefix of provisioned hosts

The changes take effect automatically. There is no need to restart the HostFactory service.

### Configuration updates using Symphony CLI
{: #update-max-worker-nodes-symphony-cli}

1. Find and edit the following file on your management node. Replace ***CLUSTER*** value with the name of your cluster. The default value is ***HPCCluster***. 
``/data/<CLUSTER>/sym731/hostfactory/conf/providers/ibmcloudgen2inst/ibmcloudgen2instprov_templates.json``

2.  Modify the values for following:

    a. ***maxNumber*** property will update the maximum number of worker node.

     
    b. ***imageId*** changes the ID of the custom image.


    c.***vmType*** changes the instance type. If you modify the instance type, you will also need to modify the corresponding values for ***ncpus*** and ***nram*** in the **attributes** section. For example, if you choose ``bx2-2x8``, this instance type has 2 VCPUs and 8GB of RAM. Since VCPUs includes hyper-threads, use VCPUs divided by 2, to get ***ncpus***. Set ***ncpus*** to 1 and nram to 8192.
    
    d. ***postProvisionFile*** changes the location  of the post provision script.


    e. ***hostPrefix*** changes the prefix of provisioned hosts.

3. Save the file.

4. Restart the HostFactory service by using the following commands:

      ``$egosh service stop HostFactory``
      
      ``$egosh service start HostFactory``

For more information on this topic please reference 
 [Dynamically updating host factory configuration](https://www.ibm.com/docs/en/spectrum-symphony/7.3.1?topic=bursting-dynamically-updating-host-factory-configuration) topic.

