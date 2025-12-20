# API Configuration Update

This document describes the API changes made to make the MQTT configuration fully customizable.

## Overview

The API has been updated to support configurable MQTT endpoints and connection parameters through environment variables. This allows users to customize their deployment without modifying source code.

## What Changed

### 1. MQTT Topics (API Endpoints)

Previously hardcoded MQTT topics are now configurable:

| Environment Variable | Default Value | Description |
|---------------------|---------------|-------------|
| `MQTT_TOPIC_STATUS` | `sherangthebell/status` | Bell online status topic |
| `MQTT_TOPIC_BELL` | `sherangthebell/bell` | Bell ring event topic |
| `MQTT_TOPIC_TAKE` | `sherangthebell/take` | Take her out event topic |

### 2. MQTT Connection Parameters

Additional MQTT configuration options:

| Environment Variable | Default Value | Description |
|---------------------|---------------|-------------|
| `MQTT_BROKER` | `broker.hivemq.com` | MQTT broker hostname |
| `MQTT_PORT` | `1883` | MQTT broker port |
| `MQTT_CLIENT_NAME` | `sherangthebell` | MQTT client identifier |

### 3. Arduino Updates

Both Arduino sketches (`SheRangTheBell` and `SheRangTheBellClient`) have been updated to use configurable topic definitions in their `defs.h` files.

## Migration Guide

### For Existing Deployments

No changes required! The default values match the previous hardcoded values, ensuring backward compatibility.

### For Custom Deployments

1. Copy `.env.example` to `.env`
2. Update the MQTT topic variables to match your requirements
3. Restart the application

Example custom configuration:
```bash
# Custom MQTT Topics
MQTT_TOPIC_STATUS=mydog/status
MQTT_TOPIC_BELL=mydog/ring
MQTT_TOPIC_TAKE=mydog/outside
```

### For Arduino Devices

Update the topic definitions in your Arduino `defs.h` file to match your Python configuration:

```c
#define mqttTopicStatus "mydog/status"
#define mqttTopicBell "mydog/ring"
#define mqttTopicTake "mydog/outside"
```

## Testing

All changes have been tested for:
- ✅ Backward compatibility with default values
- ✅ Custom configuration via environment variables
- ✅ Code quality and security (CodeQL scan passed)

## Benefits

1. **Flexibility**: Customize API endpoints without code changes
2. **Security**: Sensitive configuration in `.env` (not tracked in git)
3. **Multiple Deployments**: Run multiple instances with different topics
4. **Easier Testing**: Use separate topics for development/production
