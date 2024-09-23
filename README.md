Assetto-Corsa-Server-Manager-01
Installing Assetto Corsa Server Manager in a Proxmox LXC Contatiner with CGNAT Network with the help of Tailscale

Step 1: Create an LXC Container in your Proxmox server.

Step 2: /dev/null/tun issue with Tailscale
- Go to your Proxmox host shell.
- On the proxmox host look in /etc/pve/lxc/, for the ID of the LXC you want to use Tailscale in. For example in my case for LXC ID=202, I edited /etc/pve/lxc/202.conf
- Add the code from LXC.conf
