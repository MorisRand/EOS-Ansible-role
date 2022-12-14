# Description:

Ansible role to deploy [EOS](https://github.com/cern-eos/eos) client for Ubuntu 20.04 using JINR EOS setup and
JINR authentication services.

JINR configuration for Kerberos and LDAP is stored in `roles/setup_eos/files` folder. One
needs to copy these files into respected directories in a target system.

Configuration is provided by Valery Mitsin and can be downloaded from `lxpub` -- path is `/stp/EOS/ubuntu.tgz`.

The role is located under `roles/setup_eos`. Tasks performed by the role are
listed in `roles/setup_eos/tasks`.

> Configuration sensitive files are encrypted using [transcrypt](https://github.com/elasticdog/transcrypt)

## Ansible role usage

The reference usage of role can be found in `eos.yml` playbook. It updates
system packages, install Kerberos and LDAP, configures them to use JINR
providers (`lxpub`), manages EOS installation and then creates user that can be 
authenticated by JINR Kerberos to check that deployment works.

It can be run with the following command
```bash
ansible-playbook -i <sudo_user>@<host>, eos.yml --ask-become-pass
```
> Note `,` after the target host! It is extremely important.

After successful run one should be able to mount EOS using FUSEX to `/eos`

```bash
sudo eosxd /eos
```

## Manual installation:
For instruction how to proceed manually see [this guide](step-by-step-guide.md)
