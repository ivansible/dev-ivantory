# ivansible.dev_ivantory

This role will:

- install github (and gitlab) access keys (via `ivansible.dev_user` role);
- setup bash and git aliases for development (via `ivansible.dev_user` role);
- install ansible locally for user (via `ivansible.dev_ansible` role);
- check out `ivantory` inventory and playbooks from github;
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

    real_ssh_port: <optional>
This optional setting lets the user override an auto-detected ssh port value,
which may be incorrect if ansible is run by the hashicorop packer or through
a reverse ssh or port forwarder. An incorrect value is generally harmless
though, as the role uses it only to fill in a comment.

    divan_repo: git@github.com:username/reponame.git

URL of github repository with ivansible.

    divan_branch: master

Repository branch to checkout.

    divan_dir: ~/devel/ivantory

Directory for checkout.

Note: Only clone will be performed. If repository is already checked out
(and probably modified), it will not be updated. This prevents problems
with rewriting possible local modifications by ansible git module.


    divan_cipher: aes-256-cbc

Crypto cipher for secret settings.

    divan_password: ""

Crypto password for secret settings.
Empty string means that repository is not encrypted.

    divan_check_file: ""

If this setting is not empty, it defines relative path of a check file.
Ansible will compare file contents with is base name, and if they do not
match, report decryption failure and abort the scenario.


    divan_cryptor_exe: /usr/local/bin/transcrypt.sh
Location of cryptor binary.

    divan_cryptor_configured_flag: ""
Cryptor sets this Git flag when configuration is done.

    divan_cryptor_release: latest

Cryptor version to use. Check out
https://github.com/elasticdog/transcrypt/releases
for new releases.
If you set this to `latest` instead of concrete release number, the role
will detect latest release automatically.

    divan_cryptor_reinstall: false

Normally, if cryptor is already installed, it will not be upgraded.
This flag allows for upgrades.


    divan_roles_dir: ~/devel/galaxy
    divan_roles_list_file: galaxy.yml
    divan_roles_check_role: ivansible.lin_base

The script will run `ansible-galaxy` and import roles from given `list file`
into given role `subdir`ectory if given `check role` does not exist.


    divan_github_token: ""

Perform galaxy login with given github token.


    divan_venv_dir: ~/.ansible/venv

Directory for ansible virtual environment.


    divan_skip_prepare: false

Set this to `true` if you have already run `dev_user` and `dev_ansible` roles.


## Tags

- `divan_prepare_user` -- Install SSH access keys, setup bash and git aliases
- `divan_prepare_ansible` -- Install local user ansible
- `divan_checkout` -- Checkout inventory and playbooks
- `divan_decrypt` -- Decrypt secret files in the repository
- `divan_harden_secrets` -- Harden file permissions in secret directories
- `divan_galaxy` -- Galaxy login and role import
- `divan_ssh_config` -- Add example of login settings for vagrant boxes
                        to `~/.ssh/config`
- `divan_links` -- Create symbolic links for galaxy roles, requirements and virtualenv


## Dependencies

This role will invoke `ivansible.dev_user` with `dev_user_setup_system` enabled.

Note: Currently this dependency is **not** recorded in `meta/main.yml`.

You are also recommended to enable `dev_user_install_keys` in inventory,
else roles won't work (usually eligible for `permitted` inventory group).


## Example Playbook

    - hosts: vag2
      roles:
        - { role: ivansible.dev_ivantory }


## Testing

    ansible-playbook plays-all/run-role.yml -e role=ivansible.dev_ivantory -l vag2
    ./scripts/role.sh .dev-ivantory vag2


## License

MIT


## Author Information

Created in 2018 by [IvanSible](https://github.com/ivansible)
