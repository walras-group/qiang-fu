---
name: logo-search
description: Search and retrieve company/brand logos using the logo.dev API. Use this skill whenever the user wants to find a logo, get a logo URL, search for a brand's logo, display a company logo, look up a logo by brand name or domain, or embed logos into UI components. Trigger even if the user says things like "find me the logo for X", "get the Apple logo", "I need a logo URL for my component", "show the logo of company X", or "search logos". Also use when building UI that needs brand/company logos automatically fetched.
---

# Logo Search via logo.dev

This skill enables searching and retrieving company logos using the [logo.dev](https://www.logo.dev) API. There are two main operations:
1. **Get a logo image directly** by domain/ticker/name (uses Publishable Key `pk_`)
2. **Search for a brand** by name to discover its domain, then fetch the logo (uses Secret Key `sk_`)

## Setup

Before using this skill, make sure the project has a `.env` file with logo.dev API keys. If there isn't one, create or update it based on `.env.example`.

### .env.example

If the project doesn't have a `.env.example` already mentioning logo.dev keys, add this section:

```
# logo.dev API Keys
# Register at https://www.logo.dev to get your keys
# Dashboard: https://www.logo.dev/dashboard/api-keys

# Publishable key (pk_) — safe for client-side use, used for logo image URLs
LOGO_DEV_PUBLISHABLE_KEY=pk_your_publishable_key_here

# Secret key (sk_) — server-side only, used for brand search API
LOGO_DEV_SECRET_KEY=sk_your_secret_key_here
```

> If the user hasn't registered yet, tell them: "You'll need a free account at logo.dev. Visit https://www.logo.dev/dashboard/api-keys to get your `pk_` and `sk_` keys, then add them to your `.env` file."

---

## API Reference

### 1. Logo Image API (direct image URL)

**Endpoint:** `https://img.logo.dev/{identifier}?token=pk_YOUR_KEY`

The `identifier` can be:
- A domain: `apple.com`, `stripe.com`
- A stock ticker: `AAPL`
- A crypto symbol: `BTC`
- A company name (less reliable — prefer domain when known)

**Key parameters:**

| Param | Default | Notes |
|-------|---------|-------|
| `token` | required | Your `pk_` publishable key |
| `size` | `128` | Pixels, max 800 |
| `format` | `png` | `jpg`, `png` (supports transparency), `svg` (Enterprise only) — **always use `png`** for transparent backgrounds |
| `theme` | `auto` | `dark` or `light` — only meaningful with transparent images |
| `greyscale` | `false` | Desaturate the logo |
| `fallback` | `monogram` | `monogram` (letter icon) or `404` |
| `retina` | `false` | 2× resolution source at the same rendered `size` |

**Example:**
```
https://img.logo.dev/stripe.com?token=pk_abc123&size=64&format=png&retina=true
```

### 2. Brand Search API (find domain by name)

Use this when you only have a brand name and need to find its domain first.

**Endpoint:** `GET https://api.logo.dev/search?q={query}`

**Auth:** Secret key as Bearer token in the `Authorization` header:
```
Authorization: Bearer sk_YOUR_SECRET_KEY
```

**Parameters:**
- `q` (required): brand name to search
- `strategy` (optional): `typeahead` (default, good for live search) or `match` (exact/near-exact)

**Response:** JSON array of up to 10 results:
```json
[
  { "name": "Stripe", "domain": "stripe.com" },
  { "name": "Stripe Inc", "domain": "stripe.com" }
]
```

Use the returned `domain` directly in the Logo Image API.

---

## Common Workflows

### Workflow A: User knows the domain
Simply construct the image URL. Always use `format=png` for transparent backgrounds:
```
https://img.logo.dev/apple.com?token=pk_YOUR_KEY&size=128&format=png
```

### Workflow B: User knows only the brand name
1. Call the Brand Search API with the `sk_` key to find the domain
2. Use the returned `domain` with the Logo Image API
3. Construct and return the final image URL

### Workflow C: Embedding logos in UI code (recommended: CDN direct)
The best way to embed logos is to use the `img.logo.dev` URL directly — it's served from a CDN, so no need to download or self-host anything. Logos stay up-to-date automatically.

```html
<!-- HTML — format=png ensures transparent background -->
<img src="https://img.logo.dev/stripe.com?token=pk_YOUR_KEY&size=64&format=png" alt="Stripe" />
```

```jsx
// React — format=png ensures transparent background
<img src={`https://img.logo.dev/${domain}?token=${process.env.NEXT_PUBLIC_LOGO_DEV_PUBLISHABLE_KEY}&size=64&format=png`} alt={name} />
```

- The `pk_` key is visible in the HTML — this is intentional and safe by design
- To prevent abuse, you can restrict the key to specific domains in the logo.dev dashboard
- Keep `sk_` keys server-side only — never embed them in client code or HTML

---

## Implementation Notes

- **Key safety:** The `pk_` key is safe to use in frontend/client code. The `sk_` key must stay server-side only (never in browser JS, never committed to git).
- **Fallback behavior:** By default, logo.dev returns a letter monogram when a logo isn't found. If you want a hard 404 instead, pass `fallback=404`.
- **Format choice:** Always use `format=png` — PNG supports transparency so logos render cleanly on any background color. Only use `jpg` if file size is a hard constraint and the background is guaranteed white.
- **Missing .env:** If keys aren't set, tell the user what's missing and point them to https://www.logo.dev/dashboard/api-keys.

---

## Output Format

When the user asks to "find" or "get" a logo, provide:
1. The direct image URL (ready to copy/paste or embed)
2. A brief note on what was used (domain vs. search result)
3. If brand search was used, mention the matched domain so the user can verify it's the right company

When the user asks to embed logos in code, generate the code with the URL already constructed — don't just explain how to do it.
