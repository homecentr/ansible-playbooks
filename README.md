# Ansible playbooks

## Structure and conventions

```yaml
inventories/
├── <environment>
|   ├── group_vars/
|   |   ├── <group-name>/
|   |   |   └── some-file.yml # Variables applied to hosts in the group
|   |   └── all.yml           # Variables applied to all hosts
|   ├── host_vars/
|   |   └── <hostname>.yml    # Contains variables applied to specific hosts
|   ├── hosts                 # List of hosts in the environment and their mapping to groups/
|   └── secrets               # Secrets stored as ansible variables encrypted by ansible-vault
playbooks/
|   ├── proxmox-ve.yml        # Initialized and configures a Proxmox VE node for hosting VMs
|   ├── swarm-cluster.yml     # Initializes Swarm cluster and configures swarm nodes for hosting services
|   └── swarm-services.yml    # Deploys Docker swarm stacks running the services
roles/
|   ├── services
|   |   └── <role>            # Roles contains Swarm cluster services which present value for the end user
|   ├── utils
|   |   └── <role>            # General roles which can be used ad-hoc
.vault-pass.gpg               # Vault password encrypted by a hardware token
apply                         # Script to ease up running playbooks
vault                         # Script to ease up managing the encrypted secrets
```

## Environments

- **Lab** - test environment used to develop the roles. Usually running locally inside of HyperV on a developer workstation.
- **Production** - the actual deployment used by the users.

## Applying playbooks

Simply run the followin bash command:

```
./apply <environment> <playbook>
```

for example `./apply lab swarm-services`

The script automatically installs dependencies from Ansible Galaxy and runs the playbook.

> Note that the first time applying playbooks on a clean server, you need to use whatever authentication is available (most likely a password based one). During the first run the roles configure the SSH daemon to only allow non-root user with an RSA key and/or hardware device like YubiKey you need to use for subsequent logins.

## Ansible Vault
The vault passphrase is committed into the repository because it is encrypted using gpg with YubiKey. To set from scratch, follow [this guide](https://disjoint.ca/til/2016/12/14/encrypting-the-ansible-vault-passphrase-using-gpg/). This does not give you access to the original passphrase, just the tooling to set up your own. Each user must have their own encrypted version of the password.

To change the encrypted variables, use the `vault` script, for example `./vault edit lab` to edit secrets for the Lab environment.

## Variable naming conventions

| Variable prefix | Description |
|-----------------|-------------------------|
| `secret_` | Variable value is sensitive and should be stored in the `secrets` file for each environment. |
| `homecentr_<role>_` | Customizable variable defined by one of the roles. |