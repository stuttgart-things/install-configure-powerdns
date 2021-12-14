# stuttgart-things/install-configure-powerdns

This Ansible role can completely set up and configure a PowerDNS DNS server with a mariadb backend and a managment frontend with a NGINX reverse proxy for secure access. The entire ansible logic is based on api calls. No client binary is required.
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

## Requirements and Dependencies:
- Ubuntu 20.04
- Fedora 34
- CentOS 8
- CentOS 7

### Features:
- Install PowerDNS with frontend and backend with NGINX reverse proxy
- Create DNS records

## Version:
```
DATE            WHO            WHAT
2021-12-08      Marcel Zapf    Add feature to disable DNSStubListener systemd-resolved
2021-12-13      Marcel Zapf    Initial commit
```

License
-------

BSD

Author Information
------------------

Marcel Zapf; 11/2021; SVA GmbH