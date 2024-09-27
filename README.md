# Assetto-Corsa-Server-Manager-With-Docker-In-An-LXC-Cortainer
## Installing Assetto Corsa Server Manager in a Proxmox LXC Contatiner with CGNAT Network with the help of Tailscale.

## Step 1: Create an LXC Container in your Proxmox server.

## Step 2: /dev/null/tun issue with Tailscale.
- Shutdown the LXC Container
- Go to your Proxmox host shell.
- On the proxmox host look in /etc/pve/lxc/, for the ID of the LXC you want to use Tailscale in. For example in my case for LXC ID=202, I edited /etc/pve/lxc/202.conf
```
nano /etc/pve/lxc/[NODEID].conf
```
- Add the code from [LXC.conf](https://github.com/CottonKendy/Assetto-Corsa-Server-Manager-01/blob/main/LXC.conf).
- Start the LXC container.

## Step 3: Install Docker.
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

## Step 4: Go to the home directory and create a new directory. Name it "acserver". Then cd into it.
```
sudo mkdir acserver
cd acserver
```

## Step 5: Go into your "acserver" directory. And create two files.
```
sudo touch docker-compose.yaml
sudo touch config01.yml
```

## Step 6: Edit the files you've created.
```
sudo nano docker-compose.yaml
```
- Add the code from [/acserver/docker-composer.yaml](https://github.com/CottonKendy/Assetto-Corsa-Server-Manager-01/blob/main/acserver%2Fdocker-compose.yaml)
```
sudo nano config01.yml
```
- Add the code from [/acserver/config01.yml](https://github.com/CottonKendy/Assetto-Corsa-Server-Manager-01/blob/main/acserver%2Fconfig01.yml)


## Step 7: Go into your mnt directory. And create a new directory named "sim". And CD into it.
```
cd /mnt
sudo mkdir sim
cd sim
```

## Step 8: Create two new directories and two new files.
```
sudo mkdir volumemanager01
sudo mkdir content
sudo touch server-manager01.db
sudo touch server-manager01.log
```

## Step 9: Go back to the "acserver" directory.
```
cd /home/acserver
```

## Step 10: Run this ***docker*** command.
```
sudo docker run --entrypoint /bin/sh --volume /mnt/sim/volumemanager01:/data -it steamcmd/steamcmd:latest
```

## Step 11: Run this ***steamcmd*** command.
```
steamcmd +force_install_dir /data +login [steam username] [steam password] +@sSteamCmdPlatformType windows +app_update 302550 +quit
```
> Enter your Steam Guard code if asked. You can get it from your Steam app from your phone.
```
exit
```

## Step 12: Go back to mnt directory as we need to copy the content from "/volumemanager01/content" directory into the "content" directory we created earlier.
```
cd /mnt/sim/volumemanager01/content
sudo cp -R ./* ../../content
```

## Step 13: Fixing Permissions for the Volume. Go back to the "sim" directory and use this commands.
```
cd ../../ (to go back into the "sim" directory)
sudo chown -R [user]:[user] volumemanager01
sudo chown -R [user]:[user] content
cd ../ (to go back into the "mnt" directory)
sudo chown -R [user]:[user] sim
```

## Step 14: Fixing Permissions for the "acserver" directory. Go into the "home" directory. Then cd into it.
```
cd /home
sudo chown -R [user]:[user] acserver
cd acserver
```

## Step 15: Running the Docker Compose File.
```
sudo docker compose up
```
> We are not yet going to use the "-d" command to see if the container is working properly.
> If all looks well and working. Go into the IP address of the LXC Container to see if the webpage is working.
> If working, run the command with the "-d" to run it in the background
```
sudo docker compose up -d
```

## Step 16: Login to the AC Server Webpage. And create a new password after first login.
```
username: admin
passowrd: servermanager
```

## Step 17: Setting Up Tailscale. Open up the Tailscale website on a new browser tab.
- Create an account if you haven't yet.
- On the Tailscale website, click on Machines, add a client, then choose the Linux version.
- Copy the command and run it on your LXC Container.
- After the installation, run this command:
```
sudo tailscale up
```
- Copy the link that will appear. And open it on a new browser tab.
- Sign in into your Tailscale account to add the machine into your list.
- Copy the Tailscale IP address of the ac-server machine and keep it safe for now.
- Install the Tailscale Client on your Windows machine/pc as well.
- Check your machine list, it should list both the ac-server machine and your windows pc. Make sure that both appear as connected.

## Step 18: Changing the settings to work properly with your AC Server IP Address.
- Go to options
- In the UDP and TCP Port put: 29600
- In the HTTP Port put: 28081
- Turn off the "Register to Lobby" to prevent the server from trying to get listed on the Kunos main list. (This server acts as a LAN server.)
- Scroll down to click on "Save".
> Optional: Change the "Name", "Password", and "Admin Password" as you see fit.

## Step 19: Running a Test race.
- Click on Quick Race.
- Choose a track and car.
- Start Server
- On the upper left side of the webpage, click on the join button.
- Click on Join on the newly opened webpage. If promted to open "Content Manager", click it.
> Notice that the server will not appear and fail to load as the IP address that the ac-server link is using is the CGNAT Public IP address.

## Step 20: Remember the ac-server IP address from Tailscale? We will be using that now.
- On your Join Link. Replace the IP address at the end of the link with the IP from the Tailscale Machine.
> Sample Link: https://acstuff.ru/s/q:race/online/join?httpPort=28081&ip=136.xxx.xxx.227  -> change the 136.xxx.xxx.227 into 100.xxx.xxx.1
- Now try the new link with the Tailscale IP address.

## Enjoy :)

## Getting other people to join your server
We will need to share the ac-server machine from Tailscale to other users.

Step 1: Sign in into your Tailscale account.

Step 2: Click on your ac-server machine.

Step 3: Click on share, then choose "Copy Share Link".
> You can click on reusable link if you plan on sharing it with a lot of friends.

Step 4: Share the link to them, make sure that they already have Tailscale on their Windows Machine and the Tailscale Client is running.

## Enjoy Part 2 :)
