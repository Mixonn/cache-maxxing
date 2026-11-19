# Changelog

All notable changes to this project are documented here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project uses [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [0.7.2] - fork: auto-compact and stats in the desktop app

### Fixed
- Auto-compact never ran in the desktop app, and `/cmaxx-stats` never counted a reply there. The app drives Claude Code the way the SDK does: `session.start` says the session is not interactive and names no surface, and the app's window attaches right after. The plugin took that first word for `claude -p` and left every desktop chat alone. A session now counts as a chat when the engine says it is interactive or once any surface is attached; `claude -p` and a bare SDK host, with nothing attached, are still left alone.

## [0.7.1] - fork: updates from the fork

### Changed
- `/cmaxx-update` checks this fork's version (`Mixonn/cache-maxxing`), so it offers the fork's releases instead of saying an older upstream version is the latest. The price list is still downloaded from the original repository, where it is maintained.
- The README's install steps point at the fork.

## [0.7.0] - fork: handoff button

### Added
- A **🔄 Handoff** button on the 🔴 bar: one click does what `/cmaxx-handoff` does. In the desktop app the line for the new chat is put on the clipboard (start a new chat and paste); in the terminal and VS Code the chat clears to a fresh one with the line in its box. The button reads **⏳ Writing…** while Haiku writes the briefing.

### Changed
- `/cmaxx-handoff` and the button share one handoff, and only one runs at a time.

## [0.6.0] - fork: auto-compact

### Added
- **Auto-compact.** A big chat whose work is not finished is compacted in place while its cache is still active: 5 minutes before the cache expires, or at once when the 5-hour or weekly usage limit reaches 93%. Haiku judges "finished" from the last 30 messages, once per reply; a finished chat, or one Haiku gives no clear answer for, is left alone.
- A notice row and a pop-up after each auto-compact, and an `Auto-compacts` row in `/cmaxx-stats`.
- Settings: **Auto-compact** (on/off), **Auto-compact: minutes left** (5), **Auto-compact: usage limit (%)** (93).
- The newest rate-limit reading is shared between open chats, so an idle chat sees the limit another chat just reached.

### Changed
- The plugin now calls models by itself (Haiku for the judgment, the chat's own model for the compaction) unless Auto-compact is off. The README's Privacy section says what is sent.

## [0.5.0] - 2026-11-19

Thanks to @giulioc84 (#1–#7), @xyaso66-del (#9), @lysol321 (#10) and @jstockall (#8) for the careful reports and the fix.

### Changed
- The 🟢 countdown shows whole minutes (`48 min left`), rounded up, so it no longer ticks every second; from 🟡 on it still shows minutes and seconds.
- Memory time `auto` follows each reply's own cache write: 1 hour on a subscription, 5 minutes with an API key, a cloud provider or usage credits (#4).
- The 🔴 tip's handoff saving uses the size of your own fresh chats (median first cache write of your last 20), not a fixed 13K (#3).

### Fixed
- `/cmaxx-handoff` failed on Windows: the skill file is read with CRLF line endings too, and `.gitattributes` keeps LF on checkout (#9, #10).
- `/cmaxx-handoff` accepts Haiku's brief only if it has the last request and the next steps, and the briefing always keeps your last message word for word (#1).
- `/cmaxx-handoff` kept messages that start with an image or quote a handoff path; a commit seen only in the command is marked unverified (#2).
- A second handoff of the same expired chat no longer adds a second saving to `/cmaxx-stats` (#3).
- On resume, the countdown no longer starts at the end of the last reply instead of its start (#4).
- A compact reported as 0 tokens no longer shows a free rebuild; `/cmaxx-stats` counts a rebuild when a reply reads far less than the chat while the cache should be warm (#5).
- `/cmaxx-stats`: a failed write keeps the turn's counts; a new chat is never re-read from its transcript later; a "<$0.01" handoff saving keeps its tokens; Haiku's price tier counts uncached input (#6).
- `/cmaxx-update`: an older price download never replaces newer saved prices; versions with a pre-release or build tag compare correctly (#6).
- `/cmaxx-update`: offline now says "GitHub not reachable (offline?)" instead of a raw error, and it no longer says "Updated" when Claude Code's marketplace copy was behind.
- `/cmaxx-handoff` finds the transcript under `CLAUDE_CONFIG_DIR` when that is set (#8, by @jstockall).
- README: a blocked Haiku gives a facts-only briefing, not "no handoff".

## [0.4.0] - first public release

### Added
- **The bar** above the chat box in the Claude desktop app, VS Code and the terminal: a live cache countdown, the next message's cost (dollars or "× more"), how full the chat is, and one tip (🟢 keep going, 🟡 reply soon, 🟠 small chat fine, 🔴 expired with what a handoff saves).
- A single pop-up when a big chat's cache expires, and a heads-up when you send into it. Messages are never blocked.
- Instant updates on model switch (`/model`) and compaction (`/compact`). Older chats are filled in on resume; restarts and sleep are handled.
- **`/cmaxx-handoff`:** Haiku writes a briefing from the local transcript, saved under `.claude/handoffs/` (git-ignored); a fresh chat starts from it.
- **`/cmaxx-stats`:** savings, cache hit rate, rebuilds paid and replies, for this chat and all chats.
- **`/cmaxx-update`:** updates the plugin where possible, and refreshes the model and price list.
- **`/cmaxx`:** shows or hides the bar.
- Settings: rebuild limit, warning time, long-chat size, cost style, cache time.

[Unreleased]: https://github.com/5m0k3/cache-maxxing/compare/v0.5.0...HEAD
[0.5.0]: https://github.com/5m0k3/cache-maxxing/compare/v0.4.0...v0.5.0
[0.4.0]: https://github.com/5m0k3/cache-maxxing/releases/tag/v0.4.0
