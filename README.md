<div align="center">

<img src="assets/icon.png" alt="cache-maxxing icon" width="120" height="120">

# cache-maxxing

**See when Claude's prompt cache expires, and what your next message will cost, before you send it.**

[![version](https://img.shields.io/badge/version-0.5.0-blue)](CHANGELOG.md)
[![license: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-orange)](https://code.claude.com/docs/en/discover-plugins)

</div>

---

## About

> **This is a fork** of [5m0k3/cache-maxxing](https://github.com/5m0k3/cache-maxxing). It adds one thing: **auto-compact**. A big chat whose work is not finished compacts itself shortly before its cache expires (or when your usage limit is nearly reached), so the chat you come back to is small and cheap to continue. Everything else is the original plugin.

Ever feel like your Claude subscription quota disappears way faster than it should? Usually, it's not how **much** you're building - it's **when** you hit Send.

Claude keeps your chat "warm" only for a while so follow-up messages barely dent your limit. But wait too long between messages, and Claude has to re-read your entire chat history at full cost - **up to 80× more quota** than a normal reply.

**cache-maxxing** puts an easy-to-read bar right above your chat box (in the desktop app, VS Code, and terminal) to protect your quota while you build:

- **Know before you hit Send:** See a live countdown timer and whether your next message is cheap (🟢), running out of time (🟡), or about to take a huge bite out of your limit (🔴).
- **Never lose your flow with `/cmaxx-handoff`:** When a long chat gets too expensive to continue, run one command to hand off a smart summary of your progress into a fresh chat—no re-explaining your project, and no massive quota spike.
- **Zero setup headaches, zero slowdown:** Everything is calculated right on your machine using numbers Claude Code already tracks. The bar sends nothing anywhere.

| | |
|---|---|
| <img src="assets/screenshots/desktop_green.png" alt="Green bar: cache active, next message cheap" width="420"> | <img src="assets/screenshots/vscode_yellow.png" alt="Yellow bar: cache ending soon" width="420"> |
| **🟢 Cache active.** Messages are cheap right now. | **🟡 Ending soon.** The cheap moment to reply. |
| <img src="assets/screenshots/desktop_red.png" alt="Red bar: cache expired, with what a handoff saves" width="420"> | <img src="assets/screenshots/cli_orange.png" alt="Orange bar: cache expired, but the rebuild is cheap" width="420"> |
| **🔴 Expired.** What a rebuild costs, and what a handoff saves. | **🟠 Expired.** But the rebuild cost is low. |

## Features

| | |
|---|---|
| ⏱️ **Live countdown** | Minutes left on the cache, ticking every second; it survives restarts, sleep and switching chats. |
| 💲 **Next-message cost** | In dollars (API prices) or as "× more" (better for subscriptions), plus how full the chat is (`290K / 1M`). |
| 🚦 **One clear tip** | 🟢 keep going · 🟡 reply soon · 🟠 expired small chat, fine · 🔴 expired, with what a handoff would save. |
| 🔔 **One pop-up** | A single warning when a big chat's cache expires, and a heads-up if you send into it. Your message is never stopped. |
| 🗜️ **Auto-compact** | Walk away mid-task: 5 minutes before the cache expires, Haiku judges whether the work is finished, and an unfinished chat is compacted in place. A finished chat is left alone. |
| 🔄 **`/cmaxx-handoff`** | Haiku (effort: High) writes a briefing of the chat; a fresh chat picks up from it. The expired chat is never resent to the main model. |
| 📊 **`/cmaxx-stats`** | Money and tokens saved, cache hit rate, rebuilds paid: for this chat and for all chats. |
| ⬆️ **`/cmaxx-update`** | Updates the plugin, or at least its model and price list, so new models are priced on day one. |
| 🔒 **Local by default** | No network. Model calls only from the two commands above, when you run them, and from auto-compact, which you can turn off. |


## Install

Requires a recent Claude Code: the desktop app, the VS Code extension, or the CLI. It's tested on 2.1.325 and later.

**Desktop app**
1. Click **+** next to the chat box, then **Plugins**.
2. **Add marketplace** and enter `5m0k3/cache-maxxing`.
3. **Add plugin** → cache-maxxing.

**Terminal or VS Code** (at the Claude Code prompt)

```
/plugin install cache-maxxing --marketplace 5m0k3/cache-maxxing
```

Answer `y` to add the marketplace and press Enter for the user scope. The bar shows after your next message.

## Usage

| Command | What it does |
|---|---|
| `/cmaxx` | Hide or show the bar in this chat (the **✕** on the bar hides it too). |
| `/cmaxx-handoff` | Write a briefing of this chat with Haiku and start a fresh chat from it. (Doesn't trigger the old chat's model) |
| `/cmaxx-stats` | Show savings, cache hit rate, rebuilds and replies: this chat and all chats. |
| `/cmaxx-update` | Update to the latest version, or refresh the model and price list. |

What each light means, the exact texts and the rules behind them: [How the bar decides](#how-the-bar-decides).

## Settings

In the plugin's settings, under cache-maxxing:

| Setting | Default | What it changes |
|---|---|---|
| Rebuild limit ($) | 0.50 | Above this rebuild cost, the bar suggests a handoff instead of carrying on. |
| Warning time (minutes) | 15 | How early the bar turns 🟡 before the 1-hour cache expires. |
| Long chat size (tokens) | 300000 | When the 🟢 bar starts suggesting a handoff once you're done. |
| Show cost as | dollars | `times` shows "40× more" instead of dollars, which suits subscription plans. |
| Memory time | auto | `auto` is 1 hour (every Claude subscription chat). Choose `5` with an API key. |
| Auto-compact | on | `off` never compacts a chat by itself. |
| Auto-compact: minutes left | 5 | How long before the cache expires an unfinished chat is compacted. |
| Auto-compact: usage limit (%) | 93 | At this much of your 5-hour or weekly limit, unfinished chats are compacted at once. |

## Keep it updated

Plugins from GitHub don't update by themselves: Claude Code auto-updates only Anthropic's own marketplaces by default.

- **Once:** in a terminal session, `/plugin` → **Marketplaces** → cache-maxxing → **Enable auto-update**.
- **Any time:** run `/cmaxx-update`. If a newer version is out, it updates the plugin (in the terminal) or tells you how (desktop app, VS Code). Either way it refreshes the model and price list, so a newly released model is priced at once.
- A chat on a newly launched model ends with `new model? try /cmaxx-update`.

## How the bar decides

From top to bottom, the first rule that fits:

1. No reply yet in this chat: ⚪️ "cache-maxxing will start after you send a message".
2. Cache expired or reset, and the rebuild costs more than the rebuild limit: 🔴, suggesting `/cmaxx-handoff` with what it would save.
3. Cache expired or reset, rebuild below the limit: 🟠 small chat, fine to keep going.
4. Under 15 minutes left on the 1-hour cache: 🟡 reply soon.
5. Active, but the chat is past the long-chat size: 🟢 with a handoff suggestion for when you're done.
6. Otherwise: 🟢 keep going.

```
🟢 47:12 min left · next message ≈ $0.01 · 50K / 1M · ✅ Keep going
🟡 12:30 min left · next message ≈ $0.05, later ≈ $1.82 (40× more) · 228K / 1M · ⏳ Reply soon or try /cmaxx-handoff
🔴 cache expired · next message ≈ $2.32 (40× more) · 290K / 1M · 🔄 Try /cmaxx-handoff to save 232K tokens | $2.21
```

Costs are estimates at standard API list prices, and a floor: a reply that uses tools sends several requests. On a subscription you don't pay these dollars; read them as how much of your usage a message takes. "Active" means very likely active: in our measurements about 2 in 100 messages missed the cache even within the time limit.

## Auto-compact

Once a second, for each chat open in a running app, the plugin checks whether all of this holds:

1. Auto-compact is on, and the chat is interactive (never `claude -p` or the SDK).
2. The chat is on the 1-hour cache, the cache is still active with at least a minute left, and it was not reset.
3. A rebuild would cost more than the rebuild limit: a small chat is cheap to rebuild anyway.
4. Either the cache has 5 minutes or less left, **or** your 5-hour or weekly limit is at 93% or more (a lock-out lasts longer than the cache). A window already at 100% is skipped: it could not pay for the compaction.

Then Haiku reads the chat's last 30 messages and answers one word. **Unfinished** (next steps outstanding, a failing check, or Claude waiting for your answer): the chat is compacted with the engine's own `/compact`, told to keep the open work. **Finished** (the request answered, the pull request opened, nothing pending), or no clear answer: nothing happens. A reply is judged once.

Afterwards the chat holds a notice: `cache-maxxing compacted this chat on its own (290K → 31K): its cache was about to expire and the work looked unfinished.`

What it costs: Haiku's judgment is a fraction of a cent. The compaction is done by the chat's own model and reads the still-active cache, so it uses about as much of your limit as one normal message.

## Privacy

- **The bar itself sends nothing anywhere.** Everything it shows is worked out on your computer.
- **Auto-compact calls models by itself, unless you turn it off.** It sends Claude Haiku an abbreviated copy of the chat's last 30 messages (long ones cut, tool output left out) to judge whether the work is finished. If it is not, the chat is compacted exactly as `/compact` would: the chat's own model summarizes it.
- **Two commands reach outside, when you run them:**
  - `/cmaxx-handoff` sends Claude Haiku an abbreviated copy of the chat (the last 120 messages, long ones cut, tool output left out). The briefing is saved in your project under `.claude/handoffs/`, which gets its own `.gitignore`, so briefings are never committed.
  - `/cmaxx-update` downloads this plugin's version number and price list from this repository and sends nothing. GitHub sees your IP address, as for any download.
- **Stored on your computer:** a small record per chat (time of the last reply, chat size, model) for 7 days, and counts for `/cmaxx-stats` (numbers only, each chat's for 90 days, the totals for good). **Never the chat's text!**

## Limitations

- Auto-compact works only for chats open in a running app: a closed chat has no process to run it, and nothing runs while the computer sleeps. A chat that wakes up with an expired cache is left as it is.
- At the usage-limit trigger, every open unfinished chat compacts at about the same time. Each costs about one normal message, and together they can use up what is left of the window.
- "Finished" is Haiku's judgment of the last 30 messages. It can be wrong both ways: a finished chat compacted, or an unfinished one left to expire.
- If your organization blocks Haiku, `/cmaxx-handoff` says no handoff was written; the chat is untouched.
- The handoff's saving is an estimate: Haiku's share and a fresh chat's start are taken from measured medians, priced as Claude Haiku 5.5.
- Switching model and back within the hour shows "cache reset", although the first model's cache may still be warm.
- `/cmaxx-stats` reads an older chat's history from its transcript only up to 4 MiB; a bigger chat that started before you installed the plugin is counted from when the plugin first saw it.
- A brand-new chat in the desktop app shows the bar only after your first message: the app starts the chat, and its plugins, at that point.

## FAQ

**Does it cost anything to run?** The bar, no. `/cmaxx-handoff` makes one small Haiku call, typically about a cent at API prices.

**I'm on a subscription. Are the dollar amounts real?** They're API list prices, a way to compare. Set **Show cost as** to `times` to see "40× more" instead.

## Contributing

Issues and pull requests are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for setup, tests and the project rules, and [SECURITY.md](SECURITY.md) to report a vulnerability privately. Changes are listed in [CHANGELOG.md](CHANGELOG.md).

## License

[MIT](LICENSE) © 5m0k3
