# Opal deployment (by Coral release)
We deploy Opal by deploying the Coral reelase made possible by INESTEC (https://eucanconnect.inesctec.pt/).

## Installation
After running this playbook you need to manually execute three steps:

- setup the host (install docker and python)
- deploy the stack (install opal, agate and mica)
- configure SSL (setup institute certificates)

### Setup the host
To setup the host, please execute (run as user with root privileges):

```bash
cd /usr/local/coral/coral-docker-dist_x.x.x/scripts/setup-host/linux/;
./setup-host.sh
```

Follow the instructions in the script.

**IMPORTANT: after reboot run the same script again**

Follow the instructions in the script.

### Deploy the stack
Please execute (run as user with root privileges):

```bash
cd /usr/local/coral/coral-docker-dist_x.x.x/;
python3.6 coral.py --deploy --domain domain.ext --email admin@domain.ext
```

Follow the instructions in the script.

You are all set! The application is available under: http://domain.ext/.

### Configure SSL
You need to either buy certificates or use letsencrypt to generate certificates to make sure you are running SSL.

## Troubleshooting

**Ansible**
If you want to use password authentication to your server, you need to install `sshpass`.
On linux:
```
apt-get install sshpass

# or

yum install sshpass
```

On mac: `brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb`




