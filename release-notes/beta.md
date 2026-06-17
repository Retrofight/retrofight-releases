# RetroFight Beta Release Notes

RetroFight beta introduces online 1v1 arcade fighting game sessions through the custom RetroFight FBNeo runtime. The client provides game lobbies, challenge approval, runtime launch, connection status, and match cleanup after the game closes.

The beta networking path uses GGPO UDP direct play. RetroFight uses its server for lobby, matchmaking, signaling, and candidate exchange; the real-time match traffic is handled directly between players by GGPO.

## Available Builds

- `RetroFight-win-x64.exe`: Windows installer.
- `RetroFight-win-x64.zip`: portable Windows build.

The RetroFight FBNeo runtime is included with the client package. Game ROMs are not included.

## Validated So Far

- Windows installer starts and installs correctly.
- Portable ZIP starts correctly.
- Same-machine testing with two Electron instances.
- Same-LAN testing with two different machines.
- Long match testing over 20 minutes.
- More than 10 separate matches during beta validation.
- Forced runtime crash testing with stable return to the lobby.
- Clean Windows VM startup without Microsoft Visual C++ Redistributable
  2015-2022 x86 and without legacy DirectX 9 D3DX installed.
- DEV and PROD GitHub release pipelines on the release branch.

## Still To Validate

- WAN matches across two real home NAT connections.
- Restrictive NAT or firewall cases where UDP direct fails.
- Final competitive policy for disputes, forfeits, and penalties.

## Known Issue

After rejecting a challenge, the next challenge can sometimes fail to accept on the first attempt. Rejecting once more usually clears the flow and allows the next challenge to be accepted. Current evidence points to frontend challenge event handling rather than a server-side failure.

## Troubleshooting

The current RetroFight FBNeo beta runtime is packaged to start on clean Windows
installs without requiring users to install legacy DirectX 9 D3DX or Microsoft
Visual C++ Redistributable packages first. The x86 runtime statically links the
MSVC runtime and no longer imports `d3dx9_43.dll`, `MSVCP140.dll`, or
`VCRUNTIME140.dll`.

If RetroFight FBNeo closes immediately with exit code `0xC0000135`, Windows
could not find a required DLL. Check the diagnostics folder from `Window > Open
Diagnostics Folder` and confirm that the runtime folder still contains
`ggponet.dll`.

Legacy DirectX 9 and Visual C++ Redistributable are not bundled and are not
required for the current dependency-clean runtime during normal play. If
diagnostics, older runtime builds, or local experiments still report missing
legacy components, install them from Microsoft:

- DirectX 9 D3DX: download and run the official Microsoft DirectX End-User
  Runtime Web Installer:

```txt
https://www.microsoft.com/en-us/download/details.aspx?id=35
```

- Visual C++ x86 runtime: install Microsoft Visual C++ Redistributable
  2015-2022 x86 from Microsoft:

```txt
https://aka.ms/vs/17/release/vc_redist.x86.exe
```

Install the x86 package even on 64-bit Windows when testing old x86 runtime
builds.

## Networking Notice

RetroFight FBNeo beta does not include a UDP relay or TURN-like fallback for GGPO. If UDP direct play fails between two players, the match cannot automatically fall back to a relay in this beta.

The older TCP relay work developed for a legacy RetroArch path does not automatically solve GGPO UDP connectivity and is not part of the first beta release path.

## ROM Notice

RetroFight does not include, distribute, or download game ROMs. Users must provide only game files they legally own and are authorized to use.

## Out Of Scope For This Beta

- UDP relay or TURN-like fallback for GGPO.
- Automatic fallback to legacy runtimes.
- Persistent accounts.
- Rankings, Elo, or Glicko.
- Spectator mode.
- Advanced lobby chat.
- Competitive anti-cheat.
- Full multi-game catalog features such as favorites, filters, and extended metadata.
