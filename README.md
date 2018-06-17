# Role ivansible.dev-ivansible

This role will:

- install github and gitlab access keys (via `ivansible.dev-user` role);
- setup bash and git aliases for development (via `ivansible.dev-user` role);
- check out `ivansible` inventory and playbooks from gitlab;
- configure cipher and password for
  [transcrypt](https://github.com/elasticdog/transcrypt);
- decypher the repository;
- harden permissions of secret files;
- *[TODO]* install galaxy roles.

After that you can control `ivansible` inventory and run playbooks.


## Requirements

You should have SSH agent running on the controller machine to provide
RSA key with write access to the ivansible github repository.
This key will be forwarded to the installation.


## Variables

Available variables are listed below, along with default values.
The values will be usually configured in
`group_vars/workspace/secret/dev-ivansible.yml`


    divan_repo: git@githab.com:username/reponame

URL of gitlab repository with ivansible.

    divan_branch: master

Repository branch to checkout.

    divan_dir: ~/dev/ivansible2

Directory for checkout.

Note: Only clone will be performed. If repository is already checked out
(and probably modified), it will not be updated. This prevents problems
with rewriting possible local modifications by ansible git module.


    divan_cipher: aes-256-cbc

Transcrypt cipher for secret settings.

    divan_password: ""

Transcrypt password for secret settings.
Empty string means that repository is not encrypted.

    divan_check_file: ""

If this setting is not empty, it defines relative path of a check file.
Ansible will compare file contents with is base name, and if they do not
match, report decryption failure and abort the scenario.


    divan_transcrypt_release: latest

Transcrypt version to use. Check out
https://github.com/elasticdog/transcrypt/releases
for new releases.
If you set this to `latest` instead of concrete release number, the role
will detect latest release automatically.

    divan_transcrypt_reinstall: false

Normally, if transcrypt is already installed, it will not be upgraded.
This flag allows for upgrades.


    divan_python: python3

What python version to use for ansible: `python` or `python3`.


    divan_roles_subdir: roles-galaxy
    divan_roles_list_file: roles-import.yml
    divan_roles_check_role: ivansible.lin-base

The script will run `ansible-galaxy` and import roles from given `list file`
into given role `subdir`ectory if given `check role` does not exist.


    divan_github_token: ""

Perform galaxy login with given github token.


## Tags

- `divan_prepare_user` - Install SSH access keys,
                                 setup bash and git aliases
- `divan_decrypt` - Decrypt secret files in the repository
- `divan_harden_secrets` - Harden file permissions
                                   in secret directories
- `divan_galaxy` - Galaxy login and role import
- `divan_ssh_config` - Add example of login settings
                               for vagrant boxes to `~/.ssh/config`


## Dependencies

This role will invoke `ivansible.dev-user` with `dev_user_install_keys`.

**Note:** Currently this dependency is **not** recorded in `meta/main.yml`.


## Example Playbook

    - hosts: dock2
      roles:
        - { role: ivansible.dev-ivansible }


## Testing

    ansible-playbook plays-all/test-role.yml -e role=dev-ivansible -l dock2


## License

MIT


## Author Information

Created in 2018 by [IvanSible](https://github.com/ivansible)
