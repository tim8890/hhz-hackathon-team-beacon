# hhz-hackathon-team-beacon

## Project goal

A sensor network as part of an IoT architecture should be implemented and beacons be deployed in order to provide access to the sensor data. The rooms at Herman Hollerith Centre shall be equipped with sensors measuring values like temperature, humidity and CO2 in order to assure an optimal climatic environment for students and lecturers. The sensor data of distinct areas can be viewed via a web interface. The access to the web interface will be provided by BLE beacons that have Eddystone URL enabled.

Easy maintainability, stability as well as scalability are of highest importance. 


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
- Enable SSH access by creating an empty file titled `ssh` in root directory (/boot) of Raspberry Pi 3
- Connect Raspberry Pi 3 via LAN (and configure WLAN via SSH)

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

`sudo reboot`

**Note:** Ideally the ethernet interface (eth0) should be used at HHZ as there are less obstacles to be overcome.

**Issue:** There is no static IP address for the Raspberry Pi as a DHCP server with IP leasing is used.

- Expand file system and set timezone to 'Berlin' and keyboard layout to Generic 105-German in internationalization settings

`sudo raspi-config`

- Install Home Assistant on Raspberry Pi 3

```
wget -Nnv https://raw.githubusercontent.com/home-assistant/fabric-home-assistant/master/hass_rpi_installer.sh && chown pi:pi hass_rpi_installer.sh && bash hass_rpi_installer.sh
```

### Setup of sensor network
- Wire the radio to gateway and sensors (Arduino) according to [MySensors instructions](https://www.mysensors.org/build/connect_radio).
- Wire measurement sensor (DHT22) to Arduino according to [MySensors instructions](https://www.mysensors.org/build/humidity).
- Upload sketches to Arduinos following the [official guide](https://www.arduino.cc/en/Main/Howto).

**Issue:** When using an Arduino clone with CH340 USB chip on a Mac there is currently no official USB FTDI driver so that the Arduino IDE will not find the USB port for these clones. Installing a wrong unsigned driver will result in kernel panic when connecting the Arduino. A fixed driver can be found [here](https://github.com/MaKin211/ch340g-ch34g-ch34x-mac-os-x-driver).

**Note:** For the DHT22 sensor to properly work the modified DHT-Library has to be imported as referenced in the [MySensors doc](https://www.mysensors.org/build/humidity).


### Configuration of Home-Assistant
- The single point for configuration is the configuration.yaml inside `home/homeassistant/.homeassistant`. It is possible to outsource parts of the configuration for better maintainability.

- Add the MySensors component according to [MySensors component doc](https://home-assistant.io/components/mysensors/).

- As soon as Home-Assistant is restarted the reachable sensors will be added as entities and can be further used. Newly added sensors will be added automatically as they are seen by the Gateway.

- All sensors are persisted inside a .pickle file which guarantees that disconnected sensors will be recognized as soon as they are available again instead of being identified as new ones.

**Issue:** With the wrong baudrate configured, an error like `mysensors.mysensors: Error decoding message from gateway, probably received bad byte.` might occur. It has to be ensured that the baudrate set up in the Arduino sketch matches the baudrate configured in the MySensors section of Home-Assistant. 

## General commands
- Change to homeassistant user:

`sudo su -s /bin/bash homeassistant`

- Change to virtual enviroment:

`source /srv/homeassistant/homeassistant_venv/bin/activate`

- Update HA (after activation of virtual environment):

`pip3 install --upgrade homeassistant`

- Type `exit` to logout the hass user and return to the pi user. The home-assistant service can be restarted via `sudo service 
home-assistant restart` as pi user or the Services menu of Home-Assistant.
