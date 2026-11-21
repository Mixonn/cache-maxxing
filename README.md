<div align="center">

<img src="assets/icon.png" alt="cache-maxxing icon" width="120" height="120">

# cache-maxxing

**See when Claude's prompt cache expires, and what your next message will cost, before you send it.**

[![version](https://img.shields.io/badge/version-0.7.2-blue)](CHANGELOG.md)
[![license: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-orange)](https://code.claude.com/docs/en/discover-plugins)

</div>

---

## About

> **This is a fork** of [5m0k3/cache-maxxing](https://github.com/5m0k3/cache-maxxing). It adds four things: **auto-compact** (a big chat whose work is not finished compacts itself shortly before its cache expires, or when your usage limit is nearly reached, so the chat you come back to is small and cheap to continue), a **🔄 Handoff** button on the 🔴 bar, **🔥 keepwarm** (one click keeps a chat's cache warm while you are away, and stops by itself when the work is done or your usage limit is near) and a **question before an expensive message** (Send anyway or Don't send, with the price). Keepwarm and the question are ported from [karanb192/cache-tax](https://github.com/karanb192/cache-tax) (MIT). `/cmaxx-update` checks this fork's version; the price list still comes from the original, where it is maintained. Everything else is the original plugin.

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
| 🔔 **One pop-up** | A single warning when a big chat's cache expires. |
| 🛑 **Ask before an expensive message** | Typing into a 🔴 chat, Claude Code first asks **Send anyway ($2.32)** or **Don't send**. Set it to `warn` for a pop-up only. |
| 🔥 **Keepwarm** | One click on the bar (or `/cmaxx-warm`) keeps the cache warm while you are away, 8 hours by default. It stops by itself when the work looks finished or your usage limit reaches 93%. |
| 🗜️ **Auto-compact** | Walk away mid-task: 5 minutes before the cache expires, Haiku judges whether the work is finished, and an unfinished chat is compacted in place. A finished chat is left alone. |
| 🔄 **`/cmaxx-handoff`** | Haiku (effort: High) writes a briefing of the chat; a fresh chat picks up from it. The expired chat is never resent to the main model. On the 🔴 bar it is also a **🔄 Handoff** button. |
| 📊 **`/cmaxx-stats`** | Money and tokens saved, cache hit rate, rebuilds paid: for this chat and for all chats. |
| ⬆️ **`/cmaxx-update`** | Updates the plugin, or at least its model and price list, so new models are priced on day one. |
| 🔒 **Local by default** | No network. Model calls only from the two commands above, when you run them, from keepwarm, when you start it, and from auto-compact, which you can turn off. |


## Install

Requires a recent Claude Code: the desktop app, the VS Code extension, or the CLI. It's tested on 2.1.325 and later.

**Any application**  
Just paste this repository's link (https://github.com/Mixonn/cache-maxxing) in the chat window and ask Claude to install it for you.

or see below for your specific application:

**Desktop app**
1. Click **+** next to the chat box, then **Plugins** → **Manage Plugins**.
2. On top right press **+ Add** → **Add marketplace** → **Add from a repository** and enter `Mixonn/cache-maxxing`. Remove the original cache-maxxing first: both are named the same.
3. **Add plugin** → cache-maxxing.

**Terminal or VS Code** (at the Claude Code prompt)

```
/plugin install cache-maxxing --marketplace Mixonn/cache-maxxing
```

Answer `y` to add the marketplace and press Enter for the user scope. The bar shows after your next message (may need a restart).

## Usage

| Command | What it does |
|---|---|
| `/cmaxx` | Hide or show the bar in this chat (the **✕** on the bar hides it too). |
| `/cmaxx-handoff` | Write a briefing of this chat with Haiku and start a fresh chat from it. (Doesn't trigger the old chat's model) |
| **🔄 Handoff** button | The same, in one click, on the 🔴 bar: a big chat whose cache has expired. |
| `/cmaxx-warm` | Keep this chat's cache warm: `/cmaxx-warm` (8h, or your default), `/cmaxx-warm 3h`, `90m`, `2h30m`; `/cmaxx-warm off` stops it. |
| **🔥 Keepwarm** button | The same from the bar: asks for how long (8h first), then turns into **⏹ Stop keepwarm**. |
| `/cmaxx-stats` | Show savings, cache hit rate, rebuilds, keepwarm pings and replies: this chat and all chats. |
| `/cmaxx-update` | Update to the latest version, or refresh the model and price list. |

**The handoff button.** You forgot about a big chat and its cache expired: sending a message now would re-read all of it at full price. Click **🔄 Handoff** on the 🔴 bar instead. Haiku reads the chat from your disk (never the expired cache) and writes a briefing: the goal, decisions, what failed, the next steps. Then:

- **Desktop app:** the line `Read .claude/handoffs/….md and continue from where we left off.` is on your clipboard. Start a new chat (⌘N), paste, send. The old chat stays where it was.
- **Terminal and VS Code:** the chat clears to a fresh one with that line already in the box; send it.

While the briefing is written (up to a minute), the button reads **⏳ Writing…**. It costs about a cent of Haiku. The button shows only on 🔴: on 🟠 the chat is small and simply carrying on is cheap.

**The keepwarm button.** You are stepping away from a big chat mid-task and want it exactly as it is when you come back. Click **🔥 Keepwarm** on the 🟢 or 🟡 bar and pick a window. The bar then ends with `🔥 keepwarm 7h 09m left · 1 ping $0.06`, and the button reads **⏹ Stop keepwarm**. See [Keepwarm](#keepwarm).

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
| Before an expensive message | ask | `ask`: on the 🔴 bar Claude Code asks Send anyway or Don't send first. `warn`: a pop-up only, the message always goes out. |
| Keepwarm: default hours | 8 | The window the 🔥 button offers first and `/cmaxx-warm` uses with no argument (up to 48). |
| Keepwarm: usage limit (%) | 93 | Keepwarm stops at this much of your 5-hour or weekly limit. |

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
🟢 48 min left · next message ≈ $0.01 · 50K / 1M · ✅ Keep going
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

### What happens when

| Situation | What auto-compact does |
|---|---|
| You leave a big chat mid-task | With 5 minutes left on the cache, Haiku judges it unfinished and the chat is compacted. You come back to a small chat with a notice in it. |
| Claude asked you a question and you left | Same: waiting for your answer counts as unfinished. |
| The work is done (request answered, pull request opened) | Judged once, left alone. The cache expires and the bar turns 🔴 with a **🔄 Handoff** button, if you need the chat again. |
| The chat is small (rebuild under the rebuild limit) | Never judged, never compacted: rebuilding it is cheap anyway. |
| You reply before the 5-minute mark | Nothing happens. The reply restarts the hour, and that reply gets its own judgment later. |
| Your 5-hour or weekly limit reaches 93% | Every open, big, unfinished chat whose cache is still active is judged and compacted right away, however much cache time is left. An idle chat learns of the limit within 30 seconds of the chat that hit it. |
| The limit is already at 100%, or that window has reset since | Nothing: a used-up window cannot pay for a compaction, and a reset window is no reason for one. |
| Haiku is unavailable or gives no clear answer | Nothing, and it is not asked again about that reply. |
| A turn is running when the compaction starts | The compaction is refused and retried every 5 seconds, without asking Haiku again, until a minute before the cache expires. |
| Another plugin's hook vetoes the compaction | It is not tried again for that reply. |
| The computer sleeps, or the app or the chat is closed | Nothing runs. Waking up with more than a minute of cache left carries on as above; waking up after the cache expired leaves the chat as it is. |
| The chat was just compacted (by you or by auto-compact), or you switched model | Nothing until your next reply: the cache is already gone, and the chat is already small. |
| The chat grows big again after an auto-compact | It can be auto-compacted again, after a later reply, by the same rules. |
| API key, cloud provider or usage credits (5-minute cache) | Never: five minutes is too short to act on. |
| `claude -p`, the SDK, subagents | Never. |
| Auto-compact is `off` | Never; the rest of the plugin works as in the original. |

## Before an expensive message

When the bar is 🔴 (the cache expired or was reset, and rebuilding it costs more than the rebuild limit), a message you type is held and Claude Code's own dialog asks:

```
cache-maxxing: this chat's cache expired 1h 00m ago, so this message rewrites 290K tokens ≈ $2.32 (a warm reply ≈ $0.06, 40× less). /cmaxx-handoff could save 232K tokens | $2.21 instead. Send it anyway?
[Send anyway ($2.32)]  [Don't send]
```

- **Send anyway** sends the message unchanged.
- **Don't send** drops it; nothing is paid. Sending it again asks again.
- **Closing the dialog** drops it once; sending it again goes through without asking, as you have seen the price.
- Only messages you type are asked about: slash commands, `claude -p`, the SDK, scheduled and background prompts always go through.
- If anything in the plugin fails, the message goes through.

Set **Before an expensive message** to `warn` for the original behaviour: a pop-up, and the message always goes out.

## Keepwarm

A chat's cache lives an hour after its last request. Keepwarm sends one tiny request, a *ping*, 10 minutes before it would expire: a tool-less fork of the chat asked to reply with one word. The ping reads the whole cache, which restarts its hour, and costs about one cache read (`300K` tokens on Opus 5.5 ≈ $0.06 at list prices), against ≈ $2.40 to rebuild that chat cold. The fork answers into nothing: your chat never shows it. Ported from cache-tax, whose measured run shows a fork at minute 50 resetting the hour on a subscription.

Before the first ping after each of your replies, Haiku judges whether the work is finished (the same judgment auto-compact uses, asked once per reply and shared between the two).

### What happens when

| Situation | What keepwarm does |
|---|---|
| You start it and leave mid-task | Haiku judges the work unfinished; a ping every ~50 minutes keeps the cache warm until the window ends. |
| The work is finished (request answered, pull request opened, nothing pending) | Stops before the first ping, with a pop-up: `keepwarm stopped, the work looks finished (no pings)`. |
| Haiku is unavailable or gives no clear answer | Keeps pinging: you asked for it. |
| Your 5-hour or weekly limit reaches 93% (configurable) | Stops. If the work is unfinished, auto-compact's own limit trigger then compacts the chat. Starting keepwarm at the limit says why it won't. |
| The window ends | Stops. The cache still has up to an hour; auto-compact then acts on it as usual. |
| You come back and reply | Your reply refreshes the cache itself; keepwarm carries on until its window ends and judges the new reply before its next ping. |
| A ping reads nothing, or writes a tenth or more of what it read | Stops: the cache was already gone and a rebuild was just paid; pinging on would pay it again. |
| The API is busy (429, 5xx) | Retried after 1, 2 and 4 minutes while the cache can still be reached, then stops. |
| The computer sleeps past the cache | Stops at wake-up, without a ping (a ping then would pay the rebuild). |
| `/clear`, the chat closes, the app restarts | The window ends with its chat; start it again in the new one. |
| The chat was just compacted, or you switched model | Waits for your next reply: the old cache is gone and the next reply writes the new one. |
| API key, cloud provider or usage credits (5-minute cache) | Refuses to start: a ping every four minutes is not what this is for. |
| Auto-compact | Does not compact a chat for its expiring cache while keepwarm holds it; its usage-limit trigger still applies. |

## Privacy

- **The bar itself sends nothing anywhere.** Everything it shows is worked out on your computer.
- **Auto-compact calls models by itself, unless you turn it off.** It sends Claude Haiku an abbreviated copy of the chat's last 30 messages (long ones cut, tool output left out) to judge whether the work is finished. If it is not, the chat is compacted exactly as `/compact` would: the chat's own model summarizes it.
- **Keepwarm calls the chat's own model, only while a window you started runs.** Each ping is a fork of the chat with a fixed one-line prompt, sent to the same model and account as the chat itself; Haiku gets the same abbreviated copy as auto-compact's judgment.
- **The question before an expensive message** is Claude Code's own dialog; nothing is sent.
- **Two commands reach outside, when you run them:**
  - `/cmaxx-handoff` sends Claude Haiku an abbreviated copy of the chat (the last 120 messages, long ones cut, tool output left out). The briefing is saved in your project under `.claude/handoffs/`, which gets its own `.gitignore`, so briefings are never committed.
  - `/cmaxx-update` downloads this fork's version number (from `Mixonn/cache-maxxing`) and the price list (from the original `5m0k3/cache-maxxing`) and sends nothing. GitHub sees your IP address, as for any download.
- **Stored on your computer:** a small record per chat (time of the last reply, chat size, model) for 7 days, and counts for `/cmaxx-stats` (numbers only, each chat's for 90 days, the totals for good). **Never the chat's text!**

## Limitations

- Auto-compact works only for chats open in a running app: a closed chat has no process to run it, and nothing runs while the computer sleeps. A chat that wakes up with an expired cache is left as it is.
- At the usage-limit trigger, every open unfinished chat compacts at about the same time. Each costs about one normal message, and together they can use up what is left of the window.
- "Finished" is Haiku's judgment of the last 30 messages. It can be wrong both ways: a finished chat compacted, or an unfinished one left to expire.
- Keepwarm runs only while the app runs and the computer is awake, and its window lives in memory: a restart ends it.
- How a ping's cache read counts against a subscription's 5-hour and weekly limits is not documented; the dollar figures are API list prices. Watch your usage during the first window.
- If your organization blocks Haiku, `/cmaxx-handoff` writes a facts-only briefing that keeps your last message, and says why.
- The handoff's saving is an estimate: Haiku's share and a fresh chat's start are taken from measured medians, priced as Claude Haiku 5.5.
- Switching model and back within the hour shows "cache reset", although the first model's cache may still be warm.
- `/cmaxx-stats` reads an older chat's history from its transcript only up to 4 MiB; a bigger chat that started before you installed the plugin is counted from when the plugin first saw it.
- A brand-new chat in the desktop app shows the bar only after your first message: the app starts the chat, and its plugins, at that point.

## FAQ

**Does it cost anything to run?** The bar, no. `/cmaxx-handoff` makes one small Haiku call, typically about a cent at API prices. Keepwarm costs one cache read per ping, about every 50 minutes, while you leave it on.

**Is keepwarm allowed?** The original cache-maxxing left it out: Anthropic's Consumer Terms bar access by "automated or non-human means" unless via an API key or explicitly permitted. This fork adds it as something you start yourself, per chat, for a window you choose, through Claude Code's own plugin API, and it stops by itself. Whether that is within the terms is not settled here; it is your call, and Keepwarm stays off until you click it.

**I'm on a subscription. Are the dollar amounts real?** They're API list prices, a way to compare. Set **Show cost as** to `times` to see "40× more" instead.

## Contributing

Issues and pull requests are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for setup, tests and the project rules, and [SECURITY.md](SECURITY.md) to report a vulnerability privately. Changes are listed in [CHANGELOG.md](CHANGELOG.md).

## License

[MIT](LICENSE) © 5m0k3
