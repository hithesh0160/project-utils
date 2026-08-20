# Appium Android

Appium is used for Android UI automation when browser-level testing is not enough.

## Used In

- `Appium-Tests` - Java Appium client, TestNG, Maven, Android emulator workflow.

## Maven Dependencies

```xml
<dependency>
  <groupId>io.appium</groupId>
  <artifactId>java-client</artifactId>
  <version>9.1.0</version>
</dependency>
<dependency>
  <groupId>org.testng</groupId>
  <artifactId>testng</artifactId>
  <version>7.11.0</version>
</dependency>
```

## Local Checklist

- Android SDK installed.
- Emulator or physical device available.
- Appium server installed and running.
- APK path is known.
- Desired capabilities match the device and app.

## GitHub Actions Pattern

```yaml
- uses: android-actions/setup-android@v2
- uses: reactivecircus/android-emulator-runner@v2
  with:
    api-level: 30
    target: google_apis
    arch: x86_64
    profile: pixel
    script: adb devices
```

## When To Use

- Native Android flows.
- Permissions, notifications, or system UI.
- App behavior that cannot be tested in a browser.

## Avoid When

- A unit test or browser test can cover the behavior.
- The app has not stabilized enough for UI automation.

