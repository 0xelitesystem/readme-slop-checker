# readme-slop-checker

Audit a GitHub README for AI-generated cliche patterns. Single HTML file. BYOK Claude API.

**Live demo:** https://0xelitesystem.github.io/readme-slop-checker/

## What it does

Paste a public GitHub repo (`owner/repo` or a github.com URL) and an Anthropic API key. The tool fetches the repo's default README, runs the [anti-slop-audit](https://github.com/0xelitesystem/prompt-templates/blob/main/prompts/anti-slop-audit.md) prompt against it via the Claude API, and returns a structured report:

- Verdict: pass or fail
- Severity counts: blocker, major, minor, nit
- Per-finding: category, exact quote, issue, suggested rewrite

## Use it

The hosted version runs at `https://0xelitesystem.github.io/readme-slop-checker/` once GitHub Pages is enabled (see below).

Or open `index.html` directly in any modern browser. No build step. No server required.

## Live deployment via GitHub Pages

After pushing the repo:

1. Go to **Settings**, **Pages**.
2. Under "Build and deployment", set **Source** to `Deploy from a branch`.
3. Pick branch `main`, folder `/ (root)`, click **Save**.
4. Wait roughly 60 seconds. The URL appears at the top of the same page.

That URL is your public, free-to-host slop checker.

## How keys are handled

- API key stays in the browser tab only. It is held in a JS variable for the duration of the request.
- It is not stored in localStorage, sessionStorage, cookies, or anywhere else.
- It is sent only to `https://api.anthropic.com/v1/messages` over HTTPS.
- Closing or refreshing the tab discards the key. Paste it again on next use.

If you fork this and host it, the same applies: keys never leave the browser except to Anthropic.

## Why direct browser access works

The tool sets the `anthropic-dangerous-direct-browser-access: true` header on requests to the Anthropic API. This is the documented way to call the API directly from a browser. CORS is supported when this header is present.

For high-volume use, consider proxying through your own backend so users do not need their own API keys. This tool does not provide that path.

## Tech

- Single HTML file (~500 lines including styles and JS)
- No frameworks, no dependencies, no build step
- Vanilla JS, fetch API, base64 decode for GitHub README content
- Tested in current Chrome, Firefox, Safari

## Costs

Each audit is one Claude API call. Most READMEs cost a fraction of a cent on Sonnet 4.6. Long READMEs (50KB plus) may cost a few cents. You see the cost in your Anthropic console.

## Limitations

- Public GitHub repos only. Private repos require a GitHub token, which this tool intentionally does not handle.
- One README per request. Branch and path overrides are not exposed in the UI.
- The audit is opinionated. The banned-vocabulary list reflects the maintainer's style. Fork and edit `ANTI_SLOP_PROMPT` in `index.html` to change it.

## Customizing

Two things you might want to change:

1. **The banned-vocabulary list.** Open `index.html`, search for `ANTI_SLOP_PROMPT`, edit the list inside the prompt string.
2. **The visual style.** All styles are in the `<style>` block at the top of `index.html`. Color tokens are defined as CSS variables in `:root`.

## Contributing

Pull requests welcome. Keep the constraint: one HTML file, zero dependencies. If you want to add features that need a build step or a backend, fork into a sibling repo.

## License

MIT. See [LICENSE](LICENSE).
