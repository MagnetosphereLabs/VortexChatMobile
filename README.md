# Vortex Chat Mobile

The Android and iOS client fork for [Vortex Node](https://github.com/MagnetosphereLabs/VortexNode), based on Fluxer’s Flutter interface.

**This fork is not yet a Vortex-compatible mobile release.** The inherited Fluxer API client does not implement Vortex account unlock or message encryption. Changing its server URL is insufficient, and upstream APKs or TestFlight builds are not Vortex clients. Use the Vortex web client or a qualified Linux release while the mobile protocol integration is completed.

## Shared implementation

Android and iOS should share one Dart adapter backed by the same native cryptographic protocol implementation. The web and desktop reference is `fluxer_app/src/vortex` in [VortexChat](https://github.com/MagnetosphereLabs/VortexChat/tree/vortex/integration-20261022). The adapter must perform OPAQUE account unlock, identity verification, MLS membership transitions, archive and attachment encryption, and media-key derivation locally before invoking network transport.

The generated Dart SDK supplies the existing view models. It must not send decrypted messages, passwords, attachment names or content keys to a Fluxer-compatible API. Native interoperation must use the exact protocol and fixtures already exercised against Vortex Node.

## Development checkout

```sh
git clone --branch vortex/integration-20261022 --recurse-submodules https://github.com/MagnetosphereLabs/VortexChatMobile.git
cd VortexChatMobile
flutter pub get
flutter analyze
```

Submodules point to the Vortex forks and remain pinned to explicit commits. Do not use `git submodule update --remote` in a release build. Flutter and Dart requirements are declared in `pubspec.yaml`; the inherited platform build workflows remain reference material until Vortex integration passes interoperability tests.

## Native release requirements

- Protect wrapping keys with Android Keystore or iOS Keychain and encrypt local databases and attachments.
- Configure Vortex bundle IDs, Android application IDs, URL schemes, app groups, share/broadcast extensions and signing together.
- Deliver generic push wakeups through APNs, FCM or UnifiedPush. Resolve messages on the device; notification relays must never receive conversation text, names or keys.
- Verify two-device messaging, membership removal, recovery, calls, screen sharing, suspended-device notifications and upgrades before distributing an APK, AAB or iOS archive.
- Keep publisher credentials in protected GitHub environments or repository secrets, outside source control.

The Node installer runs on a server; mobile installation uses signed application packages. Installing this Flutter source does not install a Node. Federation is outside the initial single-Node protocol integration.

## License

The upstream AGPL-3.0-or-later license and source notices remain in place. [Original upstream documentation](UPSTREAM_README.md) is retained for development reference; its downloads and support links belong to Fluxer.
