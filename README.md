# hhz-hackathon-team-beacon

## Project goal


## Development process
- Install Raspbian Jessie Lite (Kernel 4.4) on Raspberry Pi 3
- Enable SSH access by creating an empty 'ssh' file in root directory of Raspberry Pi 3


- Connect Raspberry Pi 3 via LAN and configure WLAN via SSH
```
sudo iwlist wlan0 scan
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
- Enter SSID and PSK as follows:
```
network={
    ssid="MySSID"
    psk="MyWifiPassword"
}
```
