# Users

- cnc: [![build status](https://source.cnc.sk/ansible/role-users/badges/master/build.svg)](https://source.cnc.sk/ansible/role-users/commits/master)
- github: [![Build Status](https://travis-ci.org/hudecof/ansible_users.svg?branch=master)](https://travis-ci.org/hudecof/ansible_users)

This is simple user and user groups managment. It suits my needs but pathches are welcome.

# Role Variables
--------------

There are 3 main variables `users_available`, `users` and `users_deleted`.

## List of users

`users_avaiable` is list of all users as dictionary, where the key is the user name. Each user could have several attributes. I have this variable in `vars/users.yml` and is included in the playbook in `vars` section. See variable preferencein tablel beelowto find out their requirements and default values.

- `name`: full name of the user
- `password`: password hash
- `shell`: path to the shell
- `uid`: user id. It's good to keep the id same accross the systems
- `group`: primary group for user, if not exists is created with the same **gid** as is user account
- `groups`: list of the supplementary groups
- `is_admin`: boolean, this ads user to **sudo**/**wheel** group based on the OS
- `comment`: is the comment field in passwd file, if not used **name** is default

```
users_available:
  user1:
    name: Name1
    password: <password hash>
    shell: /bin/bash
    uid: 2000
    groups: ['users']
  user2:
    name: Name2
    password: <password hash>
    shell: /bin/bash
    uid: 2001
    groups: ['users', 'devops']
```

## Users to create

`users` is the list of the users. In this list you could override some values from `users_available`.

- `groups`
- `group`
- `shell`
- `uid`
- `password`

```
users:
  - username: user1
    groups: ['users', 'sudo']
  - username: user2
    uid: 20100
    is_admin: True
```

The `username` is a key of the `users_available` dictionary.

## Users to delete

`users_deleted` is list of all users to be deleted

    users_deleted:
      - username: user1
      - username: userX

## Users SSH keys

`users_upload_ssh_keys` is boolean, default **True**. When **False** the uploading of the users **ssh keys** is skipped.

`users_dir_ssh_keys`tell, where to fund the users ssh keys. It should be absilute path, in my environment it's **{{ playbook_dir }}/files/users**.

Task will look for `{{ users_dir_ssh_keys }}/<username>.pub` file.

`users_ssh_key_path` is used, when you want to place the authorized keys not in home directory. The keys will be in `{{ users_ssh_key_path}}/<username>.pub`. You need to modify also **sshd_config** variable **AuthorizedKeysFile** to `{{ users_ssh_key_path }}/%u.pub`

## Other variables
`users_update_password` value is one of supported by the ansible module **user**, default is **on_create**.

`users_create_user_group` is boolean, default **True**. When **False** the user group creation task is skipped.

`users_default_shell` is default user shellwhen not specified, default is **/bin/bash**.

## Variable precendce

|   |  users_available | users | required  | default  |
|---|---|---|---|---|
| uid | yes | yes | yes | n/a |
| name | yes  | no | yes | n/a |
| password | yes | yes | no  | omit |
| shell | yes | yes | no  | see `users_default_shell` |
| uid | yes | yes | no  | omit |
| password | yes | yes | no  | omit |
| group | yes | yes | no | `username` |
| groups | yes | yes | no | omit |
| is_admin | yes | yes | no | n/a |


# Dependencies

None

# License

BSD

Author Information
------------------

Peter Hudec
CNC, a.s.
Slovakia
