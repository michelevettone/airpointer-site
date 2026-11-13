# Brief: the Air Pointer product site

`airpointer.michelevettone.com` — a static site on Cloudflare Pages. This file is the starting point
for anyone (human or agent) working on it.

## What it is

Air Pointer turns an iPhone into a pointer for a Mac or PC over Bluetooth: aim, trackpad, keyboard,
present mode. No account, no cloud, no tracking. The site has three pages: the product landing, the
privacy policy, the support page.

## Current release state

The iPhone app is in public beta on TestFlight: <https://testflight.apple.com/join/q9EmkjqU>

The App Store listing (`id 6833520222`, <https://apps.apple.com/app/id6833520222>) is not live yet.
The free Mac companion is on GitHub Releases and the Windows one on the Microsoft Store; neither is distributed through TestFlight.

## Audience

Two kinds, in this order:

1. Someone who just heard about the app and wants to know what it does and install it.
2. Someone who already uses it and needs the Mac/Windows companion, or help.

Not a developer audience. Not a place for feature lists longer than a screen.

## What must not change

| Thing | Why |
|---|---|
| `/privacy` and `/support` paths | They are hardcoded in the iPhone app (`Config.swift`) and in App Store Connect |
| Static HTML/CSS, no JavaScript, no framework, no build step | Deploy is `npx wrangler pages deploy . --project-name airpointer` |
| No cookies, no analytics, no third-party scripts | The product's whole claim is "no tracking"; it also avoids a cookie banner |
| English only | The app is localized, the site is not |
| Download links point to `releases/latest/download/…` | The asset names are a contract |

## Assets

- App icon: `iPhone/…/AppIcon.png` (1024², no alpha) — in this repo as `icon-512.png`
- Menu bar mark: transparent glyph, `favicon-light.png` / `favicon-dark.png`
- Screenshots: `AppStore/screenshots/iphone/<locale>/` in `michelevettone/PhonePointer`,
  1320×2868, six per locale
- Site screenshots: `assets/aim.png`, `trackpad.png`, `keyboard.png`, `present.png` — four of the six
- Demo video: `AppStore/demo.mp4` in the same repo (not used on the site yet)

## Copy

The source of truth for the English product text is `AppStore/metadata.md` in
`michelevettone/PhonePointer` (description, keywords, promotional text, review notes). The 13 localized
listings live in `AppStore/localized/`.

Do not invent claims. Everything the site says must be true of the app: free companion app, Bluetooth
only, no account, no data collected.

- The app keeps the screen awake while it is in the foreground. Never tell users to keep the screen
  on or unlocked; the only requirement is that Air Pointer stays in the foreground.
- Do not say the app is on the App Store until the listing is live: the current iPhone CTA is TestFlight.

## Deploy

```sh
cd ~/Developer/airpointer-site
npx wrangler pages deploy . --project-name airpointer
```

Pushing to GitHub may not deploy: the Pages webhook has been unreliable. Run the command and verify
the live site; that is the deployment.

## Maintenance

- **Cache.** Cloudflare serves `style.css` and the screenshots with `max-age=14400`. When the CSS or a
  screenshot changes, bump the `?v=` query on its URL in the HTML. Otherwise a browser can keep the
  old file for up to four hours.
- **iPhone CTA.** The site links the TestFlight beta from the hero, the iPhone download card, the
  footer and Support. When the App Store listing is live, replace those links, restore the official
  white App Store badge (Apple badge API:
  `https://tools.applemediaservices.com/api/badges/download-on-the-app-store/white/en-us`), and
  uncomment `apple-itunes-app` in the three pages.
- **Clean URLs.** Cloudflare serves `/privacy` and `/support` from `privacy.html` and `support.html`.
  A local `python3 -m http.server` needs `/privacy.html` and `/support.html` directly.
- **No scripts.** The mode switcher is a CSS-only radio group; scroll reveals are CSS scroll timelines
  with a static fallback. Do not add JavaScript or third-party scripts.

## Current structure

`index.html` (landing), `privacy.html`, `support.html`, `style.css`, favicons, `assets/` with the four
screenshots. No JavaScript.
