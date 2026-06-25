# RetroFight Beta Release Notes

RetroFight beta introduces online 1v1 arcade fighting game sessions through the custom RetroFight FBNeo runtime. The client provides game lobbies, challenge approval, runtime launch, connection status, and match cleanup after the game closes.

The beta networking path uses GGPO UDP direct play. RetroFight uses its server for lobby, matchmaking, signaling, and candidate exchange; the real-time match traffic is handled directly between players by GGPO.

## Available Builds

- `RetroFight-Windows-Setup-0.6.5-beta.0-x64.exe`: Windows installer.
- `RetroFight-Windows-0.6.5-beta.0-x64.zip`: portable Windows build.
- `RetroFight-Linux-0.6.5-beta.0-x64.AppImage`: Linux x64 AppImage.
- `RetroFight-Linux-0.6.5-beta.0-x64.deb`: Linux x64 Debian package.

The client package includes the RetroFight FBNeo base runtime. The active
RetroFight FBNeo executable is provided by the RetroFight service at runtime and
is verified by the client before launch.

Linux builds include an embedded Wine runtime for RetroFight FBNeo. Users do not
need to install system Wine, `wine32`, or `wine64` for the beta package. Game
ROMs are not included.

## Account Access

RetroFight beta now requires a RetroFight account.

Register or manage your account on the website:

```txt
https://retrofight-web.vercel.app
```

The desktop client supports login only. It does not include in-app
registration. Your website player name is used as your lobby and in-game display
name, and only one active client session is allowed per account.

## Legal Notices

The RetroFight website now links public legal pages in the footer, including the
Legal Notice, Terms of Use, Privacy Policy, Third-Party Content Notice,
Copyright Policy, and Legal FAQ.

Website registration requires accepting the Terms of Use and Privacy Policy.
The acceptance timestamp, locale, statement, and document versions are stored in
the structured `privacy_consent` Supabase account metadata.
The website profile page lets users accept or revoke this consent, updating
`privacy_consent.accepted` on the account metadata.

Users can request account removal from the website profile page. The flow shows
a deletion disclaimer and requires typing the account email before the account
is removed.

Every website package download now shows an important legal notice first. The
download starts only after selecting `I UNDERSTAND AND AGREE`.

The desktop client shows the first-run legal notice before first use. If the
notice is not accepted, the client exits. Acceptance is saved locally and is
requested again only when the bundled notice text changes.

## Diagnostic Telemetry

The desktop client now supports opt-in diagnostic telemetry for network observability.

Telemetry is **disabled by default**. It can be enabled or disabled at any time from the Quick Actions menu (client switch key → Diagnostics).

When enabled, the client sends the following to the RetroFight server after each match attempt:

- UDP candidate type chosen (LAN, STUN, or public)
- STUN discovery outcome
- Punch result and reason (direct, timeout, or failed)
- Probe round-trip latency in milliseconds
- Client and runtime version
- Game driver name (no ROM files, no game content)
- Runtime crash exit code when applicable

No ROM files, game inputs, display names, IP addresses, or personally identifiable data are collected. The server stores events in a local JSONL file. Telemetry consent is stored locally in the client data directory and can be revoked at any time.

## Network Probe Overlay

The client sidebar now shows a **Probe** section after each UDP direct attempt, displaying:

- Candidate type selected (LAN, STUN, or public)
- Round-trip latency measured from probe send to first response
- STUN discovery outcome

This section is visible after a match attempt concludes and is cleared when the client returns to the lobby. Jitter and packet loss metrics require GGPO protocol changes and are not available in this release.

## Validated So Far

- Windows installer starts and installs correctly.
- Portable ZIP starts correctly.
- Linux AppImage and Debian packages are produced by the release pipeline with
  embedded Wine runtime resources.
- Supabase account login through the desktop client.
- Website player name shown in game lobbies and passed to RetroFight FBNeo.
- Server-side rejection of simultaneous client sessions for the same account.
- Website legal pages, Privacy Policy, registration consent, and download
  disclaimer consent flow.
- Website account deletion flow with email confirmation.
- Desktop client first-run legal consent gate.
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

Restrictive NAT or firewall setups can still prevent GGPO UDP direct play. This beta does not include a GGPO-compatible UDP relay fallback.

## Troubleshooting

If login fails, register or check your account at
`https://retrofight-web.vercel.app`, confirm that your email and password are
correct, and close any other RetroFight client already connected with the same
account.

The current RetroFight FBNeo beta runtime is packaged to start on clean Windows
installs without requiring users to install legacy DirectX 9 D3DX or Microsoft
Visual C++ Redistributable packages first. The x86 runtime statically links the
MSVC runtime and no longer imports `d3dx9_43.dll`, `MSVCP140.dll`, or
`VCRUNTIME140.dll`.

On Linux, use the AppImage or Debian package for x64 systems. The beta package
ships with an embedded Wine runtime for launching RetroFight FBNeo; installing a
separate system Wine package is not required for normal beta use.

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
- Rankings, Elo, Glicko, public profiles, or public match history beyond the current account login.
- Spectator mode.
- Advanced lobby chat.
- Competitive anti-cheat.
- Full multi-game catalog features such as favorites, filters, and extended metadata.
