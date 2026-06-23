# RetroFight Production Release Notes

RetroFight production releases provide the current stable desktop build of the RetroFight client for Windows and Linux.

The client provides game lobbies, player challenges, runtime launch, connection status, and return-to-lobby handling after a match closes.

## Available Builds

- `RetroFight-Windows-Setup-<version>-x64.exe`: Windows installer.
- `RetroFight-Windows-<version>-x64.zip`: portable Windows build.
- `RetroFight-Linux-<version>-x64.AppImage`: Linux x64 AppImage.
- `RetroFight-Linux-<version>-x64.deb`: Linux x64 Debian package.

The client package includes the RetroFight FBNeo base runtime. The active
RetroFight FBNeo executable is provided by the RetroFight service at runtime and
is verified by the client before launch.

Linux builds include an embedded Wine runtime for RetroFight FBNeo. Users do not
need to install system Wine, `wine32`, or `wine64` for the packaged Linux
release. Game ROMs are not included.

## Account Access

RetroFight requires a RetroFight account for online play.

Register or manage your account on the website:

```txt
https://retrofight-web.vercel.app
```

The desktop client supports login only. It does not include in-app
registration. Your website player name is used as your lobby and in-game display
name, and only one active client session is allowed per account.

## Legal Notices

The RetroFight website links the public legal pages from the footer. Package
downloads require accepting the download legal notice before the artifact opens.
Website registration requires accepting the Terms of Use and Privacy Policy, and
the acceptance details are stored with the Supabase account metadata.

The desktop client shows a first-run legal notice before first use. If the
notice is not accepted, the client exits. Acceptance is stored locally and is
requested again when the bundled notice text changes.

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
