# hhz-hackathon-team-beacon

## Project goal


## Development process
### Setup of the controller (Raspberry Pi 3)
- Install Raspbian Jessie Lite (Kernel 4.4) on Raspberry Pi 3
- Enable SSH access by creating an empty 'ssh' file in root directory (/boot) of Raspberry Pi 3

- Connect Raspberry Pi 3 via LAN and configure WLAN via SSH

`ssh pi@raspberrypi` (use IP alternatively)
`sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

- Enter SSID and PSK as follows
```
network={
    ssid="MySSID"
    psk="MyWifiPassword"
}
```
- Reboot Raspberry Pi 3
```
sudo reboot
```
- Expand file system and set timezone to 'Berlin' and keyboard layout to Generic 105-German in internationalization settings
```
sudo raspi-config
```
- Install Home Assistant on Raspberry Pi 3
```
wget -Nnv https://raw.githubusercontent.com/home-assistant/fabric-home-assistant/master/hass_rpi_installer.sh && chown pi:pi hass_rpi_installer.sh && bash hass_rpi_installer.sh
```

### Setup of sensor network
- Wire the radio to gateway and sensors (Arduino) according to MySensors instructions (https://www.mysensors.org/build/connect_radio)
- Wire measurement sensor (DHT22) to Arduino according to MySensors instructions (https://www.mysensors.org/build/humidity)






## General commands
Change to homeassistant user 
`sudo su -s /bin/bash homeassistant`
Change to virtual enviroment
`source /srv/homeassistant/homeassistant_venv/bin/activate`
Update HA
`pip3 install --upgrade homeassistant`
Type `exit` to logout the hass user and return to the pi user
