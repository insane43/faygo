# GoldenEstate Config Portal

## Secret Access

The config portal can be accessed by clicking the **"Modern Homes"** filter button on the homepage or properties page. This will redirect you to the login page.

## Features

### Authentication
- **Sign Up**: Create a new account with email/password
- **Email Verification**: Users must verify their email before signing in
- **Sign In**: Login with email, password, and license key
- **Remember Me**: Option to save login credentials locally
- **License Validation**: Checks license key against Firebase Realtime Database

### Login Page (`config.html`)
- Black background with animated falling snow
- Snow effect on top of auth cards
- Dual forms: Sign In / Sign Up
- Firebase Authentication integration
- License key validation from `https://kova-42298-default-rtdb.firebaseio.com/`
- Email verification requirement
- Remember me functionality

### Dashboard (`dashboard.html`)
- **Config Editor**: JSON editor for application configuration
- **10 Themes**: 
  - Dark Gold (default)
  - Midnight Blue
  - Purple Haze
  - Emerald Night
  - Crimson Dark
  - Ocean Deep
  - Sunset Orange
  - Forest Green
  - Rose Gold
  - Cyber Pink
- **Statistics**: License type, days remaining, last modified
- **Actions**: Save, Reset, Download config
- **Auto-save theme**: Theme selection is saved to config

### License Key Validation
The system validates license keys from the Firebase database structure:
```
licenses/
  └─ userId/
      └─ licenseId/
          ├─ key: "license-key-here"
          ├─ status: "Used"
          ├─ type: "Trial"
          ├─ activatedAt: timestamp
          ├─ createdAt: timestamp
          ├─ expiryDate: timestamp
          └─ hwid: "hardware-id"
```

### Config Storage
User configurations are stored in Firebase:
```
configs/
  └─ userId/
      ├─ config: { ... }
      ├─ lastModified: timestamp
      ├─ userId: "user-id"
      └─ email: "user@email.com"
```

## Usage

1. Click "Modern Homes" button on the website
2. Sign up with email and password
3. Verify your email (check inbox)
4. Sign in with email, password, and valid license key
5. Access the config dashboard
6. Edit your configuration in JSON format
7. Choose a theme from the sidebar
8. Save your configuration

## Default Configuration
```json
{
  "appName": "GoldenEstate Config",
  "version": "1.0.0",
  "features": {
    "darkMode": true,
    "notifications": true,
    "autoSave": false
  },
  "settings": {
    "theme": "dark-gold",
    "language": "en",
    "timezone": "UTC"
  },
  "api": {
    "endpoint": "https://api.goldenestate.com",
    "timeout": 5000,
    "retries": 3
  }
}
```

## Security
- Email verification required
- License key validation
- Firebase Authentication
- Secure password requirements (min 6 characters)
- Session management with remember me option
