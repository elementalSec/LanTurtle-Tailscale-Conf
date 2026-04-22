# LanTurtle-Tailscale-Conf
Configure a Lan Turtle from Hak5 to work with Tailscale

1. wget https://pkgs.tailscale.com/stable/tailscale_1.96.4_mips.tgz 
2. gunzip tailscale_1.96.4_mips.tgz && tar -xvf tailscale_1.96.4_mips.tar
3. `ln -s /sd/tailscale_1.96.4_mips/tailscale /usr/bin/tailscale`
4. `ln -s /sd/tailscale_1.96.4_mips/tailscaled /usr/bin/tailscaled`
5. `tailscaled --state=/sd/tailscaled.state &`
6. `tailscale up`
7. create this file in /etc/init.d/tailscale
```
#!/bin/sh /etc/rc.common

# Copyright 2020 Google LLC.
# SPDX-License-Identifier: Apache-2.0

USE_PROCD=1
START=99
STOP=1

start_service() {
  procd_open_instance
  procd_set_param command /usr/bin/tailscaled

  # Set the port to listen on for incoming VPN packets.
  # Remote nodes will automatically be informed about the new port number,
  # but you might want to configure this in order to set external firewall
  # settings.
  procd_append_param command --port 41641

  # OpenWRT /var is a symlink to /tmp, so write persistent state elsewhere.
  procd_append_param command --state /etc/config/tailscaled.state
  
  # Persist files for TLS cert & Taildrop files
  procd_append_param command --statedir /etc/tailscale/

  procd_set_param respawn
  procd_set_param stdout 1
  procd_set_param stderr 1

  procd_close_instance
}

stop_service() {
  /usr/bin/tailscaled --cleanup
}
```
8. `chmod +x /etc/init.d/tailscale`
9. `/etc/init.d/tailscale start`
10. `/etc/init.d/tailscale enable`
11. `/etc/init.d/tailscale enable`
12. `ls /etc/rc.d/S*tailscale*` -> You'll see something like `/etc/rc.d/S99tailscale`
