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

## 2.00 Download the latest Jackett Server configuration file & Indexers
The Jackett preferences file is named `ServerConfig.json`. Jackett indexers are links torrent sites. In the event you want to upgrade and overwrite your `ServerConfig.json` and Jackett indexers you can with these instructions. If you use private indexers add them using the Jackett [WebUI](http://192.168.30.113:9117/UI/Dashboard).

With the Proxmox web interface go to `typhoon-01` > `113 (deluge)` > `>_ Shell` and type the following:
```
# Here we update the Jackett Server configuration file
sudo systemctl stop jackett &&
sleep 5 &&
wget -q https://raw.githubusercontent.com/ahuacate/jackett/master/ServerConfig.json -O /home/media/.config/Jackett/ServerConfig.json &&
chown 1005:1005 /home/media/.config/Jackett/ServerConfig.json &&
# Here we update the jacket indexers
rm /home/media/.config/Jackett/Indexers/* &&
svn checkout https://github.com/ahuacate/jackett/trunk/Indexers /home/media/.config/Jackett/Indexers &&
chown 1005:1005 {/home/media/.config/Jackett/Indexers/*.json,/home/media/.config/Jackett/Indexers/*.bak} &&
sudo systemctl restart jackett
```

## 00.00 Patches & Fixes
All CLI commands performed in the `typhoon-01` > `113 (deluge)` > `>_ Shell` :

**Restart Deluge**
```
sudo systemctl restart jackett
```
