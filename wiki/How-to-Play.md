# How to Play RetroFight Beta

This guide explains the basic RetroFight beta flow for Windows users.

## Before You Start

You need:

- A Windows PC.
- A RetroFight beta release build.
- Network access to the RetroFight server.
- Permission for RetroFight and RetroFight FBNeo through Windows Firewall.
- Game files that you legally own and are authorized to use.

RetroFight does not include, distribute, or download game ROMs.

## Install Or Launch

Use one of the release artifacts:

- Run `RetroFight-win-x64.exe` to install RetroFight.
- Extract `RetroFight-win-x64.zip` and launch the portable build.

If RetroFight FBNeo does not start because `d3dx9_43.dll` is missing, install the Microsoft DirectX End-User Runtime legacy package and restart RetroFight.

## Add Your Game Files

Open RetroFight and use `Window > Open ROMs Folder` to open the local ROM folder.

Place the required game ZIP files in that folder. RetroFight will not let you enter a game lobby when the required local game file is missing.

## Start A Match

1. Launch RetroFight.
2. Select a game from the catalog.
3. Enter the game lobby.
4. Choose an available player and send a challenge.
5. The challenged player can accept or reject the match.
6. When accepted, RetroFight starts signaling and UDP direct connection checks.
7. When the connection succeeds, RetroFight launches the RetroFight FBNeo runtime.
8. Play the match.
9. When the runtime closes, RetroFight returns to the lobby.

Ranked play is disabled by default during the beta release.

## Connection Status

During match startup, RetroFight can show:

- `Signaling`: players are exchanging match setup information.
- `UDP direct`: RetroFight is checking whether direct UDP play is possible.
- `Connected`: the direct path is ready and the runtime can start.
- `Direct failed`: UDP direct play did not succeed.

If direct UDP fails, this beta cannot automatically recover through a relay.

## Troubleshooting

Server unavailable:

- Check your internet or LAN connection.
- Restart RetroFight.
- Try again after confirming the server is online.

Game file missing:

- Open `Window > Open ROMs Folder`.
- Place the required game ZIP in the folder.
- Return to the catalog and select the game again.

Runtime does not start:

- Install the Microsoft DirectX End-User Runtime legacy package if `d3dx9_43.dll` is missing.
- Check whether antivirus software blocked the runtime.
- Reinstall or re-extract the RetroFight release build.

UDP direct failed:

- Allow RetroFight and RetroFight FBNeo through Windows Firewall.
- Avoid VPNs, hotspots, hotel networks, school networks, office networks, or restrictive routers during beta testing.
- Try again from a home network.
- If the network blocks UDP direct traffic, this beta does not provide a relay fallback.

Challenge flow gets stuck after rejecting:

- Reject the next challenge and try again.
- Note the player names, approximate time, and click sequence before reporting the issue.

## Where To Find Local Data

RetroFight stores per-instance data under:

```txt
%APPDATA%\RetroFight-instances\
```

The ROM folder is available from the app menu through `Window > Open ROMs Folder`.
