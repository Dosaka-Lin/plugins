# Loomy

[OpenCode](https://opencode.ai) / [magpie](https://usemagpie.ai) provider plugin for 讯飞 Loomy. Provider id: `loomy`.

## Install

```sh
magpie plugin add @magpie-community/opencode-loomy-auth
magpie plugin login loomy
```

In OpenCode:

```json
{ "plugin": ["@magpie-community/opencode-loomy-auth"] }
```

## Sign in

The plugin offers **Phone + SMS code** and **Paste Loomy login**. The second accepts a session or the same shape as an exported login:

```json
{"provider":"loomy","auth":{"accessToken":"…","userid":"…","phone":"…"}}
```

The session is kept in magpie's `plugin-auth.json` (mode 0600) or OpenCode's `auth.json`.
Loomy sessions last about 14 days and there is no refresh endpoint; sign in again when the account lapses.

**Phone + SMS code** sends the code through Loomy's CAccount endpoint, then asks for the six-digit code. **Paste Loomy login** is useful for an existing session or when a browser or phone is not at hand.

## Models

The list comes from Loomy's `GET /api/v1/models` and is read again whenever the host asks. Models are used as `loomy/<model>`, for example `loomy/DeepSeek-V4.1-Flash`.

The plugin keeps model metadata from Loomy's capabilities: context and output limits, reasoning, tool calls, images and video. A missing or failed list keeps the last list.

## Points and daily check-in

`auth.usage` shows the permanent and daily points from `GET /api/v2/points/records`.

Loomy has no separate check-in endpoint. Its daily reward is `POST /api/v1/points/first-login`, which refreshes the daily gift to about 5000 points. `auth.checkin` maps that call to magpie's daily check-in hook. The vendor call is idempotent, so a repeated press does not claim twice.

The check-in switch is off by default. In magpie, turn on **Settings → Usage → Plugins → Loomy → Daily check-in**, or press **Check in now**.

## Images

Loomy's DeepSeek models expect images as `<image_base64>…</image_base64>` text markers rather than OpenAI `image_url` blocks. The plugin converts data-URI images for those models; other models keep the standard OpenAI shape.

## Licence

MIT. This is an unofficial community plugin, not affiliated with 讯飞 or Loomy.
