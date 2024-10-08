---

copyright:
  years: 2021, 2022
lastupdated: "2022-02-01"

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

# Adding compute profile types for auto scaling
{: #add-compute-profile-types}

Complete the following steps to add compute profile types when worker nodes are automatically added by the resource connector.

1. Update the file `ibmcloudgen2_templates.json` in `$Symphony_ENDIR/resource_connector/ibmcloudgen2/conf`. A typical HPC cluster that is installed on {{site.data.keyword.cloud_notm}} must have this file in `/opt/ibm/symphony/conf/resource_connector/ibmcloudgen2/conf`. See the following sample content for an example:

    ```json
    {
    "templates": [
    {
    "templateId": "<TemplateID>",
    "maxNumber": 200,
    "attributes": {
    "type": ["String", "X86_64"],
    "ncores": ["Numeric", "1"],
    "ncpus": ["Numeric", "1"],
    "mem": ["Numeric", "1024"],
    "icgen2host": ["Boolean", "1"]
    },
    "imageId": "<IMAGE_ID>",
    "subnetId": "<SUBNET_ID>",
    "vpcId": "<VPC_ID">,
    "vmType": "cx2-2x4",
    "securityGroupIds": ["<SECURITY_GROUP_ID>"],
    "sshkey_id": "<SSH_KEY_ID>",
    "region": "us-south",
    "zone": "us-south-2"
    }
    ]
    }
    ```
    {: screen}

2. Add a new template section to the templates list. Review the values for all the properties to make sure that they map to the correct VPC configuration.
3. Run the following command to restart the `mbatchd` process and apply the changes:

    ```lsf
    badmin mbdrestart
    ```
    {: pre}

4. When the template is added, you can use the `symphony.shared` file to map the new template to a specific job. The `symphony.shared` file is located in `$Symphony_ENVDIR` (same location as `symphony.conf`). In a typical installation, this is found in the `/opt/ibm/symphony/conf` folder. Add _templateId_ as a resource in the resource section in the `symphony.shared` file. For example, **templateID String () () (template ID for the external hosts)**.
5. Add the following section to `user_data.sh` to let the virtual machines add _templateId_ as `SYMPHONY_LOCAL_RESOURCES`.

    ```shell
    if [ -n "$template_id" ]; then
    sed -i "s/(SYMPHONY_LOCAL_RESOURCES=.)"/\1 [resourcemap $template_idtemplateID]"/" $SYMPHONY_CONF_FILE
    echo "update SYMPHONY_LOCAL_RESOURCES in $SYMPHONY_CONF_FILE successfully, add [resourcemap ${template_id}*templateID]" >> $logfile
    else
    echo "templateID doesn't exist in environment variable" >> $logfile
    ```
    {: screen}

    The `user_data.sh` is specified by the `IBMCLOUDGEN2_PROVISION_FILE` variable in the `ibmcloudgen2_config.json` file. `ibmcloudgen2_config.json` is located in `/opt/ibm/symphony/conf/resource_connector/ibmcloudgen2/conf`. See the following sample file content for an example:

    ```json
    "IBMCLOUDGEN2_KEY_FILE": "/opt/ibm/symphony/conf/resource_connector/ibmcloudgen2/credentials",
    "IBMCLOUDGEN2_PROVISION_FILE": "/opt/ibm/symphony/conf/resource_connector/ibmcloudgen2/user_data.sh",
    "IBMCLOUDGEN2_MACHINE_PREFIX": "icgen2host",
    "LogLevel": "INFO"
    ```
    {: screen}

6. You can submit the change by using the -R option. See the following example:

    ```lsf
    bsub -R “templateId=Template-2” sleep 1000
    ```
    {: pre}
