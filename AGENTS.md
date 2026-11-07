# OwnTV setup handoff for Android / Google TV

Use this guide when the owner asks you to set up OwnTV on their Android TV from a computer on the same home network. The channel playlist is included beside this file as `Indonesia-public-TV.m3u`.

## Before you start

- Make sure the TV is on, connected to the home Wi-Fi, and the owner has asked you to install OwnTV.
- The computer running ADB must be on the same local network as the TV. Do not expose ADB to the public internet or create router port forwards.
- Use only the official OwnTV release: <https://github.com/ahXN00/OwnTV/releases/latest>.
- If the TV asks to allow debugging from this computer, the owner must approve that prompt on the TV.

## Connect with ADB

Install Android SDK Platform Tools on the computer if `adb` is not available. On the TV, enable Developer options and the debugging option shown by its Android TV version. Menu labels vary by manufacturer.

### Wireless debugging with pairing code

Use this when the TV has **Wireless debugging** and **Pair device with pairing code**:

1. Open the TV's Wireless debugging screen and choose Pair device with pairing code. Keep the screen open.
2. From the computer, run `adb pair TV_IP:PAIRING_PORT`, then enter the temporary code shown on the TV.
3. Use the separate IP address and connection port shown on the Wireless debugging screen: `adb connect TV_IP:CONNECTION_PORT`.
4. Run `adb devices -l` and make sure the TV appears as `device`.

The pairing port and connection port are different. Use the current values shown on the TV; they can change. Pairing normally requires Android 11 or newer on phones and Android 13 or newer on TVs.

### Older network-debugging option

If the TV only offers **Network debugging** or **ADB debugging**, enable it, then run `adb connect TV_IP:5555`. Accept the RSA authorization prompt on the TV and check `adb devices -l`.

If there is no wireless/network debugging option, stop and ask the owner to enable it. Do not try to bypass TV security or the authorization prompt.

## Install OwnTV

Download the `OwnTV.apk` asset from the official latest release, then install it:

```sh
adb install -r OwnTV.apk
adb shell monkey -p tv.own.owntv 1
```

If Android blocks installation from this source, ask the owner to approve the TV's install prompt or allow the source in Android settings. Do not install a repackaged APK from a mirror.

## Import the included live-TV playlist

Push the included M3U file to the TV's Downloads folder:

```sh
adb push Indonesia-public-TV.m3u /sdcard/Download/Indonesia-public-TV.m3u
```

In OwnTV, add it from **Settings → Sources & guide → Playlists → Add Source → Type it here → M3U**, then choose the local file. The first-run setup also offers an **Import** option. OwnTV's **Remote** option can instead transfer an M3U file from a phone or computer on the same Wi-Fi.

The included playlist has 84 streams: 77 Indonesian channels and 7 international news channels. Each stream passed a short sample decode from an Indonesia-based network route. That is only a brief playback check; streams can still fail later, and entries marked **Not 24/7** may be off air at times. The test was not performed inside OwnTV, so verify a few channels in the app. Some rows include optional referrer or user-agent hints; OwnTV may not honor every hint.

The file is live TV only; it does not provide movies or series. The Indonesian entries came from iptv-org's country playlist and the international news entries from its news playlist. iptv-org describes these as user-submitted links believed to have been made publicly available by rights holders, but it does not control the stream hosts or guarantee availability or rights. Check the broadcaster's own viewing options where a stream fails or its rights are unclear.

## Set Indonesian subtitles

Set **Settings → Sound & subtitles → Preferred audio / subtitle language → Indonesian**. OwnTV will prefer an Indonesian subtitle track when the stream supplies one.

For a film without an Indonesian subtitle track embedded in its source:

1. Create a free account at <https://www.opensubtitles.com/>.
2. Sign in inside OwnTV under **Settings → Content & metadata → OpenSubtitles**.
3. While the film is playing, open **Subtitles → Search OpenSubtitles**, choose Indonesian, and select a match. This search is per title; preferred language does not create subtitles when none are available.

OwnTV is a player, not a film catalog. To populate its Movies or Series sections, the owner must provide a separate source that includes VOD. The included M3U contains live channels only.

## Finish safely

- Test a few channels from different groups and tell the owner which ones play on the TV. A short test on another network is not a guarantee of playback on this TV.
- When setup is done, turn off Wireless debugging / Network debugging on the TV unless the owner explicitly wants it left on.
- Disconnect ADB with `adb disconnect` when finished.

## References

- OwnTV releases and setup guide: <https://github.com/ahXN00/OwnTV>
- Google ADB wireless debugging: <https://developer.android.com/tools/adb>
- OpenSubtitles: <https://www.opensubtitles.com/>
- Indonesian channel source: <https://iptv-org.github.io/iptv/countries/id.m3u>
- International news source: <https://iptv-org.github.io/iptv/categories/news.m3u>
