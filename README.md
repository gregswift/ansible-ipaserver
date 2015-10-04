ipaserver
=========

A simple, but fairly parameterized, role for setting up a FreeIPA server. Primarily tested on Fedora.

Requirements
------------

Primarily tested and functional on Fedora, but open to others.

Role Variables
--------------

There are 2 main variables that need to be provided external to the role that have no default. 

    ipaserver_admin_password
    ipaserver_dir_admin_password

The following variables are used by this role and values are defined in defaults/main.yml:

    ipaserver_base_command:     ipa-server-install -U
    ipaserver_configure_ssh: True
    ipaserver_configure_sshd: True
    ipaserver_dns_forwarder: 8.8.8.8
    ipaserver_domain: example.com       # All lowercase. Actual DNS domain.
    ipaserver_hbac_allow: True
    ipaserver_idstart: 5000
    ipaserver_idmax: False
    ipaserver_mkhomedir: True
    ipaserver_packages:
      - ipa-server
      - bind
      - bind-dyndb-ldap
    ipaserver_realm: EXAMPLE.COM        # All caps. Better to match domain, but not required
    ipaserver_setup_dns: True
    ipaserver_setup_ntp: True
    ipaserver_ssh_trust_dns: True
    ipaserver_ui_redirect: True


Example Playbook
----------------

Here is an example playbook that can readily wrap this role and still be fairly flexible.  You typically don't need to be this flexible on password source.

    - hosts: servers
      vars_files:
        - vars/private-idm.yml
      vars_prompt:
        - name: ipaserver_admin_password
          prompt: "What should the admin password be for IPA?"
          private: yes
          default: "{{ vault_ipaserver_admin_password }}"
        - name: ipaserver_dir_admin_password
          prompt: "What should the admin password be for the Directory Server?"
          private: yes
          default: "{{ vault_ipaserver_dir_admin_password }}"
      roles:
         - { role: gregswift.ipaserver }

License
-------

GPLv2

Author Information
------------------

https://github.com/gregswift/ansible-freeipa
