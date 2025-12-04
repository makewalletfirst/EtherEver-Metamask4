인기 1순위
티커 알피시 대체
블록브라우저 체인아이디
 
 
 
 
 
 build the app.

[Setup your development environment](./docs/readme/environment.md)

#### Building the app

**Clone the project**

```bash
git clone git@github.com:MetaMask/metamask-mobile.git && \
cd metamask-mobile
```

##### Firebase Messaging Setup

MetaMask uses Firebase Cloud Messaging (FCM) to enable app communications. To integrate FCM, you'll need configuration files for both iOS and Android platforms.

###### Internal Contributor instructions

1. Grab the `.js.env` file from 1Password, ask around for the correct vault. This file contains the `GOOGLE_SERVICES_B64_ANDROID` and `GOOGLE_SERVICES_B64_IOS` secrets that will be used to generate the relevant configuration files for IOS/Android.
2. [Install](./README.md#install-dependencies) and [run & start](./README.md#running-the-app) the application as documented below.

###### External Contributor instructions

As an external contributor, you need to provide your own Firebase project configuration files:

- **`GoogleService-Info.plist`** (iOS)
- **`google-services.json`** (Android)

1. Create a Free Firebase Project
   - Set up a Firebase project in the Firebase Console.
   - Configure the project with a client package name matching `io.metamask` (IMPORTANT).
2. Add Configuration Files
   - Create/Update the `google-services.json` and `GoogleService-Info.plist` files in:
   - `android/app/google-services.json` (for Android)
   - `ios/GoogleServices/GoogleService-Info.plist` directory (for iOS)
3. Create the correct base64 environments variables.

```bash
# Generate Android Base64 Version of Google Services
export GOOGLE_SERVICES_B64_ANDROID="$(base64 -w0 -i ./android/app/google-services.json)" && GOOGLE_SERVICES_B64_IOS="$(base64 -w0 -i ./ios/GoogleServices/GoogleService-Info.plist)" && echo "export GOOGLE_SERVICES_B64_IOS=\"$GOOGLE_SERVICES_B64_IOS\"" | tee -a .js.env
```

[!CAUTION]

> In case you don't provide your own Firebase project config file or run the steps above, you will face the error `No matching client found for package name 'io.metamask'`.

In case of any doubt, please follow the instructions in the link below to get your Firebase project config file.
[Firebase Project Quickstart](https://firebaseopensource.com/projects/firebase/quickstart-js/messaging/readme/#getting_started)

##### Install dependencies

```bash
yarn setup
```

_Not the usual install command, this will run scripts and a lengthy postinstall flow_

#### Running the app for native development

**Run Metro bundler**

```bash
yarn watch
```

_Like a local server for the app_

**Run on a iOS device**

```bash
yarn start:ios
```

**Run on an Android device**

```bash
yarn start:android
```

## Development Tools

### Git Hooks (Husky)

This project uses [Husky](https://typicode.github.io/husky/) to run pre-commit hooks that automatically format and lint your code before commits. The pre-commit hook runs `lint-staged` which executes:

- **Prettier** - Code formatting for `*.{js,jsx,ts,tsx,json,feature}` files
- **ESLint** - Linting and auto-fixing for `*.{js,jsx,ts,tsx}` files

#### Disabling Husky Locally

If you need to disable Husky pre-commit hooks temporarily (e.g., for emergency commits or debugging), you have several options:

##### Option 1: Skip hooks for a single commit

```bash
git commit --no-verify -m "your commit message"
```

##### Option 2: Bypass hooks with environment variable

```bash
# Disable for current session
export HUSKY=0
git commit -m "your commit message"

# Or disable for a single command
HUSKY=0 git commit -m "your commit message"
```

**Note:** While these methods allow you to bypass the pre-commit hooks, remember that the CI/CD pipeline will still run linting checks. It's recommended to fix linting issues before pushing your changes to avoid build failures.
