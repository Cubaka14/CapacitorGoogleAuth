<h1 align="center">CapacitorGoogleAuth</h1>
<p align="center"><strong><code>@cubaka14/capacitor-google-auth</code></strong></p>
<p align="center"><strong>CAPACITOR 5</strong></p>
<p align="center">
🚀 A powerful Capacitor plugin for Google Authentication across iOS, Android, and Web platforms
</p>
<br>
<p align="center">
<a href="https://www.npmjs.com/package/@cubaka14/capacitor-google-auth"><img alt="npm version" src="https://img.shields.io/npm/v/@cubaka14/capacitor-google-auth"></a>
<a href="https://www.npmjs.com/package/@cubaka14/capacitor-google-auth"><img alt="npm downloads" src="https://img.shields.io/npm/dt/@cubaka14/capacitor-google-auth"></a>
<a href="https://github.com/Cubaka14/CapacitorGoogleAuth/blob/main/LICENSE"><img alt="license" src="https://img.shields.io/npm/l/@cubaka14/capacitor-google-auth"></a>
</p>

## Overview

CapacitorGoogleAuth is a **cross-platform authentication plugin** that enables seamless Google Sign-In integration for Capacitor applications. Whether you're building for iOS, Android, or the web, this plugin provides a unified API that handles the complexity of Google OAuth 2.0 authentication across all platforms.

### ✨ Key Features

- **🔄 Cross-Platform**: Single API works on iOS, Android, and Web
- **🔒 Secure**: Implements Google OAuth 2.0 best practices
- **📱 Native UI**: Uses platform-native authentication interfaces
- **🔧 TypeScript**: Full TypeScript support with comprehensive type definitions
- **⚡ Capacitor 5**: Built for the latest Capacitor framework
- **🌐 Framework Agnostic**: Works with Angular, Vue, React, and vanilla JavaScript
- **🔄 Token Management**: Automatic token refresh and secure storage
- **📖 Well Documented**: Comprehensive documentation and examples

### 🎯 Perfect For

- **Mobile Apps**: Ionic, React Native, or any Capacitor-based mobile application
- **Web Applications**: PWAs and SPAs requiring Google authentication
- **Hybrid Development**: Projects targeting multiple platforms with shared authentication logic
- **Enterprise Apps**: Applications requiring secure, scalable authentication solutions

### 📖 Documentation

- **[Project Overview](./PROJECT_OVERVIEW.md)** - Understanding the plugin architecture and features
- **[Developer Guide](./DEVELOPER_GUIDE.md)** - Deep dive into the codebase and development workflow
- **[Usage Examples](./EXAMPLES.md)** - Comprehensive examples for different frameworks and use cases
- **[Contributing Guide](./CONTRIBUTING.md)** - How to contribute to this project

## About This Fork

This is a maintained fork of the original `@codetrix-studio/capacitor-google-auth` plugin. This fork provides:

- 🔄 **Continued Maintenance**: Regular updates and bug fixes
- 🚀 **Latest Capacitor Support**: Compatibility with newest Capacitor versions  
- 🛠️ **Community Contributions**: Welcoming community improvements and features
- 📚 **Enhanced Documentation**: Better guides and examples

## Contributions

PRs are welcome and much appreciated that keeps this plugin up to date with Capacitor and official Google Auth platform library feature parity.

Try to follow good code practices. You can even help keeping the included demo updated.

PRs for features that are not aligned with the official Google Auth library are discouraged.

(We are beginner-friendly here)

## Install

#### 1. Install package

```sh
npm i --save @cubaka14/capacitor-google-auth

# pnpm 
pnpm add @cubaka14/capacitor-google-auth

# yarn 
yarn add @cubaka14/capacitor-google-auth
```

#### 2. Update capacitor deps

```sh
npx cap update
```

## Quick Start

Here's a minimal example to get you started:

```typescript
import { GoogleAuth } from '@cubaka14/capacitor-google-auth';

// 1. Initialize the plugin (call once when your app starts)
await GoogleAuth.initialize({
  clientId: 'YOUR_CLIENT_ID.apps.googleusercontent.com',
  scopes: ['profile', 'email'],
});

// 2. Sign in a user
try {
  const user = await GoogleAuth.signIn();
  console.log('User signed in:', user.name, user.email);
  console.log('Access token:', user.authentication.accessToken);
} catch (error) {
  console.error('Sign in failed:', error);
}

// 3. Sign out
await GoogleAuth.signOut();
```

### Platform Setup Checklist

Before using the plugin, ensure you have:

- [ ] **Google Console**: Created OAuth 2.0 credentials in [Google Cloud Console](https://console.cloud.google.com/)
- [ ] **Web Client ID**: For web and general configuration  
- [ ] **iOS Client ID**: For iOS apps (if different from web)
- [ ] **Android Client ID**: For Android apps (if different from web)
- [ ] **iOS**: Added URL scheme to `Info.plist`
- [ ] **Android**: Configured app signing certificates

> 💡 **Tip**: Check out our [Demo Applications](./demo/) for complete working examples!

## Updating

If need migrate to different Capacitor versions [see instruction for migrate plugin to new version](#migration-guide).

## Usage

### WEB

Register plugin and manually initialize

```ts
import { GoogleAuth } from '@cubaka14/capacitor-google-auth';

// use hook after platform dom ready
GoogleAuth.initialize({
  clientId: 'CLIENT_ID.apps.googleusercontent.com',
  scopes: ['profile', 'email'],
  grantOfflineAccess: true,
});
```

or if need use meta tags (Optional):

```html
<meta name="google-signin-client_id" content="{your client id here}" />
<meta name="google-signin-scope" content="profile email" />
```

#### Options

- `clientId` - The app's client ID, found and created in the Google Developers Console.
- `scopes` – same as [Configure](#Configure) scopes
- `grantOfflineAccess` – boolean, default `false`, Set if your application needs to refresh access tokens when the user is not present at the browser.

Use it

```ts
GoogleAuth.signIn();
```

#### Angular

init hook

```ts
// app.component.ts
constructor() {
  this.initializeApp();
}

initializeApp() {
  this.platform.ready().then(() => {
    GoogleAuth.initialize()
  })
}
```

sign in function

```ts
async googleSignIn() {
  let googleUser = await GoogleAuth.signIn();

  /*
    If you use Firebase you can forward and use the logged in Google user like this:
  */
  const credential = auth.GoogleAuthProvider.credential(googleUser.authentication.idToken);
  return this.afAuth.auth.signInAndRetrieveDataWithCredential(credential);
}
```

#### Vue 3

```vue
<script setup lang="ts">
import { defineComponent, onMounted } from 'vue';
import { GoogleAuth } from '@cubaka14/capacitor-google-auth';

onMounted(() => {
  GoogleAuth.initialize();
});

async function logIn() {
  const response = await GoogleAuth.signIn();
  console.log(response);
}
</script>
```

or see more [CapacitorGoogleAuth-Vue3-example](https://github.com/reslear/CapacitorGoogleAuth-Vue3-example)

### iOS

1. Create in Google cloud console credential **Client ID for iOS** and get **Client ID** and **iOS URL scheme**

2. Add **identifier** `REVERSED_CLIENT_ID` as **URL schemes** to `Info.plist` from **iOS URL scheme**<br>
   (Xcode: App - Targets/App - Info - URL Types, click plus icon)

3. Set **Client ID** one of the ways:
   1. Set in `capacitor.config.json`
      - `iosClientId` - specific key for iOS
      - `clientId` - or common key for Android and iOS
   2. Download `GoogleService-Info.plist` file with `CLIENT_ID` and copy to **ios/App/App** necessarily through Xcode for indexing.

plugin first use `iosClientId` if not found use `clientId` if not found use value `CLIENT_ID` from file `GoogleService-Info.plist`

### Android

Set **Client ID** :

1. In `capacitor.config.json`

   - `androidClientId` - specific key for Android
   - `clientId` - or common key for Android and iOS

2. or set inside your `strings.xml`

plugin first use `androidClientId` if not found use `clientId` if not found use value `server_client_id` from file `strings.xml`

```xml
<resources>
  <string name="server_client_id">Your Web Client Key</string>
</resources>
```

**Refresh method**

This method should be called when the app is initialized to establish if the user is currently logged in. If true, the method will return an accessToken, idToken and an empty refreshToken.
```ts
checkLoggedIn() {
    GoogleAuth.refresh()
        .then((data) => {
            if (data.accessToken) {
                this.currentTokens = data;
            }
        })
        .catch((error) => {
            if (error.type === 'userLoggedOut') {
                this.signin()
            }
        });
}
```

## Configure

| Name                     | Type     | Description                                                                                                                   |
| ------------------------ | -------- | ----------------------------------------------------------------------------------------------------------------------------- |
| clientId                 | string   | The app's client ID, found and created in the Google Developers Console.                                                      |
| iosClientId              | string   | Specific client ID key for iOS                                                                                                |
| androidClientId          | string   | Specific client ID key for Android                                                                                            |
| scopes                   | string[] | Scopes that you might need to request to access Google APIs<br>https://developers.google.com/identity/protocols/oauth2/scopes |
| serverClientId           | string   | This ClientId used for offline access and server side handling                                                                |
| forceCodeForRefreshToken | boolean  | Force user to select email address to regenerate AuthCode <br>used to get a valid refreshtoken (work on iOS and Android)      |

Provide configuration in root `capacitor.config.json`

```json
{
  "plugins": {
    "GoogleAuth": {
      "scopes": ["profile", "email"],
      "serverClientId": "xxxxxx-xxxxxxxxxxxxxxxxxx.apps.googleusercontent.com",
      "forceCodeForRefreshToken": true
    }
  }
}
```

or in `capacitor.config.ts`

```ts
/// <reference types="'@cubaka14/capacitor-google-auth'" />

const config: CapacitorConfig = {
  plugins: {
    GoogleAuth: {
      scopes: ['profile', 'email'],
      serverClientId: 'xxxxxx-xxxxxxxxxxxxxxxxxx.apps.googleusercontent.com',
      forceCodeForRefreshToken: true,
    },
  },
};

export default config;
```

## API

<docgen-index>

* [`initialize(...)`](#initialize)
* [`signIn()`](#signin)
* [`refresh()`](#refresh)
* [`signOut()`](#signout)
* [Interfaces](#interfaces)

</docgen-index>
<docgen-api>
<!--Update the source file JSDoc comments and rerun docgen to update the docs below-->

### initialize(...)

```typescript
initialize(options?: InitOptions) => void
```

Initializes the GoogleAuthPlugin, loading the gapi library and setting up the plugin.

| Param         | Type                                                | Description                        |
| ------------- | --------------------------------------------------- | ---------------------------------- |
| **`options`** | <code><a href="#initoptions">InitOptions</a></code> | - Optional initialization options. |

**Since:** 3.1.0

--------------------


### signIn()

```typescript
signIn() => Promise<User>
```

Initiates the sign-in process and returns a Promise that resolves with the user information.

**Returns:** <code>Promise&lt;<a href="#user">User</a>&gt;</code>

--------------------


### refresh()

```typescript
refresh() => Promise<Authentication>
```

Refreshes the authentication token and returns a Promise that resolves with the updated authentication details.

**Returns:** <code>Promise&lt;<a href="#authentication">Authentication</a>&gt;</code>

--------------------


### signOut()

```typescript
signOut() => Promise<any>
```

Signs out the user and returns a Promise.

**Returns:** <code>Promise&lt;any&gt;</code>

--------------------


### Interfaces


#### InitOptions

| Prop                     | Type                  | Description                                                                                                                                      | Default            | Since |
| ------------------------ | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------ | ----- |
| **`clientId`**           | <code>string</code>   | The app's client ID, found and created in the Google Developers Console. Common for Android or iOS. The default is defined in the configuration. |                    | 3.1.0 |
| **`scopes`**             | <code>string[]</code> | Specifies the scopes required for accessing Google APIs The default is defined in the configuration.                                             |                    |       |
| **`grantOfflineAccess`** | <code>boolean</code>  | Set if your application needs to refresh access tokens when the user is not present at the browser. In response use `serverAuthCode` key         | <code>false</code> | 3.1.0 |


#### User

| Prop                 | Type                                                      | Description                                                         |
| -------------------- | --------------------------------------------------------- | ------------------------------------------------------------------- |
| **`id`**             | <code>string</code>                                       | The unique identifier for the user.                                 |
| **`email`**          | <code>string</code>                                       | The email address associated with the user.                         |
| **`name`**           | <code>string</code>                                       | The user's full name.                                               |
| **`familyName`**     | <code>string</code>                                       | The family name (last name) of the user.                            |
| **`givenName`**      | <code>string</code>                                       | The given name (first name) of the user.                            |
| **`imageUrl`**       | <code>string</code>                                       | The URL of the user's profile picture.                              |
| **`serverAuthCode`** | <code>string</code>                                       | The server authentication code.                                     |
| **`authentication`** | <code><a href="#authentication">Authentication</a></code> | The authentication details including access, refresh and ID tokens. |


#### Authentication

| Prop               | Type                | Description                                      |
| ------------------ | ------------------- | ------------------------------------------------ |
| **`accessToken`**  | <code>string</code> | The access token obtained during authentication. |
| **`idToken`**      | <code>string</code> | The ID token obtained during authentication.     |
| **`refreshToken`** | <code>string</code> | The refresh token.                               |

</docgen-api>

## Migration guide

#### Migrating to this Fork

If you're migrating from the original `@codetrix-studio/capacitor-google-auth`:

```sh
# Remove the original package
npm uninstall @codetrix-studio/capacitor-google-auth

# Install this fork
npm install @cubaka14/capacitor-google-auth
```

Update your imports:
```typescript
// Before
import { GoogleAuth } from '@codetrix-studio/capacitor-google-auth';

// After  
import { GoogleAuth } from '@cubaka14/capacitor-google-auth';
```

> 📝 **Note**: The API remains the same, only the package name has changed.

#### Migrate from 3.3.x to 3.4.x

Install version 3.4.x:

```sh
npm i --save @cubaka14/capacitor-google-auth@^3.4
```

#### Migrate from 3.2.x to 3.3.x

Install version 3.3.x:

```sh
npm i --save @cubaka14/capacitor-google-auth^3.3
```

Follow instruction for you project [Updating from Capacitor 4 to Capacitor 5](https://capacitorjs.com/docs/updating/5-0).

#### Migrate from 3.2.1 to 3.2.2

for `Android` in file `MainActivity.onCreate`

```diff
- this.init(savedInstanceState, new ArrayList<Class<? extends Plugin>>() {{
-   add(GoogleAuth.class);
- }});
+ this.registerPlugin(GoogleAuth.class);
```

#### Migrate from 3.1.x to 3.2.x

Install version 3.2.x:

```sh
npm i --save @cubaka14/capacitor-google-auth^3.2
```

Follow instruction for you project [Updating from Capacitor 3 to Capacitor 4](https://capacitorjs.com/docs/updating/4-0).

#### Migrate from 3.0.2 to 3.1.0

```diff
- GoogleAuth.init()
+ GoogleAuth.initialize()
```

#### Migrate from 2 to 3

Install version 3.x.x:

```sh
npm i --save @cubaka14/capacitor-google-auth^3.0
```

After [migrate to Capcitor 3](https://capacitorjs.com/docs/updating/3-0) updating you projects, see diff:

##### WEB

```diff
- import "@codetrix-studio/capacitor-google-auth";
- import { Plugins } from '@capacitor/core';
+ import { GoogleAuth } from '@cubaka14/capacitor-google-auth'

- Plugins.GoogleAuth.signIn();
+ GoogleAuth.init()
+ GoogleAuth.signIn()
```

#### Migrate from 1 to 2

Install version 2.x.x:

```sh
npm i --save @cubaka14/capacitor-google-auth@2
```

for capacitor 2.x.x use [instruction](https://github.com/CodetrixStudio/CapacitorGoogleAuth/blob/79129ab37288f5f5d0bb9a568a95890e852cebc2/README.md)

## License

[MIT](./LICENSE)
