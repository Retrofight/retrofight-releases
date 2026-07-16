# RetroFight Production Release Notes

RetroFight production releases provide the current stable desktop build of the RetroFight client for Windows and Linux.

The client provides game lobbies, player challenges, runtime launch, connection status, and return-to-lobby handling after a match closes.

## Available Builds

- `RetroFight-Windows-Setup-x64.exe`: Windows installer.
- `RetroFight-Windows-x64.zip`: portable Windows build.
- `RetroFight-Linux-x86_64.AppImage`: Linux x64 AppImage.
- `RetroFight-Linux-amd64.deb`: Linux x64 Debian package.

Release artifact names are stable across versions, so the latest build is always
available at the same download URLs. The release, tag, and changelog still carry
the version number.

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
the acceptance details are stored in structured Supabase account metadata.
Users can accept or revoke this consent from the website profile page.
Users can remove their account from the website profile page after confirming
the account email in the deletion dialog.

The desktop client shows a first-run legal notice before first use. If the
notice is not accepted, the client exits. Acceptance is stored locally and is
requested again when the bundled notice text changes.

## Networking Notice

RetroFight FBNeo uses GGPO UDP direct play. RetroFight uses its server for lobby, matchmaking, signaling, and candidate exchange; when a direct path is available, real-time match traffic is handled directly between players by GGPO.

If UDP direct play cannot be established between two players, RetroFight can route match traffic through a RetroFight server UDP relay so the match can still start. Direct play still gives the best latency, so the client asks users to allow UDP through their firewall for the best connection.

## ROM Notice

RetroFight does not include, distribute, or download game ROMs. Users must provide only game files they legally own and are authorized to use.

## Recommended Reading

Read the [How to play](https://github.com/Retrofight/retrofight-releases/wiki) guide before starting your first match. RetroFight uses a "Play Now" experience — press Online, keep playing in local training, and the system finds and confirms opponents in the background.

## Out Of Scope

- Automatic fallback to legacy runtimes.
- Spectator mode.
- Advanced lobby chat.
- Competitive anti-cheat.
