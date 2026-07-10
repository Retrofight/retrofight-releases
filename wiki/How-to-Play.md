# How to Play RetroFight Beta

This guide explains the basic RetroFight beta flow for Windows and Linux users.

## Before You Start

You need:

- A Windows PC or Linux x64 system.
- A RetroFight beta release build.
- A RetroFight account registered at `https://retrofight-web.vercel.app`.
- Network access to the RetroFight server.
- Permission for RetroFight and RetroFight FBNeo through your firewall.
- Game files that you legally own and are authorized to use.

RetroFight does not include, distribute, or download game ROMs.

## Requirements

The RetroFight package includes the RetroFight FBNeo base runtime. The active
RetroFight FBNeo executable is provided by the RetroFight service at runtime and
is verified by the client before launch.

On Windows, the current RetroFight FBNeo beta runtime is packaged for clean
Windows installs. You should not need to install legacy DirectX 9 D3DX or
Microsoft Visual C++ Redistributable packages before playing.

On Linux, use the x64 AppImage or Debian package. Linux builds include an
embedded Wine runtime for launching RetroFight FBNeo, so you do not need to
install system Wine, `wine32`, or `wine64` for normal beta use.

The runtime is still x86 because the bundled GGPO library is x86, but the MSVC
runtime is statically linked on Windows. The runtime folder must keep
`ggponet.dll` next to `retrofightfbneo.exe`.

This has been validated on a clean Windows VM without Microsoft Visual C++
Redistributable 2015-2022 x86 and without legacy DirectX 9 D3DX installed.

## Create Or Use Your Account

Register or manage your RetroFight account on the website:

```txt
https://retrofight-web.vercel.app
```

The desktop client only supports login. It does not provide in-app registration.

Website registration requires accepting the Terms of Use and Privacy Policy.
The website footer links the public legal pages in English and Italian.
You can accept or revoke this privacy consent from the website profile page.
If you need to remove your account, open the website profile page, choose the
account deletion action, and type your account email in the confirmation dialog.

Use the same email and password you created on the website to sign in from the
RetroFight client. Your website player name is used as your lobby and in-game
display name.

Only one active RetroFight client session is allowed per account.

## Install Or Launch

Use one of the release artifacts:

- Run `RetroFight-Windows-Setup-0.6.5-beta.0-x64.exe` to install RetroFight on
  Windows.
- Extract `RetroFight-Windows-0.6.5-beta.0-x64.zip` and launch the portable
  Windows build.
- Launch `RetroFight-Linux-0.6.5-beta.0-x64.AppImage` on Linux x64.
- Install `RetroFight-Linux-0.6.5-beta.0-x64.deb` on Debian-based Linux x64
  distributions.

Website downloads show a legal notice before the file opens. Select
`I UNDERSTAND AND AGREE` to continue the download.

On first launch, the RetroFight client shows a legal notice before the login
screen. Select `I UNDERSTAND AND AGREE` to continue. If you do not accept the
notice, the client exits.

If RetroFight FBNeo reports a missing runtime dependency, open the diagnostics
folder from the Window menu and confirm the packaged runtime files are intact.

## Add Your Game Files

Open RetroFight and use `Window > Open ROMs Folder` to open the local ROM folder.

Place the required game ZIP files in that folder. RetroFight will not let you enter a game lobby when the required local game file is missing.

## Start A Match

RetroFight uses a "Play Now" flow: you press Play once and keep playing while the system finds opponents for you.

1. Launch RetroFight and sign in with your RetroFight account.
2. Select a game from the catalog and choose **Casual** or **Ranked**.
3. Press **Play**. RetroFight launches the game into local training/free play and starts searching for an opponent in the background — you never wait on an empty screen.
4. When an opponent is found, your game **pauses** and a **Match Found** popup shows **both players** side by side with their positions (**1P** vs **2P**, with **YOU** highlighted), each with name, flag, rank, and rating, plus the ping and connection quality between you and a **RANKED / VERSUS** badge.
5. Press **Accept** (**A** button, or Enter/Space) to start the match; press **Tab** (or **B**) to toggle Casual/Ranked for this match; press **Escape** (or **Start**) to **Decline**. If you decline or nobody accepts in time, the popup closes, your game resumes exactly where it was, and the search continues automatically.
6. When both players accept, RetroFight runs UDP direct connection checks and starts the match.
7. When a ranked match is decided you see a **Results** screen (Victory/Defeat, score, mode), then a post-match choice: **Rematch**, **New Search**, or **Exit** — nothing restarts on its own.
8. **Rematch** replays the same pair (only if both choose it in time). **New Search** finds a fresh opponent; two players who just played are not instantly re-paired for a short while, so use Rematch to replay right away. **Exit** returns to the catalog.
9. Prefer to just wait instead of playing while searching? Turn off **Play while searching** in **Settings** — the Match Found popup then appears in the app window.

Ranked is mutual opt-in: a match counts as **Ranked (FT3)** only when both players choose Ranked; otherwise it is Casual. Your preference is remembered between sessions.

## Controller And Hotkeys

RetroFight supports client navigation through keyboard, browser Gamepad API, and
native Windows XInput controllers. XInput is Windows-only. Open `Map Client Switch` from the app header
or press `F9` while in the launcher to choose a preset or customize the client
controls.

Default keyboard preset:

- Up: `ArrowUp`
- Down: `ArrowDown`
- Left: `ArrowLeft`
- Right: `ArrowRight`
- Select: `Enter`
- Back / quick actions: `Escape`
- Map Client Switch: `F9`

Default Gamepad API preset:

- Up: left stick / axis 1 negative
- Down: left stick / axis 1 positive
- Left: left stick / axis 0 negative
- Right: left stick / axis 0 positive
- Select: button 0
- Back / quick actions: button 1
- Map Client Switch: button 9

Default XInput preset:

- Up: D-pad up
- Down: D-pad down
- Left: D-pad left
- Right: D-pad right
- Select: `A`
- Back / quick actions: `B`
- Map Client Switch: `Start`

System exit hotkey:

- `Start + Back/Coin + Left`

This exit hotkey is always active while RetroFight is running. If the emulator is
open, it closes the emulator and returns to the launcher. If no emulator is
running, it exits RetroFight.

## Connection Status

During match startup, RetroFight can show:

- `Signaling`: players are exchanging match setup information.
- `UDP direct`: RetroFight is checking whether direct UDP play is possible.
- `Connected`: the direct path is ready and the runtime can start.
- `Direct failed`: UDP direct play did not succeed.

If direct UDP fails, RetroFight shows that this beta has no GGPO-compatible UDP
relay yet. Allow UDP through your firewall, try another network, or choose
another opponent.

## Troubleshooting

Server unavailable:

- Check your internet or LAN connection.
- Restart RetroFight.
- Try again after confirming the server is online.

Login failed:

- Confirm that the account was registered at `https://retrofight-web.vercel.app`.
- Check that email and password are correct.
- If the same account is already connected from another RetroFight client, close the other client and try again.
- If you changed your player name on the website, sign out and sign in again from the client.

Game file missing:

- Open `Window > Open ROMs Folder`.
- Place the required game ZIP in the folder.
- Return to the catalog and select the game again.

Runtime does not start:

- Use `Window > Open Diagnostics Folder` and check `rfbneo-diagnostics.log`.
- If the crash shows exit code `0xC0000135`, Windows could not find a required DLL; confirm `ggponet.dll` is still next to `retrofightfbneo.exe`.
- On Linux, use the packaged AppImage or Debian build. The beta includes an
  embedded Wine runtime; installing a separate system Wine package should not be
  required.
- If an older runtime build reports `d3dx9_43.dll` as missing, install the official Microsoft DirectX End-User Runtime Web Installer from `https://www.microsoft.com/en-us/download/details.aspx?id=35`.
- If an older x86 runtime build reports `MSVCP140.dll` or `VCRUNTIME140.dll` as missing, install Microsoft Visual C++ Redistributable 2015-2022 x86 from `https://aka.ms/vs/17/release/vc_redist.x86.exe`.
- Check whether antivirus software blocked the runtime.
- Reinstall or re-extract the RetroFight release build.

UDP direct failed:

- Allow RetroFight and RetroFight FBNeo through your firewall.
- Avoid VPNs, hotspots, hotel networks, school networks, office networks, or restrictive routers during beta testing.
- Try again from a home network.
- If the network blocks UDP direct traffic, this beta does not provide a relay fallback.

## Where To Find Local Data

On Windows, RetroFight stores per-instance data under:

```txt
%APPDATA%\RetroFight-instances\
```

On Linux, RetroFight stores app data in the standard Electron user data location
for your desktop environment.

The ROM folder is available from the app menu through `Window > Open ROMs Folder`.
