---
title: Yardian
description: Instructions on how to integrate a Yardian smart irrigation controller within Home Assistant.
ha_category:
  - Irrigation
  - Binary Sensor
  - Sensor
  - Switch
ha_config_flow: true
ha_release: 2023.9
ha_iot_class: Local Polling
ha_codeowners:
  - '@aeon-matrix'
ha_domain: yardian
ha_platforms:
  - binary_sensor
  - sensor
  - switch
ha_integration_type: device
---

The **Yardian** {% term integration %} allows you to control your [Yardian Smart Sprinkler Controller](https://www.yardian.com/products/yardian-pro-smart-sprinkler-controller/). It supports Yardian Pro with both the regular firmware and the [standalone firmware](https://www.yardian.com/yardian-pro-standalone-firmware/).

{% include integrations/config_flow.md %}

## Discovery

Yardian devices are automatically discovered by Home Assistant. When a new device is found on your network, it will appear as a discovered integration under **Settings** > **Devices & Services**.

## Configuration

If the device is not discovered automatically, you can add it manually. During the configuration, you will need to provide the **Host** (IP address) and the **Access Token**. You can find these inside your [Yardian App](https://www.yardian.com/app/).

![Yardian Host/Token Location](/images/integrations/yardian/yardian_config_flow.jpg)

## Entities

The integration provides several entities to monitor and control your irrigation:

### Binary sensors
- **Watering running**: Is `on` when a zone is currently irrigating.
- **Standby**:  Is `on` when the controller is in standby mode.
- **Freeze prevent**: Turns on when the controller enables freeze prevention.
- **Zone enabled**: `On` if a zone is enabled. These entities are disabled by default and created per zone.

### Sensors
- **Status**: The current operation mode (e.g., Water Control, Idle).

### Switch
- **Zone Switches**: Allows starting or stopping a specific irrigation zone.


## Actions

### yardian.start_irrigation
Start a zone for a given number of minutes. This action accepts a Yardian Zone switch entity and a duration.

| Data attribute | Optional | Description |
| ---------------- | -------- | ----------- |
| `entity_id`      | no       | The Yardian Zone switch to turn on. |
| `duration`       | no       | Number of minutes for this zone to be turned on. |

#### Example
```yaml
action: yardian.start_irrigation
target:
  entity_id: switch.yardian_zone_1
data:
  duration: 15
```

### yardian.stop_irrigation
Stop irrigation for a specific zone.

| Data attribute | Optional | Description |
| ---------------- | -------- | ----------- |
| `entity_id`      | no       | The Yardian Zone switch to turn off. |

#### Example
```yaml
action: yardian.stop_irrigation
target:
  entity_id: switch.yardian_zone_1
```
  
### yardian.stop_all_irrigation
Stop all current irrigation activities across all zones for a specific Yardian device.

| Data attribute | Optional | Description |
| ---------------- | -------- | ----------- |
| `device_id`      | no       | The Yardian device to stop all irrigation on. |

#### Example
```yaml
action: yardian.stop_all_irrigation
target:
  device_id: YOUR_YARDIAN_DEVICE_ID
```
