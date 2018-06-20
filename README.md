# Role ivansible.dev-ivansible2

This role will:

- install github and gitlab access keys (via `ivansible.dev-user` role);
- setup bash and git aliases for development (via `ivansible.dev-user` role);
- install ansible locally for user (via `ivansible.dev-ansible` role);
- check out `ivansible2` inventory and playbooks from gitlab;
- configure cipher and password for
  [transcrypt](https://github.com/elasticdog/transcrypt);
- decypher the repository;
- harden permissions of secret files;
- login to ansible galaxy using github token;
- import required roles;

After that you can control `ivansible` inventory and run playbooks.


## Requirements

You should have SSH agent running on the controller machine to provide
RSA key with write access to the ivansible github repository.
This key will be forwarded to the installation.


## Variables

Available variables are listed below, along with default values.
The values will be usually configured in
`group_vars/workspace/secret/dev-ivansible2.yml`


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


    divan_roles_subdir: roles-galaxy
    divan_roles_list_file: roles-import.yml
    divan_roles_check_role: ivansible.lin-base

The script will run `ansible-galaxy` and import roles from given `list file`
into given role `subdir`ectory if given `check role` does not exist.


    divan_github_token: ""

Perform galaxy login with given github token.


    divan_venv_dir: ~/.ansible/venv

Directory for ansible virtual environment.


    divan_skip_prepare: false

Set this to `true` if you have already run `dev-user` and `dev-ansible`.


## Tags

- `divan_prepare_user` -- Install SSH access keys, setup bash and git aliases
- `divan_prepare_ansible` -- Install local user ansible
- `divan_decrypt` -- Decrypt secret files in the repository
- `divan_harden_secrets` -- Harden file permissions in secret directories
- `divan_galaxy` -- Galaxy login and role import
- `divan_ssh_config` -- Add example of login settings for vagrant boxes
                        to `~/.ssh/config`


## Dependencies

This role will invoke `ivansible.dev-user` with `dev_user_install_keys`.

**Note:** Currently this dependency is **not** recorded in `meta/main.yml`.


## Example Playbook

    - hosts: dock2
      roles:
        - { role: ivansible.dev-ivansible2 }


## Testing

    ansible-playbook plays-all/test-role.yml -e role=ivansible.dev-ivansible2 -l dock2
    ./scripts/run-role.sh .dev-ivansible2 dock2


## License

MIT


## Author Information

Created in 2018 by [IvanSible](https://github.com/ivansible)
