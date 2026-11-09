# Request: add two verification files to hocdevotion.us so the mobile app can open devotion links

To let iPhone and Android open links like `https://hocdevotion.us/gen001/` directly in the app, the site needs to serve two small static JSON files. This doesn't change any WordPress pages or content. Phones read the files to confirm that the site owner approves the app.

## What to add

Download the ready-to-upload files from this repository, keeping their exact filenames:

- [apple-app-site-association](https://github.com/vblife/hoc5-app-info/blob/main/apple-app-site-association) ([raw file](https://raw.githubusercontent.com/vblife/hoc5-app-info/main/apple-app-site-association))
- [assetlinks.json](https://github.com/vblife/hoc5-app-info/blob/main/assetlinks.json) ([raw file](https://raw.githubusercontent.com/vblife/hoc5-app-info/main/assetlinks.json))

Use the raw files when downloading so you upload JSON rather than a GitHub HTML page. The files already contain the verified Apple App ID prefix and Google Play app signing fingerprint; no placeholders need replacing. Keep the full path lists.

Create a `.well-known` folder in the `hocdevotion.us` site root, the same directory that holds `wp-config.php`. If the folder already exists, keep its other files. Put the downloaded files in it:

1. `.well-known/apple-app-site-association`
   - The name has no extension.
   - Contains Apple App ID `P8HT6K23KY.org.hoc5.dailydevotion` and the supported devotion paths.
2. `.well-known/assetlinks.json`
   - Contains package `org.hoc5.dailydevotion` and the app's signing certificate SHA-256.

Storing these files in `hoc5-app-info` makes them available to download. They must also be uploaded to `hocdevotion.us` at the exact paths below to enable verification for that domain.

## Requirements for both URLs

- `https://hocdevotion.us/.well-known/apple-app-site-association`
- `https://hocdevotion.us/.well-known/assetlinks.json`

Each URL must:

- Return HTTP **200** directly, with no redirect, including none to `www`.
- Return `Content-Type: application/json`.
- Return the raw JSON, not a WordPress page or a 404 page.
- Work without login, cookies, CAPTCHA or JavaScript challenges.

## Things that may need adjusting

- **Content-Type for the extensionless file.** On Apache, add this to the root `.htaccess` above the WordPress rules:
  ```apache
  <Files "apple-app-site-association">
    ForceType application/json
  </Files>
  ```
- **WordPress rewrite rules.** Real files should be served before WordPress takes over the request. The default `RewriteCond %{REQUEST_FILENAME} !-f` normally handles this.
- **Security plugin, WAF or CDN** (Wordfence, Cloudflare, etc.). Please allow requests to these two exact paths. A previous automated request got an HTTP 406, so bot protection may be blocking them. Apple's and Google's verification servers must be able to fetch both files.
- **Caching.** If a CDN or cache plugin is in use, purge those two URLs after uploading.

## How to verify

Run these without `-L`, so redirects show up:

```bash
curl -i https://hocdevotion.us/.well-known/apple-app-site-association
curl -i https://hocdevotion.us/.well-known/assetlinks.json
```

Expected: `HTTP/2 200`, `content-type: application/json`, and the JSON body with no `REPLACE_WITH_` text.
