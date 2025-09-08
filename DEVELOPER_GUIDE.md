# Developer Guide - Understanding the Codebase

## Project Structure Deep Dive

### Core Plugin Architecture

The CapacitorGoogleAuth plugin follows Capacitor's standard plugin architecture:

```
src/
├── definitions.ts    # TypeScript interfaces and plugin contract
├── index.ts         # Plugin registration and exports  
└── web.ts           # Web platform implementation
```

#### `definitions.ts` - The Plugin Contract
This file defines the TypeScript interfaces that establish the contract between the plugin and consuming applications:

- **`GoogleAuthPlugin`**: Main interface defining the plugin's public API
- **`User`**: User profile data structure returned after authentication
- **`Authentication`**: Token data structure (access, ID, refresh tokens)
- **`InitOptions`**: Configuration options for plugin initialization
- **`GoogleAuthPluginOptions`**: Capacitor configuration options

#### `index.ts` - Plugin Registration
The entry point that registers the plugin with Capacitor:
```typescript
const GoogleAuth = registerPlugin<GoogleAuthPlugin>('GoogleAuth', {
  web: () => import('./web').then((m) => new m.GoogleAuthWeb()),
});
```

#### `web.ts` - Web Implementation
Implements the plugin interface for web platforms using Google's JavaScript API:
- Loads the Google Platform Library dynamically
- Handles authentication state management
- Implements OAuth 2.0 flow for web browsers

### Platform-Specific Implementations

#### iOS Implementation (`ios/Plugin/`)

**Key Files:**
- `Plugin.swift`: Main iOS plugin implementation
- `Plugin.h`/`Plugin.m`: Objective-C bridge components
- `Podfile`: CocoaPods dependencies (GoogleSignIn SDK)

**iOS Architecture:**
```swift
// Plugin.swift handles:
// 1. GoogleSignIn SDK initialization
// 2. Sign-in flow coordination
// 3. Token management and refresh
// 4. User profile extraction
```

The iOS implementation leverages:
- **GoogleSignIn SDK v6.2.4**: Official Google authentication library
- **Capacitor's CAPPlugin**: Base class for iOS plugins
- **URL Schemes**: For handling authentication redirects

#### Android Implementation (`android/src/main/java/`)

**Key Components:**
- Java/Kotlin classes implementing the plugin interface
- Gradle build configuration with Google dependencies
- Android manifest permissions and intent filters

**Android Architecture:**
```java
// Android implementation handles:
// 1. Google Sign-In API initialization  
// 2. Activity result handling for authentication
// 3. Token extraction and management
// 4. Error handling for various auth scenarios
```

### Configuration System

The plugin supports multiple configuration approaches:

#### 1. Capacitor Config (`capacitor.config.json`)
```json
{
  "plugins": {
    "GoogleAuth": {
      "clientId": "universal-client-id",
      "iosClientId": "ios-specific-client-id", 
      "androidClientId": "android-specific-client-id",
      "scopes": ["profile", "email"],
      "serverClientId": "server-client-id",
      "forceCodeForRefreshToken": true
    }
  }
}
```

#### 2. Meta Tags (Web Only)
```html
<meta name="google-signin-client_id" content="your-client-id" />
<meta name="google-signin-scope" content="profile email" />
```

#### 3. Runtime Initialization
```typescript
GoogleAuth.initialize({
  clientId: 'runtime-client-id',
  scopes: ['profile', 'email'],
  grantOfflineAccess: true
});
```

### Authentication Flow Implementation

#### Web Flow (`web.ts`)
1. **Script Loading**: Dynamically loads Google's platform.js
2. **GAPI Initialization**: Configures Google API client
3. **Sign-In Process**: Handles both standard and offline access flows
4. **Token Management**: Extracts and formats authentication tokens

```typescript
// Key methods in GoogleAuthWeb:
async signIn(): Promise<User>        // Main sign-in flow
async refresh(): Promise<Authentication>  // Token refresh
async signOut(): Promise<any>        // Sign-out process
```

#### Native Flow (iOS/Android)
1. **SDK Initialization**: Platform-specific Google SDK setup
2. **Authentication UI**: Native authentication interface
3. **Token Exchange**: Secure token handling and storage
4. **Profile Extraction**: User data retrieval from Google APIs

### Build and Development Process

#### TypeScript Compilation
```bash
npm run build     # Compiles src/ to dist/esm/
npm run watch     # Watches for changes and recompiles
```

#### Generated Output Structure
```
dist/esm/
├── index.js      # Compiled plugin registration
├── index.d.ts    # TypeScript declarations
├── definitions.js
├── definitions.d.ts
├── web.js
└── web.d.ts
```

### Testing and Demos

#### Demo Applications
The project includes demo applications showcasing different integration approaches:

**Vanilla Angular Demo (`demo/`):**
- Basic Angular application
- Demonstrates core plugin functionality
- Uses older Capacitor 3 API patterns

**Ionic Angular Demo (`demo-ionic-angular/`):**
- Modern Ionic Angular implementation
- Shows best practices for Ionic apps
- Uses current Capacitor API patterns

#### Demo Features Implemented:
- User sign-in/sign-out
- Token refresh handling
- User profile display
- Authentication state management

### Plugin Dependencies

#### Development Dependencies
- **@capacitor/core**: Core Capacitor functionality
- **@capacitor/cli**: Capacitor CLI tools
- **TypeScript**: For type-safe development
- **@capacitor/docgen**: Documentation generation

#### Runtime Dependencies
- **Web**: Google JavaScript Platform Library (loaded dynamically)
- **iOS**: GoogleSignIn Pod (~> 6.2.4)
- **Android**: Google Sign-In for Android (via Gradle)

### Error Handling Patterns

The plugin implements comprehensive error handling:

```typescript
// Common error scenarios:
// 1. Network connectivity issues
// 2. Invalid client configuration
// 3. User cancellation
// 4. Token expiration
// 5. Platform-specific authentication failures
```

### Development Best Practices

#### Code Organization
- Keep platform-specific logic separate
- Use TypeScript interfaces for type safety
- Maintain consistent error handling patterns
- Follow Capacitor plugin conventions

#### Testing Approach
- Test on all target platforms (iOS, Android, Web)
- Verify both development and production builds
- Test various authentication scenarios
- Validate token handling and refresh flows

#### Contribution Guidelines
- Maintain compatibility with official Google libraries
- Follow existing code style and patterns
- Update documentation for new features
- Test across different Capacitor versions

This codebase provides a robust foundation for Google authentication in Capacitor applications, with clear separation of concerns and platform-specific optimizations.