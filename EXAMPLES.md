# Usage Examples

This document provides comprehensive examples for integrating CapacitorGoogleAuth into different types of applications.

## Basic Implementation

### 1. Simple Sign-In/Sign-Out

```typescript
import { GoogleAuth } from '@cubaka14/capacitor-google-auth';

class AuthService {
  async initialize() {
    await GoogleAuth.initialize({
      clientId: 'your-client-id.apps.googleusercontent.com',
      scopes: ['profile', 'email'],
    });
  }

  async signIn() {
    try {
      const user = await GoogleAuth.signIn();
      console.log('✅ Signed in successfully:', user.name);
      return user;
    } catch (error) {
      console.error('❌ Sign-in failed:', error);
      throw error;
    }
  }

  async signOut() {
    try {
      await GoogleAuth.signOut();
      console.log('✅ Signed out successfully');
    } catch (error) {
      console.error('❌ Sign-out failed:', error);
    }
  }
}
```

## Framework-Specific Examples

### Angular Integration

#### Service Setup
```typescript
// auth.service.ts
import { Injectable } from '@angular/core';
import { GoogleAuth, User } from '@cubaka14/capacitor-google-auth';
import { BehaviorSubject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  private currentUser$ = new BehaviorSubject<User | null>(null);
  
  constructor() {
    this.initializeAuth();
  }

  async initializeAuth() {
    await GoogleAuth.initialize({
      clientId: 'your-client-id.apps.googleusercontent.com',
      scopes: ['profile', 'email'],
    });

    // Listen for user changes
    GoogleAuth.addListener('userChange', (user) => {
      this.currentUser$.next(user);
    });
  }

  async signIn(): Promise<User> {
    const user = await GoogleAuth.signIn();
    this.currentUser$.next(user);
    return user;
  }

  async signOut(): Promise<void> {
    await GoogleAuth.signOut();
    this.currentUser$.next(null);
  }

  getCurrentUser() {
    return this.currentUser$.asObservable();
  }

  async refreshToken() {
    try {
      const auth = await GoogleAuth.refresh();
      console.log('🔄 Token refreshed:', auth.accessToken);
      return auth;
    } catch (error) {
      console.error('❌ Token refresh failed:', error);
      // Handle re-authentication
      await this.signOut();
      throw error;
    }
  }
}
```

#### Component Usage
```typescript
// app.component.ts
import { Component, OnInit } from '@angular/core';
import { AuthService } from './services/auth.service';

@Component({
  selector: 'app-root',
  template: `
    <div class="auth-container">
      <div *ngIf="!user; else loggedIn">
        <button (click)="signIn()" [disabled]="loading">
          {{ loading ? 'Signing in...' : 'Sign in with Google' }}
        </button>
      </div>

      <ng-template #loggedIn>
        <div class="user-profile">
          <img [src]="user.imageUrl" [alt]="user.name" class="avatar">
          <h2>Welcome, {{ user.name }}!</h2>
          <p>{{ user.email }}</p>
          <button (click)="signOut()">Sign Out</button>
          <button (click)="refreshToken()">Refresh Token</button>
        </div>
      </ng-template>
    </div>
  `
})
export class AppComponent implements OnInit {
  user: any = null;
  loading = false;

  constructor(private authService: AuthService) {}

  ngOnInit() {
    this.authService.getCurrentUser().subscribe(user => {
      this.user = user;
    });
  }

  async signIn() {
    this.loading = true;
    try {
      await this.authService.signIn();
    } catch (error) {
      console.error('Sign in error:', error);
    } finally {
      this.loading = false;
    }
  }

  async signOut() {
    await this.authService.signOut();
  }

  async refreshToken() {
    await this.authService.refreshToken();
  }
}
```

### Vue 3 Composition API

```vue
<template>
  <div class="auth-container">
    <div v-if="!user.value">
      <button @click="signIn" :disabled="loading">
        {{ loading ? 'Signing in...' : 'Sign in with Google' }}
      </button>
    </div>

    <div v-else class="user-profile">
      <img :src="user.value.imageUrl" :alt="user.value.name" class="avatar">
      <h2>Welcome, {{ user.value.name }}!</h2>
      <p>{{ user.value.email }}</p>
      <button @click="signOut">Sign Out</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { GoogleAuth, User } from '@cubaka14/capacitor-google-auth';

const user = ref<User | null>(null);
const loading = ref(false);

onMounted(async () => {
  await GoogleAuth.initialize({
    clientId: 'your-client-id.apps.googleusercontent.com',
    scopes: ['profile', 'email'],
  });
});

const signIn = async () => {
  loading.value = true;
  try {
    user.value = await GoogleAuth.signIn();
  } catch (error) {
    console.error('Sign in failed:', error);
  } finally {
    loading.value = false;
  }
};

const signOut = async () => {
  try {
    await GoogleAuth.signOut();
    user.value = null;
  } catch (error) {
    console.error('Sign out failed:', error);
  }
};
</script>
```

### React Hooks

```tsx
import React, { useState, useEffect } from 'react';
import { GoogleAuth, User } from '@cubaka14/capacitor-google-auth';

const useGoogleAuth = () => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(false);
  const [initialized, setInitialized] = useState(false);

  useEffect(() => {
    const initializeAuth = async () => {
      try {
        await GoogleAuth.initialize({
          clientId: 'your-client-id.apps.googleusercontent.com',
          scopes: ['profile', 'email'],
        });
        setInitialized(true);
      } catch (error) {
        console.error('Failed to initialize GoogleAuth:', error);
      }
    };

    initializeAuth();
  }, []);

  const signIn = async () => {
    if (!initialized) return;
    
    setLoading(true);
    try {
      const userData = await GoogleAuth.signIn();
      setUser(userData);
      return userData;
    } catch (error) {
      console.error('Sign in failed:', error);
      throw error;
    } finally {
      setLoading(false);
    }
  };

  const signOut = async () => {
    try {
      await GoogleAuth.signOut();
      setUser(null);
    } catch (error) {
      console.error('Sign out failed:', error);
    }
  };

  return { user, loading, signIn, signOut, initialized };
};

const AuthComponent: React.FC = () => {
  const { user, loading, signIn, signOut } = useGoogleAuth();

  return (
    <div className="auth-container">
      {!user ? (
        <button onClick={signIn} disabled={loading}>
          {loading ? 'Signing in...' : 'Sign in with Google'}
        </button>
      ) : (
        <div className="user-profile">
          <img src={user.imageUrl} alt={user.name} className="avatar" />
          <h2>Welcome, {user.name}!</h2>
          <p>{user.email}</p>
          <button onClick={signOut}>Sign Out</button>
        </div>
      )}
    </div>
  );
};

export default AuthComponent;
```

## Advanced Use Cases

### 1. Server-Side Authentication

```typescript
// Configure for offline access
await GoogleAuth.initialize({
  clientId: 'your-web-client-id.apps.googleusercontent.com',
  serverClientId: 'your-server-client-id.apps.googleusercontent.com',
  grantOfflineAccess: true,
  scopes: ['profile', 'email', 'https://www.googleapis.com/auth/drive.readonly'],
});

// Sign in and get server auth code
const user = await GoogleAuth.signIn();
console.log('Server auth code:', user.serverAuthCode);

// Send serverAuthCode to your backend for token exchange
const response = await fetch('/api/auth/google', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ 
    serverAuthCode: user.serverAuthCode,
    userId: user.id 
  }),
});
```

### 2. Token Management with Auto-Refresh

```typescript
class TokenManager {
  private accessToken: string | null = null;
  private refreshTimer: NodeJS.Timeout | null = null;

  async initialize() {
    await GoogleAuth.initialize({
      clientId: 'your-client-id.apps.googleusercontent.com',
      scopes: ['profile', 'email'],
    });

    // Check if user is already signed in
    try {
      const auth = await GoogleAuth.refresh();
      this.accessToken = auth.accessToken;
      this.scheduleTokenRefresh();
    } catch (error) {
      console.log('No existing session found');
    }
  }

  async getValidToken(): Promise<string> {
    if (!this.accessToken) {
      const user = await GoogleAuth.signIn();
      this.accessToken = user.authentication.accessToken;
      this.scheduleTokenRefresh();
    }
    return this.accessToken;
  }

  private scheduleTokenRefresh() {
    // Refresh token every 50 minutes (tokens expire in 1 hour)
    this.refreshTimer = setTimeout(async () => {
      try {
        const auth = await GoogleAuth.refresh();
        this.accessToken = auth.accessToken;
        this.scheduleTokenRefresh();
      } catch (error) {
        console.error('Token refresh failed:', error);
        this.accessToken = null;
      }
    }, 50 * 60 * 1000);
  }

  async signOut() {
    if (this.refreshTimer) {
      clearTimeout(this.refreshTimer);
      this.refreshTimer = null;
    }
    this.accessToken = null;
    await GoogleAuth.signOut();
  }
}
```

### 3. Error Handling and Recovery

```typescript
class RobustAuthService {
  async signInWithRetry(maxRetries = 3): Promise<User> {
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
      try {
        return await GoogleAuth.signIn();
      } catch (error: any) {
        console.warn(`Sign-in attempt ${attempt} failed:`, error);
        
        if (attempt === maxRetries) {
          throw new Error(`Sign-in failed after ${maxRetries} attempts: ${error.message}`);
        }

        // Handle specific error cases
        if (error.message?.includes('network')) {
          // Wait before retrying on network errors
          await this.delay(1000 * attempt);
        } else if (error.message?.includes('cancelled')) {
          // Don't retry if user cancelled
          throw error;
        }
      }
    }
    
    throw new Error('Unexpected error in sign-in retry logic');
  }

  async makeAuthenticatedRequest(url: string, options: RequestInit = {}) {
    try {
      const token = await this.getValidToken();
      
      const response = await fetch(url, {
        ...options,
        headers: {
          ...options.headers,
          'Authorization': `Bearer ${token}`,
        },
      });

      if (response.status === 401) {
        // Token might be expired, try refreshing
        const newAuth = await GoogleAuth.refresh();
        
        // Retry request with new token
        return fetch(url, {
          ...options,
          headers: {
            ...options.headers,
            'Authorization': `Bearer ${newAuth.accessToken}`,
          },
        });
      }

      return response;
    } catch (error) {
      console.error('Authenticated request failed:', error);
      throw error;
    }
  }

  private delay(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  private async getValidToken(): Promise<string> {
    try {
      const auth = await GoogleAuth.refresh();
      return auth.accessToken;
    } catch (error) {
      // If refresh fails, try signing in again
      const user = await GoogleAuth.signIn();
      return user.authentication.accessToken;
    }
  }
}
```

### 4. Integration with Firebase

```typescript
import { initializeApp } from 'firebase/app';
import { getAuth, signInWithCredential, GoogleAuthProvider } from 'firebase/auth';

class FirebaseGoogleAuth {
  private firebaseAuth;

  constructor(firebaseConfig: any) {
    const app = initializeApp(firebaseConfig);
    this.firebaseAuth = getAuth(app);
  }

  async signInWithGoogle() {
    try {
      // 1. Sign in with CapacitorGoogleAuth
      const googleUser = await GoogleAuth.signIn();
      
      // 2. Create Firebase credential
      const credential = GoogleAuthProvider.credential(
        googleUser.authentication.idToken,
        googleUser.authentication.accessToken
      );
      
      // 3. Sign in to Firebase
      const firebaseUser = await signInWithCredential(this.firebaseAuth, credential);
      
      console.log('✅ Signed in to Firebase:', firebaseUser.user.uid);
      return firebaseUser;
    } catch (error) {
      console.error('❌ Firebase sign-in failed:', error);
      throw error;
    }
  }
}
```

## Platform-Specific Configurations

### Capacitor Configuration

```json
// capacitor.config.json
{
  "appId": "com.yourcompany.yourapp",
  "appName": "Your App",
  "plugins": {
    "GoogleAuth": {
      "scopes": ["profile", "email"],
      "serverClientId": "your-server-client-id.apps.googleusercontent.com",
      "forceCodeForRefreshToken": true,
      "clientId": "your-web-client-id.apps.googleusercontent.com",
      "iosClientId": "your-ios-client-id.apps.googleusercontent.com",
      "androidClientId": "your-android-client-id.apps.googleusercontent.com"
    }
  }
}
```

### Error Handling Patterns

```typescript
enum AuthErrorType {
  NETWORK_ERROR = 'network_error',
  USER_CANCELLED = 'user_cancelled',
  INVALID_CONFIG = 'invalid_config',
  TOKEN_EXPIRED = 'token_expired',
  UNKNOWN_ERROR = 'unknown_error'
}

class AuthErrorHandler {
  static handleError(error: any): AuthErrorType {
    const message = error.message?.toLowerCase() || '';
    
    if (message.includes('network') || message.includes('timeout')) {
      return AuthErrorType.NETWORK_ERROR;
    }
    
    if (message.includes('cancelled') || message.includes('dismissed')) {
      return AuthErrorType.USER_CANCELLED;
    }
    
    if (message.includes('invalid') || message.includes('config')) {
      return AuthErrorType.INVALID_CONFIG;
    }
    
    if (message.includes('expired') || message.includes('unauthorized')) {
      return AuthErrorType.TOKEN_EXPIRED;
    }
    
    return AuthErrorType.UNKNOWN_ERROR;
  }

  static getErrorMessage(errorType: AuthErrorType): string {
    switch (errorType) {
      case AuthErrorType.NETWORK_ERROR:
        return 'Network error. Please check your internet connection.';
      case AuthErrorType.USER_CANCELLED:
        return 'Sign-in was cancelled.';
      case AuthErrorType.INVALID_CONFIG:
        return 'Invalid configuration. Please contact support.';
      case AuthErrorType.TOKEN_EXPIRED:
        return 'Session expired. Please sign in again.';
      default:
        return 'An unexpected error occurred. Please try again.';
    }
  }
}
```

This comprehensive set of examples should help developers integrate CapacitorGoogleAuth into their applications effectively, covering various frameworks, use cases, and error scenarios.