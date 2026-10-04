# microG Companion (com.android.vending), gvwifi build

| | |
|---|---|
| APK | `com.android.vending-gvwifi-signed.apk` (versionCode 84022600, versionName `v0.0.0.40226`) |
| Source | microG GmsCore **master @ `fcb2c20377eecacf23a6a6007a4be4f834cdfb3a`** (2026-09-24, "Maps: improve HMS location and coordinate handling (#3700)"), copy in `source/` |
| License | Apache 2.0 (`source/LICENSE-microG.txt`) |
| Signed by | `CN=gvwifi microG Companion, O=Personal`, cert SHA-256 `fa7fcd26f16987d9b0122d3d9f3e948d74ae092192eded7c98b1b70732812871` |

## What's different from official microG Companion 0.3.16
**No source code was changed.** The differences:

1. **Built from unreleased master.** It includes microG PR #3742 *"Vending: Add Age Signals
   service"* (merged 2026-09-24, slated for 0.3.17). That adds
   `com.google.android.finsky.ageverification.service.AgeSignalsService` for the
   `com.google.android.finsky.ageverification.BIND` action, answering Play Age Signals
   queries with `NOT_SHARED`. Apps that query Age Signals fail with error **GA-5** without it.
2. **Signed with the owner's private key** (`gvwifi-los23\keys\companion.jks`), not microG's.
   Stock LineageOS only allows microG-signed Companions to spoof the Play Store signature. The
   gvwifi ROM patch `frameworks/base 0001` also accepts this key's certificate, and only for
   `com.android.vending`. **On any other ROM this APK cannot pass Play Store signature
   checks.**
3. **Version label** `v0.0.0.40226`: built from a shallow clone without git tags, so the
   commit-count part of the version is 0.

## Rebuild
```
git clone https://github.com/microg/GmsCore.git && cd GmsCore
git checkout fcb2c20377eecacf23a6a6007a4be4f834cdfb3a
echo "sdk.dir=$HOME/android-sdk" > local.properties          # SDK platform 35 + build-tools 35.0.0
export GRADLE_MICROG_VERSION_WITHOUT_GIT=1                     # needed for shallow clones
./gradlew --no-daemon :vending-app:assembleDefaultRelease      # JDK 21
apksigner sign --ks companion.jks --ks-pass file:companion.pass --ks-key-alias companion \
  --out com.android.vending-gvwifi-signed.apk \
  vending-app/build/outputs/apk/default/release/com.android.vending-84022600.apk
```

## Install / revert
- Install: `adb install --no-streaming com.android.vending-gvwifi-signed.apk`
  (If a differently signed Companion is installed, `adb uninstall com.android.vending` first.
  That clears only Companion's own settings.)
- Revert to official: uninstall, then install `revert-to-official-0.3.16/…apk`. Apps that
  query Age Signals show GA-5 again until official microG 0.3.17+ is installed. Official microG builds work on
  this ROM without the allowlist.
