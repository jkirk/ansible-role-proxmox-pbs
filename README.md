proxmox_pbs
===========

![Lint Code Base](https://github.com/jkirk/ansible-role-base/actions/workflows/superlinter.yml/badge.svg)

Simple ansible role to install Proxmox Backup Server on Debian or Proxmox VE as described in [Proxmox Backup documentation](https://pbs.proxmox.com/docs/installation.html)`

Requirements
------------

None.

Role Variables
--------------

TODO:

  If the role variable `proxmox_pbsadmins` is defined a PBS group 'admin' is
  created and the ACL is updated in that way that members of the group 'admin'
  are assigned the 'Administrator' role for the whole system (path `/`):

  ```sh
  % sudo pvesh get /access/acl
  ┌──────┬───────────────┬───────┬───────┬───────────┐
  │ path │ roleid        │ type  │ ugid  │ propagate │
  ╞══════╪═══════════════╪═══════╪═══════╪═══════════╡
  │ /    │ Administrator │ group │ admin │ 1         │
  └──────┴───────────────┴───────┴───────┴───────────┘
  ```

  The list of users which will be added to this admin group is set with this role variable:

  ```yaml
  # proxmox_pbsadmins: [ 'janedoe' , 'johndoe' ]
  ```

Dependencies
------------

None.

Additional Notes
----------------

To avoid invalid changes `/etc/hosts`, use the role [Oefenweb/ansible-hostname](https://github.com/Oefenweb/ansible-hostname) (which sets the hostname and `/etc/hosts`) and put the host in the hostgroup `proxmox` and puth the following in `group_vars/proxmox.yml`:

```yaml
---
hostname_hostname_ip_address: "{{ ansible_default_ipv4.address }}"
```

`Oefenweb/ansible-hostname` makes sure that the following line exists in `/etc/hosts`:

```lang-txt
127.0.1.1 proxmox.example.com proxmox
```

but Proxmox **needs** the external IP-Addresss to be resolvable. That means the `/etc/hosts` line should look like this:

```lang-txt
192.0.2.1 proxmox.example.com proxmox
```

Example Playbook
----------------

For this example we will assume you have defined a host group *proxmox* in the inventory file `hosts`.

`site-proxmox.yml`:

```yaml
---

    - hosts: proxmox
      roles:
         - { role: proxmox_pbs, pbs_pveadmins: [ 'janedoe', 'johndoe' ] }
```

To run this playbook you would run `ansible-playbook -i hosts site-proxmox.yml`

License
-------

MIT

Author Information
------------------

Darshaka Pathirana - <https://synpro.solutions>
