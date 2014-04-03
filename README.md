Users
========

This is simple user and user groups managment. It suits my needs but pathches are welcome.
It's based on the role https://github.com/mivok/ansible-users

Role Variables
--------------

There are 3 main variables `users_available`, `users` and `users_deleted`.

`users_avaiable` is list of all users as dictonary, where the key is the user name. Each user couldl have several attributes. I have this variable in `vars/users.yml` and is included in the playbook in `vars` section.
Attributes are

- name: aka GECOS
- password: password hash
- shell: path to the shell
- uid: user id. It's good to keep the id same accross the systems
- groups: list of the supplementary groups

Exampe:

    users_available:
      user1:
        name: Name1
        password: <password hash>
        shell: /bin/bash
        uid: 2000
        groups: ['users']
      user2:
        name: Name2
        password: password hash
        shell: /bin/bash
        uid: 2001
        groups: ['users']

`users` is the list of the users. In this list you could override some values from `users_available`.

- groups
- shell
- uid
- password

Example:

    users:
      - username: user1
        groups: ['users', 'sudo']
      - username: user2
        uid: 20100

The `username` is a key of the `users_available` dictionary.

`users_deleted` is list of all users to be deleted

    users_deleted:
      - username: user1
      - username: userX

Notes
--------------

This role always creates usergroup with the same name as the username and puts its as the primary one.

This role always creates `home` based on defaults of the target system.

This role `copy` the `ssh authorized_keys`. Put the file in `files/{{ item.username }}.pub`. I decided to use `copy` while the keys deprovisioning is much more easier. If the user do not have ssh key, create empty file.


Dependencies
------------

None

License
-------

BSD

Author Information
------------------

Peter Hudec
