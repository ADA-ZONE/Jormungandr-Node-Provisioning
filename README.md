# Jormungandr Node Provisioning

This script is intended to make it easy for you to deploy your pool to a server (Linux, tested on Ubuntu 18.04) with all the settings and configurations so you don't have to manually edit system configs and install packages every time you want to try a new setup.

Clone this repo and tweak anything you need and want to experiment. If you want to make changes to the server, it's best practice to make edits to this local repo and re-run so you always have a blueprint of your setup, which is very useful when trying new environments etc. You can also have a git history of changes. This is the power of having all your setup as code.

Ansible playbooks and resources to provision an Ubuntu 18.04 to run a
Jormungandr Systemd service.

Settings have followed the Cardano's community suggestions.
Special thanks to https://gist.github.com/ilap/54027fe9af0513c2701dc556221198b2

* You can add/edit any system settings in `vars/static.yml` - currently using settings from the gist references above
* You can edit the jormungandr ports in `bootstrap.yml`
* This script installs `chrony` and disables `timesyncd` and uses settings from the gist referenced above
* You can set the version of Jormungandr you want to install in `bootstrap.yml`, currently at `0.8.9` - e.g. 
```
...
    vars:
      jormungandr_max_unreachable_nodes_to_connect_per_event: 1
      jormungandr_rest_port: 3000
      jormungandr_public_address_port: 3001
      binary_version: 0.8.9 <-----------
...
 ```

## Prerequisites

Ansible 2.9.x and dependencies. This is an Ansible script which is software for the purposes of provisioning.

Access to server instances as defined in `inventory/static.yml` over ssh.
Make sure you edit `inventory/static.yml` to point to the server you wish to provision and that the machine you are running this from can SSH into that server - i.e. has the ssh key.

```
all:
  hosts:
    yourservername: # this is what your server will be referred as, change to what you like
      ansible_host: INSERT_SERVER_IP
      ansible_user: INSERT_SERVER_USER
```

## Dependencies

You'll need to install the roles needed for this script. Jormungandr has been abstracted as a role using Ansible's registry Galaxy.
You can install Galaxy roles with:

`ansible-galaxy install -r requirements.yml`

## Running

Once this is all done, use the below command to run this playbook and provision your server. Replace `$VARS` with a list of your pool's specific variables.

`ansible-playbook bootstrap.yml -i inventory -l yourservername -e $VARS`

NOTE: `yourservername` must match that of the one you set above in `inventory/static.yaml`

Example:

```ansible-playbook bootstrap.yml -i inventory -l myubuntuserver -e "node_sig_key=kes25519-12-sk1qqqq... node_vrf_key=vrf_sk1df3ycv... node_pool_id=7ab2f7b..."```

### Required variables

Three variables are required for the `node_secret.yaml` file:

- `node_sig_key`
- `node_vrf_key`
- `node_pool_id`

### Optional variables

You can override the trusted peers by adding `trusted_peers:` under `vars` in the `bootstrap.yaml`.

e.g.

```
...
    vars:
      jormungandr_max_unreachable_nodes_to_connect_per_event: 1
      jormungandr_rest_port: 3000
      jormungandr_public_address_port: 3001
      binary_version: 0.8.9
      trusted_peers:
        - address: ...
            id: ...
        - address: ...
            id: ...
...
 ```
