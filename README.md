# ESPHome water pulse meter

## Description:
MCU: ESP8266_AC or WemosD1_mini

## Links:

ESP8266_AC: 

or 

WemosD1 mini:
AC/DC Power supply:



## Schema Wemos D1 mini:

![Wemos D1 mini](https://github.com/peca2345/ESPHome-water-pulse-meter/blob/main/WemosD1mini_schema.png)

## Schema ESP8266_AC:
![ESP8266 AC](https://github.com/peca2345/ESPHome-water-pulse-meter/blob/main/ESP8266_AC_schema.png)


## ESPHome code:

esphome:
  name: home-water-pulse-meter
  
esp8266:
  board: esp01_1m
  
logger:
api:
captive_portal:

ota:
  password: "4ddc5ac48879ecc7d06b3c031u497e8d"

wifi:
  networks:
  - ssid: !secret wifi_ssid
    password: !secret wifi_password
  manual_ip:
    static_ip: 192.168.1.232
    gateway: 192.168.1.1
    subnet: 255.255.255.0  
    dns1: 192.168.1.1
    dns2: 1.1.1.1


time:
  - platform: sntp
    id: my_time

sensor:
  - platform: pulse_meter
    pin:
      number: GPIO5
      mode: INPUT_PULLUP
    unit_of_measurement: 'pulse'
    name: 'water_pulse_meter'
    icon: 'mdi:water'
    internal_filter: 100ms
    filters:
      - multiply: 0.5
    total:
      name: 'water_pulse_meter_total'
      icon: 'mdi:water'
      unit_of_measurement: 'l'
      id: total
      filters:
      - multiply: 0.5

  - platform: total_daily_energy
    name: "water_pulse_meter_daily"
    power_id: total  
    unit_of_measurement: l 


## Tips:
