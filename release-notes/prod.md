# RetroFight Production Release Notes

RetroFight production releases provide the current stable Windows build of the RetroFight client and the custom RetroFight FBNeo runtime.

The client provides game lobbies, player challenges, runtime launch, connection status, and return-to-lobby handling after a match closes.

## Available Builds

- `RetroFight-win-x64.exe`: Windows installer.
- `RetroFight-win-x64.zip`: portable Windows build.

The RetroFight FBNeo runtime is included with the client package. Game ROMs are not included.

## Account Access

RetroFight requires a RetroFight account for online play.

Register or manage your account on the website:

```txt
https://retrofight-web.vercel.app
```

The Windows client supports login only. It does not include in-app
registration. Your website player name is used as your lobby and in-game display
name, and only one active client session is allowed per account.

## Networking Notice

RetroFight FBNeo uses GGPO UDP direct play. RetroFight uses its server for lobby, matchmaking, signaling, and candidate exchange; real-time match traffic is handled directly between players by GGPO.

RetroFight does not currently include a UDP relay or TURN-like fallback for GGPO. If UDP direct play fails between two players, the match cannot automatically fall back to a relay.

## ROM Notice

RetroFight does not include, distribute, or download game ROMs. Users must provide only game files they legally own and are authorized to use.

## Recommended Reading

Read the [How to play](https://github.com/Retrofight/retrofight-releases/wiki) guide before starting your first match.

## Out Of Scope

- UDP relay or TURN-like fallback for GGPO.
- Automatic fallback to legacy runtimes.
- Rankings, Elo, Glicko, public profiles, or public match history unless explicitly announced in a future release.
- Spectator mode.
- Advanced lobby chat.
- Competitive anti-cheat.
