hcloud_networks
=========

Role to provision networks and subnets on Hetzner Cloud.

Requirements
------------

* Hetzner Python library on target host: [hcloud](https://pypi.org/project/hcloud/)
* Ansible Hetzner collection on control node: [Hetzner.Hcloud](https://docs.ansible.com/ansible/latest/collections/hetzner/hcloud/)

Role Variables
--------------

**Default Variables**

| Variable                     | Type | Description                                                        | Default                      |   |
|------------------------------|------|--------------------------------------------------------------------|------------------------------|---|
| hcloud_network_api_token_rw  | str  | Read-write Hetzner Cloud api token                                 | -                            |   |
| hcloud_network_api_token_ro  | str  | Read-only Hetzner Cloud api token, defaults to rw-token if not set | hcloud_network_api_token_rw  |   |

**Network defining variables** 

These values are not predefined and need to be set as parameters when calling the role.

| Variable                         | Subitem  | Type     | Description                      | Example         |
|----------------------------------|----------|----------|----------------------------------|-----------------|
| hcloud_network_private_networks  |          | lst/dict | List of internal networks.       |                 |
|                                  | name     | str      | Name of internal network.        | internal_net    |
|                                  | ip_range | str      | IP range of internal network.    | 10.0.0.0/16     |
|                                  | state    | str      | State of internal network.       | present, absent |
| hcloud_network_private_subnets   |          | lst/dict | List of internal subnets.        |                 |
|                                  | network  | str      | Name of parent internal network. | internal_net    |
|                                  | ip_range | str      | IP range of internal subnet.     | 10.0.0.1/24     |
|                                  | state    | str      | State of internal subnet.        | present, absent |


Dependencies
------------

none

Example Playbook
----------------

    - name: "Manage networks on Hetzner Cloud."
      hosts: localhost
      gather_facts: false

      roles:

        - name: hcloud_networks
          vars:
            hcloud_network_api_token_rw: "{{ vault_hcloud_token_rw }}"
            hcloud_network_api_token_ro: "{{ vault_hcloud_token_ro }}"
            hcloud_network_private_networks:
              - name: internal_net
                ip_range: 10.0.0.0/16
                state: absent
            hcloud_network_private_subnets:
              - network: internal_net
                ip_range: 10.0.1.0/24
                state: absent
              - network: internal_net
                ip_range: 10.0.2.0/24
                state: absent

License
-------

BSD

Author Information
------------------

[flxsrbr](https://github.com/flxsrbr)
