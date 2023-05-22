---
title: kanidm
description: 
published: true
date: 2023-05-21T20:02:00.353Z
tags: 
editor: markdown
dateCreated: 2023-05-21T19:55:09.379Z
---

# Kanidm

[Kanidm](https://kanidm.github.io/kanidm/stable/) is an identity management server. We use it to manage users across multiple nodes.

We have two groups: a posix group `geekpie-hpc` for everyone, and an admin group `geekpie_admins`.

`geekpie_admins` is used for manage accounts, which is a subgroup of:
- `idm_people_manage_priv`: create new person
- `idm_group_write_priv`: add person into a group
- `idm_account_unix_extend_priv`: enable posix for a person
- `idm_account_write_priv`: add ssh key to person

To begin with, export environment variable KANIDM_URL, and login with your `geekpie_admins` user.

```bash
export KANIDM_URL="https://hpc-idm.geekpie.icu:8443"
kanidm login --name geekpie
```

## Create a user

To create a user called John Smith, and add it to `geekpie-hpc` group:

```bash
kanidm person create jsmith "John Smith"
kanidm person update jsmith --mail "jsmith@shanghaitech.edu.cn" # --legalname
kanidm group add-members geekpie-hpc jsmith
```

Then enable posix, set ssh key and password.

```bash
# In kanidm uid is the same as gid. I recommend you to manually allocate a gid.
# Please see https://github.com/geekpiehpc/AnsiblePlaybook/blob/main/group_vars/epyc.yml for old uids.
kanidm person posix set jsmith --gidnumber 2345 # --shell /usr/bin/bash
kanidm person ssh add_publickey jsmith id_rsa (cat ~/.ssh/id_rsa.pub)
# Don't need this the user do not need sudo
kanidm person posix set_password jsmith
```

## Install

```bash
curl -L -o kanidm.deb https://github.com/kanidm/kanidm/releases/download/latest/kanidm_Ubuntu_22.04_1.1.0-beta.13-2023051108041ddac86_x86_64.deb
curl -L -o kanidm_unixd.deb https://github.com/kanidm/kanidm/releases/download/latest/kanidm-unixd_Ubuntu_22.04_1.1.0-beta.13-2023051108091ddac86_x86_64.deb

sudo dpkg -i kanidm.deb kanidm_unixd.deb
```

`/etc/kanidm/unixd`:
```ini
pam_allowed_login_groups = ["geekpie-hpc"]
default_shell = "/usr/bin/bash"
home_alias = "name"
use_etc_skel = true
uid_attr_map = "name"
gid_attr_map = "name"
```

`/etc/kanidm/config`:

```ini
uri = "https://hpc-idm.geekpie.icu:8443"
verify_ca = true
verify_hostnames = true
```

Edit `/usr/share/pam-configs/kanidm-unixd `. Change priority to 0, otherwise you will be asked sudo password twice!

Restart services
```bash
sudo systemctl restart kanidm-unixd
sudo systemctl restart kanidm-unixd-tasks.service
```

### Setup PAM and nsswitch

PAM

```bash
# THIS DIRTY HACK AND IS ACTUALLY UPSTREAM PACKAGING PROBLEM
sudo mv /etc/pam.d/kanidm-unixd /usr/share/pam-configs/
sudo pam-auth-update # check kanidm
```

For nssitwch, edit `/etc/nsswitch.conf`:

```yaml
passwd:         files systemd kanidm
group:          files systemd [SUCCESS=merge] kanidm
```

Then Add sudoers file

```bash
echo '%geekpie-hpc   ALL=(ALL:ALL) ALL' | sudo EDITOR='tee -a' visudo /etc/sudoers.d/geekpie
```

Add ssh config by creating `/etc/ssh/sshd_config.d/60-kanidm.conf`:
```ssh_config
AuthorizedKeysCommand /usr/bin/env kanidm_ssh_authorizedkeys %u
AuthorizedKeysCommandUser nobody
```

Restart sshd service
```bash
sudo systemctl restart sshd.service
```
