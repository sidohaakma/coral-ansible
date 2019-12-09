# SSH Host Signer Ansible Role
An Ansible role to securely sign SSH host keys with an SSH certificate authority. The CA private key never leaves your local machine.

## Requirements
The machine running this playbook is expected to have an `ssh-keygen` binary on the path. It should be new enough to support SSH CAs.

## Variables
* `ssh_host_signer_ca_key` - The path to the CA key used to sign host keys. (defaults to `/etc/ssh/ca_key`)
* `ssh_host_signer_id` - The ID of the certificate to be generated. (defaults to `{{ ansible_fqdn }}`)
* `ssh_host_signer_hostnames` - The comma seperated hostnames for which the certificate should be valid. (defaults to `{{ ansible_fqdn }},{{ ansible_hostname }}`)
* `ssh_host_signer_key_directory` - The path on the server to look for SSH host keys to sign. (defaults to `/etc/ssh/`)
* `ssh_host_signer_ssh_config` - The path to the SSH config file which the certificates will be added to. (defaults to `/etc/ssh/sshd_config`)
