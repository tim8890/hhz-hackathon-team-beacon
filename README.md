# hhz-hackathon-team-beacon

## Project goal


## Development process
- Install Raspbian Jessie Lite (Kernel 4.4) on Raspberry Pi 3
- Enable SSH access by creating an empty 'ssh' file in root directory (/boot) of Raspberry Pi 3


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
- Expand file system and set timezone to Berlin in internationalization settings
```
sudo raspi-config
```
- Install Home Assistant on Raspberry Pi 3
```
wget -Nnv https://raw.githubusercontent.com/home-assistant/fabric-home-assistant/master/hass_rpi_installer.sh && chown pi:pi hass_rpi_installer.sh && bash hass_rpi_installer.sh
```
