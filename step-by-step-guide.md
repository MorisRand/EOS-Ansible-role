# Guide to manual installation of EOS-client with JINR Kerberos and LDAP.
## Dependencies

```bash
sudo DEBIAN_FRONTEND=noninteractive apt install libnss-ldapd libpam-ldapd krb5-user libpam-krb5 libpam-ccreds auth-client-config
```
> `DEBIAN_FRONTEND=noninteractive` will prevent manual configuration of packages for Kerberos and LDAP since we will want to use JINR-provided ones

## Setting up JINR configuration for Kerberos and LDAP
Each file from `roles/setup_eos/files` folder should be copied to your target system considering it as relative to `/`. So contents of `roles/files/etc` should be copied to `/etc`. The whole pack as tarball can be copied from the `lxpub` --- `/stp/EOS/ubuntu.tgz` (thanks to Valery Mitsin for the configs).

## Update sshd settings
Your `/etc/ssh/sshd_config` should contain the following lines:
```bash
KerberosAuthentication yes
KerberosOrLocalPasswd yes
KerberosTicketCleanup yes
PasswordAuthentication yes
```
> Don't forget to restart sshd daemon after changes
```bash
sudo systemctl restart sshd.service
```

## Install EOS repos
Get signing key for apt repos:
```bash
curl -sL http://storage-ci.web.cern.ch/storage-ci/storageci.key | sudo apt-key add -
```

Add repos:
```bash
sudo add-apt-repository 'deb [arch=amd64] http://storage-ci.web.cern.ch/storage-ci/debian/xrootd/ bionic release'
sudo add-apt-repository 'deb [arch=amd64] http://storage-ci.web.cern.ch/storage-ci/debian/eos/citrine/ bionic tag'
```

Install EOS-client and FUSEX.
```bash
sudo apt install xrootd-client=4.10.1 xrootd-client-libs=4.10.1 xrootd-libs=4.10.1 -y --allow-downgrades
sudo apt install eos-client=4.5.15 eos-fusex=4.5.15 -y --allow-downgrades
sudo apt-mark hold xrootd-client xrootd-client-libs xrootd-devel xrootd-client-devel xrootd-libs eos-client eos-fusex
```
> Note that we have to hold versions so eos console works

## Mount EOS
User authenticated by JINR Kerberos can mount the EOS using FUSEX: 
```bash
sudo eosxd /eos
```
