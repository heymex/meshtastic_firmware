# Development Mode - Rate Limit Bypass

This document describes the development mode feature that allows bypassing rate limits for small-scale private test environments.

## Overview

Development mode bypasses rate limiting to speed up firmware development and testing. **WARNING: Only enable this in small-scale private test environments!** Enabling this in production or large networks can cause network congestion and performance issues.

## What Gets Bypassed

When `MESHTASTIC_DEVELOPMENT_MODE` is enabled, the following rate limits are bypassed:

1. **Traceroute** - 30 second limit → No limit
2. **Position/Waypoint/Alert/Telemetry** - 10 second limit → No limit  
3. **Text Messages** - 2 second limit → No limit
4. **NodeInfo minimum send time** - 5 minutes (normal) / 60 seconds (short timeout) → No limit
5. **TraceRoute cooldown** - 30 second cooldown → No limit

## How to Enable

### Option 1: Uncomment in configuration.h

Edit `src/configuration.h` and uncomment line 465:

```cpp
#define MESHTASTIC_DEVELOPMENT_MODE 1
```

### Option 2: Define in build environment

Add the flag to your build command or platformio.ini:

```ini
build_flags = 
    -DMESHTASTIC_DEVELOPMENT_MODE=1
```

Or via command line:
```bash
-DMESHTASTIC_DEVELOPMENT_MODE=1
```

## Modified Files

The following files were modified to support development mode:

- `src/configuration.h` - Added development mode flag definition
- `src/mesh/PhoneAPI.cpp` - Bypasses PhoneAPI rate limits
- `src/modules/TraceRouteModule.cpp` - Bypasses traceroute cooldown
- `src/modules/NodeInfoModule.cpp` - Bypasses NodeInfo minimum send time

## Important Notes

- **Network Safety**: Rate limits exist to prevent network congestion. Bypassing them can cause issues in larger networks.
- **Testing Only**: This feature is intended for development and small-scale testing only.
- **Compile-Time Flag**: This is a compile-time flag, so you must rebuild the firmware to enable/disable it.
- **Other Limits**: Some limits (like channel utilization checks) are still enforced for network safety.

## Example Use Cases

- Rapid testing of node discovery and advertisement
- Quick iteration on traceroute functionality
- Testing message delivery without waiting for rate limit windows
- Development workflows where waiting minutes between tests slows productivity

