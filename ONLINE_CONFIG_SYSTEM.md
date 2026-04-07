# Online Config System - Complete Integration Guide

## Overview

This system allows users to configure their Scythe Internal cheat through a web dashboard, with configs stored in Firebase and automatically fetched by the C++ injector at runtime.

## System Architecture

```
User Dashboard (goldenestate.homes/dashboard)
    ↓ (Saves config to Firebase)
Firebase Realtime Database
    ↓ (Queries by license key)
API Endpoint (goldenestate.homes/api/config?license=KEY)
    ↓ (HTTP GET request)
C++ Injector (private.exe)
    ↓ (Replaces placeholders)
Lua Script (internal.lua)
    ↓ (Executes in Roblox)
Game
```

## Components

### 1. Web Dashboard (`dashboard.html`)

**Location**: `C:\Users\rayzh\Downloads\GoldEstates\dashboard.html`

**Features**:
- Firebase Authentication (email/password)
- License key validation
- JSON config editor with Monaco Editor
- Auto-save to Firebase
- Saved configurations management

**Config Structure**:
```json
{
  "settings": {
    "general": {
      "enabled": true,
      "debug": true
    },
    "aimbot": {
      "enabled": true,
      "mode": "mouse",
      "smoothness": 1,
      "fov": 200,
      "fovRadius": 10,
      "holdMode": true,
      "wallCheck": false,
      "teamCheck": false,
      "sticky": {
        "enabled": true,
        "distance": 300
      }
    },
    "triggerbot": {
      "enabled": true,
      "fireDelay": 0.1
    },
    "movement": {
      "speedEnabled": true,
      "speedMultiplier": 12
    },
    "visuals": {
      "chams": {
        "enabled": false,
        "fill": { "color": [255, 0, 0], "alpha": 0.5 },
        "outline": { "color": [255, 255, 255], "alpha": 0 }
      }
    },
    "keybinds": {
      "aimbot": {
        "toggle": "L",
        "activate": "MouseButton2"
      },
      "triggerbot": {
        "toggle": "P",
        "activate": "MouseButton2"
      },
      "movement": {
        "speedToggle": "G"
      },
      "visuals": {
        "chamsToggle": "N"
      }
    }
  }
}
```

**Firebase Storage**:
- Path: `configs/{userId}`
- Fields:
  - `config`: The JSON configuration object
  - `licenseKey`: User's license key (for API lookup)
  - `lastModified`: ISO timestamp
  - `userId`: Firebase user ID
  - `email`: User email

### 2. API Endpoint (`api/config.html`)

**Location**: `C:\Users\rayzh\Downloads\GoldEstates\api\config.html`

**Purpose**: Serves user configs as JSON based on license key

**Endpoint**: `https://goldenestate.homes/api/config?license=YOUR_LICENSE_KEY`

**How it works**:
1. Receives license key as query parameter
2. Queries Firebase `configs` collection
3. Finds user with matching `licenseKey`
4. Returns their `config` object as JSON
5. Returns error if license not found

**Response Format**:
```json
// Success
{
  "settings": { ... }
}

// Error
{
  "error": "License not found or no config available"
}
```

### 3. C++ Injector (`private.cpp`)

**Location**: `C:\Users\rayzh\Downloads\Scythe-Internal-Src\private\private.cpp`

**Key Function**: `LoadConfigAndApplyToScript()`

**Process**:
1. User enters license key
2. License is validated via KovaAuth
3. After successful auth, injector loads embedded Lua script
4. Calls `LoadConfigAndApplyToScript()` with license key
5. Makes HTTPS GET request to `goldenestate.homes/api/config?license=KEY`
6. Parses JSON response
7. Recursively processes nested config objects
8. Replaces placeholders in Lua script with actual values
9. Executes modified script in Roblox

**Placeholder Replacement**:
- Nested paths: `{{settings.aimbot.enabled}}` → `true`
- Arrays: `{{settings.visuals.chams.fill.color}}` → `255, 0, 0`
- Strings: `{{settings.aimbot.mode}}` → `mouse`
- Numbers: `{{settings.aimbot.fov}}` → `200`
- Booleans: `{{settings.general.enabled}}` → `true`

**Error Handling**:
- If HTTP request fails → Uses default embedded script
- If config parsing fails → Uses default embedded script
- If license not found → Uses default embedded script
- Always falls back gracefully

### 4. Lua Script (`internal.lua`)

**Location**: `C:\Users\rayzh\Downloads\Scythe-Internal-Src\private\Internal-Script\internal.lua`

**Placeholders Used**:
```lua
local AIMBOT_ENABLED = {{settings.aimbot.enabled}}
local AIMBOT_SMOOTHNESS = {{settings.aimbot.smoothness}}
local AIMBOT_FOV = {{settings.aimbot.fov}}
local SPEED_MULTIPLIER = {{settings.movement.speedMultiplier}}
local CHAMS_FILL_COLOR = Color3.fromRGB({{settings.visuals.chams.fill.color}})
-- etc...
```

**After Replacement**:
```lua
local AIMBOT_ENABLED = true
local AIMBOT_SMOOTHNESS = 1
local AIMBOT_FOV = 200
local SPEED_MULTIPLIER = 12
local CHAMS_FILL_COLOR = Color3.fromRGB(255, 0, 0)
```

## User Workflow

### First Time Setup

1. **Create Account**:
   - Visit `goldenestate.homes/config`
   - Click "Sign Up" tab
   - Enter email and password
   - Verify email via link sent to inbox

2. **Sign In**:
   - Enter email, password, and license key
   - System validates license from Firebase
   - Redirects to dashboard

3. **Configure Settings**:
   - Edit JSON config in Monaco editor
   - Click "Save Config" to store in Firebase
   - Config is now linked to your license key

### Using the Injector

1. **Run Roblox**
2. **Run `private.exe`**
3. **Enter License Key** (same one used in dashboard)
4. **Injector Process**:
   - Validates license via KovaAuth
   - Extracts embedded DLL
   - Attaches to Roblox
   - Fetches your config from Firebase
   - Applies config to Lua script
   - Executes in Roblox

5. **Your custom settings are now active!**

## Testing the System

### Test Config Fetch

1. **Get a valid license key** from Firebase
2. **Open browser** to:
   ```
   https://goldenestate.homes/api/config?license=YOUR_LICENSE_KEY
   ```
3. **Should see JSON response** with your config

### Test C++ Integration

1. **Build the injector** in Visual Studio
2. **Run Roblox**
3. **Run `private.exe`**
4. **Enter your license key**
5. **Watch console output**:
   ```
   [+] User config fetched from server
   [+] User config loaded and applied to script
   ```

### Test Lua Execution

1. **After injection**, check Roblox output
2. **Should see**:
   ```
   === CONFIG VALUES ===
   AIMBOT_ENABLED: true
   AIMBOT_SMOOTHNESS: 1
   AIMBOT_FOV: 200
   ====================
   ```

## Firebase Database Structure

```
private-c2211-default-rtdb/
├── configs/
│   └── {userId}/
│       ├── config: { settings: {...} }
│       ├── licenseKey: "user-license-key"
│       ├── lastModified: "2024-04-07T08:30:00.000Z"
│       ├── userId: "firebase-user-id"
│       └── email: "user@example.com"
│
├── savedConfigs/
│   └── {userId}/
│       └── {configId}/
│           ├── id: "timestamp"
│           ├── name: "Config Name"
│           ├── content: "JSON string"
│           └── date: "2024-04-07T08:30:00.000Z"
│
└── licenses/ (from KovaAuth database)
    └── {userId}/
        └── {licenseId}/
            ├── key: "license-key"
            ├── status: "Used"
            ├── type: "Trial"
            ├── expiryDate: timestamp
            └── hwid: "hardware-id"
```

## Security Features

1. **Email Verification**: Required before dashboard access
2. **License Validation**: Checked against Firebase database
3. **HTTPS**: All API requests use secure connections
4. **Firebase Auth**: Industry-standard authentication
5. **License-Config Binding**: Configs only accessible with valid license

## Deployment Checklist

### Dashboard Deployment

- [ ] Upload `dashboard.html` to `goldenestate.homes/dashboard`
- [ ] Upload `config.html` to `goldenestate.homes/config`
- [ ] Upload `api/config.html` to `goldenestate.homes/api/config`
- [ ] Verify Firebase config credentials
- [ ] Test login flow
- [ ] Test config save/load

### C++ Injector

- [ ] Verify `winhttp.h` is included
- [ ] Verify `LoadConfigAndApplyToScript()` function exists
- [ ] Verify URL points to `goldenestate.homes/api/config`
- [ ] Build in Release mode
- [ ] Test with valid license key
- [ ] Verify config fetch in console output

### Lua Script

- [ ] Verify all placeholders use `{{settings.*}}` format
- [ ] Verify array placeholders for color values
- [ ] Test script execution in Roblox
- [ ] Verify config values print correctly

## Troubleshooting

### Config Not Loading

**Symptom**: Injector says "using default script"

**Fixes**:
1. Check internet connection
2. Verify license key is correct
3. Check if config exists in Firebase dashboard
4. Verify `licenseKey` field is set in Firebase config
5. Check API endpoint is accessible

### Placeholder Not Replaced

**Symptom**: Lua script has `{{...}}` in output

**Fixes**:
1. Verify placeholder path matches config structure
2. Check config JSON is valid
3. Ensure value exists in config
4. Check for typos in placeholder names

### License Validation Fails

**Symptom**: "License not found" error

**Fixes**:
1. Verify license exists in KovaAuth database
2. Check license status is "Used"
3. Verify license hasn't expired
4. Ensure HWID matches (if applicable)

## Advanced Features

### Multiple Configs

Users can save multiple configs in the dashboard:
1. Edit config
2. Click "+" button in Saved Configs panel
3. Enter config name
4. Load any saved config later

### Config Sharing

To share a config:
1. Download config JSON from dashboard
2. Send file to friend
3. Friend can paste JSON into their dashboard
4. Save to their account

### Default Fallback

If online config fails, the injector uses the embedded Lua script with default values. This ensures the cheat always works, even offline.

## API Reference

### GET /api/config

**Parameters**:
- `license` (required): User's license key

**Response**:
```json
{
  "settings": {
    // User's configuration
  }
}
```

**Error Response**:
```json
{
  "error": "Error message"
}
```

**Status Codes**:
- 200: Success
- 400: Missing license parameter
- 404: License not found

## Support

For issues or questions:
1. Check console output for error messages
2. Verify all components are deployed correctly
3. Test each component individually
4. Check Firebase console for data
5. Review this documentation

---

**System Status**: ✅ Fully Operational
**Last Updated**: April 7, 2026
**Version**: 1.0.0
