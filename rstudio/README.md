# Analysis Jenkins Ansible

Update Jenkins VM instance with Ansible.

Execute for roll out:

```bash
ansible-playbook -i inventory.ini playbook.yml **optional: [ --extra-vars "run_as_gcc=true" ]**
```

You can toggle a number of settings. You can use ```--extra-vars``` flag to set them. Currently we support the flags below: 
* run_as_gcc | *features only used by GCC, can set to true if you are on GCC infrastructure*
* ci | *features not performed when ci = true*

> note: multiple vars can be set space separated. Example: ```--extra-vars "run_as_gcc=true"```.

## Third party usage
What you need to do after this installation.

- SSH
  - Authorised keys
- Install a web-server
  - SSL
    - Certificates
- Firewall

## SSH-configuration
There are some prerequisites to run this playbook.

- You have to create a ```.ssh_ansible``` file on the project-root to run this playbook. 
- You have to have root-privileges on the target machine
- You have to create a key pair for host key signing and put them in the molgenis_ca folder. You also need to add the public key to the known_hosts file

## Roles:

- ssh_host_key_signing | *allows you to connect to servers without asking for a knownhost entry*

- preinstall | *initialize configuration (setup repos for instance)*
- sshd | *allows users to login without a password*
- httpd | *add a webserver, in this case Apache*
- node_exporter | *add node_exporter to expose load and other system features*
- jenkins | *add jenkins to the node*

## Role: [ssh_host_key_signing] - Signs the host keys to prevent entries in knownhosts
You need to make a directory ```ssh-host-ca```. Then you need to add the security certificate from the vault to this directory.
You can do this commandline (with the vault client, check: [vault cli](https://www.vaultproject.io/docs/commands/)). 
First of all you need to make the service available locally.

```
kubens

molgenis-jenkins
molgenis-hubot
...
vault-operator (or somehting similar)

kubens vault-operator
kubectl get pods

etcd-operator-#hash#
...
vault-#hash#

kubectl port-forward vault-#hash# 8200:8200
```

Secondly you need to add the vault configuration to your own instance that is running Ansible. 

```bash
vi ~/.vault-token
```

Then execute the following commands:

```vault read -field=value -tls-skip-verify secret/ops/certificate/ssh_host_key_signing/backup.pub > roles/tools/files/.backup.pub```
```vault read -field=value -tls-skip-verify secret/ops/certificate/ssh_host_key_signing/molgenis_ca.pub > ssh_host_molgenis_key.pub```
```vault read -field=value -tls-skip-verify secret/ops/certificate/ssh_host_key_signing/molgenis_ca > ssh_host_molgenis_key``` 
```vault read -field=value -tls-skip-verify secret/ops/certificate/ssh_host_key_signing/knownhosts > ~/.ssh/knownhosts```

To set the correct permissions you need to set the right scope on the following files:

```chmod 400 ssh_host_molgenis_key*```

That's it!

## Role: [ preinstall ] - Initialize configuration
Add the following repositories

MOLGENIS stable repo
* molgenis-release.repo

CentOS non-native tooling
* epel-release.repo

* Install molgenis-ops-tools (for ntp, firewall and other tools)
* Configure home dir backup user
  >note: you need to overwrite the .id_rsa_bitpolII.pub to 
* Configure firewall
* Synchronise NTP

## Role: [ httpd ] - Add httpd

Install the service httpd and enable it on boot time.

* Configure HTTPD 
  >note: with hostname located in the inventory.ini file
* Install certificates on host
  >note: you need to have access to the backup server on the machine you are running Ansible on 
  
## Role: [ node_exporter ] - Add monitoring
Install supervisor and node exporter to create monitoring statistics which can be scraped by tools like Prometheus.

## Role: [ jenkins ] - Add a new Jenkins instance to the system
Installs the Jenkins RPM on the host and adds it to the bootcycle