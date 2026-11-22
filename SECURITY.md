# 🔐 Security policy

## Reporting a vulnerability

Please report security issues **privately**. Use GitHub's **Security → Report a vulnerability** (private advisory) on this repository, or contact the maintainer through their GitHub profile. Don't open a public issue for a vulnerability.

Please include:

- what is affected (page, function, endpoint)
- steps to reproduce
- the impact you expect

You should get an acknowledgement within a few days.

## Supported versions

Only the latest commit on `main` is supported.

---

## Known security considerations

### API keys in client code

`js/app.js` contains an **OpenRouteService API key** in `ORS_CONFIG.apiKey`. Anything shipped to a browser or packed into an APK is public: anyone can view the page source and reuse the key, and use up its quota.

**Recommended fix:**

1. **Rotate** the current key in the ORS dashboard.
2. Route requests through a tiny proxy (for example a Cloudflare Worker, Vercel function or Netlify function). The proxy holds the key as a secret and forwards only `POST /v2/directions/driving-car/geojson`.
3. Point `ORS_CONFIG.base` at the proxy and remove `apiKey` from the client.
4. Optionally, rate-limit by IP and restrict CORS to your own origin.

### SMS alerts

`sendSmsAlert()` is a **simulation**: it logs to the console and updates the status text. A real SMS provider (Twilio, MSG91, Fast2SMS) needs a secret key that must **never** be in client code. Add a server-side relay (for example `POST /send-sms`) that holds the key, validates the number, and rate-limits requests.

Phone numbers are stored only in the user's own `localStorage` (`climo_sms_alert`). They are never sent anywhere.

### User-generated content

*Local feedback* posts are kept only in the current page. Before display, only `<` is escaped; switching that handler to `escapeHTML()` would be more robust. If posts are ever saved or shared:

- validate them and store them as plain text on the server
- escape them on render (use `escapeHTML()`)
- add moderation and rate-limiting

### Third-party services

Every API CLIMO uses is called directly from the browser, so the user's IP address is visible to those services:

- Open-Meteo
- Nominatim
- OpenRouteService
- jsDelivr
- tile servers

Follow each provider's usage policy, especially [Nominatim](https://operations.osmfoundation.org/policies/nominatim/), which allows at most 1 request per second and requires a valid referer.

### Android build

CI produces a **debug** APK. For distribution, sign release builds with a keystore stored in GitHub Actions secrets (see [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md#release-build-signed)).
