# Jormungandr Node Provisioning

This script is intended to make it easy for you to deploy your pool to a server (Linux, tested on Ubuntu 18.04) with all the settings and configurations so you don't have to manually edit system configs and install packages every time you want to try a new setup.

Clone this repo and tweak anything you need and want to experiment. If you want to make changes to the server, it's best practice to make edits to this local repo and re-run so you always have a blueprint of your setup, which is very useful when trying new environments etc. You can also have a git history of changes. This is the power of having all your setup as code.

Ansible playbooks and resources to provision an Ubuntu 18.04 to run a
Jormungandr Systemd service.

Settings have followed the Cardano's community suggestions.
Special thanks to https://gist.github.com/ilap/54027fe9af0513c2701dc556221198b2

## Prerequisites

Ansible 2.9.x and dependencies. This is an Ansible script which is software for the purposes of provisioning.

Access to server instances as defined in `inventory/` over ssh.
Make sure you edit `inventory/` to point to the server you wish to provision and that the machine you are running this from can SSH into that server - i.e. has the ssh key.

## Dependencies

You'll need to install the roles needed for this script. Jormungandr has been abstracted as a role using Ansible's registry Galaxy.
You can install Galaxy roles with:

`ansible-galaxy install -r requirements.yml`

## Running

Once this is all done, use the below command to run this playbook and provision your server. Replace `$VARS` with a list of your pool's specific variables.

`ansible-playbook bootstrap.yml -i inventory -e $VARS`

Example:

```ansible-playbook bootstrap.yml -i inventory -l myubuntuserver -e "node_sig_key=kes25519-12-sk1qqqq... node_vrf_key=vrf_sk1df3ycv... node_pool_id=7ab2f7b..."```

### Required variables

Three variables are required for the `node_secret.yaml` file:

- `node_sig_key`
- `node_vrf_key`
- `node_pool_id`

