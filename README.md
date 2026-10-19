# Chimera

**All-in-one console modding suite for Windows.**

Xbox 360 · PS3 · PS4 · Nintendo Switch · Wii · Wii U · Xbox Live · Steam

![platform](https://img.shields.io/badge/platform-Windows%2010%2F11-blue)
![runtime](https://img.shields.io/badge/runtime-self--contained-brightgreen)
![licence](https://img.shields.io/badge/binary-closed--source-lightgrey)

---

## What is Chimera?

Chimera is one app for every console you own. Pick a platform in the sidebar, point it at your drives and consoles, and get on with it. No chasing seven different homebrew utilities, no mismatched CLI tools, no scattered save editors — the whole modding workflow lives in one window.

- **Xbox 360** — Profile editor, achievement unlocker, avatar award unlocker, game adder, save editor + resigner, STFS/XEX inspection, Horizon-style content browser, HD Key extractor, Xbox Live events capture, FTP file manager.
- **PS3** — Edat tools, PKG inspection, PARAM.SFO editor, cheat database management, SELF/EBOOT patching, webMAN FTP manager.
- **PS4** — PKG tools (LibOrbisPkg-grade), save editor, patch binary, Apollo savepatch runner, GoldHEN FTP manager, cheat trainer importer.
- **Nintendo Switch** — Library manager (NSP/XCI/NSZ/XCZ), backup manager, Switch Toolbox launcher, save editor, NSP/XCI tooling, CFW installer hub, **RCM Payload Injector** (silent libusbK driver install + payload inject in one window), FTP manager.
- **Wii / Wii U** — ISO tools (WIT/wbfs conversion), backup manager, OSC homebrew browser, dol/elf utilities, FTP managers for ftpiiu / Haxchi.
- **Xbox Live** — Live profile sign-in, modern Live interoperability tools, event capture + unlocker, achievement/avatar flows.
- **Steam** — Steam depot downloader, Steam Guard tools, Steam vault management.

Everything runs in a single 65 MB `.exe`. No .NET install, no VC++ redist dance, no "install this driver first" chain.

---

## Download

Grab the latest release from the [**Releases page**](https://github.com/Can0nical/Chimera-releases/releases/latest).

The binary is a single `Chimera.exe` — download it, double-click it, done.

**System requirements**

- Windows 10 (build 1809+) or Windows 11, 64-bit
- ~250 MB free disk (temp extract on first launch)
- Internet connection for account sign-in + online features (store, cheat database, admin backend)
- **Administrator rights** — Chimera needs them for driver installs, raw drive reads, ETW capture, and other low-level operations. UAC fires on launch.

---

## First-time setup

### 1. Launch

Double-click `Chimera.exe` from wherever you saved it. Windows will prompt for admin — accept the UAC. On first launch the self-contained runtime extracts to `%LocalAppData%\Microsoft\Chimera\...` (one-time, ~3 seconds). Subsequent launches are instant.

### 2. Sign in / create account

The welcome window appears with three options:

- **Sign in** — email + password + (if you have 2FA on) a 6-digit TOTP code.
- **Create account** — email + username + password → 6-digit verification code sent to your inbox → confirm.
- **Magic link** — passwordless: email → 6-digit code → signed in.

Forgot password? There's a link right under the password field.

Chimera remembers your session (DPAPI-sealed locally — nobody but your Windows account can read it). On next launch you'll see a quick **"Welcome back, [name]"** card and Chimera opens straight to the dashboard.

### 3. Pick a platform

Left sidebar lists every supported console. Click one to expand its tool section:

- **Nintendo Switch** → Library Manager, Backup Manager, Save Editor, NSP/XCI Tools, CFW Installer Hub, **RCM Payload Injector**, FTP Manager, Toolbox launcher.
- **Xbox 360** → Profile Viewer, Achievement Unlocker, Avatar Award Unlocker, Game Adder, Save Editor, Save Resigner, HD Key, XBL Events, STFS/XEX tools, Horizon browser, FTP Manager.
- **PS4** → PKG Tools, Save Editor, Patch Binary, Apollo, GoldHEN FTP, Cheat Trainer, more.
- …and so on for every platform.

Not sure what a tool does? Hover its sidebar entry — the subtitle on the opened page always explains.

---

## Flagship tutorial: boot Atmosphère on your Switch in RCM mode

The RCM Payload Injector page is Chimera's one-click Switch jailbreak flow — no Zadig, no TegraRcmGUI, no scattered scripts. Everything in one page.

### Step 1 — put your Switch in RCM

- Insert your RCM jig on pin 10 of the right Joy-Con rail (or short pin 10 by hand).
- Hold **Volume Up**.
- Hold **Home** (no screen will come on — that's correct).
- Press **Power** once.
- Release everything. Screen stays black — that's RCM.

### Step 2 — plug into the PC via USB-C

Windows will enumerate the device as **APX** under Device Manager (yellow bang — no driver yet).

### Step 3 — Install libusbK driver

In Chimera, open **Nintendo Switch → RCM Payload Injector**. The status card on the left will say **"NO RCM"** (red pixel-art Switch).

Click **"Install libusbK driver"**. Chimera will:

1. Mint a one-off self-signed code-signing cert and trust it in Windows's publisher store.
2. Also trust the libusbK publisher bundled in the driver package.
3. Generate a signed catalogue (.cat) for the driver package on the fly.
4. Shell Microsoft's DPInst silently with the signed package.

Result: **one UAC-less action** (Chimera is already admin), driver bound to `VID_0955 PID_7321`, status pill flips green within half a second. The whole thing takes about 5 seconds total.

> **Why so many steps?** Modern Windows won't install any OEM driver without a validly-signed catalogue, and the driver bundle ships without one. Chimera's installer solves this cleanly without ever asking you to toggle test-signing mode or disable driver signing enforcement.

### Step 4 — pick a payload

The **PAYLOAD** card has your bundled starter payloads pre-selected (hekate, Fusée, memloader, biskeydump, SX Loader, ReiNX). Click any pill from the **Recent payloads** row to switch.

> **The bundled scene payloads are vintage 2019 (good for firmware ≤ 9.x only).** For a modern Switch on firmware 14 and above, download the current `fusee.bin` from [Atmosphère releases](https://github.com/Atmosphere-NX/Atmosphere/releases) and drop it into `%APPDATA%\Chimera\payloads\`. It'll show up in the picker automatically. (Latest Atmosphère supports firmware 1.0.0 all the way through 20.x — one `fusee.bin` fits every modern Switch.)

### Step 5 — Inject

Hit **"⚡ Inject payload"**. Chimera streams the payload to the Switch's Boot ROM, triggers the hand-off, and your Switch boots straight into hekate (or whatever payload you picked).

**Done.** You now have a jailbroken Switch for the rest of this power cycle.

---

## Other common flows

### Xbox 360 — Unlock achievements on a profile

1. Point Chimera at your Xbox 360 profile (SPA/CON) in the file picker on the Profile Viewer page.
2. Switch to the **Achievement Unlocker** page.
3. Tick the achievements you want to unlock → **Unlock selected** → Chimera patches the profile and re-signs it in place.
4. Transfer the profile back to the console (via FTP, USB stick, or Horizon's device browser).

### Xbox 360 — Capture Xbox Live events (ETW)

1. Sign into Xbox Live on this PC (the Chimera app detects the Xbox app's XAuth session).
2. Open **Xbox Live → Events**.
3. Set the target title ID (or let Chimera infer from the current foreground game).
4. **Start capture**. `netsh trace` + `logman` run under the hood with Chimera's elevated session — the full ETW stream is dumped and parsed into a human-readable event feed.
5. Use the captured events for achievement unlock replay, profile audit, or diagnostics.

### PS4 — Load a cheat trainer with GoldHEN

1. Open **PS4 → FTP Manager**, connect to your console (port `2121`, user `anonymous`).
2. Switch to the **Cheat Trainer** importer page.
3. Drag a `.goldhen` cheat JSON onto the drop zone → Chimera formats + validates it + uploads it to `/data/GoldHEN/cheats/<game>/`.
4. In-game, trigger GoldHEN's menu (L1+L2+L3+R3 default), load the cheats, enjoy.

### Switch — Clean up your dump library

1. Open **Nintendo Switch → Library Manager**.
2. Point it at your NSP/XCI dump folder.
3. Chimera recursively scans and groups by base title ID (base / update / DLC separated).
4. Review the 5-fact strip (total titles / base / updates / DLC / size) and the sortable DataGrid.
5. Reveal, batch-delete, or re-export.

### Multi-console FTP

Every FTP page across every platform is the same dual-pane widget. Add a console once (hostname + port + credentials) and it's remembered across Switch sys-ftpd, PS4 GoldHEN, PS3 webMAN, Vita VitaShell, Wii U ftpiiu, Xbox 360 Aurora. Passwords are DPAPI-sealed on disk — nobody else on your PC can read them.

---

## Where Chimera stores things

| What                        | Where                                                             |
|-----------------------------|-------------------------------------------------------------------|
| Account session + settings  | `%APPDATA%\Chimera\`                                              |
| Switch RCM payloads         | `%APPDATA%\Chimera\payloads\`                                     |
| Console profiles (FTP)      | `%APPDATA%\Chimera\profiles\`                                     |
| Downloads (homebrew, mods)  | `%USERPROFILE%\Downloads\Chimera\` (configurable in Settings)     |
| Driver install cache        | `%APPDATA%\Chimera\drivers\`                                      |
| Toolbox / Backup pointers   | configured per tool, remembered between launches                  |
| Theme, palette, typography  | `%APPDATA%\Chimera\appearance.json` (live-editable in Settings)   |

Uninstall is a drag-to-recycle on the .exe plus (optionally) wiping `%APPDATA%\Chimera\`. Nothing else is installed system-wide beyond the libusbK driver, which you can remove manually from Device Manager.

---

## Troubleshooting

**UAC prompt fires on every launch.** By design — Chimera needs admin for raw drive reads, ETW capture, driver installs, and other low-level operations. If you want to skip UAC permanently, create a Task Scheduler task that runs Chimera elevated and launch via that task's shortcut.

**"NO RCM" never flips to green with Switch plugged in.** Driver isn't bound. Open Device Manager, verify the APX device is under **libusbK Usb Devices**. If not, hit **Install libusbK driver** in Chimera. If the install reports failure, check `%TEMP%\chimera-apx-install.log` for the specifics.

**Switch doesn't actually boot after "Payload injected".** Your payload is probably too old for your firmware. The bundled 2019-era payloads only cover firmware ≤ 9.x. Download a current `fusee.bin` from Atmosphère releases and drop it in `%APPDATA%\Chimera\payloads\`.

**App crashes on startup with no window.** Usually a corrupted settings file. Delete `%APPDATA%\Chimera\settings.json` and relaunch.

**Can't connect to the mod database / store pages are blank.** Settings → Advanced → **Mod database base URL** — make sure it's reachable. Default is Chimera's hosted mirror.

**Report a bug.** Open an issue on this repo. Include: your Windows version (`winver`), the output of `%TEMP%\chimera-*.log`, and a short reproduction.

---

## FAQ

**Is Chimera free?** Yes.

**Is Chimera open source?** No. The binary distribution lives here on this repo; the source is private.

**Does Chimera upload my saves / profiles / dumps anywhere?** No. Everything stays on your machine. Account authentication is the only network traffic to Chimera's own backend; everything else is local-only.

**Will this get me banned on Xbox Live / PSN / Nintendo Switch Online?** Modding your console carries risk. Chimera is a tool — how you use it determines consequences. Don't go online on consoles you've modded unless you've accepted the ban risk.

**Does Chimera support older firmware / older consoles?** Chimera supports the modern scene state across each platform. For truly ancient firmware (Xbox 360 pre-2008, Wii original NAND, etc.), you may still need a dedicated tool. Chimera will tell you when a platform-specific requirement isn't met.

**I'm on Linux / macOS.** Chimera is Windows-only. The underlying stack (Avalonia 12, native libusb/DPInst/DPAPI) is deeply tied to Windows APIs. No cross-platform build planned.

---

## Author

Built by **GHOST** (`Can0nical` on GitHub).

Private source, public binary. Issues and feature requests welcome on this repo.
