# teleinfo-mqtt


  ![Travis](https://img.shields.io/travis/fmartinou/teleinfo-mqtt/master)
  ![Maintainability](https://img.shields.io/codeclimate/maintainability/fmartinou/teleinfo-mqtt)
  ![Docker pulls](https://img.shields.io/docker/pulls/fmartinou/teleinfo-mqtt)

<a href="https://www.buymeacoffee.com/61rUNMm" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>  
===========================================

Read teleinfo from serial port and publish with mqtt.

## Usage
Run the official Docker image (i386, amd64, armv6, armv7, arm64).

### Run
```
docker run -d --name teleinfo-mqtt          \
    --device=/dev/ttyUSB0:/dev/ttyUSB0      \ # add serial port device from host
    - e MQTT_URL=mqtt://my_mqtt_broker:1883 \ # set mqtt broker url
    fmartinou/teleinfo-mqtt
```

### Configure
Configuration uses environment variables.

| Env var         | Description                               | Default value          |
|-----------------|-------------------------------------------|------------------------|
|LOG_LEVEL        | Log level (INFO, DEBUG, ERROR)            | INFO                   |
|SERIAL           | Serial Port location                      | /dev/ttyUSB0           |
|MQTT_URL         | MQTT Broker connection URL                | mqtt://localhost:1883  |
|MQTT_USER        | MQTT user     (optional)                  |                        |
|MQTT_PASSWORD    | MQTT password (optional)                  |                        |
|IDENTIFIER       | Identifier for Home-Assistant Discovery   |                        |
|DISCOVERY_PREFIX | Topic prefix for Home-Assistant Discovery | homeassistant          |
|EMIT_INTERVAL    | Interval between 2 MQTT emissions (ms)    | 0                      |

### MQTT topics
The frames are published to the topic `teleinfo`.

If you define an identifier, the frames are published to the topic `teleinfo/$identifier`.

### MQTT messages
MQTT JSON messages may vary upon your Electricity meter.  
Values are sanitized and converted to Numbers if possible (you can still access the original `raw` value if necessary).

Example
```json
{
    "ADCO": {
        "raw": "12345678901",
        "value": 12345678901
    },
    "OPTARIF": {
        "raw": "HC..",
        "value": "HC"
    },
    "ISOUSC": {
        "raw": "45",
        "value": 45
    },
    "HCHC": {
        "raw": "9058688",
        "value": 9058688
    },
    "HCHP": {
        "raw": "9752846",
        "value": 9752846
    },
    "PTEC": {
        "raw": "HP..",
        "value": "HP"
    },
    "IINST": {
        "raw": "22",
        "value": 22
    },
    "IMAX": {
        "raw": "90",
        "value": 90
    },
    "PAPP": {
        "raw": "4110",
        "value": 4110
    },
    "PAPP": {
        "raw": "A",
        "value": "A"
    }
}
```

### Home Assistant MQTT discovery
Home Assistant can automatically discover the teleinfo sensor. \
[See here](https://www.home-assistant.io/docs/mqtt/discovery/). 

#### MQTT Integration view
![Integration](docs/images/integration.png)

#### MQTT Entities view
![Integration](docs/images/entity.png)

### Changelog

#### 3.0.0
- :star: Add Homeassistant MQTT Discovery capabilities 
- :star: Upgrade all dependencies

#### 2.3.1
- :star: Upgrade all dependencies

#### 2.3.1
- :star: Upgrade all dependencies

#### 2.3.0
- :star: Upgrade to node 14
- :star: Upgrade all dependencies

#### 2.2.0
- :star: Prevent identical frames to be published

#### 2.1.0
- :star: Add ARM v6 support

#### 2.0.0
- :star: Add multi arch Docker images (i386 / amd64 / armv7 / arm64)

#### 1.0.0
- :star: Initial release
