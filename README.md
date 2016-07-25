# Ansible SSH Key

[Ansible](https://www.ansible.com) role that generates an SSH key for a user.

Setting the `ssh_key_root_link` to `true` will also symlink the key into the root user's .ssh directory.



## Role Variables

**SSH key properties**

* `ssh_key_user` (or `user`) - **required** - The user for which to generate an SSH key (defaults to the value of the `user` variable).
* `ssh_key_bits` `[integer]` - The number of bits in the generated key (defaults to 8192).
* `ssh_key_comment` - The comment at the end of the SSH key block (defaults to `{{ ssh_key_user }}@{{ ansible_hostname }}`, e.g. `jdoe@mycomputer`).

**Non-root user's SSH key location**

* `ssh_key_dir` - The .ssh directory where the key will be generated (defaults to `{{ host_user_homes }}/{{ ssh_key_user }}/.ssh`, where `host_user_homes` is provided by the AlphaHydrae.multipass role).
* `ssh_key_file` - The location of the SSH key file (defaults to `{{ ssh_key_dir }}/id_rsa`).

**Root user's SSH key location**

* `ssh_key_root_dir` - The .ssh directory of the root user (defaults to `{{ host_root_home }}/.ssh`; the `host_root_home` variable is provided by the AlphaHydrae.multipass role).
* `ssh_key_root_file` - The location of the root user's SSH key file (defaults to `{{ ssh_key_root_dir }}/id_rsa`).

**Root symlink variables**

* `ssh_key_root_link` `[boolean]` - Whether to symlink the generated key into the root user's .ssh directory (defaults to `false`; has no effect if `ssh_key_user` is the root user).



## Dependencies

The [AlphaHydrae.multipass](https://github.com/AlphaHydrae/ansible-multipass) role is used to determine the base directory where user home directories are located.
The role sets the following facts to reasonable defaults depending on the target host's platform:

* `host_root_user` - The username of the root user (e.g. `root`).
* `host_root_group` - The group of the root user (e.g. `root`).
* `host_root_home` - The home directory of the root user (e.g. `/root`).
* `host_user_homes` - The base directory where user home directories are located (e.g. `/home` on Linux or `/Users` on OS X).



## Example Playbook

    - hosts: servers
      roles:
        - role: AlphaHydrae.ssh-key
          ssh_key_user: jdoe

**Custom SSH key properties**

    - hosts: servers
      roles:
        - role: AlphaHydrae.ssh-key
          ssh_key_user: jdoe
          ssh_key_bits: 2048
          ssh_key_comment: my-key

**With a default user defined (e.g. at the playbook level)**

    - hosts: servers
      vars:
        user: jdoe
      roles:
        - role: AlphaHydrae.ssh-key

**Symlink the key into the root user's .ssh directory**

    - hosts: servers
      roles:
        - role: AlphaHydrae.ssh-key
          ssh_key_user: jdoe
          ssh_key_root_link: true
