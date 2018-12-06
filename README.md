user
====

A simple ansible role to bootstrap user accounts with SSH key based logins and passwordless sudo.

For something more sophisticated use a different role, the ansible-galaxy role (robertdebock.users)[https://galaxy.ansible.com/robertdebock/users] looks promising.

Requirements
------------

For each user with SSH key based login you need a public key file like this:

`files/ssh/$USERNAME.pub`

This file will then be deployed to `/home/$USERNAME/.ssh/authorized_keys`.

Role Variables
--------------

Use `group_vars/*` to deploy the users to specific host groups. If you want to deploy the users on all hosts use `group_vars/all`.


```
# The users you want to create.
users: [ "jane_doe", "john_doe" ]

# Create additional user groups. The user you create will
# automatically be a part of their user's group, ie. john_doe:john_doe.
groupname: 'sysadmin'

admin: True
```

Dependencies
------------

None.

Example Playbook
----------------

For this example we will assume you have defined a host group *servers* in `hosts`.

In this example the users *jane_doe* and *john_doe* are created, will also be part of the group *sysadmin* and will have passwordless sudo rights.

`site.yml`:

```
---

    - hosts: servers
      roles:
         - { role: jkirk.user, users: [ 'jane_doe', 'john_doe' ], groupname: 'sysadmin', admin: True }
```

To run this playbook you would do `ansible-playbook -i hosts site.yml`

License
-------

MIT

Author Information
------------------

Darshaka Pathirana - https://synpro.solutions
