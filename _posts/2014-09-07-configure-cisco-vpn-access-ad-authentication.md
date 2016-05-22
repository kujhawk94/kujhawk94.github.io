---
layout: post
title:  "Configure Cisco ASA remote access VPN with Active Directory authentication"
date:   2014-07-19
categories: vpn
redirect_from: "/archives/90"
---
The following modification to the ASA device allows VPN users to authenticate against their Windows domain passwords on either `ldap1.domain.local`  or `ldap2.domain.local` . Before the authentication query is performed, the ASA connects to the domain via `cn=Windows Name, CN=Users, DC=domain, DC=local`  and the associated password. Valid VPN users are members of the AD security group `CN=VPN users,OU=Security,DC=domain,DC=local`.

NB: Correct LDAP distinguised names are a PITA to find.  The command line tool [dsquery][1] is extremely helpful.  For example, if the sAM account name (sAMAccountName) in question is `windowsname` , then the following will show its distinguished name:
    dsquery user -samid windowsname
The distinguished name for the security group could also be found with dsquery – note the quotation marks necessary to handle the group name that includes a space:
    dsquery group -samid "VPN users"

## AAA Server configuration

In the following configuration steps, replace the `192.168.x.x`  addresses with the addresses of the two LDAP servers.  The attribute map `ASAMAP`  determines which active directory security group's members are allowed to connect to the VPN.

    plainville-asa(config)# sh run aaa-server LDAP
    aaa-server LDAP protocol ldap
    aaa-server LDAP host 192.168.x.x
    ldap-base-dn dc=domain, dc=local
    ldap-scope subtree
    ldap-naming-attribute sAMAccountName
    ldap-login-password *
    ldap-login-dn cn=Windows Name, CN=Users, DC=domain, DC=local
    server-type microsoft
    ldap-attribute-map ASAMAP
    aaa-server LDAP host 192.168.x.x
    ldap-base-dn dc=domain, dc=local
    ldap-scope subtree
    ldap-naming-attribute sAMAccountName
    ldap-login-password *
    ldap-login-dn CN=Windows Name,CN=Users,DC=pmc,DC=local
    server-type microsoft
    ldap-attribute-map ASAMAP

## LDAP attribute map

Replace the value `ASAGroupPolicyName` with the VPN group policy which will use the LDAP authentication.

    plainville-asa(config)# sh run ldap 
    ldap attribute-map ASAMAP
    map-name memberOf cVPN3000-IETF-Radius-Class
    map-value memberOf "CN=VPN users,OU=Security,DC=domain,DC=local" ASAGroupPolicyName

## Resources
* [Remote Access VPN on ASA - Authentication using LDAP Server - AAA Identity and NAC][2]
* [Using your Active Directory for VPN authentication on ASA][3]
* [Cisco ASA Series Command Reference][4]
* [Cisco ASA: LDAP Authentication &amp; Authorization for VPN Clients][5]

[1]:http://technet.microsoft.com/en-us/library/cc725702.aspx
[2]:https://supportforums.cisco.com/document/139241/remote-access-vpn-asa-authentication-using-ldap-server
[3]:http://www.networkworld.com/article/2228531/cisco-subnet/using-your-active-directory-for-vpn-authentication-on-asa.html
[4]:http://www.cisco.com/c/en/us/td/docs/security/asa/asa82/command/reference/cmd_ref/s5.html
[5]:http://www.compressedmatter.com/guides/2010/8/19/cisco-asa-ldap-authentication-authorization-for-vpn-clients.html