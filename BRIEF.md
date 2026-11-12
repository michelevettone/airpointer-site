# Brief: the Air Pointer product site

`airpointer.michelevettone.com` — a static site on Cloudflare Pages. This file is the starting point
for anyone (human or agent) redesigning it.

## What it is

Air Pointer turns an iPhone into a pointer for a Mac or PC over Bluetooth: aim, trackpad, keyboard,
present mode. No account, no cloud, no tracking. The site has three pages: the product landing, the
privacy policy, the support page.

## Audience

Two kinds, in this order:

1. Someone who just heard about the app and wants to know what it does and install it.
2. Someone who already uses it and needs the Mac/Windows companion, or help.

Not a developer audience. Not a place for feature lists longer than a screen.

## What must not change

| Thing | Why |
|---|---|
| `/privacy` and `/support` paths | They are hardcoded in the iPhone app (`Config.swift`) and in App Store Connect |
| Static HTML/CSS, no framework, no build step | Deploy is `wrangler pages deploy . --project-name airpointer` |
| No cookies, no analytics, no third-party scripts | The product's whole claim is "no tracking"; it also avoids a cookie banner |
| English only | The app is localized, the site is not |
| Download links point to `releases/latest/download/…` | The asset names are a contract |

## Assets

- App icon: `iPhone/…/AppIcon.png` (1024², no alpha) — in this repo as `icon-512.png`
- Menu bar mark: transparent glyph, `favicon-light.png` / `favicon-dark.png`
- Screenshots: `AppStore/screenshots/iphone/<locale>/` in `michelevettone/PhonePointer`,
  1320×2868, six per locale
- Demo video: `AppStore/demo.mp4` in the same repo

## Copy

The source of truth for the English product text is `AppStore/metadata.md` in
`michelevettone/PhonePointer` (description, keywords, promotional text, review notes). The 13 localized
listings live in `AppStore/localized/`.

Do not invent claims. Everything the site says must be true of the app: free companion app, Bluetooth
only, no account, no data collected.

## Deploy

```sh
cd ~/Developer/airpointer-site
wrangler pages deploy . --project-name airpointer
```

Pushing to GitHub does not deploy: the Pages webhook is not connected, so the command above is the
deployment.

## Current structure

`index.html` (landing), `privacy.html`, `support.html`, `style.css`, favicons, `assets/` with three
screenshots. No JavaScript.
