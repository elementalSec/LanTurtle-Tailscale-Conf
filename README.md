# LanTurtle-Tailscale-Conf
Configure a Lan Turtle from Hak5 with SD card to work with Tailscale (https://hak5.org/products/lan-turtle)

Kind of a port from here: https://github.com/adyanth/openwrt-tailscale-enabler just some parts I couldn't get working so I did a bit of a mix-n-match.

# Pre Tailscale install/configuration
1. Follow these steps for setting up the Lan Turtle https://docs.hak5.org/lan-turtle/lan-turtle-by-hak5/
2. Make sure you're running the latest firmware which is 6.2 from 2019 -> https://downloads.hak5.org/turtle/mk1
3. Make sure you have the recovery firmware on hand in case you sort of brick this thing like I did _multiple times_ (https://downloads.hak5.org/api/devices/lanturtle/firmwares/recovery)
4. Finally, the Lan Turtle should be connected to the internet for this (I think that goes without saying)


# Installing and configuring tailscale
1. `cd /sd`
2. `wget https://pkgs.tailscale.com/stable/tailscale_1.96.4_mips.tgz`
3. `gunzip tailscale_1.96.4_mips.tgz && tar -xvf tailscale_1.96.4_mips.tar`
4. Before we do anything else let's update the packages and download some dependencies
   1. `opkg update`
   2. `opkg install libustream-openssl ca-bundle kmod-tun`
5. `ln -s /sd/tailscale_1.96.4_mips/tailscale /usr/bin/tailscale` (symbolic link since there isn’t any real room outside of /sd to store these files)
6. `ln -s /sd/tailscale_1.96.4_mips/tailscaled /usr/bin/tailscaled` 
7. `tailscaled --state=/sd/tailscaled.state &` 
8. `tailscale up` it will provide a login link like so `https://login.tailscale.com/a/dasjdhaksda398` authenticate to your tailnet, approve the device if you have that setting enabled, and remove key expiry or we will lose access to the device after x days
9. create this [file](etc/init.d/tailscale) in /etc/init.d/tailscale (Allows tailscale to start on boot and all that good stuff we need)
10. `chmod +x /etc/init.d/tailscale`
11. `/etc/init.d/tailscale start`
12. `/etc/init.d/tailscale enable`
13. `/etc/init.d/tailscale enable`
14. `ls /etc/rc.d/S*tailscale*` -> you should see something like `/etc/rc.d/S99tailscale`
