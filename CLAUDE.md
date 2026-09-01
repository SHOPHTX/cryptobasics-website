# CryptoBasics Website — Project Context

Static site for cryptobasics.co, a plain-English crypto education brand founded by Kenzo (David Tran). Hosted via GitHub Pages from `docs/` in this repo (SHOPHTX/cryptobasics-website). Courses sold via Gumroad (seller: cryptobasicsco).

## Critical operating constraints

- **This sandbox cannot push to GitHub.** No credentials are configured. All commits from Claude are local-only (`git add && git commit`, never `git push`). Kenzo must push from his own machine. Always mention the local-commit backlog when relevant (currently several commits queued — check `git log` / `git rev-list --count origin/main..HEAD`).
- **Gumroad price/listing changes require the Claude in Chrome extension**, which has repeatedly disconnected mid-session. If `list_connected_browsers` returns empty, walk Kenzo through: confirm extension is toggled on in `chrome://extensions`, check for the "Claude debugging" banner on the page (its absence means the handshake never completed), and as a last resort have him fully remove/reinstall the extension — that's what fixed it previously.
- **Brand sourcing bar (non-negotiable):** news cards and regulation-tracker updates must be confirmed fact, not opinion/prediction/rumor. Require at least one credible primary or major secondary source; prefer 2+ independent sources. This directly reflects the site's "no fake news, no hopium, we show our work" value — don't relax it even if asked to move faster.
- **Watch for duplicate `<style>`/`<script>` tag bugs.** Earlier page-assembly work (via python string concatenation) introduced duplicate opening/closing tags on `docs/news/index.html` twice already (both fixed). When building new pages by copy/pasting blocks, double check open/close tag counts before committing (`grep -c '<style>' file` should equal `grep -c '</style>' file`, same for `<script>`).

## Current site structure

- `docs/index.html` — homepage. Hero → free-Vol.1 banner → "Letter from the Founder" section (photo + short trust-forward note + "Read My Story" CTA to /about/) → product bundle cards → trimmed news section (one featured "⭐ Top Story" card + 4 compact "More Headlines" teasers + "See All News →" link) → contact/IG. CTA row links to Guides, Crypto News, Jargon Buster, Regulation Tracker, Our Story.
- `docs/news/index.html` — full news archive. Live crypto price ticker (BTC/ETH/etc., CoinGecko-backed) at top, followed by a distinct purple "⚠️ Unconfirmed" rumor ticker (currently: Clarity Act vote timing, Japan carry-trade unwind theory) — this rumor ticker is manually curated only, never auto-updated. Below that, the full filterable `.nc` card list, newest first. Nav links back to homepage and to the Regulation Tracker.
- `docs/regulation-tracker/index.html` — new page (added this session). Plain-English scoreboard of which countries have passed real crypto regulation. Opens with a "US Spotlight" callout (GENIUS Act done for stablecoins / CLARITY Act stalled in Senate), then a status-pill list (✅ Full / 🟡 Partial / ❌ Banned) grouped by region (Americas, Europe, Asia-Pacific, Middle East & Central Asia). Sourced to official regulators + major outlets. Linked from homepage CTA row and news page nav.
- `docs/about/index.html` — "Our Story." Currently opens straight into credentials (Army Veteran / Business graduate / Entrepreneur). **In-progress redesign** (see Trust positioning below) will open instead with a trust-question hook paired with a deadpan Buck-the-Coin photo, before the credentials land. Not yet implemented — still waiting on the photo file from Kenzo.
- `docs/_archive/homepage-classic-2026-09-01.html` — full backup copy of the homepage exactly as it was before the floating-hub redesign work began. Git tag `homepage-v1-before-hub-redesign` marks the same point in commit history if a full revert is ever needed.
- `docs/_prototypes/` — homepage redesign concept demos (not live pages, not linked from anywhere, safe to ignore for site functionality; see "Homepage redesign" section below for what they are and current feedback status).
- Individual volume pages + bundle products live on Gumroad, not in this repo.

## News card format (must match exactly)

Each card is a `.nc` div with `data-category` — regulation=yellow `#FFD600`, adoption=green `#4ADE80`, hotgoss=pink `#FF2D78`, markets=blue `#3B82F6`. Collapsed trigger shows date + category pill + headline; expanded body has "🤔 WTF Does This Mean?" (zero-jargon explanation) and "💡 Why It Matters," plus a "📎 Sources:" row with real links. Tone: witty, never hypey, no price predictions, explains as if to someone who's never touched crypto.

New cards go at the top of `docs/news/index.html`'s card list. If a new card is more recent than the homepage's current Top Story, promote it and bump the old Top Story into the "More Headlines" teaser list (max 4 teasers, drop oldest).

## Gumroad state (as of last check)

- Vol. 1 is **$0+ (pay-what-you-want, free)** — intentional, drives homepage/about-page CTAs.
- Vol. 2–9 individually: $21.99 each.
- Bundles: Start Here (Vol 1-3) **$39** (discount math adjusted for free Vol.1), Go Deeper (Vol 4-6) $49, See the Whole Picture (Vol 7-9) $49, Full Course Series (Vol 1-9) $99.

## Brand direction: Buck the Coin

Going all-in on Buck (the yellow coin mascot) for brand recognition. Current thinking, not yet fully implemented on the actual About/homepage:

- **Trust positioning:** lead with the "why should you trust me" objection head-on rather than avoiding it, paired with the free Vol.1 offer as the proof ("go decide for yourself before you spend anything"). Chosen copy direction: *"Why should you trust a guy and his cartoon coin? You shouldn't — blindly. That's why Volume 1 is free."* Keep the copy flat/deadpan — the humor should come from the photo, not from the text also trying to be funny (stacking two jokes reads as corny).
- **Photo direction:** prefer the eye-level candid Buck+Kenzo photo (staring each other down on a desk) over the posed studio shot — deadpan/caught-off-guard reads as more confident and less try-hard.
- **CTA priority:** the founder/trust section's primary button should link straight to the free Vol. 1 Gumroad page (immediate action), not just to the About page — don't make people click twice to get to the payoff.
- **"Meet Buck" vs. "Our Story":** keep these separate. Our Story stays focused on Kenzo's credibility; a distinct, short "Meet Buck" block (personality/catchphrases) shouldn't be merged into it, since mixing dilutes both.
- **Still pending from Kenzo:** the actual founder+Buck photo file needs to be supplied before the homepage/About page trust-section updates can be implemented.

## Homepage redesign (in progress, not yet built into the live site)

Kenzo feels the homepage has gotten visually busy/cluttered as content accumulated (bundles, founder letter, news teasers, ticker, widgets all stacked). Direction agreed: turn the homepage into a calm "hub" — visitors land, quickly indicate what they're after, and get routed to a dedicated page — rather than scrolling through everything every time.

**Decisions locked in:**
- Homepage gets fully replaced (not a new screen bolted in front of the old one). Bundle pitch moves to its own `/courses/` page (not yet created); founder letter either trims into the hub or folds into `/about/`; News, Jargon Buster, Regulation Tracker, About stay where they are, just reached via the hub instead of a scroll.
- Backup taken before any real changes: `docs/_archive/homepage-classic-2026-09-01.html` + git tag `homepage-v1-before-hub-redesign`. Safe to fully revert at any time.

**Concept iteration so far (two throwaway prototypes built for feedback, saved at `docs/_prototypes/`, NOT linked/live):**
1. `hub-prototype-v1-glow.html` — ambient ombré glow blobs, soft fade-in qualifier question ("what brings you here today?") transitioning into floating destination cards. **Kenzo's feedback: "looks generic and kind of boring."** Diagnosis: this is the generic AI/SaaS dark-mode-glow-blob template everyone uses now — technically polished but no brand personality, doesn't achieve the "wow" he wants.
2. `hub-prototype-v2-arcade.html` — pivoted to an **arcade / game-show energy** direction per Kenzo's explicit pick (over "trading-floor chaos" and "Buck-centric illustrated" alternatives also pitched). Spinning-coin "insert coin" title screen → chunky pressable Start button (hard offset drop-shadows, not soft blur) → "choose your path" qualifier tiles → "Level Select" screen where each destination (Courses, News, Jargon Buster, Regulation Tracker, Our Story) is a numbered level card that 3D-flips on hover to reveal description + "Enter →", with the best-fit level for the chosen path badged "BEST FIT" and promoted to front of the grid. Solid deliberate color blocks instead of blurred ambient color, plus scanline + diagonal stripe texture for arcade-cabinet feel.

**Status: awaiting Kenzo's reaction to v2 (the arcade version).** This was just shown to him — no feedback captured yet in this handoff. Next step when resuming: ask what's landing / not landing on v2 specifically (e.g. is the numbered "LVL 1/2/3" framing right, is the coin-flip gimmicky, right energy but needs turning up/down, etc.), iterate the prototype further based on that feedback, and only once a direction is approved should it get built into the real `docs/index.html` (plus the new `/courses/` page for the relocated bundle content). Do not start building the real site pages until Kenzo explicitly approves a direction — this is still in the "throwaway concept, react to it" phase, not implementation.

## X/Twitter policy

No live X API connection available (would require a paid X developer tier + keys Claude shouldn't handle). Workable middle ground: individual tweets can be embedded (X's free `blockquote` + `widgets.js` embed, works fine on a static site) as a primary-source supplement to news cards — but only from verified/official accounts stating a fact, never as a replacement for the independent-source sourcing bar, and never for opinions/predictions.

## Scheduled automation

**`crypto-basics-daily-news`** — runs daily ~4:02 PM local time. Two jobs: (1) scans for verified crypto news (24-48hr window), drafts cards matching the exact site format if something clears the sourcing bar, promotes to Top Story if newer; (2) checks whether any country's regulation status has genuinely changed and updates `docs/regulation-tracker/index.html` accordingly (adding/updating a country entry, or updating the US Spotlight callout if GENIUS/CLARITY Act status changes) — this part only fires on genuine verified changes, most days there's nothing to update here. Commits locally (never pushes), and reports back the running total of unpushed local commits so Kenzo always knows his push backlog. Does not touch the manually-curated Unconfirmed rumor ticker. Skips silently on days with nothing verifiable — never forces a card or a regulation update.

## Known minor issues

- `docs/index.html` has a harmless 1-div count imbalance pre-dating recent changes — not a rendering bug, just noted in case it resurfaces during future edits.
- `docs/news/index.html`'s CSS still has unused `.news-red-ticker`/`.red-ticker-*` rules left over from before it was replaced by the live price ticker — dead code, safe to remove whenever convenient.
