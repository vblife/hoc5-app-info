# Use the HOC5 app icon for devotion link previews

The app shares each devotion's title and canonical `hocdevotion.us` link.
The icon hosted in this repository can be used as the website's default social
preview image. Browser favicon tags alone do not reliably supply link thumbnails.

## Image

- URL (available after merge and GitHub Pages publication):
  `https://vblife.github.io/hoc5-app-info/assets/hoc5-app-icon.png`
- Format: PNG, 1024 × 1024 pixels, no transparency.
- Source: the mobile app repository's `hoc5-ios.png`, copied without modification.

## Configure WordPress on hocdevotion.us

Use the site's existing social metadata settings, if available, to set the image
URL above as the default Open Graph image for devotion posts. If metadata is
managed in a theme or plugin, emit the following in each devotion page's HTML
`<head>`. Replace the example title, URL, and description with that post's values,
escaped for HTML attributes. Avoid duplicate or conflicting Open Graph tags.

```html
<meta property="og:type" content="article" />
<meta property="og:site_name" content="HOC5 Daily Devotion" />
<meta property="og:title" content="第1天 (創1:1-13)" />
<meta property="og:url" content="https://hocdevotion.us/gen001/" />
<meta property="og:description" content="每日經文、牧長分享與靈修。" />
<meta property="og:image" content="https://vblife.github.io/hoc5-app-info/assets/hoc5-app-icon.png" />
<meta property="og:image:type" content="image/png" />
<meta property="og:image:width" content="1024" />
<meta property="og:image:height" content="1024" />
<meta property="og:image:alt" content="HOC5 Daily Devotion app icon" />
```

These tags belong on `hocdevotion.us` because that is the domain of the shared
devotion links. Adding them only to the app information site would affect only
links to that site. This repository does not deploy changes to WordPress.

## Verify after publication

1. Confirm the image URL returns HTTP 200 with `Content-Type: image/png`, without
   login or a bot challenge.
2. Clear the affected WordPress/CDN page caches and inspect the devotion page's
   server-rendered HTML for the correct `og:image` and post-specific metadata.
3. Share the devotion again from the installed app to WeChat on a real device.
   Existing cards may retain cached previews. Open Graph metadata supplies a
   preview candidate; WeChat decides how to render it. If the thumbnail is still
   absent, investigate native sharing with an explicit thumbnail.

Reference: [Open Graph image metadata](https://ogp.me/#structured).
