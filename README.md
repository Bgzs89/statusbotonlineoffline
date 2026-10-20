# Lucifer Bot Status Monitor

A lightweight Lua script for **LUCIFER** executor that monitors multiple bot instances and reports their online/offline status to Discord via webhook. Real-time status updates with activity tracking.

---

## Features

- **Multi-bot monitoring** — Tracks all connected bot instances simultaneously
- **Discord webhook integration** — Sends rich embeds with custom emojis
- **Activity detection** — Reports active features (AutoFarm, AutoGeiger, AutoSpam, AutoFish, AutoHarvest, AutoCrime, AutoCook, AutoCollect)
- **Disconnection alerts** — Instantly notifies when a bot goes offline
- **Auto-update message** — Edits single webhook message instead of spamming (updates every 2 minutes)
- **Lightweight** — ~30s polling interval, minimal resource usage

---

## Requirements

- **LUCIFER** executor (Roblox)
- Discord webhook URL
- Internet access for HTTP requests

---

## Installation

1. **Copy `downloadcheckbot.lua`** to your LUCIFER scripts folder
2. **Edit the script** and replace:
   ```lua
   link = "your webhook link"          -- Your Discord webhook URL
   messageId = nil                     -- Keep nil on first run
   ```
3. **Execute** `downloadcheckbot.lua` in LUCIFER
4. **Copy the message ID** from Discord (right-click message → Copy Message ID)
5. **Paste it** into `messageId = "your_message_id_here"`
6. **Re-execute** — bot will now edit the same message on updates

---

## Configuration

| Variable | Description | Example |
|----------|-------------|---------|
| `link` | Discord webhook URL | `https://discord.com/api/webhooks/...` |
| `messageId` | Discord message ID to edit | `"123456789012345678"` or `nil` (first run) |
| Poll interval | Check frequency (line 98) | `sleep(30000)` = 30 seconds |
| Update threshold | Full status refresh (line 97) | `120` = 2 minutes |

---

## Discord Output Preview

```
😈 BotName terputus!

🎵 Status Semua Bot

🟢 BotName1 - Online | 🌍 WorldName
   ┗ 🌾 AutoFarm, 📡 AutoGeiger, 💬 AutoSpam

🔴 BotName2 - Offline

⚠️ Last Update: 😂 14:32:15
```

---

## File Structure

```
statusbotonlineoffline/
├── downloadcheckboot.lua      # Loader (paste this in LUCIFER)
└── updated checkbot v1.4.lua  # Main script (auto-fetched from GitHub)
```

> **Note:** The main script is hosted on GitHub and auto-loaded. Update the raw URL in `downloadcheckbot.lua` line 7 if you fork.

---

## How It Works

1. Loader fetches latest script from GitHub raw URL
2. Initializes `prevStatus` table with current bot states
3. Main loop runs every 30 seconds:
   - Checks each bot's `BotStatus.online`
   - Detects disconnections (was online → now offline)
   - Builds rich embed with status + activities
   - Sends new webhook (first run) or edits existing (subsequent)
   - Full status refresh every 2 minutes

---

## Customization

**Add/remove monitored activities** — Edit the `checks` table in `getActivity()`:
```lua
local checks = {
    { bot.auto_farm,    "🌾 AutoFarm"    },
    { bot.auto_geiger,  "📡 AutoGeiger"  },
    -- Add your custom features here
}
```

**Change embed color** — Line 71: `wbh.embed1.color = 0xFFB6C1` (hex)

**Adjust timing** — Line 97-98:
```lua
sendOrEditStatus(now - lastStatusUpdate >= 120)  -- 2 min full refresh
sleep(30000)                                      -- 30 sec poll interval
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "HttpClient not found" | Ensure LUCIFER has HTTP library enabled |
| Webhook not sending | Verify webhook URL, channel permissions |
| Message not editing | Ensure `messageId` is correct string, not number |
| Script errors | Check LUCIFER console (F9) for stack traces |

---

## Security Notes

- **Never commit real webhook URLs** — Use environment variables or local config
- **Webhook tokens are sensitive** — Treat like passwords
- **Message IDs are public** — But only editable with webhook token

---

## License

MIT — Free to use, modify, distribute.

---

## Support

- **Issues**: [GitHub Issues](https://github.com/Bgzs89/statusbotonlineoffline/issues)
- **Discord**: Add webhook to your server for notifications