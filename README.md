# user

[![Lint Code Base](https://github.com/jkirk/ansible-role-user/actions/workflows/linter.yml/badge.svg)](https://github.com/jkirk/ansible-role-user/actions/workflows/linter.yml)
[![Ansible Molecule](https://github.com/jkirk/ansible-role-user/actions/workflows/molecule.yml/badge.svg)](https://github.com/jkirk/ansible-role-user/actions/workflows/molecule.yml)

A simple ansible role to bootstrap user accounts with SSH public key based logins and passwordless sudo.

For something more sophisticated use a different role, the ansible-galaxy role [robertdebock.users](https://galaxy.ansible.com/robertdebock/users) looks promising.

## Requirements

For each user with SSH key based login you need a public key file like this:

`files/ssh/$USERNAME.pub`

This file will then be deployed to `/home/$USERNAME/.ssh/authorized_keys`.

## Role Variables

Use `group_vars/*` to deploy the users to specific host groups.

See: [defaults/main.yml](https://github.com/jkirk/ansible-role-user/tree/master/defaults/main.yml)

If you want to deploy the users on all hosts you can use `group_vars/all` or pass it as parameters to role (see example below).

## Dependencies

None.

## Example Playbooks

For this example we will assume you have defined a host group _servers_ in `hosts`.

In this example the users _jane_doe_ and _john_doe_ are created, will also be part of the group _sysadmin_ and will have passwordless sudo rights.

`site.yml`

```yaml
---
- hosts: servers
  roles:
    - {
        role: jkirk.user,
        users: ["jane_doe", "john_doe"],
        groupname: "sysadmin",
        admin: true,
      }
```

It makes sense to put the user role in a bootstrap-playbook like this:

`bootstrap.yml`

```yaml
---
# This playbook prepares the system to be managed by Ansible.
#
# Example:
#
#   ansible-playbook -u root bootstrap.yml

- name: Prepare system to be managed by Ansible
  hosts: all
  become: false
  gather_facts: false
  roles:
    - robertdebock.bootstrap
    - {
        role: jkirk.user,
        users: ["jane_doe", "john_doe"],
        groupname: "sysadmin",
        admin: True,
      }
```

To run this playbook you would do `ansible-playbook -u root -i hosts site.yml`

## License

MIT

## Author Information

Darshaka Pathirana - <https://synpro.solutions>
