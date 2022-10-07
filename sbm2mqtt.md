### Docker

Build the Docker image:

```bash
docker build . -t sbm2mqtt
```

To spin up the Docker container, either use the following command replacing the environmental variable defaults with your own:

```bash
docker run -d --net=host --privileged -it -e REPORTING_INTERVAL=30 sbm2mqtt
```

Edit the `sbm2mqtt_config.py` file with your own information.


Available environmental variables

| Variable             | Default         | Description                                        |
| -------------------- | --------------- | -------------------------------------------------- |
| `MQTT_HOST`          | 127.0.0.1       | IP address of the MQTT broker                      |
| `MQTT_PORT`          | 1883            | Port of the MQTT broker                            |
| `MQTT_CLIENT`        | sbm2mqtt        | Name of the MQTT client                            |
| `MQTT_USER`          | xxxxxx          | MQTT user name                                     |
| `MQTT_PASS`          | xxxxxx          | MQTT password                                      |
| `MQTT_TOPIC`         | switchbot_meter | MQTT topic to monitor in Home Assistant, etc.      |
| `REPORTING_INTERVAL` | 300             | Time in seconds between each execution of sbm2mqtt |


### Integration with Home Assistant

Execute sbm2mqtt and note the MAC addresses of any SwitchBot meters it finds.

Add three ```sensor:``` entries to ```configuration.yaml``` for each Meter, as follows:

```yaml
sensor:
- platform: mqtt
  name: 'name_of_this_meter_temperature'
  state_topic: 'switchbot_meter/xx:xx:xx:xx:xx:xx' # MAC address of this meter
  value_template: '{{ value_json.temperature }}'
  unit_of_measurement: '°C' # Change to '°F' as appropriate
- platform: mqtt
  name: 'name_of_this_meter_humidity'
  state_topic: 'switchbot_meter/xx:xx:xx:xx:xx:xx' # MAC address of this meter
  value_template: '{{ value_json.humidity }}'
  unit_of_measurement: '%'
  icon: mdi:water-percent
- platform: mqtt
  name: 'name_of_this_meter_battery'
  state_topic: 'switchbot_meter/xx:xx:xx:xx:xx:xx' # MAC address of this meter
  value_template: '{{ value_json.battery }}'
  unit_of_measurement: '%'
  icon: mdi:battery
```
