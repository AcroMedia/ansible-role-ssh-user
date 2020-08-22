# ansible-role-ssh-user

Control SSH user logins and configure users' public SSH keys on a linux machine.

This role is a convenient wrapper around Ansible's `user` and `authorized_key` modules.

## Requirements

- OS: Debian or Red Hat
- If using this role over ssh, you must obviously already have a working / sudo-able login to a machine, as you would with any ansible role. In order to create users as part of your bootstrap process, the role would need to be running against localhost.
- This role is meant for maintenance use after your system has been initially bootstrapped: E.g. maintaining access for employees.
- The only authentication method supported by the role is public key. Password logins are not supported.

## Role Variables

Variables are illustrated in the example below.

See also: defaults/main.yml and tasks/main.yml

## Dependencies

None.

## Example Playbook

```yaml
- hosts: servers
  gather_facts: true
  become: true
  vars:
    ssh_user_logins:
      - username: automator1
        state: present
        key: "ssh-rsa AAAAB3Nz...truncated...xxxxxxxxxxxx key comment"
        groups:
          - sudo
          - automationOperators

      - username: realAdmin
        state: present
        key: "ssh-rsa AAAAB3Nz...truncated...yyyyyyyyyyyy key comment"
        groups:
          - sudo

      - username: semiPrivilegedUser
        state: present
        key: https://my-private-git-server.com/john-doe.keys
        shell: /bin/zsh
        groups:
          - adm

      - username: ex-employee
        state: absent

  roles:
    - name: Configure SSH users
      role: acromedia.ssh-user
```

## License

GPLv3

## Author Information

[Acro Media Inc](https://www.acromedia.com/)
