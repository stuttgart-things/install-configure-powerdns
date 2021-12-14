# stuttgart-things/install-configure-powerdns

This Ansible role can completely set up and configure a PowerDNS DNS server with a mariadb backend and a managment frontend within a podman container with a NGINX reverse proxy for secure access. The entire ansible logic is based on api calls. No client binary is required.
In addition to the installation, this role can also be used to create dns entrys and much more.

### Role installation:
<details><summary><b>Install this role on your ansible host (click here)</b></summary>

```
cat <<EOF > /tmp/requirements.yaml
roles:
- src: git@codehub.sva.de:Lab/stuttgart-things/supporting-roles/install-configure-powerdns.git
  scm: git
- src: git@codehub.sva.de:Lab/stuttgart-things/supporting-roles/install-configure-vault.git
  scm: git
- src: git@codehub.sva.de:Lab/stuttgart-things/supporting-roles/install-requirements.git
  scm: git
- src: git@codehub.sva.de:Lab/stuttgart-things/supporting-roles/deploy-podman-container.git
  scm: git
- src: git@codehub.sva.de:Lab/stuttgart-things/supporting-roles/install-configure-podman.git
  scm: git

collections:
- name: containers.podman
  version: 1.6.1
- name: community.general
  version: 3.4.0
- name: community.crypto
  version: 1.7.1
- name: ansible.posix
  version: 1.2.0

EOF
ansible-galaxy install -r /tmp/requirements.yaml --force && ansible-galaxy collection install -r /tmp/requirements.yaml -f
```
</details>

For more information about stuttgart-things role installation visit: [Stuttgart-Things howto install role](https://codehub.sva.de/Lab/stuttgart-things/meta/documentation/doc-as-code/-/blob/master/howtos/howto-install-role.md)

## Example playbooks to use this role

<details><summary>Install and initializing a powerdns server within a podman container and get cert from vault (click here)</summary>

### Ansible command:
```
ansible-playbook -i inventory.ini playbook.yml
```

### Playbook: playbook.yml
```
---
- hosts: "powerdns-server"
  become: true

  vars:

    powerdns_install: true

    vault_ca_cert_role_name: labul.sva.de
    vault_url: "https://vault.labul.sva.de:8200"
    vault_token: "example-token-12345"

    vault_cert: true
  
  roles:
    - install-configure-powerdns
```

### Playbook: inventory.ini
```
[powerdns-server]
example.com
```
</details>

<details><summary>Create or delete DNS entrys based on ansible vars profile (click here)</summary>

### Ansible command:
```
ansible-playbook -i inventory.ini playbook.yml
```

### Playbook: playbook.yml
```
---
- hosts: "powerdns-server"

  vars:

    pdns_api_executor: localhost
    pdns_url: "https://ns-sthings.tiab.labda.sva.de:8443"
    pdns_token: "password123"

    entry_zone: "sthings.tiab.ssc.sva.de."
    pdns_create_record:
          - fqdn: "*.atalanta.sthings.tiab.ssc.sva.de."
            content: 10.100.136.242
            record_type: A
            zone: "{{ entry_zone }}"
            state: present
            ttl: 60
            note: Created with ansible
          - fqdn: "vault.sthings.tiab.ssc.sva.de."
            content: "vault-ssc.labul.sva.de."
            record_type: CNAME
            zone: "{{ entry_zone }}"
            state: present
            ttl: 60
            note: Created with ansible
  roles:
    - install-configure-powerdns
```

### Playbook: inventory.ini
```
[powerdns-server]
example.com
```
</details>

<details><summary>Create or delete DNS zone based on ansible vars profile (click here)</summary>

### Ansible command:
```
ansible-playbook -i inventory.ini playbook.yml
```

### Playbook: playbook.yml
```
---
- hosts: "powerdns-server"

  vars:

    pdns_api_executor: localhost
    pdns_url: "https://ns-sthings.tiab.labda.sva.de:8443"
    pdns_token: "password123"

    pdns_create_zone:
      - name: "sthings.tiab.ssc.sva.de."
        state: present
        kind: NATIVE
  roles:
    - install-configure-powerdns
```

### Playbook: inventory.ini
```
[powerdns-server]
example.com
```
</details>

## Requirements and Dependencies:
- Ubuntu 20.04
- Fedora 34
- CentOS 8
- CentOS 7

### Features:
- Install PowerDNS with frontend and backend with NGINX reverse proxy
- Create/delete DNS records
- Create/delete DNS zones

## Version:
```
DATE            WHO            WHAT
2021-12-14      Marcel Zapf    Add logic to create pdns zones
2021-12-14      Marcel Zapf    Add feature to disable DNSStubListener systemd-resolved
2021-12-13      Marcel Zapf    Initial commit
```

License
-------

BSD

Author Information
------------------

Marcel Zapf; 11/2021; SVA GmbH
