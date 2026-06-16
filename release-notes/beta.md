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
- DEV and PROD GitHub release pipelines on the release branch.

## Still To Validate

- WAN matches across two real home NAT connections.
- Restrictive NAT or firewall cases where UDP direct fails.
- Final competitive policy for disputes, forfeits, and penalties.

## Known Issue

After rejecting a challenge, the next challenge can sometimes fail to accept on the first attempt. Rejecting once more usually clears the flow and allows the next challenge to be accepted. Current evidence points to frontend challenge event handling rather than a server-side failure.

## Troubleshooting

If RetroFight FBNeo does not start and RetroFight reports a missing DirectX file such as `d3dx9_43.dll`, the legacy DirectX 9 runtime is missing.

Use the Electron menu item `Window > Install DirectX9 FBNeo` to launch the bundled DirectX setup. You can also open the installed RetroFight folder and run:

```txt
resources\dxredist\DXSETUP.exe
```

The `resources\dxredist\` folder contains the bundled DirectX 9 runtime packages required by RetroFight FBNeo.

If RetroFight FBNeo still closes immediately after DirectX 9 is installed, install the Microsoft Visual C++ Redistributable 2015-2022 x86 runtime. RetroFight FBNeo is currently an x86 executable and requires `MSVCP140.dll` and `VCRUNTIME140.dll`. A crash exit code of `0xC0000135` usually means Windows could not find one of the required DLLs.

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
