# React Vite Capacitor

This stack is useful for web apps that also ship as Android apps through Capacitor.

## Used In

- `Workout-Timer` - React, Vite, Capacitor, Firebase, AdMob, local notifications, TTS, PWA.

## Common Commands

```bash
npm install
npm run dev
npm run build
npx cap sync android
npx cap open android
```

## Variant Builds

```bash
npm run dev:free
npm run dev:premium
npm run build:free
npm run build:premium
```

## Useful Tools

- `vite` - fast frontend dev/build.
- `@capacitor/core`, `@capacitor/android` - native Android wrapper.
- `firebase` and `firebase-tools` - auth, hosting, Firestore, deploys.
- `vite-plugin-pwa` - PWA support.
- `patch-package` - preserve local dependency patches.
- `lucide-react` - icon system.

## Capacitor Checklist

- Run web build before sync.
- Keep Android package names clear per variant.
- Store signing files outside Git.
- Check native permissions for notifications, ads, auth, and TTS.
- Verify behavior on real Android when using native plugins.

## Patch Package

```bash
npm install
npx patch-package package-name
```

Keep generated patches in `patches/` and make sure `postinstall` runs `patch-package`.

