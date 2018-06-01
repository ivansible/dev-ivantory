# Role ivansible.dev-ivansible

This role will checkout and activate ivansible ivantory and playbooks
from gitlab, install github keys, configure cipher and password for
[transcrypt](https://github.com/elasticdog/transcrypt).
Also, it will install the _ansible vault_ password file.


## Requirements

You should have SSH agent running on the controller machine
to provide RSA key with write access to the ivansible github
repository. This key will be forwarded to the installation.


## Variables

Available variables are listed below, along with default values.
The values will be usually configured in
`group_vars/all/secret/dev-ivansible.yml`

    dev_ivansible_repo: git@githab.com:username/reponame

URL of gitlab repository with ivansible.

    dev_ivansible_transcrypt_cipher: aes-256-cbc

Transcrypt cipher for secret settings.

    dev_ivansible_transcrypt_password: secretword

Transcrypt password for secret settings.


## Dependencies

This role will invoke `ivansible.dev-user` with `dev_user_install_keys`.


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
