# Mobile App Deployment

This guide walks through the process of building and deploying the Matra mobile app for iOS and Android.

## Prerequisites
- Apple Developer Account (for iOS)
- Google Play Developer Account (for Android)
- Expo account
- Node.js v16+ and npm
- Expo CLI installed globally

## Development Setup
```bash
# Install Expo CLI
npm install -g expo-cli

# Clone the repository
git clone https://github.com/your-org/matra-mobile.git
cd matra-mobile

# Install dependencies
npm install

# Configure environment variables
cp .env.example .env
# Edit .env with your API URLs and other configuration
```

## iOS Deployment

### 1. Configure app.json
```json
{
  "expo": {
    "name": "Matra",
    "slug": "matra",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "ios": {
      "bundleIdentifier": "com.yourcompany.matra",
      "buildNumber": "1",
      "supportsTablet": true,
      "config": {
        "usesNonExemptEncryption": false
      }
    }
  }
}
```

### 2. Build the iOS App
```bash
# Build the iOS app
expo build:ios

# Follow the prompts to select build type (archive or simulator)
# Choose to manage certificates with Expo or use your own
```

### 3. Submit to TestFlight
1. Wait for the build to complete (monitor on the Expo dashboard)
2. Download the .ipa file
3. Upload to App Store Connect using Transporter
4. Configure TestFlight information in App Store Connect
5. Invite testers

### 4. Prepare for App Store Release
1. Complete all App Store listings and metadata
2. Add screenshots for various device sizes
3. Complete App Review Information section
4. Submit for App Review

## Android Deployment

### 1. Configure app.json for Android
```json
{
  "expo": {
    "android": {
      "package": "com.yourcompany.matra",
      "versionCode": 1,
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#FFFFFF"
      },
      "permissions": [
        "CAMERA",
        "USE_BIOMETRIC",
        "USE_FINGERPRINT"
      ]
    }
  }
}
```

### 2. Build the Android App
```bash
# Build the Android app
expo build:android

# Choose between APK and app bundle (prefer app bundle for Play Store)
```

### 3. Publish to Google Play Console
1. Wait for the build to complete
2. Download the .aab or .apk file
3. Create a new app in Google Play Console
4. Upload the .aab file to the internal testing track
5. Complete store listing information
6. Invite testers via email

### 4. Prepare for Production Release
1. Complete content rating questionnaire
2. Set up pricing and distribution
3. Add privacy policy URL
4. Submit for review

## Continuous Integration/Deployment
For automated builds and deployment, consider setting up:

1. GitHub Actions workflow:
```yaml
name: Build and Deploy
on:
  push:
    branches: [ main ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: 16.x
      - uses: expo/expo-github-action@v6
        with:
          expo-version: 4.x
          token: ${{ secrets.EXPO_TOKEN }}
      - name: Install dependencies
        run: npm ci
      - name: Publish to Expo
        run: expo publish --non-interactive
```

2. EAS (Expo Application Services) for more advanced CI/CD

## Push Notifications Setup
1. Configure Firebase for Android
2. Configure Apple Push Notification service for iOS
3. Update the Expo configuration to use the notification credentials 