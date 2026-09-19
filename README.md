> [!NOTE]
> Free unsigned macOS build. This fork automatically publishes a ready to install .app for Safari on macOS, rebuilt whenever a new
> SponsorBlock version is released upstream. Download it from the Releases tab, then see instructions bellow for install.
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

## Repository Contents

This repository contains the generated Xcode files for the Safari extension available [in the App Store](https://apps.apple.com/us/app/sponsorblock-for-youtube/id1573461917).

See [this wiki page](https://github.com/ajayyy/SponsorBlock/wiki/Safari) for how to build this yourself.
