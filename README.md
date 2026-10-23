# BEAMid

Shows this computer's name, IP addresses and system info on the desktop, drawn onto the wallpaper you already have. Handy for spotting which machine is which in a rack or at FOH, and reading off an IP without opening anything.

## Install
- **Mac**: `dist/BEAMid-1.0.0-mac-arm64.dmg` (Apple Silicon). The app isn't signed, so the first time: right-click BEAMid → **Open** → **Open**.
- **Windows**: `dist/BEAMid-Setup-1.0.0.exe` installs it; `dist/BEAMid-Portable-1.0.0.exe` runs without installing. SmartScreen will warn on first launch: **More info → Run anyway**.

The installed version starts with the computer (untick **Start BEAMid when this computer starts** to stop that).

## How it works
- BEAMid reads your current wallpaper, draws the info card on top, and sets the result as the wallpaper. On Mac every screen gets its own picture; on Windows one picture is used for all screens, sized for the main one.
- It checks for a new IP address every 5 seconds and everything else every minute. Disk space and uptime only redraw the wallpaper every 30 minutes.
- Change your wallpaper as usual while BEAMid runs: it notices within a minute and draws on the new one.
- **Hide from desktop** (window or tray menu) and **Quit** put your original wallpaper back.
- Runs in the **menu bar** on Mac and the **system tray** on Windows. The tray menu lists the IP addresses: click one to copy it.

## Limits
- Mac: the wallpaper is set for the current Space only. Other Spaces keep their own wallpaper.
- Mac: newer macOS hides the Wi-Fi network name from apps without Location access, so that line may be missing.
- Dynamic or solid-colour wallpapers: the card is drawn on a still picture (or the colour, or a dark BEAM background).

## Development
```
npm start                 # run the app (changes the real wallpaper)
npm run dry               # test mode: draws the wallpapers in the window only, desktop unchanged
npm run icon              # redraw build/icon.png
npm run dist:mac          # build the Mac .dmg
npm run dist:win          # build the Windows installer + portable
npm run release           # publish a Windows update to GitHub (see scripts/release.js)
```
This Mac has no Node.js installed, so run through Electron: `BEAMID_DRY_RUN=1 node_modules/electron/dist/Electron.app/Contents/MacOS/Electron .`

Code: `main.js` (tray, window, keeping the wallpaper current), `lib/sysinfo.js` (computer and network info), `lib/wallpaper.js` (reading/setting the wallpaper on Mac and Windows), `lib/render.js` (draws a screen's wallpaper off-screen), `src/overlay.*` (the card), `src/app.*` (settings window). Settings live in the app's user-data folder (`settings.json`), drawn wallpapers in `wallpapers/`.
