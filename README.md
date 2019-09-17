# Jackett Build
Not a lot to do here except maybe push a update.

Network Prerequisites are:
- [x] Layer 2 Network Switches
- [x] Network Gateway is `192.168.1.5`
- [x] Network DNS server is `192.168.1.5` (Note: your Gateway hardware should enable you to a configure DNS server(s), like a UniFi USG Gateway, so set the following: primary DNS `192.168.1.254` which will be your PiHole server IP address; and, secondary DNS `1.1.1.1` which is a backup Cloudfare DNS server in the event your PiHole server 192.168.1.254 fails or os down)
- [x] Network DHCP server is `192.168.1.5`

Other Prerequisites are:
- [x] Synology NAS, or linux variant of a NAS, is fully configured as per [SYNOBUILD](https://github.com/ahuacate/synobuild#synobuild).
- [x] Proxmox node fully configured as per [PROXMOX-NODE BUILDING](https://github.com/ahuacate/proxmox-node/blob/master/README.md#proxmox-node-building).
- [x] pfSense is fully configured as per [HAProxy in pfSense](https://github.com/ahuacate/proxmox-reverseproxy/blob/master/README.md#haproxy-in-pfsense)
- [x] Deluge LXC with Deluge SW installed as per [Deluge LXC - Ubuntu 18.04](https://github.com/ahuacate/proxmox-lxc-media/blob/master/README.md#400-deluge-lxc---ubuntu-1804).

Tasks to be performed are:
- [ ] 1.00 Configure Deluge Preferences
- [ ] 2.00 Download the latest Execute Plugin for FileBot deluge-postprocess.sh script
- [ ] 3.00 Download the latest Label Plugin configuration file
- [ ] 4.00 Download the latest Autoremoveplus Plugin configuration file
- [ ] 00.00 Patches & Fixes

## 1.00 Configure Jackett Preferences
Jackett is installed on the Deluge LXC container.

Your Jacket should be ready to go when you installed Jacket as shown instructions [HERE](https://github.com/ahuacate/proxmox-lxc-media/blob/master/README.md#500-jackett-lxc---ubuntu-1804). 

But if you want to modify or update your config with the latest from our GitHub repository read-on. 

## 2.00 Download the latest Jackett Server configuration file
The Jackett settings file is named `ServerConfig.json`. In the event you want to upgrade or overwrite your `ServerConfig.json` you can with these instructions. 

With the Proxmox web interface go to `typhoon-01` > `113 (deluge)` > `>_ Shell` and type the following:
```
systemctl stop jackett &&
sleep 5 &&
wget  https://raw.githubusercontent.com/ahuacate/deluge/master/deluge-postprocess.sh -P /home/media/.config/deluge &&
chmod +rx /home/media/.config/deluge/deluge-postprocess.sh &&
chown 1005:1005 /home/media/.config/deluge/deluge-postprocess.sh &&
sudo systemctl restart deluge
```

