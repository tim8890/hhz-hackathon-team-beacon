# hhz-hackathon-team-beacon

## Project goal


## Used hardware
- Raspberry Pi 3
- Arduino Nano
- Radio module NRF24L01+
- Air/Humidity sensor DHT-22
- Onyx Beacon

## Architecture

Nodes -> Children Nodes


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

### Configuration of Home-Assistant
- The single point for configuration is the configuration.yaml inside `home/homeassistant/.homeassistant`. It is possible to outsource parts of the configuration for better maintainability.

- Add the MySensors component according to MySensors component instructions (https://home-assistant.io/components/mysensors/)

- As soon as Home-Assistant is restarted the reachable sensors will be added as entities and can be further used. Newly added sensors will be added automatically as they are seen by the Gateway.

- All sensors are persisted inside a .pickle file which guarantees that disconnected sensors will be recognized as soon as they are available again

## General commands
Change to homeassistant user 

```sudo su -s /bin/bash homeassistant```

Change to virtual enviroment

```source /srv/homeassistant/homeassistant_venv/bin/activate```

Update HA (after activation of virtual environment)

```pip3 install --upgrade homeassistant```

Type `exit` to logout the hass user and return to the pi user. The home-assistant service can be restarted via `sudo service 
home-assistant restart` as pi user or the Services menu of Home-Assistant.
