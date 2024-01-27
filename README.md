# smarthome.dietpi
With an sbc running [DietPi](https://dietpi.com) execute the following commands.

## Home Assistant
```
sudo usermod -a -G docker dietpi
docker run -d --name homeassistant --privileged --restart=unless-stopped -e TZ=America/Los_Angeles -v /home/dietpi/homeassistant/config:/config --network=host ghcr.io/home-assistant/home-assistant:stable
```

## MQTT
```
sudo mkdir -p /home/dietpi/mosquitto/config
sudo touch /home/dietpi/mosquitto/config/mosquitto.conf
sudo printf "listener 1883\nallow_anonymous true" > /home/dietpi/mosquitto/config/mosquitto.conf
docker run -d -it --name mosquitto --restart=unless-stopped -e TZ=America/Los_Angeles -p 1883:1883 -p 9001:9001 -v /home/dietpi/mosquitto/config/mosquitto.conf:/mosquitto/config/mosquitto.conf -v /home/dietpi/mosquitto/data:/mosquitto/data -v /home/dietpi/mosquitto/log:/mosquitto/log eclipse-mosquitto
```

## Node Red
```
sudo mkdir /home/dietpi/nodered/data
sudo chown 1000 /home/dietpi/nodered/data
docker run -d --name nodered --restart=unless-stopped -e TZ=America/Los_Angeles -it -p 1880:1880 -v /home/dietpi/nodered/data:/data nodered/node-red
```
