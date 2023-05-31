---

copyright:
  years: 2023
lastupdated: "2023-05-12"

keywords: 

subcollection: hpc-spectrum-symphony

---

{:support: data-reuse='support'}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Symphony multicluster Troubleshooting
{: #smc-troubleshooting-spectrum-symphony}

## What happens if a Lone symphony cluster won’t join with SMC while adding Lone symphony cluster with SMC ?
{: #smc-troubleshoot-topic-1}
{: troubleshoot}
{: support}

You are getting status as ERROR, when trying to join Lone Symphony cluster with SMC.
{: tsSymptoms}

```text
[root@hpc-primary-smc]# smcadmin cluster list
NAME     STATUS     MEMBERSHIP 
lone       error      nonmember
```

This error happens when:
{: tsCauses}

- The outbound connection configuration is unsuccessful
- SMCP service may be down in lone symphony cluster
- The network communication between SMC and lone symphony cluster is not communicable.
- The VEMKD port used is SMC may be not correct.


To make the outbound connection: 
{: tsResolve}

- check whether in lone symphony cluster enabled outbound as `<ego:EnvironmentVariable name="SMC_PROXY_INBOUND_CONNECTION">Y</ego:EnvironmentVariable>` in SMCP.xml
- If SMCP service is down in lone symphony cluster, execute command `egosh service start SMCP` to start SMCP service
- If network connection not happening, check whether you added SMC cidr block in Lone symphony cluster security group vice versa.


## How to resolve “End of file or no input: ‘Operation interrupted or timed out'“ when trying to add lone symphony cluster to smc?
{: #smc-troubleshoot-topic-2}
{: troubleshoot}
{: support}

You see thius message:
{: tsSymptoms}

```text
[root@hpc-primary-smc]# smcadmin cluster add -c lone1 -p 17870 -m "hpc-lone-primary-0,hpc-symphony-secondary-0"
End of file or no input: 'Operation interrupted or timed out'
```

When you try to add a lone symphony cluster with SMC, because of a time out issue you get the warning message. The message is a warning and does not impact adding lone symphony cluster with SMC.
{: tsCauses}

This error won’t impact on adding a lone symphony cluster with SMC. Keep on addding other lone symphony clusters.
{: tsResolve}

## What happen after provisioning SMC cluster, immediately if you try to logon as egosh user in secondary nodes of SMC?
{: #smc-troubleshoot-topic-3}
{: troubleshoot}
{: support}

After provisioning SMC cluster, if you immediately try to logon on secondary nodes of SMC you see this message: 
{: tsSymptoms}

```text
[root@hpc-secondary-smc]# egosh user logon -u Admin -x Admin
bash: egosh command not found
```

It takes 10 min to sync primary host data of SMC to secondary nodes of SMC using rsync command.
{: tsCauses}

Wait for 10 min after provisioning the SMC cluster, then try to login.
{: tsResolve}
