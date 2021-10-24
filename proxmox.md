# Proxmox

## Creating VM templates

### Create the template

1. Create the VM from which the template will be created from.
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

### Clone the template

1. Clone the template - create a VM from it.
1. Change the machine's hostname in:

   - `/etc/hosts`
   - `/etc/hostname`

1. Regenerate the SSH host keys:

   `sudo dpkg-reconfigure openssh-server`

1. Reboot
