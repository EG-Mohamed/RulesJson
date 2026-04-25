# RulesJson

> Convert Laravel validation rules into a ready-to-use JSON request body — instantly, in the browser.

[![MIT License](https://img.shields.io/badge/license-MIT-red.svg)](./LICENSE)
[![No Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen.svg)]()
[![Client Side](https://img.shields.io/badge/runs-client--side-blue.svg)]()

🌐 **Live:** [rules-json.msaied.com](https://rules-json.msaied.com) &nbsp;·&nbsp; 📦 **GitHub:** [EG-Mohamed/RulesJson](https://github.com/EG-Mohamed/RulesJson)

---

## The Problem

Every time you write a new `FormRequest`, you have to manually reconstruct the JSON body in your API client to test it. It's repetitive, slow, and easy to get wrong — especially with nested objects, arrays, and `confirmed` fields.

## The Solution

Paste your `$rules` array into **RulesJson** and get a fully populated, copy-ready JSON body in one click. Works with any HTTP client.

---

## Features

**Input formats**
- Pipe strings — `"required|string|max:255"`
- PHP associative arrays — `'field' => 'required|string'`
- PHP-style arrays — `['required', 'email']`
- Unquoted JS objects — `{ name: "required|string" }`

**Rule handling**
- Dot-notation → properly nested output objects (`address.city`, `meta.title`)
- Wildcard arrays → typed single-item array (`tags.*` → `["example_string"]`)
- `confirmed` → auto-generates the `_confirmation` sibling field
- `in:a,b,c` → picks the first valid enum option
- `nullable` without an explicit type → resolves to `null`
- `min:` on integers → output value respects the minimum
- `image` / `file` + `nullable` → resolves to `null`

**Smart field inference**

Fields are named-aware — no generic `"value"` placeholders:

| Field name pattern | Example output |
|---|---|
| `email` | `user@example.com` |
| `phone`, `mobile` | `+1234567890` |
| `slug` | `example-slug` |
| `uuid` | `550e8400-e29b-...` |
| `birth_date`, `date` | `2025-04-25` |
| `city` | `Cairo` |
| `country` | `EG` |
| `currency` | `USD` |
| `token` | `eyJhbGci...` |
| `url`, `link` | `https://example.com` |
| `color` | `#FF2D20` |

**UI**
- Syntax-highlighted JSON output
- Live stats — root fields, nested keys, arrays, nullable count, size
- `Format` button — pretty-prints input in place
- Copy to clipboard with `execCommand` fallback (works over HTTP and WebViews)
- `Ctrl + Enter` keyboard shortcut
- Zero dependencies — Vanilla JS + Tailwind CDN

---

## Usage

### Option A — Live

[rules-json.msaied.com](https://rules-json.msaied.com)

### Option B — Self-host

```bash
git clone https://github.com/EG-Mohamed/RulesJson.git
cd RulesJson
open index.html
```

No build step. No dependencies. Just open `index.html` in any browser.

---

## Example

**Input:**
```php
[
    'name'          => 'required|string|max:255',
    'email'         => 'required|email|unique:users',
    'password'      => 'required|string|min:8|confirmed',
    'role'          => 'required|string|in:admin,editor,viewer',
    'is_active'     => 'required|boolean',
    'avatar'        => 'nullable|image',
    'address.city'  => 'required|string',
    'tags'          => 'required|array',
    'tags.*'        => 'string|max:50',
]
```

**Output:**
```json
{
  "name": "John Doe",
  "email": "user@example.com",
  "password": "Password@123",
  "password_confirmation": "Password@123",
  "role": "admin",
  "is_active": true,
  "avatar": null,
  "address": {
    "city": "Cairo"
  },
  "tags": ["example_string"]
}
```

---

## Supported Rule → Output Map

| Rule | Output |
|---|---|
| `string` | Field-name aware string |
| `integer` | `1` (or `min:` value) |
| `numeric` | `1.0` |
| `boolean` | `true` |
| `email` | `"user@example.com"` |
| `url` | `"https://example.com"` |
| `date` | `"YYYY-MM-DD"` (today) |
| `uuid` | Full UUID v4 string |
| `ip` | `"192.168.1.1"` |
| `image` / `file` | `null` (binary — not serializable) |
| `in:a,b,c` | `"a"` (first option) |
| `nullable` (no type) | `null` |
| `confirmed` | Adds `field_confirmation` sibling |
| `array` + `field.*` | Single-item typed array |
| `address.city` | `{ "address": { "city": "Cairo" } }` |

---

## Contributing

Contributions are welcome.

1. Fork the repository
2. Create a feature branch — `git checkout -b feature/your-feature`
3. Commit — `git commit -m "feat: describe your change"`
4. Push and open a Pull Request

Please open an issue first for significant changes.

---

## License

MIT — see [LICENSE](./LICENSE)

---

## Author

**Mohamed Said**  
[msaied.com](https://msaied.com) · [GitHub @EG-Mohamed](https://github.com/EG-Mohamed)

---

*Runs entirely client-side. No data leaves your browser.*