# 🌾 windots — Installation Guide

Step-by-step guide to deploy the **windots** configs on a fresh Windows 11 machine.

> [!WARNING]
> Not plug-and-play — cherry-pick what you need and **back up** existing configs first.

---

## 1. Prerequisites

```powershell
winget --version
```

Install the required Nerd Font (needed for prompt, status bar, and terminal icons):

```powershell
winget install JetBrains.Mono
```

Clone the repo:

```powershell
git clone https://github.com/skordeanz/windots.git C:\Windots
```

---

## 2. Install the Apps

```powershell
# System / UI
winget install glzr-io.glazewm            # tiling window manager
winget install AmN.yasb                   # status bar
winget install RamenSoftware.Windhawk     # Windows UI mods
winget install Nilesoft.Shell             # right-click menu

# Terminal & CLI
winget install JanDeDobbeleer.OhMyPosh    # shell prompt
winget install Microsoft.PowerShell       # PowerShell 7+
winget install Fastfetch-cli.Fastfetch    # system fetch

# Apps
winget install ZedIndustries.Zed          # editor
winget install Flow-Launcher.Flow-Launcher # launcher
winget install qBittorrent.qBittorrent    # torrent client
winget install AutoHotkey.AutoHotkey      # automation
winget install Microsoft.VisualStudioCode # editor (optional)
winget install Vendicated.Vencord         # Discord (optional)
winget install Spicetify.Spicetify        # Spotify (optional)
```

**Manual installs:** [Zen Browser](https://zen-browser.app/), [Thunderbird](https://www.thunderbird.net/), [mpv](https://mpv.io/), [Spotify](https://spotify.com), [Discord](https://discord.com).

---

## 3. Deploy Configs

Each section: **Source → Destination**.

### 🪟 System

#### GlazeWM
```
configs/glazewm/config.yaml → %APPDATA%\.glzr\glazewm\config.yaml
```
```powershell
Copy-Item C:\Windots\configs\glazewm\config.yaml "$env:APPDATA\.glzr\glazewm\" -Force
```
Launches YASB automatically at startup (see `startup_commands`). Requires `%APPDATA%\..\ProgramData\Microsoft\Windows\Start Menu\Programs\Yasb.lnk` or adjust that path.

#### YASB
```
configs/yasb/ → %APPDATA%\yasb\
```
```powershell
Copy-Item C:\Windots\configs\yasb\* "$env:APPDATA\yasb\" -Recurse -Force
```

#### Windhawk
No file deploy — install the mods (**Start Menu Styler**, **Taskbar Styler**, **Notification Center Styler**, **Explorer Styler**) then load each JSON via **Mod → Advanced → Mod Settings → Load**:
- `configs/windhawk/startmenu.json`
- `configs/windhawk/taskbar.json`
- `configs/windhawk/notification.json`
- `configs/windhawk/explorer.json`

#### Nile Soft Shell
```
configs/nilesoft/ → %APPDATA%\Nile Soft Shell\
```
```powershell
Copy-Item C:\Windots\configs\nilesoft\* "$env:APPDATA\Nile Soft Shell\" -Recurse -Force
```

#### AutoHotkey
```
configs/autohotkey/ → anywhere (e.g. %USERPROFILE%\ahk)
```
```powershell
Copy-Item C:\Windots\configs\autohotkey\* "$env:USERPROFILE\ahk\" -Recurse -Force
```
- `AppVol.ahk` — app volume controls
- `mute-global.ahk` — global mic mute

Add shortcuts to `shell:startup` (`Win + R` → `shell:startup`) to auto-run.

### 🖥️ Terminal & CLI

#### Oh My Posh
```
configs/ohmyposh/zen.toml → %USERPROFILE%\.config\ohmyposh\zen.toml
```
```powershell
New-Item -ItemType Directory "$env:USERPROFILE\.config\ohmyposh" -Force | Out-Null
Copy-Item C:\Windots\configs\ohmyposh\zen.toml "$env:USERPROFILE\.config\ohmyposh\" -Force
```

#### PowerShell profile
```
configs/powershell/Microsoft.PowerShell_profile.ps1 → $PROFILE
```
```powershell
Copy-Item C:\Windots\configs\powershell\Microsoft.PowerShell_profile.ps1 $PROFILE -Force
```
> [!NOTE]
> The profile contains a hardcoded path — update it to your username:
> ```powershell
> oh-my-posh init pwsh --config "C:\Users\<you>\.config\ohmyposh\zen.toml" | Invoke-Expression
> ```

Requires [Terminal-Icons](https://github.com/devblackops/Terminal-Icons):

```powershell
Install-Module Terminal-Icons -Scope CurrentUser
```

Restart the terminal.

#### Windows Terminal
```
configs/terminal/settings.json → %LOCALAPPDATA%\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json
```
```powershell
Copy-Item C:\Windots\configs\terminal\settings.json "$env:LOCALAPPDATA\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\" -Force
```

#### Fastfetch
```
configs/fastfetch/ → %APPDATA%\fastfetch\
```
```powershell
Copy-Item C:\Windots\configs\fastfetch\* "$env:APPDATA\fastfetch\" -Force
```
> [!NOTE]
> `config.jsonc` has a hardcoded `logo.source` — point it at `C:/Windots/configs/fastfetch/cat.txt` (or `windows.txt`).

### 🖱️ GUI Apps

#### Flow Launcher
```
configs/flowlauncher/ → %APPDATA%\FlowLauncher\
```
```powershell
Copy-Item C:\Windots\configs\flowlauncher\* "$env:APPDATA\FlowLauncher\" -Recurse -Force
```
Restart Flow Launcher and pick the **Catppuccin Mocha** theme in Settings.

#### Zed
```
configs/zed/ → %APPDATA%\Zed\
```
```powershell
Copy-Item C:\Windots\configs\zed\* "$env:APPDATA\Zed\" -Force
```

#### VSCode
```
configs/vscode/settings.json → %APPDATA%\Code\User\settings.json
```
```powershell
Copy-Item C:\Windots\configs\vscode\settings.json "$env:APPDATA\Code\User\" -Force
```
Install the [Catppuccin](https://marketplace.visualstudio.com/items?itemName=Catppuccin.catppuccin-vsc) extension.

#### Zen Browser
1. `about:config` → `toolkit.legacyUserProfileCustomizations.stylesheets = true`
2. `about:support` → **Profile Folder** → **Open Folder**
3. Copy into the profile's `chrome/` folder:
   - `configs/browser/userChrome.css`
   - `configs/browser/userContent.css`
   - `configs/browser/bookmarks/bookmarks.html` (optional; import via Bookmarks Manager)

#### qBittorrent
```
configs/qbittorrent/catppuccin-mocha.qbtheme → %APPDATA%\qBittorrent\themes\
```
```powershell
Copy-Item C:\Windots\configs\qbittorrent\catppuccin-mocha.qbtheme "$env:APPDATA\qBittorrent\themes\" -Force
```
Apply in **Tools → Options → Behavior → Interface → UI Theme**.

#### mpv
```
configs/mpvplayer/ → %APPDATA%\mpv\
```
```powershell
Copy-Item C:\Windots\configs\mpvplayer\* "$env:APPDATA\mpv\" -Recurse -Force
```

#### Thunderbird
1. Install `configs/thunderbird/catppuccin-mocha-lavender.xpi` via **Add-ons → Gear → Install Add-on From File**.
2. Copy chrome theme into your profile:
```
configs/thunderbird/chrome/ → %APPDATA%\Thunderbird\Profiles\<profile>\chrome\
```

#### Spicetify
```
configs/spicetify/marketplace-settings.json → %APPDATA%\spicetify\
```
```powershell
Copy-Item C:\Windots\configs\spicetify\marketplace-settings.json "$env:APPDATA\spicetify\" -Force
```

#### Vencord
Import `configs/vencord/vencord-settings.json` via **Vencord Settings → Import**.

#### Stylus
Install the [Stylus](https://addons.mozilla.org/firefox/addon/styl-us/) extension, then import `configs/stylus/stylus.json` via **Manage → Import**.

#### Raycast
Import `configs/raycast/raycast-settings.rayconfig` via **Raycast → Import Data**.

### 🖱️ File Explorer

```
configs/explorer/ → manual
```
- **Cursors:** copy `configs/explorer/cursor/*` to `C:\Windows\Cursors\` (admin), apply via Settings → Ease of Access → Cursor & pointer, or right-click `install.inf` → Install.
- **Explorer Blur:** copy `configs/explorer/explorer-blur/` to a folder, run `register.cmd` (admin), remove with `uninstall.cmd`.
- **Theme:** copy `configs/explorer/theme/` to `%APPDATA%\Microsoft\Windows\Themes\` and apply the `.theme` file.

---

## 4. Verification Checklist

- [ ] Oh My Posh prompt renders in PowerShell
- [ ] Windows Terminal uses the Catppuccin scheme
- [ ] GlazeWM tiles windows (`Alt+Enter` for terminal)
- [ ] YASB bar shows Nerd Font icons
- [ ] Flow Launcher opens (`Alt+Space` default)
- [ ] Right-click menu shows the Nile Soft theme
- [ ] Fastfetch prints the logo

---

## 5. Adding a New Config

1. Add the files under `configs/<name>/`.
2. Install the app: `winget install <AppId>` (or note manual install) in section **2**.
3. Add one block to section **3** in this format:

```
#### <App Name>
<source> → <destination>

    Copy-Item C:\Windots\configs\<name>\* "<destination>" -Recurse -Force
```
4. Optionally update the **README** configs table and this guide's checklist.
