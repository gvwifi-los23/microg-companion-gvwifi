# microG Companion with Play Age Signals, for the gvwifi ROM

microG Companion (`com.android.vending`) built **unmodified** from microG GmsCore master at
`fcb2c20377eecacf23a6a6007a4be4f834cdfb3a`, which includes microG PR #3742
("Vending: Add Age Signals service", released in 0.3.17). Apps that query Age Signals fail with error GA-5 without it.

It is signed with the ROM maintainer's own key (cert SHA-256
`fa7fcd26f16987d9b0122d3d9f3e948d74ae092192eded7c98b1b70732812871`). The
[`lineage-gvwifi`](https://github.com/gvwifi-los23/lineage-gvwifi) ROM allowlists exactly that certificate, only for
`com.android.vending` (`patches/frameworks/base/0002`). **On any other ROM this build cannot pass
Play Store signature checks**; use official microG there.

See [`BUILD-NOTES.md`](BUILD-NOTES.md) for the exact build and signing commands. The signing key is
not in this repo. Install with `adb install --no-streaming <apk>`, after microG Services.

License: Apache-2.0 (microG), see `LICENSE-microG.txt`.
