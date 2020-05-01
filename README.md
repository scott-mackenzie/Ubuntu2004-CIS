Ubuntu 20.04 LTS - CIS STIG
================

[![Build Status](https://travis-ci.com/florianutz/Ubuntu2004-CIS.svg?branch=master)](https://travis-ci.com/florianutz/Ubuntu2004-CIS)
[![Ansible Role](https://img.shields.io/badge/role-florianutz.Ubuntu2004--CIS-blue.svg)](https://galaxy.ansible.com/florianutz/Ubuntu2004-CIS/)

Configure Ubuntu 20.04 LTS machine to be CIS compliant. Level 1 and 2 findings will be corrected by default.

This role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

## IMPORTANT INSTALL STEP

If you want to install this via the `ansible-galaxy` command you'll need to run it like this:

`ansible-galaxy install -p roles -r requirements.yml`

With this in the file requirements.yml:

```
- src: https://github.com/scott-mackenzie/Ubuntu2004-CIS.git
```

Based on [CIS Ubuntu Benchmark v1.0.0 - 08-13-2018 ](https://community.cisecurity.org/collab/public/index.php).

This repo originated from work done by [Florian Utz](https://github.com/florianutz/Ubuntu2004-CIS)

Requirements
------------

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.

Role Variables
--------------
There are many role variables defined in defaults/main.yml. This list shows the most important.

**Ubuntu2004cis_notauto**: Run CIS checks that we typically do NOT want to automate due to the high probability of breaking the system (Default: false)

**Ubuntu2004cis_section1**: CIS - General Settings (Section 1) (Default: true)

**Ubuntu2004cis_section2**: CIS - Services settings (Section 2) (Default: true)

**Ubuntu2004cis_section3**: CIS - Network settings (Section 3) (Default: true)

**Ubuntu2004cis_section4**: CIS - Logging and Auditing settings (Section 4) (Default: true)

**Ubuntu2004cis_section5**: CIS - Access, Authentication and Authorization settings (Section 5) (Default: true)

**Ubuntu2004cis_section6**: CIS - System Maintenance settings (Section 6) (Default: true)  

##### Disable all selinux functions
`Ubuntu2004cis_selinux_disable: false`

##### Service variables:
###### These control whether a server should or should not be allowed to continue to run these services

```
Ubuntu2004cis_avahi_server: false  
Ubuntu2004cis_cups_server: false  
Ubuntu2004cis_dhcp_server: false  
Ubuntu2004cis_ldap_server: false  
Ubuntu2004cis_telnet_server: false  
Ubuntu2004cis_nfs_server: false  
Ubuntu2004cis_rpc_server: false  
Ubuntu2004cis_ntalk_server: false  
Ubuntu2004cis_rsyncd_server: false  
Ubuntu2004cis_tftp_server: false  
Ubuntu2004cis_rsh_server: false  
Ubuntu2004cis_nis_server: false  
Ubuntu2004cis_snmp_server: false  
Ubuntu2004cis_squid_server: false  
Ubuntu2004cis_smb_server: false  
Ubuntu2004cis_dovecot_server: false  
Ubuntu2004cis_httpd_server: false  
Ubuntu2004cis_vsftpd_server: false  
Ubuntu2004cis_named_server: false  
Ubuntu2004cis_bind: false  
Ubuntu2004cis_vsftpd: false  
Ubuntu2004cis_httpd: false  
Ubuntu2004cis_dovecot: false  
Ubuntu2004cis_samba: false  
Ubuntu2004cis_squid: false  
Ubuntu2004cis_net_snmp: false  
```  

##### Designate server as a Mail server
`Ubuntu2004cis_is_mail_server: false`


##### System network parameters (host only OR host and router)
`Ubuntu2004cis_is_router: false`  


##### IPv6 required
`Ubuntu2004cis_ipv6_required: true`  


##### AIDE
`Ubuntu2004cis_config_aide: true`

###### AIDE cron settings
```
Ubuntu2004cis_aide_cron:
  cron_user: root
  cron_file: /etc/crontab
  aide_job: '/usr/sbin/aide --check'
  aide_minute: 0
  aide_hour: 5
  aide_day: '*'
  aide_month: '*'
  aide_weekday: '*'  
```

##### SELinux policy
`Ubuntu2004cis_selinux_pol: targeted`


##### Set to 'true' if X Windows is needed in your environment
`Ubuntu2004cis_xwindows_required: no`


##### Client application requirements
```
Ubuntu2004cis_openldap_clients_required: false
Ubuntu2004cis_telnet_required: false
Ubuntu2004cis_talk_required: false  
Ubuntu2004cis_rsh_required: false
Ubuntu2004cis_ypbind_required: false
```

##### Time Synchronization
```
Ubuntu2004cis_time_synchronization: chrony
Ubuntu2004cis_time_Synchronization: ntp

Ubuntu2004cis_time_synchronization_servers:
  - uri: "0.pool.ntp.org"
    config: "minpoll 8"
  - uri: "1.pool.ntp.org"
    config: "minpoll 8"
  - uri: "2.pool.ntp.org"
    config: "minpoll 8"
  - uri: "3.pool.ntp.org"
    config: "minpoll 8"
```  
##### 1.4.3 | PATCH | Ensure authentication required for single user mode
It is disabled by default as it is setting random password for root. To enable it set:
```yaml
Ubuntu2004cis_rule_1_4_3: true
```
To use other than random password:
```yaml
Ubuntu2004cis_root_password: 'new password'
```

##### 3.4.2 | PATCH | Ensure /etc/hosts.allow is configured
```
Ubuntu2004cis_host_allow:
  - "10.0.0.0/255.0.0.0"  
  - "172.16.0.0/255.240.0.0"  
  - "192.168.0.0/255.255.0.0"    
```  

```
Ubuntu2004cis_firewall: firewalld
Ubuntu2004cis_firewall: iptables
```

##### 5.3.1 | PATCH | Ensure password creation requirements are configured
```
Ubuntu2004cis_pwquality:
  - key: 'minlen'
    value: '14'
  - key: 'dcredit'
    value: '-1'
  - key: 'ucredit'
    value: '-1'
  - key: 'ocredit'
    value: '-1'
  - key: 'lcredit'
    value: '-1'
```


Dependencies
------------

Ansible >= 2.4 and <= 2.7 (2.8 is not yet supported)

Example Playbook
-------------------------

```
- name: Harden Server
  hosts: servers
  become: yes

  roles:
    - Ubuntu2004-CIS
```

To run the tasks in this repository, first create this file one level above the repository
(i.e. the playbook .yml and the directory `Ubuntu2004-CIS` should be next to each other),
then review the file `defaults/main.yml` and disable any rule/section you do not wish to execute.

Assuming you named the file `site.yml`, run it with:
```bash
ansible-playbook site.yml
```

Tags
----
Many tags are available for precise control of what is and is not changed.

Some examples of using tags:

```
    # Audit and patch the site
    ansible-playbook site.yml --tags="patch"
```

License
-------

MIT
