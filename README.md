# Yeelight to MQTT bridge

Works with Yeelight WiFi devices. So far I've tested it on the following ones (non exhaustive list) :
- Yeelight LED Ceiling - YLXD01YL 
- Yeelight LED Bulb 1S Color - YLDP13YL
- Yeelight Lightstrip - YLDD04YL
- Yeelight Mijia Desk Lamp - MJTD01YL
- Yeelight Bedside - MJCTD01YL

You need to activate developer mode (https://www.yeelight.com/faqs/lan_control)

Bridge accept following MQTT set:
```
"home/light/main-color/status/set" -> on
```

will turn on light and translate devices state from gateway:
```
"home/light/main-color/status" on
"home/light/main-color/ct" 3500
"home/light/main-color/bright" 3
"home/light/main-color/rgb" 1247743
```

## Folder structure
Create the following folder structure for your docker container:
```
yeelight/
|-- docker-compose.yaml
`-- config
     |-- config.yaml
```
## docker-compose.yaml
Sample docker-compose.yaml file for user:
```
yeelight:
  image: "monster1025/yeelight-mqtt"
  container_name: yeelight
  volumes:
    - "./config:/app/config"
  restart: always
```
## config.yaml
Copy file config/config-sample.yaml from GitHub to your local docker /config folder and rename it to config.yaml
Your MQTT broker and your Yeelight devices will be configured in this file.
```
mqtt:
  server: IP or Domain Name
  port: 1883 
  username: YourUserName
  password: YourPassword
  prefix: home (Root Topic)

sids:
  192.168.0.1:
    model: light
    name: kitchen-roof
```

With this configuration, the MQTT topics for this kitchen Yeelight device will be :
```
home/light/kitchen-roof/status
home/light/kitchen-roof/ct
home/light/kitchen-roof/bright
home/light/kitchen-roof/rgb
```

## Known bugs:
- Lamp's alive status updated only at startup.
