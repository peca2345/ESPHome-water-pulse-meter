# ESPHome - water pulse meter

## Description:

Tutorial for reading water meter pulses with cable reed contact using ESP8266 and ESPHome.

## Tutorial:

ESPHome: [YTB](https://www.youtube.com/watch?v=mj-24SZLQKk) 

## Links:

Water meter: ENBRA DE-13-MI001-PTB002 with reed sensor. ( 1l = 1 pulse )

and

ESP8266 AC 1ch relay: [Ali](https://www.aliexpress.com/item/1005001631140958.html?dp=61e453ee0fa2025c4ba43400&cn=ah&aff_fcid=bc98419ec5b0425abf957a328f7c1556-1645572122033-04627-_AnTGXs&tt=CPS_NORMAL&aff_fsk=_AnTGXs&aff_platform=portals-)

USB/TTL programmer (set): [Ali](https://www.aliexpress.com/item/4000120687489.html?spm=a2g0o.productlist.0.0.1c584f39Jcl6io&algo_pvid=9b1a0e4e-551b-4ba6-af72-74ef9ac96b87&aem_p4p_detail=2022022216244713238668511815050004685000&algo_exp_id=9b1a0e4e-551b-4ba6-)

or 

WemosD1 mini: [Ali](https://www.aliexpress.com/item/1005003430551175.html?spm=a2g0o.productlist.0.0.2b6649f2d9lNvP&algo_pvid=999515b6-0dda-4fce-94ff-490172a0ba2c&aem_p4p_detail=2022022216235813155484689130080004686856&algo_exp_id=)

AC/DC Power supply: [Ali](https://www.aliexpress.com/item/1005001432291885.html?dp=61e453ee0fa2025c4ba43400&cn=ah&aff_fcid=e91f783db22b487fac9475b898675279-1645575787579-00803-_d6jWDbY&aff_fsk=_d6jWDbY&aff_platform=link-c-)



## Schema - Wemos D1 mini:

![Wemos D1 mini](https://github.com/peca2345/ESPHome-water-pulse-meter/blob/main/WemosD1mini_schema.png)

## Schema - ESP8266 AC:
![ESP8266 AC](https://github.com/peca2345/ESPHome-water-pulse-meter/blob/main/ESP8266_AC_schema.png)


## ESPHome code:

```
esphome:
  name: home-water-pulse-meter
  
esp8266:
  board: esp01_1m
  
logger:
api:
captive_portal:

ota:
  password: "4ddc5ac48879ecc7d06a3c031e497e8d"

wifi:
  networks:
  - ssid: !secret wifi_ssid
    password: !secret wifi_password

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
    internal_filter: 100ms # shut down and monitor the log if there are problems
    filters:
      - multiply: 0.5 # shut down and monitor the log if there are problems
    total:
      name: 'water_pulse_meter_total'
      icon: 'mdi:water'
      unit_of_measurement: 'l'
      id: total
      filters:
      - multiply: 0.5 # shut down and monitor the log if there are problems

  - platform: total_daily_energy
    name: "water_pulse_meter_daily"
    power_id: total  
    unit_of_measurement: l
```


## Tips:

In case of a problem with reading data from the sensor, disable filters/multiply and internal_filter in the program and check the log.
