---

copyright:
  years: 2022, 2025
lastupdated: "2025-03-19"

keywords:

subcollection:  hpc-spectrum-symphony

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

# Enabling User Management for IBM Spectrum Symphony using Windows Active Directory
{: #integrate-scale-ad-auth-tut}

## Introduction
{: #scale-ad-auth-intro}

You can configure {{site.data.keyword.symphony_full_notm}} to use Active Directory (AD) server as the primary directory service for user authentication. The document outlines step-by-step procedures for installing and configuring Active Directory and DNS Server on Windows Server 2019 using PowerShell. Also, it covers the process of connecting RHEL 8.4 OS-based Symphony Cluster node directly to AD by using Samba Winbind to ensure encryption compatibility, and joining it to an Active Directory domain.

The purpose of this documentation is to enable system administrators to seamlessly configure {{site.data.keyword.symphony_full_notm}} with Active Directory for user authentication, allowing for efficient user management and access control.

## General prerequisites
{: #gen-prerequisites}

Before you proceed with the configuration process, ensure that the prerequisites are met:

1.	For Installing and Configuring Active Directory and DNS Server:
	*  A Windows Server 2019 system with administrative privileges.
	*  Basic familiarity with PowerShell commands and Windows Server management.
2.	For Connecting RHEL Systems Directly to AD using Samba Winbind:
	*  An RHEL system capable of running Samba Winbind.
	*  Familiarity with RHEL command-line interface and system configuration.
	*  Access to the Active Directory domain and appropriate permissions to join the domain.

## Network prerequisites
{: #network-prereq}

Before you proceed with the configuration of IBM Storage Scale to use Active Directory and connecting RHEL systems to AD, ensure that the following network prerequisites are met:

1.	Network connectivity: You need stable and reliable network connectivity between the Windows Server 2019 system (where Active Directory and DNS are installed) and the RHEL systems that are joined to the domain. Verify that no network communication issues or firewalls are set up that block essential ports.

2.	Firewall rules: Review and update firewall rules to allow the necessary communication between the Windows Server 2019 system, RHEL systems, and the domain controllers. Key ports that are used for AD communication include TCP/UDP 53 (DNS), TCP/UDP 88 (Kerberos), TCP 135 (RPC), TCP/UDP 389 (LDAP), TCP/UDP 445 (SMB), and TCP/UDP 636 (LDAPS).

3.	Domain controller reachability: Confirm that the RHEL systems can reach the Active Directory domain controllers without any connectivity issues. Use tools like "ping" or "nslookup" to verify the ability to resolve the domain controller's hostname and IP address from the RHEL systems.

4.	Time synchronization: Ensure that all systems that are participating in the Active Directory domain, including the Windows Server 2019 system and RHEL systems, have their clocks that are synchronized with a reliable time source. Time synchronization is critical for proper authentication and Kerberos ticket validation.

5.	Domain DNS configuration: The Active Directory domain must be properly configured DNS settings. The domain controller's IP address must be set as the primary DNS server on all systems (including the Windows Server 2019 system and RHEL systems) that are part of the AD domain.

6.	DNS resolution: Verify that both forward and reverse DNS resolutions are functioning correctly. The domain controller's hostname must be resolvable from the Windows Server 2019 system and RHEL systems, and the Windows Server 2019 system hostname must be resolvable from the RHEL systems.

7.	DNS domain name: Ensure that the DNS domain name of the Active Directory matches the domain name that is used during the configuration process. In this case, the domain name "POCDOMAIN.LOCAL" used for the Active Directory must be consistent throughout the configuration.

8.	Active Directory user account with administrative privileges: Ensure that an Active Directory user account with administrative privileges is available to be used during the configuration process. This account is used to promote the Windows Server 2019 system as a domain controller and to join the RHEL systems to the AD domain.

### Windows Server and Powershell
{: #windows-server-powershell-prereq}

For this procedure you need:
* A Windows Server 2019 system with administrative privileges.
* Basic familiarity with PowerShell commands and Windows Server management.

## Step 1 -  Installing and Configuring Active Directory and DNS Server on Windows Server 2019 using PowerShell
{: #step-1-inst-conf-ad-dns}

1. Open PowerShell with Administrative Privileges:

    a. Press the "Windows + X" keys on your keyboard to access the Power User menu.

    b. From the list, choose "Windows PowerShell (Admin)" to start an elevated PowerShell session with administrative privileges.

2. Install the Active Directory Domain Services Role and DNS Server Role by running these PowerShell commands:

   ```
   powershell code
   # Install the Active Directory Domain Services Role and DNS Server Role
    Install-WindowsFeature -Name AD-Domain-Services, DNS -IncludeManagementTools
    ```

3. Promote the Server to a Domain Controller with Integrated DNS by running these PowerShell commands:

   ```
   powershell code
   # Set the required parameters
   $DomainName = "POCDOMAIN.LOCAL"
   $SafeModePassword = ConvertTo-SecureString -AsPlainText "MySecureDSRMpwd2023!" -Force

   # Configure and promote the server as a domain controller with integrated DNS
   Install-ADDSForest -DomainName $DomainName -SafeModeAdministratorPassword  $SafeModePassword -DomainMode Win2019 -ForestMode Win2019 -InstallDNS
   ```

   Example values:
   *  Domain Name: POCDOMAIN.LOCAL
   *  DSRM Password: MySecureDSRMpwd2023!

   When you run the PowerShell commands, the Active Directory Domain Services and DNS Server roles are installed and configured on the server.

   The server automatically restarts to complete the domain controller promotion process.

4. After the server restarts, log in using the domain administrator account that you created during the promotion process to verify Active Directory and DNS Configuration.

### Verify Active Directory and DNS Configuration
{: #verfy-dns-ad-config}

Verify that Active Directory and DNS are configured correctly by checking:

1. Verify Active Directory Management

   Open "Server Manager", and in the "Dashboard", confirm "Active Directory Users and Computers" and "DNS" are listed under "Tools", indicating successful installation of the Active Directory and DNS management tools.

2. Start Active Directory Users and Computers to manage user accounts, groups, and organizational units (OUs) within the domain.
3. Access DNS Manager to manage DNS zones and records for the domain.
4. On a client system within the same network, configure the DNS settings to point to the IP address of the newly promoted domain controller.
5.  Attempt to join the client system to the "POCDOMAIN.LOCAL" domain. A successful connection confirms proper DNS resolution and functional Active Directory domain services.

## Step 2 - Creating a user group and users in Active Directory for users that access the Symphony cluster
{: #create-user-group-ad-symphony}

Create a user group named "Symphony-group" and a user named "Symphonyuser01" in the Active Directory domain "pocdomain.local" using the Active Directory Users and Computers (ADUC) management tool on a Windows Server:

Creating user groups and users in the "pocdomain.local" Active Directory domain requires administrative privileges within that domain. Ensure that you have the necessary permissions to create groups and users. Also, always set strong passwords and follow security best practices when you create user accounts and groups in Active Directory to maintain a secure network environment.
{: note}

1. Open Active Directory Users and Computers:

    a. Log in to the Windows Server with administrative privileges.

    b. Click Start and search for "Active Directory Users and Computers."

    c. Start the "Active Directory Users and Computers" management console.

2. Create a User Group ***Symphony-group***:

    a. In the ADUC console, navigate to the Organizational Unit (OU) within the "pocdomain.local" domain where you want to create the user group.

    b. Right-click on the OU and select New and then Group.

    c. In the New Object - Group dialog box, enter Symphony-group as the Group name.

    d. Choose the Group scope (for example, Global) and Group type (for example, Security).

    e. Click OK to create the ***Symphony-group*** user group.

3. Create a User ***Symphonyuser01***:

    a. In the ADUC console, navigate to the same or a different Organizational Unit (OU) within the "pocdomain.local" domain where you want to create the user "Symphonyuser01."

    b. Right-click on the OU and select New and then User.

    c. In the New Object - User dialog box, enter the required information for the new user **symphonyuser01*:

      * Full name: Enter the user's full name (for example, LSF User 01).

      * User logon name: Enter the user's logon name (for example, Symphonyser01).

      * User Principal Name (UPN): The UPN is automatically generated based on the user logon name and the domain name (for example, Symphonyuser01@pocdomain.local).

      * Password: Set a secure password for the user account and choose whether the user must change the password on the first logon.

      * User cannot change the password: Check this option if you want to prevent the user from changing their password.

      * Password never expires: Check this option if you want the user's password to never expire.

      * Account is disabled: By default, the account is enabled. If you want to create the user account in a disabled state, clear this option.

    d. Click Next to continue through any additional wizard steps.

    e. Review the information entered, and click Finish to create the new user "Symphonyuser01."

4.  Add "Symphonyuser01" to "Symphony-group" User Group:

    a. In the ADUC console, locate the ***Symphony-group*** user group that you created earlier.

    b. Right-click on the ***Symphony-group*** user group and select Properties**.
    c.  In the Properties dialog box, go to the **Members** tab.

    d. Click Add and then enter ***Symphonyuser01*** in the **Enter the object names to select** field.

    e. Click Check Names to validate the username.

    f. Click OK to add "Symphonyuser01" to the "Symphony-group" user group.

    g.  Click Apply and then **OK** to save the changes.


## Step 3 - Integrating Symphony Cluster running on RHEL 8.4 based Systems Directly to AD using Samba Winbind
{: #scale-integrate-symph-cluster-ad-samba}

### Overview of Direct Integration by using Samba Winbind
{: #integrate-symph-samba-intro}

To connect an RHEL system to Active Directory (AD), two components are needed: Samba Winbind and realmd. Samba Winbind interacts with the AD identity and authentication source, while realmd detects available domains and configures the underlying RHEL system services.

### Supported Windows Platforms and Operating systems for Direct Integration
{: #supported-windows-platforms}

Direct integration with AD forests is compatible with the following forest and domain functional levels:
*  Forest functional level range: Windows Server 2008 - Windows Server 2016
*  Domain functional level range: Windows Server 2008 - Windows Server 2016

Supported operating systems for direct integration include:
*  Windows Server 2022 (RHEL 8.7 and above)
*  Windows Server 2019
*  Windows Server 2016
*  Windows Server 2012 R2

Windows Server 2019 and Windows Server 2022 do not introduce new functional levels and use the highest functional level of Windows Server 2016.
{: note}

### Ensuring Support for Common Encryption Types in AD and RHEL
{: #encryption-types-ad-rhel}

Samba Winbind supports RC4, AES-128, and AES-256 Kerberos encryption types by default. However, RC4 encryption is deprecated and disabled by default due to security considerations. AD user credentials and trusts can still rely on RC4 encryption, leading to authentication issues.

To ensure compatibility, you have two options:
*  Enable AES encryption support in Active Directory.
*  Enable RC4 support in RHEL.

For enabling RC4 support in RHEL, the steps differ depending on the RHEL version. It is recommended to refer to the official documentation for detailed instructions.

### Joining Symphony cluster with Symphony cluster node
{: #procedure-ad-samba}

Samba Winbind is an alternative to the System Security Services Daemon (SSSD) for connecting a Red Hat Enterprise Linux (RHEL) system with Active Directory (AD). This section describes how to join an RHEL system to an AD domain by using realmd to configure Samba Winbind.

Join a Symphony Cluster node that is hosted on RHEL 8.4 OS to an AD domain using Samba Winbind and `realmd`:

1. Install and update the following packages:

    ```
    # yum install realmd oddjob-mkhomedir oddjob samba-winbind-clients \
    samba-winbind samba-common-tools samba-winbind-krb5-locator

    #yum update
    ```

2. Updating the /etc/hosts with adding AD domain IP and name.  For example:

    ```
    [root@amit-rhel84 ~]# cat /etc/hosts
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    10.243.0.41 addc1.POCDomain.local
    ```

    addc1.POCDomain.local is the AD server FQDN name
    {: note}

3. Update the DNS entries in `/etc/resolv.conf` file by using:

    ```
    sudo nmcli connection modify "System eth0" ipv4.dns "10.243.0.41" ipv4.ignore-auto-dns yes
    ```

    This command not only updates the DNS entries in the `/etc/resolv.conf` file but also ensures that the DNS entries are not auto-updated to cloud-based DNS servers

4. Confirm the changes in the DNS file:

    ```
    [root@amit-rhel84 ~]# cat /etc/resolv.conf
    # Generated by NetworkManager
    nameserver 10.243.0.41
    [root@amit-rhel84 ~]#
    ```

    In addition to the confirmation, ping the Domain Controller with Name : - Ping POCDOMAIN.LOCAL shell:

    ```
    [root@amit-rhel84 ~]# ping POCDOMAIN.LOCAL
    PING POCDOMAIN.LOCAL (10.243.0.41) 56(84) bytes of data.
    64 bytes from addc1.POCDomain.local (10.243.0.41): icmp_seq=1 ttl=128 time=0.365 ms
    64 bytes from addc1.POCDomain.local (10.243.0.41): icmp_seq=2 ttl=128 time=0.722 ms
    64 bytes from addc1.POCDomain.local (10.243.0.41): icmp_seq=3 ttl=128 time=0.581 ms
    64 bytes from addc1.POCDomain.local (10.243.0.41): icmp_seq=4 ttl=128 time=0.525 ms
    ```

5.  Use `nslookup` to ensure that the AD domain is resolvable:

    ```
    [root@amit-rhel84 ~]#  nslookup pocdomain.local
    Server:         10.243.0.41
    Address:        10.243.0.41#53

    Name:   pocdomain.local
    Address: 10.243.0.41
    ```

6.  If your Active Directory requires the deprecated RC4 encryption type for Kerberos authentication, enable support for these ciphers in RHEL:

    `# update-crypto-policies --set DEFAULT:AD-SUPPORT`

    After you run this command, it updates the crypto policies and asks to reboot the system.

7.  Back up the existing /etc/samba/smb.conf Samba configuration file:

    `# mv /etc/samba/smb.conf /etc/samba/smb.conf.bak`

8.  Join the RHEL 8.x host to the Active Directory domain. As mentioned in the example to join a domain named POCDOMAIN.LOCAL:

    `# realm join --membership-software=samba --client-software=winbind POCDOMAIN.LOCAL`

    When you use this command, the realm utility automatically:
    - Creates a /etc/samba/smb.conf file for a membership in the pocdomain.local domain.
    - Adds the winbind module for user and group lookups to the /etc/nsswitch.conf file.
    - Updates the Pluggable Authentication Module (PAM) configuration files in the /etc/pam.d/ directory.
    - Starts the winbind service and enables the service to start when the system boots.

9.  (Optional) Set an alternative ID mapping back end or customized ID mapping settings in the /etc/samba/smb.conf file. For more information, see the [Understanding and configuring Samba ID mapping](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/deploying_different_types_of_servers/assembly_using-samba-as-a-server_deploying-different-types-of-servers){: external}.

10.  Edit the `/etc/krb5.conf` file and add this section:

        ```shell
        [plugins]
        localauth = {
        module = winbind:/usr/lib64/samba/krb5/winbind_krb5_localauth.so
        enable_only = winbind
        }
        ```

11.  Verify that the winbind service is running. For example:


        ```
        #systemctl status winbind
        [root@amit-rhel84 ~]# systemctl status winbind
        winbind.service - Samba Winbind Daemon
        Loaded: loaded (/usr/lib/systemd/system/winbind.service; enabled; vendor preset: disabled)
        Active: active (running) since Wed 2023-08-09 08:16:16 EDT; 1h 42min ago
        Docs: man:winbindd(8)
          man:samba(7)
          man:smb.conf(5)
        Main PID: 4889 (winbindd)
        Status: "winbindd: ready to serve connections..."
        Tasks: 5 (limit: 49264)
        Memory: 10.9M
        CGroup: /system.slice/winbind.service
               ├─4889 /usr/sbin/winbindd --foreground --no-process-group
               ├─4893 /usr/sbin/winbindd --foreground --no-process-group
               ├─4894 /usr/sbin/winbindd --foreground --no-process-group
               ├─4924 /usr/sbin/winbindd --foreground --no-process-group
               └─5997 /usr/sbin/winbindd --foreground --no-process-group
        ```

To enable Samba to query domain user and group information, the winbind service must be running before you start smb.
{: important}

If you installed the samba package to share directories and printers, enable and start the smb service:

`# systemctl enable --now smb`

### Verification steps
{: #verification-steps}

1.  Display AD users details, such as the AD administrator account in the AD domain. For example:

    ```
    # getent passwd " POCDOMAIN\administrator"
    POCDOMAIN\administrator:*:2000500:2000513::/home/administrator@POCDOMAIN:/bin/bash
    ```
2.  Query the members of the domain users group in the AD domain:

    ```
    # getent group " POCDOMAIN\Domain Users"
    POCDOMAIN\domain users:x:2000513:
    ```

3.  (Optional) Verify that you can use domain users and groups when you set permissions on files and directories. For example, to set the owner of the /srv/samba/example.txt file to AD\administrator and the group to AD\Domain Users:

    ```
    # sudo chown "POCDOMAIN\administrator":"POCDOMAIN\Domain Users" example.txt
    ```

4.  Verify that Kerberos authentication works as expected. On the AD domain member, obtain a ticket for the administrator@POCDOMAIN.LOCAL principal:
    ```
    # kinit administrator@POCDOMAIN.LOCAL
    ```

5.  Display the cached Kerberos ticket:

    ```
    # klist
	Ticket cache: KCM:0
	Default principal: Administrator@POCDOMAIN.LOCAL

	Valid starting       Expires              Service principal
	12/11/2023 08:26:47  12/11/2023 18:26:47  krbtgt/POCDOMAIN.LOCAL@POCDOMAIN.LOCAL
		renew until 12/18/2023 08:26:23

    ```
6.  Display the available domains:

    ```
	#realm list
	pocdomain.local
	  type: kerberos
	  realm-name: POCDOMAIN.LOCAL
	  domain-name: pocdomain.local
	  configured: kerberos-member
	  server-software: active-directory
	  client-software: winbind
	  required-package: oddjob-mkhomedir
	  required-package: oddjob
	  required-package: samba-winbind-clients
	  required-package: samba-winbind
	  required-package: samba-common-tools
	  login-formats: POCDOMAIN\%U
	  login-policy: allow-any-login
	#wbinfo --all-domains
	BUILTIN
	AD-CLIENT
	POCDOMAIN
    ```
Additional resources - If you do not want to use the deprecated RC4 ciphers, you can enable the AES encryption type in AD.

### To provide root user permissions to Active Directory (AD) users
{: #provide-root-permission}

To provide root user permissions to AD users of "POCDOMAIN.LOCAL" domain on a Linux system:
1.  Open a terminal or connect to the Linux system.
2.  Edit the sudoers file by using the visudo command:
    `sudo visudo`
3.  Locate the section in the sudoers file that configures user privileges:

    `# Allow root to run any commands anywhere root ALL=(ALL) ALL`

4.  Add the following line below the root user entry to allow AD users in the "POCDOMAIN\Domain Administrators" group to run commands with root privileges:
    `%POCDOMAIN\\Domain\ Administrators ALL=(ALL) ALL`

    This line grants root user privileges to all AD users in the "POCDOMAIN\Domain Users" group. The double backslashes ("") are used to escape special characters.
5.  Save the changes to the sudoers file and exit the editor.
    AD users who are members of the "POCDOMAIN\Domain Users" group can now run commands with root privileges by using the sudo command. For example:
    `sudo command_to_execute_as_root`

    When prompted, the AD user must enter their own password to authenticate and run the command with elevated privileges.

   Exercise caution when you grant root user permissions to AD users. It's important to grant these privileges only to trusted individuals who require them for specific tasks. Regularly reviewing user privileges and following security best practices helps maintain a secure system environment.
{: important}

## Step 4 - Configuring setup on the Symphony cluster side
{: #configure-symphony-cluster-side}

Following are the steps to configure Kerberos Authentication on Linux Hosts for {{site.data.keyword.symphony_full_notm}} Integration with Active Directory:

1. Shutdown the Cluster

    Log in to any management host in the cluster by using the cluster administrator credentials.

    ```
    #Example
    egosh user logon –u Admin –x Admin
    ```

    Run the following commands to gracefully shut down the cluster:

    ```
    #Example
    soamcontrol app disable all
    egosh service stop all
    egosh ego shutdown all
    ```

2. Modify Security Configuration on Management Hosts:
    On each management host, edit the `$EGO_CONFDIR/ego.conf` file.

    Set EGO_SEC_PLUGIN to "sec_ego_gsskrb."

    ```
    #Example
    EGO_SEC_PLUGIN=sec_ego_gsskrb
    ```

    Set EGO_SEC_CONF to specify the plug-in's configuration:

    ```
    #Example
    EGO_SEC_CONF="/EGOShare/kernel/conf,600,ERROR,/opt/EGO/kernel/log"
    ```

    Set EGO_SEC_KRB_SERVICENAME to the Kerberos principal for the authentication server:

    ```
    #Example
    EGO_SEC_KRB_SERVICENAME=symphonyuser03
    ```

    Create or modify the $EGO_CONFDIR/sec_ego_gsskrb.conf file with the provided parameters:

    ```
    #Example
    REALM=POCDomain.local
    KRB5_KTNAME=/etc/krb5.keytab
    KERBEROS_ADMIN=egoadmin
    ```

    KRB5_KTNAME=/etc/krb5.keytab
    The KRB5_KTNAME parameter designates the absolute path to the keytab file. This file contains keys for the service principal that is used in Kerberos authentication. Here, the path is set to "/etc/krb5.keytab," ensuring that the necessary keys are retrieved for authentication.
    {: note}

    KERBEROS_ADMIN=egoadmin
    The KERBEROS_ADMIN parameter specifies the Kerberos principal that maps to the user name of the built-in cluster administrator (Admin).
    In this configuration, it is set to "hpcegoadmin." This principal is significant for administrative tasks within the {{site.data.keyword.symphony_full_notm}} cluster and is used for authenticating as the cluster administrator.
    {: note}

3. Apply Configuration to Compute and Client Hosts.

    Repeat the modification steps on compute and client hosts, adjusting configurations in their respective `$EGO_CONFDIR/ego.conf` and `$EGO_CONFDIR/sec_ego_gsskrb.conf` files.

4. Start the Cluster and Enable Applications.

    Once configurations are applied across hosts, start the cluster and enable applications by using the following commands:

    ```
    #Example
    egosh ego start
    soamcontrol app enable appName
    ```
5. Log on to Cluster Management Host.

    From a management host in the POCDomain.local realm, log in as the Admin user by using the egosh command-line. Enter the password of the Kerberos principal defined in the KERBEROS_ADMIN parameter.

    ```
    #Example
    egosh user logon –u Admin –x passwordegoadmin
    ```

6. Add AD Users (If Required)

    If the ENABLE_AD_USERS_MANAGE parameter in the `$EGO_CONFDIR/sec_ego_gsskrb.conf` file on Linux management hosts was set to 'N,' use the egosh user add command to add AD users to {{site.data.keyword.symphony_full_notm}}. Provide any random string as the password; it will not be used for user logon.

    ```
    #Example egosh user add –u ad1tester –x 111
    ```

7. Assign Roles to Users

    Assign roles to the user accounts by using the egosh user assignrole command. For example, to assign the consumer admin role to ad1tester and ad2tester:

    ```
    #Example
    egosh user assignrole –u ad1tester –r CONSUMER_ADMIN –p /SymTesting/Symping73.2
    ```

8. Login as an AD user

    Log in as an AD user by using the egosh command line. For example, to log in as ad1tester:

    ```
    #Example
    egosh user logon –u ad1tester –x passwordad1tester
    ```

## Conclusion
{: #conclusion}

This guide serves as a vital resource for system administrators looking to seamlessly integrate {{site.data.keyword.symphony_full_notm}} with Active Directory within the {{site.data.keyword.cloud_notm}} environment. Addressing prerequisites and delivering step-by-step instructions, it ensures readiness for both Active Directory and RHEL systems that are deployed on IBM Virtual Servers. From installing Active Directory on Windows Server 2019 to configuring Kerberos authentication, the guide provides administrators with a reliable reference to achieve efficient user management and access control for {{site.data.keyword.symphony_full_notm}} within the {{site.data.keyword.cloud_notm}} infrastructure.
