# Ansible playbooks

## Environments

### Lab

### Production

## Playbooks

### Applying playbooks
- Install Ansible dependencies using `ansible-galaxy install -r ./requirements.yml`
- Note that the first time applying playbooks on a clean server, you need to use whatever authentication is available (most likely password). During the first run the roles configure the SSH daemon to only allow non-root user with an RSA key and/or hardware device like YubiKey.

## Ansible Vault
The vault passphrase is committed into the repository because it is encrypted using gpg with YubiKey. To set from scratch, follow [this guide](https://disjoint.ca/til/2016/12/14/encrypting-the-ansible-vault-passphrase-using-gpg/). This does not give you access to the original passphrase, just the tooling to set up your own.

Ansible vault security - will ignore ansible.cfg if it's free writable directoy...
https://docs.ansible.com/ansible/devel/reference_appendices/config.html#cfg-in-world-writable-dir

need to export a variable in the current shell session:

```
export ANSIBLE_CONFIG=./ansible.cfg
```