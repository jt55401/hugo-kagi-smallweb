# hugo-kagi-smallweb

A Hugo module that adds [Kagi Small Web](https://kagi.com/smallweb) membership badge and webring navigation to your Hugo site.

Provides three partials:
- **badge** — official Kagi Small Web animated seal (88×31 GIF), links to kagi.com/smallweb
- **webring-bar** — prev/next navigation bar built from `smallweb.txt` at build time
- **styles** — CSS for the webring bar; call from your `<head>`

## Requirements

- Hugo 0.141.0 or later (uses `resources.GetRemote` with `try` error handling)
- Your site's RSS feed must be listed in [kagisearch/smallweb](https://github.com/kagisearch/smallweb)

## Installation

Add to your `hugo.toml` / `config.toml`:

```toml
[params.kagiSmallWeb]
  enabled      = true
  feedUrl      = "https://your-site.com/index.xml"  # must match smallweb.txt exactly
  badgeVariant = 1                                   # 1 (default) or 2

[[module.imports]]
  path = "github.com/jt55401/hugo-kagi-smallweb"
```

Then run:

```bash
hugo mod get github.com/jt55401/hugo-kagi-smallweb
```

## Usage

### Styles (add to `<head>`)

```html
{{ partial "kagi-smallweb/styles.html" . }}
```

### Badge

Add to any layout where you want the animated seal to appear:

```html
{{ partial "kagi-smallweb/badge.html" . }}
```

### Webring bar

Add to any layout where you want the prev/next navigation:

```html
{{ partial "kagi-smallweb/webring-bar.html" . }}
```

### Example: hugo-coder theme integration

**Styles in `<head>`** — add to `layouts/partials/head/extensions.html`:
```html
{{ partial "kagi-smallweb/styles.html" . }}
```

**Badge on home page** — add to `layouts/partials/home/extensions.html`:
```html
{{ partial "kagi-smallweb/badge.html" . }}
```

**Webring bar in footer** — create `layouts/_partials/footer.html` to override the theme footer. Copy the theme's footer content verbatim, then append:
```html
{{ partial "kagi-smallweb/webring-bar.html" . }}
```

## Configuration reference

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `kagiSmallWeb.enabled` | bool | false | Enable the module. Both partials render nothing unless true. |
| `kagiSmallWeb.feedUrl` | string | — | Your RSS feed URL, exactly as it appears in `smallweb.txt`. |
| `kagiSmallWeb.badgeVariant` | int | 1 | Badge variant: 1 or 2. Falls back to 1 if absent or invalid. |

> **Note:** Hugo normalizes all `params` keys to lowercase internally. The keys above are shown in their TOML/config form (`feedUrl`, `kagiSmallWeb`); in template code they appear as `feedurl` and `kagismallweb`. This is handled automatically — you do not need to change your config.

## Keeping the site list fresh

The `smallweb.txt` list is cached by Hugo at build time based on HTTP cache headers. To force a fresh fetch:

```bash
hugo --ignoreCache
```

## Badge variants

- Variant 1 (default): animated blue badge
- Variant 2: animated alternate design

Both are official Kagi Small Web Seal images served from Kagi's servers.

## Notes

- "Browse" links to `kagi.com/smallweb` (no dedicated random-entry endpoint exists)
- If your `feedUrl` is not found in `smallweb.txt`, a build warning is emitted and the bar is hidden
- If the list fetch fails, the bar is hidden and a build warning is emitted; the badge is unaffected
- The webring bar renders nothing until your feed URL appears in the live `smallweb.txt`
