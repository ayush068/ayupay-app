# AyuPay Store Android app

This Android app wraps the live AyuPay storefront at `https://ayupay.org/` in a native WebView. Links for UPI apps and other external schemes are handed to Android so an installed payment app can open them.

## Build an installable APK

1. Install Android Studio and the Android SDK for API 34.
2. Open this `apps/ayupay-store-app` directory in Android Studio.
3. Let Gradle sync finish, then choose **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
4. Android Studio writes the debug APK under `app/build/outputs/apk/debug/app-debug.apk`.

For a release APK, use **Build > Generate Signed Bundle / APK** and create or select a signing key. Keep the signing key private and backed up; future updates need the same key.

The website and payment API must be deployed and reachable over HTTPS for checkout to work. This project contains no payment secrets.
