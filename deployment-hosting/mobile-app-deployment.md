# Mobile App Deployment

This internal documentation describes the process used for building and deploying the Matra mobile app for iOS and Android platforms.

## Technology Stack
- React Native (core framework)
- Expo SDK (development toolchain)
- EAS (Expo Application Services) for building and submissions

## Development Environment
```bash
# Development environment setup
npm install -g eas-cli

# Local development startup
npm install
npx expo start
```

## Build Configuration

The app's configuration is managed in `app.json`, with separate configurations for iOS and Android:

```json
{
  "expo": {
    "name": "Matra",
    "slug": "matra",
    "owner": "matra-team",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "ios": {
      "bundleIdentifier": "io.matra.wallet",
      "buildNumber": "1",
      "supportsTablet": true,
      "config": {
        "usesNonExemptEncryption": false
      }
    },
    "android": {
      "package": "io.matra.wallet",
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

## iOS Deployment Process

We use EAS Build for iOS deployment:

```bash
# Create new iOS build
eas build --platform ios

# Submit to App Store
eas submit --platform ios
```

Internal iOS deployment workflow:
1. Create builds using EAS
2. Deploy to TestFlight for internal testing
3. Invite team members through App Store Connect
4. Run internal QA process
5. Submit for App Store review when ready

## Android Deployment Process

Android deployment also uses EAS:

```bash
# Create new Android build
eas build --platform android

# Submit to Play Store
eas submit --platform android
```

Internal Android deployment workflow:
1. Generate AAB builds using EAS
2. Upload to Play Console's internal testing track
3. Share with test team via internal testing channel
4. Promote to production after QA approval

## Environment Variables and Configuration

We use EAS Secrets to manage environment variables:

```bash
# Setting environment variables
eas secret:create --scope project --name API_URL --value "https://api.matra.io"
```

Environment configurations are maintained in:
- `.env.development` - Development server configuration
- `.env.staging` - Staging environment settings
- `.env.production` - Production settings

## CI/CD Pipeline

Our internal CI/CD pipeline uses GitHub Actions:

```yaml
name: EAS Build
on:
  workflow_dispatch:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Setup Node
        uses: actions/setup-node@v2
        with:
          node-version: 16.x
      - name: Setup EAS
        uses: expo/expo-github-action@v7
        with:
          eas-version: latest
          token: ${{ secrets.EXPO_TOKEN }}
      - name: Install dependencies
        run: npm ci
      - name: Build on EAS
        run: eas build --platform all --non-interactive
```

## Version Management

We maintain separate version numbers for iOS and Android:
- iOS: Version and Build Number updated in app.json
- Android: Version Name and Version Code updated in app.json

## Push Notification Implementation

Push notifications are configured through:
- Firebase Cloud Messaging for Android
- Apple Push Notification Service for iOS

Configuration files are stored in:
- `google-services.json` (Android)
- APNs authentication key (iOS) 