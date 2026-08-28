# Website Security Checklist — For Every Project
## First, figure out what actually applies — feature by feature, not the whole site
Don't label a whole project "static, so none of this applies" or "dynamic, so all of it applies." Most sites are almost entirely static with one or two interactive pieces bolted on — a contact form, a booking widget, a comment box. Go through every interactive piece and ask, for each one on its own:

1. Does it accept typed input from a visitor?
2. Does that input get stored somewhere, or passed to a database?
3. Does it get shown back to other visitors later (like a comment or review)?

- **Stored in a database you run** → the full backend tier below applies to that one feature specifically, even if every other page on the site is plain HTML.
- **Handled by a third-party service** (Formspree, Web3Forms, an embedded booking widget) → you never touch a database, so that risk belongs to them, not you — they've already secured it. Your job is just picking a reputable one.
- **Never stored anywhere** (a WhatsApp click-to-chat link) → nothing to inject into, nothing extra needed.

Same logic applies to every item below — check it against the actual feature, not the site's general label.

## The good news first
Most of your current work — static shop sites on GitHub Pages/Netlify, no database, no login — already avoids the majority of what's on this list. No server to break into means most classic attacks don't even apply. Keep reading anyway, because a few things still matter even on the simplest site, and this list scales up for when you take on bigger projects.

## Do this on every single project, even the simplest static site
- **Force HTTPS.** Free and automatic on GitHub Pages, Netlify, and Cloudflare Pages — just make sure HTTP redirects to HTTPS, don't leave both open.
- **Never put API keys, passwords, or secrets in front-end JavaScript.** Anything in client-side code is visible to anyone who right-clicks → View Source. No exceptions.
- **Add basic security headers**: `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy: strict-origin-when-cross-origin`. Netlify and Cloudflare Pages let you set these with a simple config file in the repo.
- **Set a Content Security Policy (CSP).** Restricts what sources scripts/styles/images are allowed to load from — blocks a lot of injected-script attacks even if something slips past you.
- **If loading any script from a CDN**, use Subresource Integrity (SRI) — a hash tag that stops a tampered CDN file from silently running on your site.
- **For contact forms**, use Formspree or Web3Forms instead of building your own backend — they include spam protection (honeypot fields) you'd otherwise have to build yourself.
- **Turn on GitHub Dependabot** (free, one click in repo settings) — auto-alerts you if any JS package you're using has a known vulnerability.

## If a project ever needs a backend, database, login, or payments
This is where real risk starts. Custom software jobs, anything with a login, or e-commerce all land here.
- **Validate and sanitize every input on the server**, not just in the browser. Client-side validation is for user experience — it can always be bypassed, so it's not security.
- **Use parameterized queries / prepared statements** for any database call. Never build SQL by pasting user input into a string — that's what SQL injection actually is.
- **Encode output properly** before displaying anything a user typed back on the page — this is what stops XSS (cross-site scripting).
- **Hash passwords with bcrypt or argon2.** Never plaintext, never MD5/SHA1 alone — both are broken.
- **Secure your cookies**: `HttpOnly` (JS can't read it), `Secure` (HTTPS only), `SameSite=Strict` or `Lax` (blocks a lot of CSRF).
- **Rate-limit logins and any public API endpoint** — stops brute-force guessing and abuse.
- **Add CSRF tokens to every state-changing form** — SameSite cookies help, but a real token is the actual defense against another site tricking a logged-in visitor's browser into firing a request on yours without them meaning to.
- **Check permissions again on the server for every request** — hiding a button in the UI is not access control. Someone can still call that endpoint directly.
- **For file uploads**: check the real file type server-side (not just the extension), cap the size, store outside the public web folder, never let an uploaded file execute as code.
- **Never commit `.env` files or credentials to GitHub.** Add them to `.gitignore` from your very first commit — use environment variables on the host instead.
- **Give every API key/database user the minimum access it needs** — a key that only reads data shouldn't also be able to delete it.
- **For payments**: never build your own processing. Use PayHere (Sri Lanka) or Stripe — they carry the compliance burden, and card details should never touch your own server at all.
- **If you store real customer data yourself** (names, phone numbers, addresses) rather than passing it through a third party like Formspree — that's Sri Lanka's Personal Data Protection Act territory, not just a technical question.

## If you ever build your own API
- Authenticate every single request — API keys or tokens, no open endpoints
- Rate-limit per key/user, not just per IP address
- Never leak stack traces or internal error details in API responses — attackers read your own error messages to map out how your system works
- Version it, so you can retire an old/vulnerable version safely instead of breaking everyone at once

## Business logic flaws — the category no checklist fully covers
This is specific to what your app actually does, not a generic vulnerability class. Think attacker-style about your own feature: Can a discount code be applied unlimited times? Can two people book the exact same slot by submitting at the same instant? Does checkout trust a price sent from the browser instead of checking it again on the server? These only surface when you deliberately ask "how would I abuse this if I were trying to" about the specific thing you built.

## The part that isn't code: social engineering
Most real breaches start with a trick, not an exploit — a fake "GitHub support" email asking you to "verify" your password, or a "client" urgently asking you to run a file or click a link. Verify anything unusual through a second channel (call, don't just reply), and never enter credentials via a link in an email — type the URL yourself.

## If something gets through anyway
- Keep backups you've actually test-restored, not just backups you assume work
- Know how to rotate a leaked key or password fast, before anything else
- Have the client's contact info ready so you can tell them immediately — being slow or hiding it is what actually costs trust, not the incident itself

## Specific to vibe coding — what to double-check before shipping
- **Read every line that touches user input, a database, a file, or a key.** Don't paste and run blind.
- **Ask the AI to review its own output**: "check this for SQL injection, XSS, and exposed secrets" as a second pass. It's often genuinely good at catching this on a second look, even when it missed it the first time.
- **Watch for scope creep.** If you asked for a contact form and it scaffolds a login system or admin panel you never asked for, strip it back out — that's attack surface you don't need.
- **"It worked when I tested it" isn't proof it's secure.** Working and secure are two different questions.

## Free tools to actually test your work
- **securityheaders.com** — paste your URL, get a free grade on your headers
- **observatory.mozilla.org** — similar, more detail
- **ssllabs.com/ssltest** — grades your HTTPS/TLS setup
- **Chrome DevTools → Lighthouse tab** — built into every Chrome browser, flags several issues for free
- **GitHub Dependabot** — already mentioned above, but worth repeating: turn it on for every repo

## Operational security — protecting yourself, not just the client
- **Turn on 2FA everywhere** — GitHub, Netlify/your host, your domain registrar. If someone compromises your account, they can push malicious changes to every client site you manage at once, not just one.
- **Use a password manager, unique password per account.** No reused passwords across your own tools.
- **If a shop wants their own login to edit content**, give them a scoped account for that only — not your full deploy access.
- **Keep a simple record of what's deployed where**, so a rollback is fast if you ever need one.

---

This is genuinely the foundation your degree's later Network Security and Cybersecurity modules will go deeper on. You're getting a real head start by actually practicing it on live projects now, which sticks a lot better than reading about it later without anything real to apply it to.
