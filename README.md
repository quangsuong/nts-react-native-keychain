# 🔐 nts-react-native-keychain

A secure credentials storage module for React Native — **forked from [`react-native-keychain`](https://github.com/oblador/react-native-keychain)** with biometric caching disabled.

> This version sets `validityDuration = 0` for **biometric authentication**, ensuring the prompt always appears on access.

---

## ✨ Features

- ✅ Secure credentials storage (Android Keystore / iOS Keychain)
- ✅ Biometric auth with Face ID / Touch ID / Fingerprint
- ✅ **Biometric prompt appears every time** (no caching)
- ✅ Full support for `SECURE_HARDWARE`, `ACCESS_CONTROL`, etc.
- ✅ Drop-in replacement for the original package

---

## 📦 Installation

```bash
yarn add nts-react-native-keychain
# or
npm install nts-react-native-keychain
```

---

## ⚙️ Basic Usage

```ts
import * as Keychain from 'nts-react-native-keychain';

await Keychain.setGenericPassword('username', 'secureToken', {
  accessControl: Keychain.ACCESS_CONTROL.BIOMETRY_CURRENT_SET,
  accessible: Keychain.ACCESSIBLE.WHEN_UNLOCKED,
  securityLevel: Keychain.SECURITY_LEVEL.SECURE_HARDWARE,
  authenticationPrompt: {
    title: 'Xác thực sinh trắc học',
    cancel: 'Huỷ',
  },
});

const credentials = await Keychain.getGenericPassword({
  authenticationPrompt: {
    title: 'Xác thực lại',
    cancel: 'Đóng',
  }
});

console.log(credentials); // { username: 'username', password: 'secureToken' }
```

---

## 📌 Differences from the original

| Behavior                      | `react-native-keychain` | `nts-react-native-keychain` |
|------------------------------|--------------------------|------------------------------|
| Biometric caching (Android)  | ✅ Cached                 | ❌ Always prompt (`0s`)      |
| Validity duration            | Configurable             | **Forced to `0`**            |
| Ideal use case               | Common apps              | Banking / secure apps        |

---

## 🔧 Native Requirements

### Android

- Min SDK 23+
- BiometricPrompt API (AndroidX)
- Uses `KeyGenParameterSpec` with:
  ```kotlin
  setUserAuthenticationValidityDurationSeconds(0)
  ```

### iOS

- No extra setup
- Enable Face ID / Touch ID in Xcode capabilities
```ts
Run pod install in ios/ directory to install iOS dependencies.
If you want to support FaceID, add a NSFaceIDUsageDescription entry in your Info.plist.
Re-build your Android and iOS projects.
```

---

## 🛠 API

Same API as [`react-native-keychain`](https://github.com/oblador/react-native-keychain#api):

- `setGenericPassword()`
- `getGenericPassword()`
- `resetGenericPassword()`
- `getSupportedBiometryType()`

Includes:

```ts
Keychain.ACCESS_CONTROL
Keychain.ACCESSIBLE
Keychain.SECURITY_LEVEL
```

---

## 🧠 Author

Maintained by [@quangsuong](https://github.com/quangsuong)  
Forked from [@oblador/react-native-keychain](https://github.com/oblador/react-native-keychain)

---

## 📄 License

MIT
