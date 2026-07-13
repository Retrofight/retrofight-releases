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

## UDP Relay Fallback

When GGPO UDP direct play fails between two players, the server can now automatically allocate a UDP relay and assign each player a dedicated relay port. GGPO uses the relay endpoint transparently — no runtime changes are required.

The relay server forwards packets cross-port so that each player's GGPO instance sees source addresses matching its configured remote peer, keeping the relay fully transparent to GGPO's source-address filter.

**Infrastructure requirement:** the relay requires a dedicated host with an open UDP port range (default 40000–49999). The relay is off by default in the current deployment. The server operator enables it by setting:

```
RETROFIGHT_RELAY_ENABLED=1
RETROFIGHT_RELAY_PUBLIC_IP=<server public IP>
RETROFIGHT_RELAY_PORT_START=40000    # optional, default 40000
RETROFIGHT_RELAY_PORT_COUNT=1000     # optional, default 1000
```

Until a dedicated VPS with an open UDP port range is provisioned, the relay remains inactive. When inactive, the existing "UDP direct failed" message is shown and no match starts.

## Known Issue

Restrictive NAT or firewall setups can still prevent GGPO UDP direct play. The UDP relay fallback is implemented but requires a dedicated VPS with an open UDP port range; it is not yet active in the current deployment.

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

RetroFight FBNeo uses GGPO UDP direct play as the primary path. A UDP relay fallback is implemented server-side and is transparent to GGPO (no runtime changes required). The relay is not yet active in the current deployment — a dedicated VPS with an open UDP port range is needed. Until then, if UDP direct play fails, the match cannot start automatically.

The older TCP relay work developed for a legacy RetroArch path does not solve GGPO UDP connectivity and remains out of scope.

## ROM Notice

RetroFight does not include, distribute, or download game ROMs. Users must provide only game files they legally own and are authorized to use.

## Public Profiles and Match History

Player profiles now include optional public profile fields and a persistent match history.

**Profile settings** (available from the website profile page):

- **Public profile toggle** — when enabled, your profile and match history are visible at `/players/{your-player-name}`.
- **Avatar URL** — optional link to a profile image.
- **Country code** — optional two-letter ISO country code (e.g. IT, US).

**Match history** is recorded server-side for every confirmed, disputed, or forfeit match. It stores:

- Player IDs and display names at the time of the match
- Game driver name
- Winner, scores, and match status
- Runtime version, protocol version, and audit ID
- Timestamp

The match history is visible on your own profile page after login. Confirmed matches between two players with public profiles are also visible on the public player profile page.

**Account deletion** now anonymizes match history names before removing the account. Match records remain for statistical purposes but can no longer be linked to the deleted account.

**Public player profiles** are accessible at `/players/{player-name}`. The page shows the player's optional avatar, country, member-since date, win/match counts, and match history.

**Database migration** — apply `supabase/migrations/20260626000000_milestone5.sql` to the RetroFight Supabase project to add the `avatar_url`, `country`, `is_public`, `created_at`, and `updated_at` columns to the `profiles` table and create the `match_history` table.

**Server configuration** — set `RETROFIGHT_SUPABASE_URL` and `RETROFIGHT_SUPABASE_SERVICE_ROLE_KEY` (or `SUPABASE_URL` / `SUPABASE_SERVICE_ROLE_KEY`) on the server to enable match history persistence. Set `RETROFIGHT_MATCH_HISTORY_ENABLED=0` to disable. When not configured, the server continues to operate without match persistence.

## Competitive Ranking (Milestone 6)

RetroFight now includes a full competitive ranking system based on Glicko-2.

### Challenge types

Every challenge now specifies a **match type** and a **set format**:

- **Casual** — no rating impact; result is recorded as `played`.
- **Ranked FT1 / FT2 / FT3 / FT5** — first to N games wins; result updates Glicko-2 ratings.

The challenge UI sends `matchType` and `ftN` to the server. The server includes them in the `match_ready` event and records them in `match_history`.

### Glicko-2 hidden rating

Each player maintains a separate hidden Glicko-2 rating per game. The rating is not shown anywhere on the website — only the visible rank badge is exposed there. The desktop client's in-match HUD now shows a rounded display rating next to the rank badge (see "In-Match Side Panel HUD" below); the underlying rating deviation and volatility used by the Glicko-2 algorithm are never exposed.

**Rank thresholds (all-time):**

| Badge | Rank | Rating threshold |
|-------|------|-----------------|
| NR | 0 | No completed ranked match |
| E  | 1 | rating < 1300 |
| D  | 2 | 1300 – 1499 |
| C  | 3 | 1500 – 1699 |
| B  | 4 | 1700 – 1899 |
| A  | 5 | 1900 – 2099 |
| S  | 6 | ≥ 2100 |

New players start with rating 1500 and high RD (uncertainty). After calibration matches their rating settles and they receive a visible rank.

**Anti-boosting:** if two players play more than 2 ranked matches against each other within 24 hours, the rating impact is progressively reduced (50 % after 3–4 matches, 25 % after 5+).

### In-Match Side Panel HUD

The in-match overlay has moved from a single centered top bar to a translucent side panel docked to each edge of the video — player 1 on the left, player 2 on the right, mirrored. Each panel shows, generically across every supported game:

- **Player name**.
- **Rank badge** (`rank0.png`–`rank6.png`) — fetched from Supabase before the match starts, defaulting to NR (0) for new players or when the server cannot reach Supabase.
- **Display rating** — a rounded Glicko-2 rating shown next to the badge; hidden when no rating data is available yet.
- **Country flag** — read from the player's public profile country code (see "Public Profiles and Match History" above); hidden when the profile has no country set.
- **FT round-win pips** — replaces the old plain "score:score" text with filled/empty dots, shown once the game's per-title detector starts tracking round wins (same condition that already gated the score display).

The VS / FTn label stays centered at the top of the screen as before. The side panels are always-on translucent overlays on the video edge in this release; they do not yet detect or use pillarbox/letterbox margins when present (tracked as a future enhancement).

### Leaderboard

Per-game leaderboards are available at `/leaderboard` on the website. Each game shows the top 50 players by rank and rating. Only players with at least one completed ranked match and a public profile appear on the leaderboard.

### Audit trail and disputes

All ranked match results — confirmed, disputed, and forfeit — are stored in `match_history` with a full audit trail. Disputed results are excluded from the public leaderboard but are visible to administrators via the Supabase service role.

`ranking_history` records each player's Glicko-2 rating and rank before and after every ranked match, enabling full replay of the rating trajectory.

### Database migration

Apply `supabase/migrations/20260626000002_milestone6.sql` to the RetroFight Supabase project. This migration creates:

- `ranking_seasons` — named rating periods; active season is enforced unique.
- `player_game_ratings` — one row per (player, game, season); stores Glicko-2 values and visible rank.
- `ranking_history` — audit trail of every rating change.
- Adds `match_type` (casual/ranked) and `ft_n` (1–5) columns to `match_history`.

### Server configuration

Set `RETROFIGHT_RANKING_ENABLED=0` to disable ranking updates (default: enabled). Ranking requires `RETROFIGHT_SUPABASE_URL` and `RETROFIGHT_SUPABASE_SERVICE_ROLE_KEY` to be set. When not configured, the server continues without ranking persistence.

## Competitive Matchmaking (Milestone 7)

RetroFight now includes an opt-in automatic matchmaking queue, additive to the existing manual lobby list.

### Find Match

A new **Find Match** button appears in the lobby. Joining the queue requires the same idle precondition as sending a manual challenge. While searching, the panel shows a **Searching for opponent…** status with a Cancel button; the existing player list and manual **Challenge** buttons remain fully available as a fallback at all times.

### Fixed match format

Find Match does not ask the searcher to choose a set format. Ranked-eligible games always search **ranked FT3**; every other game searches **casual**. This keeps a game's population in a single queue instead of splitting it across every possible format combination. Manual challenges (the **Challenge** button on a specific player) are unaffected and still let two players agree on any format — VS, FT2, FT3, FT5, or FT10.

### Pairing signal

The server periodically pairs the two best-matching queued players for the same game and match format, scoring candidates by:

- **Rank proximity** — closer visible ranks (0–6) score higher.
- **Region** — a broad continent-level bucket resolved from the connecting IP address server-side (never exposed to any client), used only as a soft preference.
- **Connection quality** — a rolling per-player direct-punch success rate (always available) and, for users with telemetry enabled, an average probe latency sample.
- **Recent pairing** — opponents paired together recently are deprioritized, without ever being excluded outright.

None of these signals ever block a match: when only two players are queued for a game/format, they are paired immediately. Longer wait times also progressively raise a player's priority, so pairing never stalls even in a large queue with an unfavorable score.

### Confirmation flow

A matchmaking pairing is a **suggestion**, not an automatic match: both players still see an Accept/Decline banner, exactly like a manual challenge. If either player declines or does not respond in time, the other player is automatically returned to the search queue with a **"searching for a new match"** notice instead of being left waiting — only the player who actively declined (or never responded) returns to idle.

### Server configuration

`RETROFIGHT_MATCHMAKING_ENABLED` (default: enabled), `RETROFIGHT_MATCHMAKING_TICK_MS` (default 2000), `RETROFIGHT_MATCHMAKING_SUGGESTION_TIMEOUT_MS` (default 15000), `RETROFIGHT_NETWORK_STATS_ENABLED` (default: enabled when Supabase is configured; shares credentials with ranking/match history).

### Database migration

Apply `supabase/migrations/20260701000000_milestone7.sql` to the RetroFight Supabase project. This migration creates `player_network_stats`, a per-player per-game connection-quality table used only as an internal matchmaking scoring signal (not shown on any leaderboard or profile).

## Online Player Counts

The game selection screen now shows how many players are currently online for each title, so you know whether to expect a quick match before entering that game's lobby. Counts are polled periodically and require an active login.

## Play Now (Redesigned Matchmaking Experience)

RetroFight moves from a "browse who's online and challenge them" model to a modern fighting-game flow: you just press **Play** and start fighting. The system finds the best available opponent in the background while you play.

### One button

After picking a game you choose **Casual** or **Ranked**, then press **Online**. Matchmaking starts searching in the background right away — the app stays on screen showing **Searching for opponent…** with two buttons: **Training** and **Stop**.

### Play while you search

Waiting is optional. Press **Training** (or the **A** button) to drop into local single-player/free play for the same game while the search keeps running in the background. Press **Stop** (or **B**) to leave the queue.

### Match Found

When an opponent is found, the app comes to the front with a **Match Found** banner. If you were in Training, the training session closes and the app takes over the screen. The banner shows **both players side by side** with their assigned positions (**1P** vs **2P**, with **YOU** highlighted on your side), each with name, country flag, rank badge, rating, and an avatar slot, plus the estimated ping and connection quality between you. A clear **RANKED / VERSUS** badge shows the stakes at a glance.

Press **Accept** (**A**) to start the match, or **Decline** (**B**): the banner disappears and the search continues automatically. If you were training, a fresh training session starts again in the background. The same auto-continue happens if neither player accepts in time.

### Ranked is mutual opt-in

A single shared queue per game keeps even small populations matching quickly. A match counts as **Ranked (FT3)** only when both players choose Ranked; otherwise it is **Casual**. Your Casual/Ranked preference is remembered between sessions.

### Results and what's next

When a ranked match is decided the runtime shows a **Results** screen (Victory/Defeat, the You-vs-Opponent score, and the mode) instead of cutting to black. You then land on a **post-match choice**: **Rematch**, **New Search**, or **Exit** — nothing happens automatically, so you are never instantly thrown into another game.

- **Rematch** replays the same pair immediately (mutual opt-in: it starts only if both choose it within the window, inheriting the same mode).
- **New Search** resumes matchmaking for a fresh opponent.
- **Exit** ends the session and returns to the catalog.

To keep things fair on small populations, two players who just finished a match together are **not instantly re-paired** by matchmaking for a short cooldown — if you want to play them again right away, use **Rematch**.

The previous manual player list and direct challenge remain in the codebase and will return as a separate **Friends** mode.

### Controller-first controls (kiosk)

Because a RetroFight cabinet often has no keyboard, the whole flow is
controller-friendly. The **Match Found** banner is shown in the app and is
navigated with the controller: **A** accepts, **B** declines. While searching,
**A** starts Training and **B** stops the search.

Two always-on holds work while a game is running (honoring each game's button
mapping):

- Hold **A + B + Start** for 2 seconds to **quit** the running game (same clean
  exit as Alt+F4).
- Hold **A + B + Select** for 2 seconds to open **Map Game Inputs** (same as F5).

The keyboard shortcuts (Alt+F4, F5) keep working unchanged for setups that have a
keyboard. The two-second deliberate hold prevents accidental triggers during
normal play.

## Out Of Scope For This Beta

- UDP relay active deployment (infrastructure not yet provisioned).
- Automatic fallback to legacy runtimes.
- Ranking seasons activation (table exists; no active season created yet).
- Spectator mode.
- Advanced lobby chat.
- Competitive anti-cheat.
- Full multi-game catalog features such as favorites, filters, and extended metadata.
