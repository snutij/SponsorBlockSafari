# SponsorBlock for Safari (macOS)

[![Latest release](https://img.shields.io/github/v/release/snutij/SponsorBlockSafari?display_name=tag&label=release)](https://github.com/snutij/SponsorBlockSafari/releases/latest)
[![License](https://img.shields.io/github/license/snutij/SponsorBlockSafari)](LICENSE)

Free macOS Safari builds of [SponsorBlock](https://sponsor.ajay.app/), automatically generated from the official upstream Safari extension release.

> [!IMPORTANT]
> This is an unofficial, community-maintained distribution. It is not affiliated
> with the SponsorBlock project or Apple. For the signed and notarized version,
> including iOS support, use the [official Mac App Store app](https://apps.apple.com/us/app/sponsorblock-for-youtube/id1573461917).

## Download and install

1. Download [`SponsorBlock-Safari.app.zip`](https://github.com/snutij/SponsorBlockSafari/releases/latest) from the latest release.
2. Unzip it and move `SponsorBlock.app` to `/Applications`.
3. Open `SponsorBlock.app` once. If macOS blocks it, Control-click the app, choose **Open**, then confirm.
4. In Safari, open **Settings > Advanced** and enable **Show features for web developers**.
5. Open **Settings > Developer** and enable **Allow unsigned extensions**.
6. Open **Settings > Extensions** and enable SponsorBlock.

> [!NOTE]
> The downloaded release is unsigned. Safari resets **Allow unsigned extensions**
> whenever it restarts. Use the optional local-signing instructions below to
> avoid that step.

## Optional: sign the downloaded app with a free Apple ID

You can locally sign the downloaded app with a free Apple ID and an **Apple
Development** certificate. Safari then recognizes it as signed, so you should
not need to re-enable **Allow unsigned extensions** after each restart.

This signature is for your local installation only. Do not redistribute the
resulting app.

### Create a certificate

1. Install [Xcode](https://apps.apple.com/app/xcode/id497799835) and open it.
2. Go to **Xcode > Settings > Accounts** and add your Apple ID.
3. Select the account, choose **Manage Certificates...**, press **+**, then
   create an **Apple Development** certificate.
4. In Terminal, find the certificate identity:

   ```sh
   security find-identity -v -p codesigning
   ```

   Copy the text in quotation marks, such as
   `Apple Development: name@example.com (TEAMID)`.

### Sign the app

Move the downloaded app to `/Applications`, replace `IDENTITY` below with your
certificate identity, then run the commands in Terminal:

```sh
IDENTITY="Apple Development: name@example.com (TEAMID)"
APP="/Applications/SponsorBlock.app"
EXTENSION="$APP/Contents/PlugIns/SponsorBlock Extension.appex"

cat > /tmp/sponsorblock-app.entitlements <<'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.security.app-sandbox</key>
    <true/>
    <key>com.apple.security.files.user-selected.read-only</key>
    <true/>
    <key>com.apple.security.network.client</key>
    <true/>
</dict>
</plist>
EOF

cat > /tmp/sponsorblock-extension.entitlements <<'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.security.app-sandbox</key>
    <true/>
    <key>com.apple.security.files.user-selected.read-only</key>
    <true/>
</dict>
</plist>
EOF

xattr -cr "$APP"

codesign --force --timestamp=none \
  --sign "$IDENTITY" \
  --entitlements /tmp/sponsorblock-extension.entitlements \
  "$EXTENSION"

codesign --force --timestamp=none \
  --sign "$IDENTITY" \
  --entitlements /tmp/sponsorblock-app.entitlements \
  "$APP"

codesign --verify --deep --strict --verbose=2 "$APP"
open "$APP"
```

Enable SponsorBlock once in **Safari > Settings > Extensions**. Repeat the
signing steps after installing a new release.

## Updates

New builds are published automatically after an upstream SponsorBlock release.
To update, download the latest ZIP, replace the existing app in `/Applications`,
and repeat the local-signing steps if you use them.

## How this repository works

The GitHub Actions workflow:

1. Checks for the latest [upstream SponsorBlock release](https://github.com/ajayyy/SponsorBlock/releases).
2. Downloads its `SafariExtension.zip` asset.
3. Generates a fresh macOS-only Safari extension project with
   `safari-web-extension-packager`.
4. Builds and publishes `SponsorBlock-Safari.app.zip`.

The committed Xcode project is upstream glue code and references generated
extension assets that are not kept in this repository. It is not the supported
way to create a release build; use the workflow or the official SponsorBlock
project for development.

## License

This repository is licensed under [GPL-3.0](LICENSE). See
[LICENSE-APPSTORE.txt](LICENSE-APPSTORE.txt) for the App Store distribution
exception provided by the SponsorBlock copyright holders.
