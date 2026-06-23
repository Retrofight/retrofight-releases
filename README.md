# RetroFight

RetroFight is a desktop arcade fighting game matchmaking client focused on direct, low-latency 1v1 play on Windows and Linux.

The project combines a desktop client, online lobbies, match challenges, and a custom RetroFight FBNeo runtime built for competitive arcade sessions. The goal is to make classic arcade versus play feel immediate, readable, and reliable, with a simple flow from choosing a game to entering a match.

This repository hosts public RetroFight releases and user-facing release documentation.

## What RetroFight Is

RetroFight is designed for:

- 1v1 arcade fighting game sessions.
- Game-specific online lobbies.
- Player-to-player match challenges.
- A focused beta experience around RetroFight FBNeo.
- Clear status messages when a match cannot connect.

## Beta Status

RetroFight is currently in beta. The beta focuses on stable direct online play, account-based access, clear connection feedback, and clean Windows and Linux release packages.

Players register through `https://retrofight-web.vercel.app` and sign in from the desktop client. Linux packages include an embedded Wine runtime for RetroFight FBNeo, so users do not need to install system Wine for the packaged beta. Features such as rankings, public profiles, spectator mode, advanced chat, and relay fallback for networks where direct play is not possible remain outside the first beta.

## Content Notice

RetroFight does not include, distribute, sell, or provide game ROMs. Users are responsible for using only game files they legally own and are authorized to use.

## Legal Documents

RetroFight's public legal documents are maintained in the [retrofight-legal](https://github.com/Retrofight/retrofight-legal) repository and are an essential part of the project:

- [Legal Notice](https://github.com/Retrofight/retrofight-legal/blob/main/LEGAL_NOTICE.md)
- [Terms of Use](https://github.com/Retrofight/retrofight-legal/blob/main/TERMS_OF_USE.md)
- [First Run Disclaimer](https://github.com/Retrofight/retrofight-legal/blob/main/FIRST_RUN_DISCLAIMER.md)
- [Download Disclaimer](https://github.com/Retrofight/retrofight-legal/blob/main/DOWNLOAD_DISCLAIMER.md)
- [Third-Party Content Notice](https://github.com/Retrofight/retrofight-legal/blob/main/THIRD_PARTY_CONTENT.md)
- [Copyright Policy](https://github.com/Retrofight/retrofight-legal/blob/main/COPYRIGHT_POLICY.md)
- [Legal FAQ](https://github.com/Retrofight/retrofight-legal/blob/main/FAQ_LEGAL.md)

## Release Documentation

- [Beta release notes](release-notes/beta.md)
- [Production release notes](release-notes/prod.md)
- [How to play](https://github.com/Retrofight/retrofight-releases/wiki)
