# Telebot for RayQuiro

`telebot` is a practical Telegram bot framework for RayQuiro built on top of the current runtime.

## What this version does

- low-level Bot API helpers through `curl.exe`
- `sendMessage`, HTML, Markdown, reply, photo, document, callback answer, inline buttons
- `getMe`, `getUpdates`, `getFile`, `setWebhook`, `deleteWebhook`
- a generated PowerShell long-polling host, so you can run a real bot without changing the RayQuiro core
- command registration and `setMyCommands` sync

## Install

```powershell
rqio install telebot
```

or:

```powershell
rqio framework install RayQuiro/Telebot
```

## Use

Low-level use:

```rq
from telebot import token_from_env, send_message, send_photo, send_buttons;

var token = token_from_env("TELEGRAM_BOT_TOKEN");
send_message(token, "123456789", "Hello from RayQuiro!");
send_photo(token, "123456789", "assets/demo-photo.jpg", "Photo from RayQuiro");
send_buttons(token, "123456789", "Open links:", "{\"inline_keyboard\":[[{\"text\":\"Website\",\"url\":\"https://raytolfas.com\"}]]}");
```

Polling bot use:

```rq
from telebot import token_from_env, bot_default, on_command, on_message, on_button_command, on_callback, run_polling;

var token = token_from_env("TELEGRAM_BOT_TOKEN");
var app = bot_default(token);

on_command(app, "/start", "Welcome to RayQuiro Telebot.");
on_button_command(app, "/menu", "Choose an action:", "Website=>https://raytolfas.com;Support=>support_callback");
on_callback(app, "support_callback", "Support clicked.");
on_message(app, "I got your message.");

run_polling(app);
```

Button spec format:

- `Label=>https://example.com`
- `Label=>callback_id`
- multiple buttons in one row: `"Docs=>https://...;Support=>support_callback"`

Generated bot workspace:

- `token.txt`
- `handlers.rqtb`
- `commands.rqtb`
- `state.offset`
- `bot.ps1`

Notes:

- This `0.0.1` version is a practical Windows-first toolkit.
- It relies on `curl.exe` and PowerShell being available, which is fine on modern Windows.
- The long-polling runtime is generated into `bot.ps1`, so the framework works today without new core language features.
- When native `http/json` APIs arrive in RayQuiro, this framework can move from generated PowerShell to a pure RayQuiro runtime.
