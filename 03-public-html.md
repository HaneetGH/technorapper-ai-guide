# 3. public_html

A downloaded web root is a snapshot of files. It is not the live database.

On this project the zip was taken **before** the audit. Plugins were then deactivated in the database, feeds were retargeted, and options changed. Those fixes were not in the files. Uploading the whole folder would have put the old code, and a copy of `wp-config.php`, back on top of the live site.

## Do this

1. Keep the download on your machine. Put `wp-config.php`, `*.sql`, `error_log`, and `uploads/` in `.gitignore` if the folder is in git.
2. Diff the theme and the small root files (`robots.txt`, `.htaccess`) against what the live site is serving.
3. Change only the files that are wrong.
4. Upload those paths. For us that was FTP to `public_html/...`, not to the account home. The FTP login could see the whole hosting account, which is wider than a web-root jail. Prefer an account that can only write the site, and rotate the password if it was pasted into a chat.
5. Request the public URL and confirm the change is in the HTML. A proxy cache can keep serving the old page. A `?v=` query, or a purge if the host allows it, shows the new markup.

## Do not do this

- Do not upload the whole `public_html`.
- Do not upload `wp-config.php` from an old download. It holds the database password.
- Do not delete the live `uploads` folder because the local copy looks messy.
- Do not treat a file edit as done until the public URL returns it. Opcache and the nginx cache both lie if you only look at the laptop.
- Do not upgrade a parent theme you have edited. The upgrade replaces your files. A child theme is the durable version of this work. We edited CoverNews in place and left the update uninstalled for that reason.

## Files that actually went up

These are theme and root files, not the database.

| Path | Why |
| --- | --- |
| `robots.txt` | Point crawlers at the real sitemap. The old file sent them to a dead Attracta URL. |
| `.htaccess` | Keep the HTTPS redirect. Add basic security headers. |
| `wp-content/mu-plugins/technorapper-hardening.php` | Stop XML-RPC in PHP. The nginx edge still answered `GET` on `xmlrpc.php`, so this is not a complete block. |
| `wp-content/themes/covernews/header.php` | Remove the full-page preloader. It waited for `window` load, and ads plus comments meant that event often never came. |
| `wp-content/themes/covernews/assets/script.js` | Hide the preloader immediately if a cached page still has the node. |
| `wp-content/themes/covernews/functions.php` | Load the repair stylesheet, swap the font display, and include the homepage desks. |
| `wp-content/themes/covernews/assets/css/technorapper-fixes.css` | Layout, ticker thumbs, footer, and tap targets. Version the file so you can see it in the HTML. |
| `wp-content/themes/covernews/inc/template-images.php` | Stop `getimagesize()` on remote images during render. That call downloaded the image before any HTML was sent. |
| `wp-content/themes/covernews/inc/technorapper-desks.php` | Homepage sections for Android, PC and Hardware, and Guides, plus the archive title. |
| `wp-content/themes/covernews/inc/hooks/blocks/block-post-header-layout-1.php` | Menu and search as buttons, so they are not `javascript:` or `#` links. |
| `wp-content/themes/covernews/footer.php` | Back to top as a button. |
| `wp-content/themes/covernews/inc/widgets/widgets-common-functions.php` | “Read More” becomes “Full report:” plus the headline. |
| `wp-content/themes/covernews/inc/hooks/hook-front-page-main-banner-section-1.php` | Section labels as `h2`, under the site `h1`. |
| `wp-content/themes/covernews/inc/hooks/blocks/block-post-latest.php` | Same heading fix for Latest. |
| `wp-content/themes/covernews/js/navigation.js` | Stop putting `aria-expanded` on the menu list. |
| `wp-content/themes/covernews/inc/template-functions.php` | Page numbers include the word “Page”. |
| `wp-content/themes/covernews/inc/hooks/hook-front-page-banner-exclusive-posts.php` | Ticker images get a real width and height. |

The CoverNews patches are listed so you can see what a first pass looks like. They are not in this public repo. When those patches were no longer enough, we stopped editing the parent and shipped a small custom theme: [technorapper-wp-theme](https://github.com/HaneetGH/technorapper-wp-theme). Upload that folder into `wp-content/themes/`, activate it, and leave CoverNews on disk until you are sure. Do not upgrade CoverNews in place if you still need the old files as fallback.

The custom theme is GPL-2.0-or-later. This playbook stays MIT. Do not copy our AdSense client ID into your fork.

## What the database work covered

MCP changed data, which a file upload cannot do:

- Stripped a JSON wrapper off imported posts that were publishing as source code.
- Rebuilt category descriptions and menus without changing slugs or permalinks.
- Turned thin excerpt feeds off and pointed imports at feeds that carry the full article.
- Hid the Site Kit automatic-ad snippet. The publisher ID stayed the same. A sidebar unit remained.
- Deactivated unused plugins in the database. The plugin folders were still on disk. Deactivation is not the same as deleting the folder.
- Set a homepage meta description that is not the tagline “Since 2012”.

Feeds that only return a sentence or two were rejected. Full-text feeds that also mix in non-tech deals (one guides feed did) are a content decision, not a file upload.

## Cache

This host runs nginx in front of Apache. HTML can be cached. `X-Proxy-Cache: HIT` means you are not looking at the PHP you just edited. Confirm on a `MISS` or an `EXPIRED` response before you measure.
