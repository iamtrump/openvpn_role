openvpn_role
============

Ansible role to configure OpenVPN layer-3 internet gateway with certificate-based authentication. Should work on systems with the apt package manager.

Role Variables
--------------
### Program
* `openvpn_server_name` — server name, default: `vpn-server`.
* `openvpn_user` — user ID for OpenVPN process, default: `openvpn`.
* `openvpn_group` — group ID for OpenVPN process, default: `openvpn`.
* `openvpn_log_dir` — directory where log and status will be stored, default is `/var/log/openvpn`.
### Network
* `openvpn_address` — address for client connections, default is `{{ ansible_default_ipv4.address }}`.
* `openvpn_proto` — protocol to use (tcp/udp). Default is `udp`.
* `openvpn_port` — port, default is `161`.
* `openvpn_network` — openvpn network, default is `192.168.89.0/24`.
* `openvpn_push_dns` — list of DNS to push, default: `['8.8.8.8', '8.8.4.4']`.
### PKI
* `openvpn_pki_dir` — directory where PKI database will be stored, default is `/etc/openvpn/pki`.
* `openvpn_ca_name` — CA common name, default is `openvpn-ca`.
* `openvpn_ca_days` — number of days to certify CA certificate. Default: `3655`.
* `openvpn_ca_days_min` — if the CA expires in fewer days than the value, it will be reissued. Default: `30`.
* `openvpn_c_days` — number of days to certify user cerificates. Default: `370`.
* `openvpn_c_days_min` — certificates expiring in fewer days than the value will be reissued. Default: `7`.
### Clients
* `openvpn_local_config_dir` — local dir to copy users’ connection profiles. Default is `{{ playbook_dir }}/{{ inventory_hostname }}-openvpn-profiles`.

* `openvpn_clients` — list of clients. Attributes:
  * `name` — client's common name.
  * `ip` (optional) — static address to assign.

Role Tags
---------

You can run role with following tags:
* `openvpn_setup` — setup and configure OpenVPN.
* `openvpn_pki_info` — show information about certificates state.
* `openvpn_clients` — issue or revoke needed certificates and ensure that connection profiles exist.

Example Playbook
----------------

```
- hosts: vpn
  roles:
    - role: openvpn_role
      vars:
        openvpn_server_name: Cool server
        openvpn_clients:
          - {name: mom}
          - {name: dad, ip: 192.168.89.111}
          - {name: sister}
        openvpn_proto: tcp
      tags: openvpn
```
