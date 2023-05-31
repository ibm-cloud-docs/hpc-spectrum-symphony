---

copyright:
  years: 2023
lastupdated: "2023-05-31"

keywords:
content-type: tutorial
services:
account-plan:
completion-time:
subcollection: hpc-spectrum-symphony

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}
{:step: data-tutorial-type='step'}

# Integrating OpenLDAP with IBM Spectrum Symphony
{: #integrate-openldap-spectrum-symphony}
{: toc-content-type="tutorial"}

In the following tutorial, you learn how to integrate OpenLDAP with {{site.data.keyword.symphony_full_notm}}, which is a high-performance distributed computing platform. OpenLDAP is an open source implementation of the Lightweight Directory Access Protocol (LDAP) that can be used to store and manage information about users, groups, and other objects in a network. 

In the tutorial, you learn the steps that are involved in configuring {{site.data.keyword.symphony_full_notm}} to use OpenLDAP as the primary directory service for user authentication. You also learn about the benefits of integrating OpenLDAP with {{site.data.keyword.symphony_full_notm}}, such as centralized user management, improved security, and simplified user authentication.

This tutorial covers configuring the OpenLDAP server on a Linux&reg; system.
{: note}

## Architecture
{: #architecture}

The integration between OpenLDAP and {{site.data.keyword.symphony_full_notm}} involves the following components:

1. **OpenLDAP directory service**: This component stores and manages user authentication data, such as usernames and passwords.
2. **{{site.data.keyword.symphony_full_notm}} distributed computing platform**: This component uses the authentication data from OpenLDAP to authenticate users and enable them to run analytical and data-intensive applications.
3. **LDAP authentication process**: This process enables {{site.data.keyword.symphony_full_notm}} to communicate with OpenLDAP and authenticate users by using their existing credentials.

The OpenLDAP integration with {{site.data.keyword.symphony_full_notm}} architecture enables centralized user management, improved security, and simplified user authentication. The integration also allows for the use of existing authentication credentials, reducing the need for users to remember multiple login credentials. Overall, the architecture provides a robust and efficient solution for user authentication and directory management in distributed computing environments.

## Before you begin
{: #before-you-begin}

Before you get started, be sure to review the following prerequisites: 

### General prerequisites
{: #general-prerequisites}

To successfully integrate OpenLDAP with {{site.data.keyword.symphony_full_notm}}, the following system requirements and prerequisites must be met:

1. OpenLDAP is installed on a dedicated server or the same server as {{site.data.keyword.symphony_full_notm}}.
2. {{site.data.keyword.symphony_full_notm}} is installed on a supported operating system with the latest patches and updates.
3. You have a user account with administrative privileges on both OpenLDAP and {{site.data.keyword.symphony_full_notm}}.
4. You are familiar with LDAP authentication and directory services.
5. You have access to the Symphony configuration file and the ability to modify it.

### Network prerequisites
{: #network-prerequisites}

To successfully integrate OpenLDAP with {{site.data.keyword.symphony_full_notm}}, the following network requirements must be met:

1. **Network connectivity between the OpenLDAP server and the {{site.data.keyword.symphony_full_notm}} nodes**: Ensure that the OpenLDAP server can communicate with the {{site.data.keyword.symphony_full_notm}} nodes over the network. This can be achieved by configuring the network settings on both the OpenLDAP server and the {{site.data.keyword.symphony_full_notm}} nodes.
2. **Port requirements**: The OpenLDAP server and the {{site.data.keyword.symphony_full_notm}} nodes must be able to communicate over specific ports. By default, OpenLDAP uses port 389 for unencrypted communication and port 636 for encrypted communication. {{site.data.keyword.symphony_full_notm}} uses port 7777 for communication between the client and the server.
3. **Firewall configuration**: If a firewall is present in the network, ensure that the necessary ports are open for communication between the OpenLDAP server and the {{site.data.keyword.symphony_full_notm}} nodes.
4. **DNS configuration**: Ensure that the {{site.data.keyword.symphony_full_notm}} nodes can resolve the hostname or IP address of the OpenLDAP server. If DNS resolution is not available, configure the `/etc/hosts` file on each node to include the OpenLDAP server's hostname and IP address.

## Configure an OpenLDAP server
{: #configure-openldap-server}
{: step}

Make sure that you should have access to a Linux&reg; system with root privileges.
{: important}

1. Install the OpenLDAP server and client packages by running the following command:

    ```
    yum -y install openldap-servers openldap-clients
    ```
    {: codeblock}

2. Copy the `DB_CONFIG.example` file to the `/var/lib/ldap` directory and change its ownership to the `ldap` user by running the following commands:

    ```
    cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG 
    chown ldap. /var/lib/ldap/DB_CONFIG
    ```
    {: codeblock}

3. Start the `slapd` service and enable it to start automatically at boot time by running the following commands:

    ```
    systemctl start slapd 
    systemctl enable slapd
    ```
    {: codeblock}

4. Generate an admin password by running the `slappasswd` command. You are prompted to enter a password. For example:

    ```
    slappasswd
    ```
    {: codeblock}

    You see output that looks something like the following:

    **Example output:**

    ```
    {SSHA}FUMV8TZ9lZQxABxCBE5UZ+oU/dlwf/d4
    ```
    {: screen}

    Note the password hash that is generated (in this case, `{SSHA}FUMV8TZ9lZQxABxCBE5UZ+oU/dlwf/d4`) as you need it later.

5. Create a file that is named `chrootpw.ldif` and add the following lines to it:

    ```
    dn: olcDatabase={0}config,cn=config
    changetype: modify
    add: olcRootPW
    olcRootPW: {SSHA}FUMV8TZ9lZQxABxCBE5UZ+oU/dlwf/d4
    ```
    {: codeblock}

    Replace the `olcRootPW` value with the password hash that you generated in the previous step.

6. Import the basic schema by running the following commands:

    ```
    ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif 
    ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif 
    ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
    ```
    {: codeblock}

7. Generate a manager password by running the `slappasswd` command again. For example:

    ```
    slappasswd
    ```
    {: codeblock}

    You see output that looks something like the following:

    **Example output:**

    ```
    {SSHA}TVW9z6WLIBC3EXtFHFWnb2EVlK7EZQ3b
    ```
    {: screen}

    Note the password hash that is generated (in this case, `{SSHA}TVW9z6WLIBC3EXtFHFWnb2EVlK7EZQ3b`) as you need it later.

8. Add the manager password and enable the manager account by creating a file that is named `chdomain.ldif` and adding the following lines to it:

    ```
    # DC should be your domain
    # specify the password generated above for "olcRootPW" section
    dn: olcDatabase={1}monitor,cn=config
    changetype: modify
    replace: olcAccess
    olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
      read by dn.base="cn=Manager,dc=ibmsymphony,dc=com" read by * none
    dn: olcDatabase={2}hdb,cn=config
    changetype: modify
    replace: olcSuffix
    olcSuffix: dc=ibmsymphony,dc=com
    dn: olcDatabase={2}hdb,cn=config
    changetype: modify
    replace: olcRootDN
    olcRootDN: cn=Manager,dc=ibmsymphony,dc=com
    dn: olcDatabase={2}hdb,cn=config
    changetype: modify
    add: olcRootPW
    olcRootPW: {SSHA}TVW9z6WLIBC3EXtFHFWnb2EVlK7EZQ3b
    dn: olcDatabase={2}hdb,cn=config
    changetype: modify
    add: olcAccess
    olcAccess: {0}to attrs=userPassword,shadowLastChange by
      dn="cn=Manager,dc=ibmsymphony,dc=com" write by anonymous auth by self write by * none
    olcAccess: {1}to dn.base="" by * read
    olcAccess: {2}to * by dn="cn=Manager,dc=ibmsymphony,dc=com" write by * read
    ```
    {: codeblock}

    Replace the {SSHA} value with the password hash that you generated in the previous step.

9. Apply the changes by running the following command:

    ```
    ldapmodify -Y EXTERNAL -H ldapi:/// -f chdomain.ldif
    ```
    {: codeblock}

10. Create a file that is named `basedomain.ldif` and add the following lines to it:

    ``` 
    dn: dc=ibmsymphony,dc=com
    objectClass: top
    objectClass: dcObject
    objectclass: organization
    o: WES Migration
    dc: ibmsymphony
    dn: cn=Manager,dc=ibmsymphony,dc=com
    objectClass: organizationalRole
    cn: Manager
    description: Directory Manager
    dn: ou=People,dc=ibmsymphony,dc=com
    objectClass: organizationalUnit
    ou: People
    dn: ou=Group,dc=ibmsymphony,dc=com
    objectClass: organizationalUnit
    ou: Group
    ```
    {: codeblock}

11. Apply the changes that are made in the previous step by running the following command:

    ```
    ldapadd -x -D cn=Manager,dc= ibmsymphony,dc=com -W -f basedomain.ldif
    ```
    {: codeblock}

12. Create a file that is named `ldapuser.ldif` by using an editor, such as VI, and add the following lines to it (replace with your own required domain name in the `"dc=***,dc=***"` section):

    ```
    dn: uid=symphonyuser03,ou=People,dc=ibmsymphony,dc=com
    objectClass: inetOrgPerson
    objectClass: posixAccount
    objectClass: shadowAccount
    cn: symphonyuser03
    sn: Linux
    userPassword: {SSHA}+A6gC87JU5ugW6qthWL2HGYzsQIdN1EN
    loginShell: /bin/bash
    uidNumber: 1003
    gidNumber: 1003
    homeDirectory: /home/symphonyuser03
    dn: cn=symphonyuser03,ou=Group,dc=ibmsymphony,dc=com
    objectClass: posixGroup
    cn: symphonyuser03
    gidNumber: 1003
    memberUid: symphonyuser03
    ```
    {: codeblock}

    Replace the {SSHA} value with the password hash that you generated earlier.

13. Apply the changes by running the following command:

    ```
    ldapadd -x -D cn=Manager,dc= ibmsymphony,dc=com -W -f ldapuser.ldif
    ```
    {: codeblock}

14. To verify whether users have been created as mentioned in the previous steps, use the following command:

    ```
    ldapsearch -x -LLL -b "ou=People,dc=ibmsymphony,dc=com" "(objectClass=posixAccount)" uid cn
    ```
    {: codeblock}

The OpenLDAP server is now configured and ready to use. You can add more users and groups by creating more LDIF files and by using the `ldapadd` command to import them into the directory.

## Create a user group in OLDAP directory
{: #create-user-group}
{: step}

Now that the OpenLDAP server is configured, you need to create a user group in the OLDAP directory, which consists of users who are supposed to get access to the LSF cluster.

1. To create a group, use the `ldapadd` command to add an LDIF entry for the group to the LDAP directory. For example, to create groups called "SymphonyAdmin" and "Symphonyconsumer", you can create an LDIF file with the following contents:

    ```
    # create an organizational unit for groups
    dn: ou=groups,dc=ibmsymphony,dc=com
    objectClass: organizationalUnit
    ou: groups

    # create a group called "SymphonyAdmin"
    dn: cn=SymphonyAdmin,ou=groups,dc=ibmsymphony,dc=com
    objectClass: top
    objectClass: posixGroup
    gidNumber: 1001
    cn: SymphonyAdmin
    description: SymphonyAdmin group

    # create a group called "Symphonyconsumer"
    dn: cn=Symphonyconsumer,ou=groups,dc=ibmsymphony,dc=com
    objectClass: top
    objectClass: posixGroup
    gidNumber: 1002
    cn: Symphonyconsumer
    description: Symphonyconsumer group
    ```
    {: codeblock}

    In this example, the first entry creates an organizational unit called "groups". The next two entries create groups that are called "SymphonyAdmin" and "Symphonyconsumer".

    Each group entry specifies the `objectClass` "top" and "posixGroup" to define the group's schema. The `gidNumber` attribute specifies the group's unique identifier, while the `cn` attribute specifies the group's common name. Finally, the `description` attribute provides a brief description of the group.

2. You can save the content from the previous step in a file with a `.ldif` extension, such as `groups.ldif`, and then use the `ldapadd` command to add these group entries to your LDAP directory. See the following example `ldapadd` command:

    ```
    ldapadd -x -D cn=Manager,dc=ibmsymphony,dc=com -W -f groups.ldif

    [root@oldapserverlsfcl ~]# ldapsearch -x -D cn=Manager,dc=ibmsymphony,dc=com -W -b "ou=groups,dc=ibmsymphony,dc=com" "(objectClass=posixGroup)"

    Enter LDAP Password:

    # extended LDIF
    #
    # LDAPv3
    # base <ou=groups,dc=ibmsymphony,dc=com> with scope subtree
    # filter: (objectClass=posixGroup)
    # requesting: ALL
    #

    # SymphonyAdmin, groups, ibmsymphony.com
    dn: cn=SymphonyAdmin,ou=groups,dc=ibmsymphony,dc=com
    objectClass: top
    objectClass: posixGroup
    gidNumber: 1001
    cn: SymphonyAdmin
    description: SymphonyAdmin group
    memberUid: symphonyuser02
    memberUid: symphonyuser01

    # Symphonyconsumer, groups, ibmsymphony.com
    dn: cn=Symphonyconsumer,ou=groups,dc=ibmsymphony,dc=com
    objectClass: top
    objectClass: posixGroup
    gidNumber: 1002
    cn: Symphonyconsumer
    description: Symphonyconsumer group
    memberUid: symphonyuser03
    memberUid: symphonyuser04

    # search result
    search: 2
    result: 0 Success
    ```
    {: codeblock}

This output shows that there are two group entries: "SymphonyAdmin" and Symphonyconsumer", both in the `ou=groups,dc=ibmsymphony,dc=com` organizational unit.

Later on, you can use the `ldapmodify` command to add exiting users to these groups. In the previous `ldapsearch` output, two users are seen as the member of each of the groups.

## Configure LDAP authentication on client machines
{: #configure-ldap-authentication}
{: step}

Next you need to configure the LDAP authentication for the client machines that are running RHEL 8 as the OS and OLDAP server as the authentication provider or identity provider. In this case, the OLDAP client is the Symphony cluster, so the following steps apply to the Symphony cluster nodes that are running on top of RHEL 8 Linux&reg; machines.

In the following steps, make sure to replace the domain name in the `"dc=***,dc=***"` section with your own domain name.
{: important}

1. Run the following commands:

    ```
    yum -y install openldap-clients nss-pam-ldapd
    ```
    {: codeblock}

    ```
    yum install authselect sssd oddjob oddjob-mkhomedir
    ```
    {: codeblock}

    ```
    authconfig --enablesssd --enablesssdauth --update
    ```
    {: codeblock}

2. Enable the SSSD authentication profile:

    ```
    authselect select sssd
    ```
    {: codeblock}

3. Add the LDAP server URL and the base search DN to the `/etc/openldap/ldap.conf` file as shown in the following example:

    ```
    URI ldap://ibmsymphony.com/ or IP address will also work
    BASE dc=ibmsymphony,dc=com
    ```
    {: codeblock}

4. In the `/etc/sssd` directory, create the file `sssd.conf` with the following contents:

    ```
    [domain/default]
    autofs_provider = ldap
    cache_credentials = True
    ldap_search_base = dc=ibmsymphony,dc=com
    id_provider = ldap
    auth_provider = ldap
    chpass_provider = ldap
    ldap_uri = ldap://<ip address of the LDAP Server>

    [sssd]
    services = nss, pam, autofs
    domains = default

    [nss]
    homedir_substring = /home
    ```
    {: codeblock}

    Update the `ldap_search_base` parameter with the base DN, and update `ldap_uri` with the URL to the LDAP server.

5. Change the permissions on the `/etc/sssd/sssd.conf` file:

    ```
    chmod 600 /etc/sssd/sssd.conf
    ```
    {: codeblock}

6. Restart and enable SSSD:

    ```
    systemctl restart sssd
    systemctl enable sssd
    ```
    {: codeblock}

You have now configured SSSD to use LDAP for user authentication on your RHEL 8 system with the LDAP server "ibmsymphony.com".

## Configure Symphony cluster
{: #configure-symphony-cluster}
{: step}

Now that you've configured the LDAP client authentication, you need to configure the Symphony cluster that is hosted on the LDAP client to point to the OLDAP server, which ensures that the integration of the Symphony cluster with OLDAP server is possible for user management.

The steps add the existing user “symphonyuser01” that's created in the OLDAP server to the Symphony cluster.
{: note}

To complete the following steps, you need to use a Symphony cluster user account with high privileges. Only an account with high privileges can add the users to the Symphony cluster and then assign roles to them. Howvever, if the newly added user is assigned the `CLUSTER_Admin` role, then that user can also complete the following steps for subsequently added users.
{: tip}

1. To add the existing user, “symphonyuser01”, which was created in the OLDAP server, to the Symphony cluster, you need to log in as the cluster administrator, and then run the following commands:

    ```
    [root@icgen2host-172-19-1-69 ~]# su symadmin
    bash-4.4$ egosh user logon -u Admin -x Admin
    Admin@HPCCluster> user add
    user account: symphonyuser01
    password:
    password(type again, please):
    User account < symphonyuser01 > added successfully
    Admin@HPCCluster> user logoff
    Logged off successfully
    ```
    {: codeblock}

2. Log in with the user that was added in the previous step to the Symphony cluster by running the following command:

    ```
    [root@hpcc-symphony-primary-0 ~]# su symphonyuser01
    [symphonyuser01@hpcc-symphony-primary-0 root]$ egosh
    egosh> user logon -u symphonyuser01 -x welcome2
    Logged on successfully
    symphonyuser01@HPCCluster> resource list
    NAME     status       mem    swp    tmp   ut    it    pg   r1m  r15s  r15m  ls
    hpcc-sy* ok           14G     0M    87G   3%  8200   0.0   0.1   0.1   0.1   0
    hpcc-sy* ok             -      -      -    -     -     -     -     -     -   -
    hpcc-sy* ok           14G     0M    86G   2%     0   0.0   0.1   0.2   0.0   1
    symphonyuser01@HPCCluster> ego info
    Cluster name            : HPCCluster
    EGO master host name    : hpcc-symphony-primary-0.ibm.com
    EGO master version      : 3.9
    symphonyuser01@HPCCluster>
    ```
    {: codeblock}

3. Assign roles to the new users by running the following commands:

    ```
    Admin@HPCCluster> user assignrole -u symphonyuser01 -r CONSUMER_USER
    Role <Consumer User> is assigned to user <symphonyuser01>.
    Admin@HPCCluster> user list
    ACCOUNT        PHONE          EMAIL          DESCRIPTION
    ----------------------------------------------------------------
    Admin
    Guest
    symphonyuser02
    symphonyuser01
    smith
    ```
    {: codeblock}

## Conclusion
{: #conclusion}

You have now successfully integrated OpenLDAP with {{site.data.keyword.symphony_full_notm}} to provide centralized authentication and directory services.




































