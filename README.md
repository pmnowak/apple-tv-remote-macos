<p align="center">
  <img src="Screenshots/app-icon.png" width="128" alt="Apple TV Remote app icon">
</p>

<h1 align="center">Apple TV Remote for macOS</h1>

<p align="center"><strong>Your Apple TV remote, one click from the Mac menu bar.</strong></p>

<p align="center">
  Swipe, click, type, control playback, and switch devices without reaching for another remote.
</p>

<p align="center">
  <a href="https://github.com/pmnowak/apple-tv-remote-macos/releases/download/v2.1.0/AppleTVRemote-2.1.0-arm64.dmg"><strong>Download v2.1.0 for Apple silicon</strong></a>
  ·
  <a href="https://github.com/pmnowak/apple-tv-remote-macos/releases/latest">Release notes</a>
</p>

<p align="center">
  <img src="Screenshots/remote-dark.png" width="390" alt="Apple TV Remote menu showing a fictional Apple TV and sample Now Playing item">
</p>

## A remote that belongs on your Mac

Apple TV Remote is a compact SwiftUI menu-bar controller for Apple-silicon Macs. It discovers compatible devices on your local network, remembers paired Apple TVs in macOS Keychain, and keeps the controls close without occupying a full desktop window.

| Navigate naturally | Control playback | Stay in the flow |
| --- | --- | --- |
| Swipe on the touch surface or use directional buttons. | Play, pause, seek, mute, change volume, and control power. | Switch Apple TVs, see Now Playing, and send text when tvOS requests keyboard input. |

<p align="center">
  <img src="Screenshots/remote-controls.png" width="520" alt="Close-up of the swipe surface and playback controls in Apple TV Remote">
</p>

## Highlights

- Native SwiftUI menu-bar interface with macOS materials and system colors
- Bonjour discovery for Apple TVs and AirPlay speakers
- Swipe navigation, click/select, menu, home, directional controls, and 10-second seek
- Play/pause, power, mute, and volume controls where the active Apple TV route supports them
- Optional Now Playing metadata and artwork after separate AirPlay pairing
- Keyboard text entry when an editable field is focused on tvOS
- Credentials stored in macOS Keychain; no telemetry or cloud account

## Install

This release requires macOS 14 or later, an Apple-silicon Mac, and the open-source [`pyatv`](https://pyatv.dev/) command-line tools. The Mac and Apple TV must be on the same local network.

1. Install `pipx` and `pyatv`:

   ```sh
   brew install pipx
   pipx ensurepath
   pipx install pyatv
   ```

2. Download [`AppleTVRemote-2.1.0-arm64.dmg`](https://github.com/pmnowak/apple-tv-remote-macos/releases/download/v2.1.0/AppleTVRemote-2.1.0-arm64.dmg), open it, and drag **Apple TV Remote** to Applications.
3. Open the remote icon in the menu bar, choose an Apple TV, select **Pair Apple TV**, and enter the PIN shown on the television.

The current download is ad-hoc signed and not notarized. macOS may block the first launch; in Finder, Control-click the app, choose **Open**, then confirm. A physical Apple TV is required for pairing and end-to-end operation.

## Feature compatibility

| Capability | Support |
| --- | --- |
| Device discovery and switching | Yes |
| Companion PIN pairing | Yes |
| Swipe and directional navigation | Yes; gestures are sent when the swipe ends |
| Select, menu, home, play/pause, seek | Yes |
| Keyboard input | When tvOS has a text field focused |
| Now Playing | Requires separate AirPlay pairing; availability depends on tvOS/app metadata |
| Volume and power | Depends on the active audio route, network standby, and HDMI-CEC setup |
| Siri / voice control | No public third-party Remote Siri API is available |
| Continuous touch / pressure gestures | Not provided by the CLI-backed transport |

## How it works

The interface, Bonjour discovery, Keychain integration, and process management are native macOS code. Apple does not publish a public macOS framework for third-party Apple TV remote commands, so this app uses the reverse-engineered Companion and AirPlay support in the [`pyatv`](https://github.com/postlund/pyatv) CLI. `pyatv` is installed separately and is not bundled in the download.

Persistent credentials are stored in the login Keychain and each CLI invocation uses `--storage none`. Because credentials must be supplied to the CLI as process arguments, another process running as the same macOS user may be able to inspect them briefly. Diagnostic logs can contain device names, identifiers, addresses, discovery results, and playback/backend information; review logs before attaching them to a public issue. Logs are stored in `~/Library/Logs/Apple TV Remote/` and can be deleted when the app is closed.

## Build and test

```sh
swift test
./Scripts/build-app.sh
open build/AppleTVRemote.app
```

The unit suite covers command mapping and pairing/metadata parsing. End-to-end validation requires a real Apple TV on the same network.

## Disclaimer

This is an independent, unofficial project and is not affiliated with, endorsed by, or sponsored by Apple Inc. Apple, Apple TV, macOS, iPhone, iPad, and Siri are trademarks of Apple Inc. The screenshots use fictional media data and contain no personal device information.

No license is granted for this repository unless a license file is added. Third-party components remain subject to their own licenses.
