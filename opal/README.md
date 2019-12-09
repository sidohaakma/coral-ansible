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
cd /usr/local/coral/coral-docker-dist_2.5.1/scripts/setup-host/linux/;
./setup-host.sh
```

Follow the instructions in the script.

**IMPORTANT: after reboot run the same script again**

Follow the instructions in the script.

### Deploy the stack
Please execute (run as user with root privileges):

``bash
cd /usr/local/coral/coral-docker-dist_2.5.1/scripts/deployment;
python3.6 fresh_start.py
```

Follow the instructions in the script.

You are all set! The application is available under: https://domain.org/repo.