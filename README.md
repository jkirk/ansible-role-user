user
====

A simple ansible role to bootstrap user accounts with SSH public key based logins and passwordless sudo.

For something more sophisticated use a different role, the ansible-galaxy role [robertdebock.users](https://galaxy.ansible.com/robertdebock/users) looks promising.

Requirements
------------

For each user with SSH key based login you need a public key file like this:

`files/ssh/$USERNAME.pub`

This file will then be deployed to `/home/$USERNAME/.ssh/authorized_keys`.

Role Variables
--------------

Use `group_vars/*` to deploy the users to specific host groups.


```yaml
# The users you want to create.
users: [ "jane_doe", "john_doe" ]

# Create additional user groups. The user you create will
# automatically be a part of their user's group, ie. john_doe:john_doe.
#
# groupname is mandatory when admin is true.
groupname: 'sysadmin'

# Setup passwordless sudo for the given groupname.
admin: true
```

If you want to deploy the users on all hosts you can use `group_vars/all` or pass it as parameters to role (see example below).

Dependencies
------------

None.

Example Playbooks
-----------------

For this example we will assume you have defined a host group *servers* in `hosts`.

In this example the users *jane_doe* and *john_doe* are created, will also be part of the group *sysadmin* and will have passwordless sudo rights.

`site.yml`

```yaml
---

- hosts: servers
  roles:
     - { role: jkirk.user, users: [ 'jane_doe', 'john_doe' ], groupname: 'sysadmin', admin: true }
```

It makes sense to put the user role in a bootstrap-playbook like this:

`boostrap.yml`

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
    - { role: jkirk.user, users: [ 'jane_doe', 'john_doe' ], groupname: 'sysadmin', admin: True }
```

To run this playbook you would do `ansible-playbook -u root -i hosts site.yml`

License
-------

MIT

Author Information
------------------

Darshaka Pathirana - https://synpro.solutions
