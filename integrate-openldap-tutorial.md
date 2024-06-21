---

copyright:
  years: 2023
lastupdated: "2023-10-19"

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

This tutorial covers configuring the OpenLDAP server on an RHEL 7.9 system.
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

Make sure that you have access to an RHEL 7.9 Linux&reg; system with root privileges.
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

14. To verify whether users were created, use this command:

    ```
    ldapsearch -x -LLL -b "ou=People,dc=ibmsymphony,dc=com" "(objectClass=posixAccount)" uid cn
    ```
    {: codeblock}

The OpenLDAP server is now configured and ready to use. You can add more users and groups by creating more LDIF files and by using the `ldapadd` command to import them into the directory.

## Create a user group in the OLDAP directory
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

This output shows that there are two group entries: "SymphonyAdmin" and "Symphonyconsumer", both in the `ou=groups,dc=ibmsymphony,dc=com` organizational unit.

Later on, you can use the `ldapmodify` command to add exiting users to these groups. In the previous `ldapsearch` output, two users are seen as the member of each of the groups.

## Configure LDAP authentication on client machines
{: #configure-ldap-authentication}
{: step}

Next, you need to configure the LDAP authentication for the client machines that are running RHEL 8.4/8.6 as the OS and OLDAP server as the authentication provider or identity provider. In this case, the OLDAP client is the Symphony cluster, so the following steps apply to the Symphony cluster nodes that are running on top of RHEL 8.4/8.6 Linux&reg; machines.

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
    enumerate = true
    access_provider = simple
    ldap_id_use_start_tls = false
    ldap_tls_reqcert = never

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

7. Run the following command to enable the `mkhomedir` feature:

    ```
    sudo authselect enable-feature with-mkhomedir
    ```
    {: codeblock}

8. Verify that `mkhomedir` is enabled:

    ```
    [root@icgen2host-10-240-0-39 ~]# authselect current
    Profile ID: sssd
    Enabled features:
    - with-mkhomedir
    ```
    {: codeblock}

9. Enable and restart the `oddjobd.service`:

    ```
    systemctl enable --now oddjobd.service
    systemctl restart oddjobd.service
    ```
    {: codeblock}

SSSD is configured to use LDAP for user authentication on your RHEL 8.4/8.6 system with the LDAP server "ibmlsf.com".

## Configure Symphony cluster
{: #configure-symphony-cluster}
{: step}

After you configure the LDAP client authentication at the OS layer, you need to configure the cluster that is hosted on the LDAP client to completely integrate the Symphony cluster with the OLDAP server. This involves configuring authentication for PAM and default clients that use an authentication plug-in, which enables user management for the Symphony cluster.

### Configure user authentication for PAM and default client
{: #configure-user-auth}

1. Stop the {{site.data.keyword.symphony_short}} cluster by using the `egoshutdown.sh` command. To run the shutdown script, you need to log in to the cluster with the admin account:

    ```
    #$EGO_TOP/profile.platform
    egosh user logon -u Admin -x Admin_password
    soamcontrol app disable all
    egosh service stop all
    egosh ego shutdown all
    ```
    {: codeblock}

2. On all management hosts, create a plug-in configuration file that is named `pamauth.conf` in the `$EGO_CONFDIR` directory. If it exists, you can skip this step. Replace `/$EGO_CONFDIR /pamauth.conf` with the actual path to your configuration file. See the following example:

    ```
    vi /$EGO_CONFDIR/pamauth.conf
    ```
    {: codeblock}

3. Ensure a PAM service file (default PAM service is `sshd`) exists in the `/etc/pam.d/` directory. If the service file doesn't exist, create it and assign 644 permissions. You can skip this step if the service file exists.

    ```
    #Example for sshd service file:
    touch /etc/pam.d/sshd
    chmod 644 /etc/pam.d/sshd
    ```
    {: codeblock}

4. Edit the `pamauth.conf` file and set the values of mandatory and optional parameters. You can specify parameters like PAM_SERVICE, KEYFILE, PAM_CACHEEXPIRYTIME, INCLUDED_USERGROUP, EXCLUDED_USERGROUP, FOLLOW_GETENT_GROUP, and SEC_PAM_BYPASS. See the following example of `pamauth.conf` parameters:

    ```
    PAM_SERVICE=sshd
    KEYFILE=/tmp/seckey.conf
    PAM_CACHEEXPIRYTIME=2h
    INCLUDED_USERGROUP=Developers,Administrators
    FOLLOW_GETENT_GROUP=Y
    SEC_PAM_BYPASS=N
    ```
    {: codeblock}

5. Edit the `$EGO_CONFDIR/ego.conf` file on the management and compute hosts:

   a. On the management hosts, edit the `ego.conf` file to modify the `EGO_SEC_PLUGIN` and `EGO_SEC_CONF` parameters with the following value:

      ```
      EGO_SEC_PLUGIN=sec_ego_pam_default
      EGO_SEC_CONF=$EGO_CONFDIR,0,INFO,$EGO_TOP/kernel/log
      ```
      {: codeblock}

   b. On the compute (client) hosts, edit the `ego.conf` file to modify the `EGO_SEC_PLUGIN` and `EGO_SEC_CONF` parameters with the following value:

      ```
      EGO_SEC_PLUGIN= sec_ego_ext_co (for PAM client)
      EGO_SEC_PLUGIN=sec_ego_default (for “default client” which is ego user)
      EGO_SEC_CONF=$EGO_TOP/kernel/conf,0,DEBUG,$EGO_TOP/kernel/log
      ```
      {: codeblock}

6. Log on to the primary host as the cluster administrator and start the cluster. Use the following commands based on your shell (`bash` or `csh`):

    For `bash`:

    ```bash
    #$EGO_TOP/profile.platform
    #egosh ego start all
    #soamcontrol app enable application_name
    ```
    {: codeblock}

    For `csh`:

    ```csh
    #source $EGO_TOP/cshrc.platform
    #egosh ego start all
    #soamcontrol app enable application_name
    ```
    {: codeblock}

7. If the authentication server is required to authenticate a PAM client by using the default or PAM authentication method, log on to `EGO` as the cluster administrator (for example, Admin) and run the `egosh user add` command to map the PAM user to the `EGO` account.

### Verify user authentication for PAM and default clients
{: #verify-user-auth}

1. To verify user authentication, users need to log on by using `soamlogon` or `egosh user logon`. However, in this case, you can first retrieve the list of users from the LDAP server as shown in the following example:

    ```
    [root@pp-hpcc-sym-primary-0 ~]# ldapsearch -x -D "cn=Manager,dc=ibmlsf,dc=com" -H ldap://149.81.15.40 -W -b "ou=people,dc=ibmlsf,dc=com" "(objectClass=posixAccount)" | grep "cn:"
    Enter LDAP Password:
    cn: usrsupport
    cn: usroprt
    cn: user123
    cn: user03
    cn: user05
    cn: user007
    cn: symphuser007

    [root@pp-hpcc-sym-primary-0 ~]# soamlogon
    user account:symphuser007
    password:
    Logged on successfully
    [root@pp-hpcc-sym-primary-0 ~]# egosh
    symphuser007@HPCCluster>
    ```
    {: screen}

In the example, the LDAP user “symphuser007” is able to log on to the Symphony cluster.

2. To add the LDAP user "symphuser007" to the Symphony Cluster, see the following example:

    ```
    Admin@HPCCluster> user add
    user account: symphuser007
    password:
    password(type again, please):
    User account < symphuser007 > added successfully
    Admin@HPCCluster> user logoff
    Logged off successfully

    Admin@HPCCluster> user list
    ACCOUNT        DESCRIPTION
    ----------------------------------------------------------------
    symphuser007
    adm           	 adm
    Admin
    bin           	 bin
    chrony
    cockpit-ws     	  User for cockpit we*
    cockpit-wsins* User for cockpit-ws*
    daemon         	daemon
    sssd           	User for sssd
    nscd           	NSCD Daemon
    nslcd          	LDAP Client User
    dbus           	System message bus
    egoadmin
    ftp           	FTP User
    games          	games
    halt           	halt
    lp             	lp
    lsfuser123
    lsfoprt
    mail           	mail
    nobody         	Kernel Overflow User
    operator       	operator
    polkitd        	User for polkitd
    postgres       	PostgreSQL Server
    rngd           	Random Number Gener*
    root           	root
    rpc            	Rpcbind Daemon
    rpcuser        	RPC Service User
    scalemgmt      IBM Spectrum Scale
    scalepm        	IBM Spectrum Scale *
    setroubleshoot
    shutdown       shutdown
    sshd           	Privilege-separated*
    sync           	sync
    systemd-cored* systemd Core Dumper
    systemd-resol* systemd Resolver
    tcpdump
    tss            	Account used for TP*
    unbound        	Unbound DNS resolver
    vpcuser        	VPC Cloud User
    @adm
    @audio
    @bin
    @cdrom
    @chrony
    ```
    {: screen}

3. To log in with the LDAP user "symphuser007" on the primary cluster node and run Symphony cluster commands, see the following example:

    ```
    [symphuser007@pp-hpcc-sym-primary-0 root]$ egosh
    egosh> user logon
    user account:symphuser007
    password:
    Logged on successfully
    symphuser007@HPCCluster> resource list
    NAME     status       mem    swp    tmp   ut    it    pg   r1m  r15s  r15m  ls
    hpcc-sy* ok           14G     0M    87G   3%  8200   0.0   0.1   0.1   0.1   0
    hpcc-sy* ok             -      -      -    -     -     -     -     -     -   -
    hpcc-sy* ok           14G     0M    86G   2%     0   0.0   0.1   0.2   0.0   1
    symphuser007@HPCCluster> ego info
    Cluster name            : HPCCluster
    EGO master host name    : pp-hpcc-sym-primary-0.ibm.com
    EGO master version      : 3.9
    symphuser007@HPCCluster>
    ```
    {: screen}

## Conclusion
{: #conclusion}

OpenLDAP is integrated with {{site.data.keyword.symphony_full_notm}} to provide centralized authentication and directory services.
