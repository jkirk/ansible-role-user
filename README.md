user
====

A simple ansible role to bootstrap user accounts with SSH key based logins and passwordless sudo.

Requirements
------------

For each user with SSH key based login you need a public key file like this:

`files/ssh/$USERNAME.pub`

This file will then be deployed to `/home/$USERNAME/.ssh/authorized_keys`.

Role Variables
--------------

Use `group_vars/*` to deploy the users to specific host groups. If you want to deploy the users in all hosts use `group_vars/all`.


```
# The users you want to create.
user_names: [ "jane_doe", "john_doe" ]

# Optionally create additional user groups. If empty, the user you create will
# automatically be a part of their user's group, ie. john_doe:john_doe.
user_groups: []


sudoers_groups: []
```

Dependencies
------------

None.

Example Playbook
----------------

For this example we will assume you have defined a host group *servers* in `hosts`.

In this example the users *jane_doe* and *john_doe* are created and will also be part of the group *sysadmin*.
All users which are part of the *sysadmin* group will have passwordless sudo rights.


`site.yml`:

```
---

    - hosts: servers
      roles:
         - { role: jkirk.user, user_names: [ 'jane_doe' ], user_groups: [ 'sysadmin' ], sudoers_groups [ ' sysadmin' ] }
```

To run this playbook you would do `ansible-playbook -i hosts site.yml`

License
-------

MIT

Author Information
------------------

Darshaka Pathirana - https://synpro.solutions
