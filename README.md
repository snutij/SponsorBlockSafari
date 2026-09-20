> [!NOTE]
> Free unsigned macOS build. This fork automatically publishes a ready to install .app for Safari on macOS, rebuilt whenever a new
> SponsorBlock version is released upstream. Download it from the Releases tab, then see instructions below for install.
> The signed version is on the Mac App Store and supports iOS too.

## Install

1. Download `SponsorBlock-Safari.app.zip` below and unzip it.
2. Move `SponsorBlock.app` to `/Applications`.
3. Launch it once. If macOS blocks it, right click the app and choose **Open**.
4. In Safari, open **Settings > Advanced** and tick **Show features for web developers**.
5. Open **Settings > Developer** and tick **Allow unsigned extensions**.
6. Open **Settings > Extensions** and enable SponsorBlock.

> [!IMPORTANT]
> The app stays installed across restarts, but macOS resets the
> "Allow unsigned extensions" setting every time Safari is relaunched.
> This is an Apple restriction for apps that are not code signed.

The signed and notarised version is available on the Mac App Store.

## Optional: sign the downloaded app with a free Apple ID

If you have a free Apple ID, you can sign the downloaded macOS app locally with
an Apple Development certificate. This avoids Safari's unsigned-extension reset,
so you should not need to re-enable **Allow unsigned extensions** every time
Safari restarts.

1. Open Xcode, then go to **Xcode > Settings > Accounts** and add your Apple ID.
2. Select the account, open **Manage Certificates...**, press **+**, and create
   an **Apple Development** certificate.
3. Move `SponsorBlock.app` to `/Applications`.
4. Run:

```sh
IDENTITY="Apple Development: your@email.example (TEAMID)"
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

To find the exact value for `IDENTITY`, run:

```sh
security find-identity -v -p codesigning
```

After signing, enable SponsorBlock once in **Safari > Settings > Extensions**.
Repeat these signing steps after downloading a new release.

## Repository Contents

This repository contains the generated Xcode files for the Safari extension available [in the App Store](https://apps.apple.com/us/app/sponsorblock-for-youtube/id1573461917).

See [this wiki page](https://github.com/ajayyy/SponsorBlock/wiki/Safari) for how to build this yourself.
