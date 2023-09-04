# Tunguska Homelab

Implementation of an _opinionated_ homelab using ansible. This project started to solve a personal itch and now expanded to server any homelab deployment. 

All roles assume that the server is a Debian 11 machine, but no other package or dependency is assumed. We leverage [linuxserver]() packages when possible, [ZFS]() and other standard linux tools. We try to avoid out-of-repository packages as possible.

## Roles available

There are two types of roles: _base roles_ and _service roles_. Base roles can be deployed without depending on any other role. They build the fundamentals for a homelab server, and can be deployed independently.

Service roles need a sequence o base roles to be functional. We use a dependency list for these roles in each playbook.

### Base roles

- bootstrap: start configuring a barebones Debian 11 installation with sudo and Pytyhon
- linuxbase: install the basic packages that all other roles will use
- containerserver: install Docker and other container tools
- webserver: deploys [Caddy]() as webserver and reverse proxy for all services

### Service roles

- databaseserver: databases and block object storage services
- dnsfilter: [blocky]() and [pihole]() installation
- fileserver: configures Samba and ZSH NFS file sharing services
- homeserver: services for auxiliary tools and services. Deploys [FileBrowser](), [Grafana](), [Prometheus](), [Statping](), [Vaultwarden]() and [Code Server]().
- linuxbackup: configures [borgbackup]() and [borgmatic]()
- mediaserver: deploys parts of the *arr stack ([Radarr](), [Sonarr]()), [jackett](), [Jellyfin](), [transmission](), [beets]() and [headphones]().
- radioserver: configures [SDR]() using [SoapySDR]()
- soundserver: configures [Shairport] Sync() for Airplay
- virtualizationserver: uses [libvirt]() to enable [KVM]()-based virtual machines
- xmbcserver: deploys [Kodi]() as XMBC solution

## Usage

1. First, configure your roles and servers on the `inventories/production/hosts.ini` to suit your needs. 
2. Check that you copied your ssh keys to the server (using `ssh-copy-id` for example). 
3. Evaluate the contents of the `inventories/production/group_vars/all/all.yml` and configure it to your needs.
4. Create a vault file and configure the secret values there. More instructions can be found [here]().
5. Choose one of the roles and edit the `roles/<name of the role>/vars/main.yml` file to configure role-specific variables. 
6. Create a Python virtualenv using the [venv]() module and install the required packages with

```bash
pip install -r requirements.txt
```

7. Activate the virtualenv and run one of the service roles:

```bash
ansible-playbook -i inventories/production/hosts.ini roles/mediaserver.yml
```

The installation and configuration process should proceed. 

That should be it. Good luck!


