# CryptoBasics Website — Project Context

Static site for cryptobasics.co, a plain-English crypto education brand founded by Kenzo (David Tran). Hosted via GitHub Pages from `docs/` in this repo (SHOPHTX/cryptobasics-website). Courses sold via Gumroad (seller: cryptobasicsco).

## Critical operating constraints

- **This sandbox cannot push to GitHub.** No credentials are configured. All commits from Claude are local-only (`git add && git commit`, never `git push`). Kenzo must push from his own machine. Always mention the local-commit backlog when relevant.
- **Gumroad price/listing changes require the Claude in Chrome extension**, which has repeatedly disconnected mid-session. If `list_connected_browsers` returns empty, walk Kenzo through: confirm extension is toggled on in `chrome://extensions`, check for the "Claude debugging" banner on the page (its absence means the handshake never completed), and as a last resort have him fully remove/reinstall the extension — that's what fixed it previously.
- **Brand sourcing bar (non-negotiable):** news cards must be confirmed fact, not opinion/prediction/rumor. Require at least one credible primary or major secondary source; prefer 2+ independent sources. This directly reflects the site's "no fake news, no hopium, we show our work" value — don't relax it even if asked to move faster.

## Current site structure

- `docs/index.html` — homepage. Hero → free-Vol.1 banner → "Letter from the Founder" section (photo + short trust-forward note + "Read My Story" CTA to /about/) → product bundle cards → trimmed news section (one featured "⭐ Top Story" card + 4 compact "More Headlines" teasers + "See All News →" link) → contact/IG.
- `docs/news/index.html` — full news archive. Live crypto price ticker (BTC/ETH/etc., CoinGecko-backed) at top, followed by a distinct purple "⚠️ Unconfirmed" rumor ticker (currently: Clarity Act vote timing, Japan carry-trade unwind theory) — this rumor ticker is manually curated only, never auto-updated. Below that, the full filterable `.nc` card list, newest first.
- `docs/about/index.html` — "Our Story." Currently opens straight into credentials (Army Veteran / Business graduate / Entrepreneur). **In-progress redesign** (see below) will open instead with a trust-question hook paired with a deadpan Buck-the-Coin photo, before the credentials land.
- Individual volume pages + bundle products live on Gumroad, not in this repo.

## News card format (must match exactly)

Each card is a `.nc` div with `data-category` — regulation=yellow `#FFD600`, adoption=green `#4ADE80`, hotgoss=pink `#FF2D78`, markets=blue `#3B82F6`. Collapsed trigger shows date + category pill + headline; expanded body has "🤔 WTF Does This Mean?" (zero-jargon explanation) and "💡 Why It Matters," plus a "📎 Sources:" row with real links. Tone: witty, never hypey, no price predictions, explains as if to someone who's never touched crypto.

New cards go at the top of `docs/news/index.html`'s card list. If a new card is more recent than the homepage's current Top Story, promote it and bump the old Top Story into the "More Headlines" teaser list (max 4 teasers, drop oldest).

## Gumroad state (as of last check)

- Vol. 1 is **$0+ (pay-what-you-want, free)** — intentional, drives homepage/about-page CTAs.
- Vol. 2–9 individually: $21.99 each.
- Bundles: Start Here (Vol 1-3) **$39** (discount math adjusted for free Vol.1), Go Deeper (Vol 4-6) $49, See the Whole Picture (Vol 7-9) $49, Full Course Series (Vol 1-9) $99.

## Brand direction: Buck the Coin

Going all-in on Buck (the yellow coin mascot) for brand recognition. Current thinking, not yet fully implemented:

- **Trust positioning:** lead with the "why should you trust me" objection head-on rather than avoiding it, paired with the free Vol.1 offer as the proof ("go decide for yourself before you spend anything"). Chosen copy direction: *"Why should you trust a guy and his cartoon coin? You shouldn't — blindly. That's why Volume 1 is free."* Keep the copy flat/deadpan — the humor should come from the photo, not from the text also trying to be funny (stacking two jokes reads as corny).
- **Photo direction:** prefer the eye-level candid Buck+Kenzo photo (staring each other down on a desk) over the posed studio shot — deadpan/caught-off-guard reads as more confident and less try-hard.
- **CTA priority:** the founder/trust section's primary button should link straight to the free Vol. 1 Gumroad page (immediate action), not just to the About page — don't make people click twice to get to the payoff.
- **"Meet Buck" vs. "Our Story":** keep these separate. Our Story stays focused on Kenzo's credibility; a distinct, short "Meet Buck" block (personality/catchphrases) shouldn't be merged into it, since mixing dilutes both.
- **Still pending from Kenzo:** the actual founder+Buck photo file needs to be supplied before the homepage/About page updates can be implemented.

## X/Twitter policy

No live X API connection available (would require a paid X developer tier + keys Claude shouldn't handle). Workable middle ground: individual tweets can be embedded (X's free `blockquote` + `widgets.js` embed, works fine on a static site) as a primary-source supplement to news cards — but only from verified/official accounts stating a fact, never as a replacement for the independent-source sourcing bar, and never for opinions/predictions.

## Scheduled automation

**`crypto-basics-daily-news`** — runs daily ~4:02 PM local time. Scans for verified crypto news (24-48hr window), drafts cards matching the exact site format if something clears the sourcing bar, promotes to Top Story if newer, commits locally (never pushes), and reports back the running total of unpushed local commits so Kenzo always knows his push backlog. Does not touch the manually-curated Unconfirmed rumor ticker. Skips silently on days with nothing verifiable — never forces a card.

## Known minor issues

- `docs/index.html` has a harmless 1-div count imbalance pre-dating this session's changes — not a rendering bug, just noted in case it resurfaces during future edits.
- `docs/news/index.html`'s CSS still has unused `.news-red-ticker`/`.red-ticker-*` rules left over from before it was replaced by the live price ticker — dead code, safe to remove whenever convenient.
