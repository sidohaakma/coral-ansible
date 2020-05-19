# Opal installation 
Tested installations with 2 target OS's.

- Ubuntu Xenial
- CentOS 7

## Usage
Run the playbook on the target machine: `ansible-playbook install_os_????.yml -i "opal.domain.ext," --user ???? --ask-pass -e '{"firewall":{"enabled": false}, "opal":{"administrator":{"password": "xxxx", "username": "administrator"}, "version": "2.16.0"}}'`

## Development

### Vagrant

#### Ubuntu
```
vagrant init ubuntu/xenial64

vagrant up
```