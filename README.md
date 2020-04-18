# Rsync Server

## Purpose

This Ansible role is meant to configure an host to be used as an [rsync](https://en.wikipedia.org/wiki/Rsync) storage

## Installation

Add the role to a playbook's requirements and then:
```
ansible-galaxy install -r ansible-requirements.yml
ansible-playbook -i inventory/hosts setup.yml
```

## Client setup

Generate an ssh key pair and put the public one in the `rsync_server_pub_keys` array of this playbook

```
ssh-keygen -t ed25519 -o -a 100 -C '' -f /tmp/key
```

And then upload you files using:

```
SSH_ARGS="-ipath_to_priv_key"
REMOTE="$rsync_server_user@serverip:./"

rsync --partial -t --links --delete -h -r --progress -e "ssh $SSH_ARGS" "local_path/" "$REMOTE"
```

## How it works

A client that wish to upload file using `rsync` will connect using a previously authorized ssh key, the remote server (that this playbook configure) will create a jailed filesystem in a predefined location using the `rrsync` script that will allow the client to only touch and see authorized files
