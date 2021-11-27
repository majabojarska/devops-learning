# Proxmox

## Post-install

### Non-subscription software repositories

1. Disable enterprise repository:

   `mv /etc/apt/sources.list.d/pve-enterprise.list /etc/apt/sources.list.d/pve-enterprise.list.disabled`

1. Enable no-subscription repository ([source](https://pve.proxmox.com/wiki/Package_Repositories#sysadmin_no_subscription_repo)):

   `echo "deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list`

1. Upgrade the node:

   `apt update && apt upgrade -y`

1. Remove the subscription notice in the web client. Credits go to [John McLaren](https://johnscs.com/remove-proxmox51-subscription-notice/):

   `sed -Ezi.bak "s/(Ext.Msg.show\(\{\s+title: gettext\('No valid sub)/void\(\{ \/\/\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js && systemctl restart pveproxy.service`



## Creating VM templates

### Create the template

1. Create the VM from which the template will be created.
1. Install the QEMU guest agent:

   `sudo apt install qemu-guest-agent`

1. Enable QEMU guest in the VM's options.
1. Install cloud init:

   `sudo apt install cloud-init`

1. Empty out the `/etc/machine-id` file. This path is for Debian and might vary depending on other OSes. Useful one-liner:

   `sudo truncate -s 0 /etc/machine-id`.

1. Remove pre-generated SSH host keys. They will have to be regenerated on the clones later.

   `sudo rm /etc/ssh/ssh_host_*`.

1. Clean out any cache, orphaned packages, build dependencies, etc. Hints:

   - `sudo apt autoremove`
   - `sudo apt clean`

1. Power off the VM and convert it to a template.
1. Remove CD drive from template's hardware page.
1. Add a CloudInit drive.
1. Configure CloudInit.

### Clone the template

1. Clone the template - create a VM from it.
1. Change the machine's hostname in:

   - `/etc/hosts`
   - `/etc/hostname`

1. Regenerate the SSH host keys:

   `sudo dpkg-reconfigure openssh-server`

1. Reboot

# Manually delete a user

Delete user from `/etc/pve/user.cfg` and `/etc/pve/priv/shadow.cfg`.

## Firewall

- Do not enable firewall before configuring it (Default input policy is to `DROP` everyting).
- Lower-level rules override higher-level rules. For example, datacenter rules are overridden by node rules, if firewall is enabled on the node.

## Proxmox CLI

### Virtual Machines

`qm` - Qemu/KVM Virtual Machine Manager

`qm start <id>` - Start VM.

`qm shutdown <id>` - Gracefully shutdown VM.

`qm reset <id>` - Reset VM (kill and start immediately).

`qm stop <id>` - Kill VM immediately.

`qm set <id> --onboot 0` - Disable start on boot for VM.

`qm config <id>` - Get VM config.

### Containers

`pct` - Tool to manage Linux Containers (LXC) on Proxmox VE

`pct list` - List containers.

`pct config <id>` - Get container config.

`pct enter <id>` - Open shell session on container.

