---
name: seek-like-fravia
description: |
  Fravia-style information seeking discipline. Use when a claim, an inference, a contact email/phone, a quote, a regulatory status, or a fact will be load-bearing in any artifact that ships — outbound emails, decks, proposals, vendor outreach, public-facing copy, or any decision the user will act on. Goes beyond a single web search: attacks the question from multiple vocabulary angles, distrusts first results, verifies via primary sources, captures the URL + date + verbatim quote, and reports residual uncertainty honestly.

  TRIGGER when: writing or about to write any claim about a named person, organization, product, contact email, contact phone, partnership program, regulatory status, market datum, eligibility rule, court decision, or competitor capability that the user will act on. Also when the user explicitly invokes "Fravia," "seek," "verify," "double-check," "don't trust the first result," or similar verification language.

  DO NOT TRIGGER when: the user is asking for a quick conversational answer they're not acting on; when the claim is from well-established consensus reference (basic syntax, well-known math); when the user has explicitly told you to skip verification.
---

# Seek Like Fravia

Fravia+ (Francesco Vianello, 1952-2009) taught that finding information on the web is a craft, not a query. The web answers naïve questions naïvely; it rewards skilled seekers. Apply his discipline whenever a claim will ship.

The protocol exists to prevent one specific failure mode: confidently asserting a wrong load-bearing fact in an outbound artifact because the first search result looked authoritative. The cost of that failure is high (broken trust, retracted emails, embarrassing decks); the cost of the protocol is low (15-30 minutes of parallel searches). Run the protocol.

## The seven principles

1. **Don't trust the first result.** First-page web results are SEO-optimized, frequently outdated, often AI-summarized hallucinations of prior pages. The gold is usually on page 2-3, in a specialized vertical, or in the primary source the article was paraphrasing.

2. **Cheapest probe first.** Between two ways to get the same evidence, pick the one that costs less context, time, and money. `ls` before `cat`. A targeted `site:` dork before a broad search. A single primary-source URL before a five-source synthesis.

3. **Vocab-shift.** If "carrier partnership program" returns nothing useful, try "affiliate program," "training partner," "official partner," "channel partner," "instructor partner," "reseller program," "ambassador program." Each industry has its own jargon for the same concept. The right keyword unlocks the whole shelf.

4. **Negative space.** Sometimes the target itself is opaque. Search what's AROUND it. Competitor announcements about the target. Regulator filings referencing it. Job postings that name the team. Press releases from partners. The shape of the absence tells you what's there.

5. **Primary source or it didn't happen.** A blog post saying "X said Y" is not evidence of X saying Y. Find the original quote, the original document, the original filing. If it's gone, archive.org / Wayback. If even that's gone, mark the claim as unverifiable — do not ship it.

6. **Capture verbatim, with URL and date.** Paraphrase loses information. Quote the exact words; record the URL; record the publication date (and your fetch date, if different). The future you (or the auditor) needs this to re-verify.

7. **Recognize when you're being lied to.** SEO content farms, AI-generated summaries of nothing, propaganda dressed as analysis, stale facts presented as current, "expert" sites that are affiliate fronts. Cross-check anything load-bearing against a second independent source before committing.

## The tactical playbook

Run these layers in order. Stop when you have enough evidence — do not run them all if the answer is already verified at a lower layer.

### Layer 1: The primary source attempt

- Go directly to the entity's own website. About / Leadership / Press / Partners / FAQ pages.
- If a person — their LinkedIn, their employer's staff page, their published bio, their conference speaker page.
- If a regulatory fact — the regulator's own filing or rule text. SEC EDGAR, state Secretary of State, federal/state agency websites, court opinion PDFs.
- If a court decision — the court's own opinion text, not the news article about it.
- If a press release — the original on the company's newsroom, not the syndicated wire republication.

### Layer 2: The targeted dork

- `site:<domain>` to constrain to a specific authoritative source.
- `intext:"<exact phrase>"` to find verbatim usage.
- `filetype:pdf` to find official documents, not blog summaries.
- `inurl:<keyword>` to find specific page types (e.g., `inurl:partner`, `inurl:press`, `inurl:investors`).
- Quoted exact phrases for unique strings; minus operators to exclude noise.
- Date-bounded queries when recency matters (Google's `tbs=qdr:y` for past year, etc.).

### Layer 3: The vocab shift

- Generate 3-5 synonymous queries using different industry vocabulary.
- Try the formal, the informal, and the jargon variants.
- Try the opposite framing (search for what would be there if the claim were false — "X discontinued," "X shutdown," "X acquired by," "X bankruptcy").
- Try the adjacent-field framing — if it's an insurance question, also search the regulator-side terminology; if it's a technology question, also search the legal/compliance terminology.

### Layer 4: The cross-source confirmation

- A claim with one source is a rumor. A claim with two independent sources is evidence.
- "Independent" means not citing each other and not derived from the same press release. Two news outlets running the same wire story is one source, not two.
- For a person/role claim, two independent sources should ideally be: the entity's own page + a third-party page (LinkedIn, news article, regulatory filing).

### Layer 5: The archive fallback

- If the page is gone or has changed, check `https://web.archive.org/web/*/<URL>` for prior snapshots.
- For company status / financial / org claims, archive.org snapshots from the relevant time period are often more authoritative than today's marketing site.
- Save the snapshot URL as the cite, not just the live URL.

### Layer 6: The negative-space probe

- What does the target's competitor say about them?
- What does the regulator that oversees them say?
- What does a partner announcement say (often reveals structural details the target wouldn't volunteer)?
- What do former employees say (LinkedIn, Glassdoor, Reddit, industry forums)?
- What's missing from their own site (no "About" page = small/early; no "Press" page = no recent news; no "Partners" page = no partnerships)?

### Layer 7: The honest report

Distinguish:
- **Verified** — primary source confirmed, URL + date + verbatim quote captured.
- **Inferred** — multiple secondary sources align, no primary source surfaced.
- **Assumed** — single source, or industry convention, or reasonable extrapolation.
- **Unknown** — no source surfaced; cannot be claimed.

Quote verbatim, cite URL + date, mark each load-bearing claim with its level.

## Output format

When the seeking is in service of a downstream artifact, produce a verification table:

| Claim | Status | Source | Quote | Notes |
|-------|--------|--------|-------|-------|
| <exact statement> | Verified / Inferred / Assumed / Unknown | URL + date | "verbatim" | residual uncertainty |

Or as a narrative if a table doesn't fit, but cover the same four columns of information per claim.

## When the result is "I can't verify"

This is a valid outcome. Three honest moves:

1. **Omit the claim** from the downstream artifact.
2. **Replace with a placeholder** and flag for the user to fill in or strike before shipping.
3. **Ask the user** if the answer matters and there's no way for you to get it.

NEVER ship an unverified load-bearing claim. The Authoritative Source Verification Gate (in global CLAUDE.md) forbids it for person/org factual claims in outbound artifacts; the Fravia discipline extends the same logic to any inference the user will act on.

## Anti-patterns

- **Stopping at the first plausible answer.** If the first result *looks* right, the Fravia discipline asks: have I tested whether it's right, or have I tested whether it sounds right? These are different tests.
- **Paraphrasing into the artifact.** If you can't quote it verbatim with a URL, you don't actually know it.
- **Treating "no results" as "not there."** "No results for query X" means query X didn't surface it. Try query Y, Z, W before concluding.
- **Asserting recency without a date stamp.** "Current" / "latest" / "as of this writing" are weasel words unless backed by a date.
- **Generated-by-AI-about-AI loops.** When a search result is itself an AI summary of a prior AI summary, the chain has no primary source. Walk back until you hit a human-authored or first-party document.

## Worked example: MHI insurance carrier outreach verification (2026-05-21)

A prior session generated a Tier-1 carrier outreach list with contact emails and program details. Initial draft included `partnerships@usconcealedcarry.com` (USCCA), `partnerships@uslawshield.com`, and a Lockton-Affinity-as-NRA-Sportsman channel. User invoked the Fravia protocol: "seek like fravia to verify."

Sequence run (parallel where possible):

- **Layer 2 dork:** `site:usconcealedcarry.com partnership OR affiliate OR "training partner"` → surfaced the actual USCCA Official Partner Program page and real contact: `officialpartners@uscca.com` / (888) 617-5384. The guessed-by-convention `partnerships@usconcealedcarry.com` was wrong.
- **Layer 1 primary:** US LawShield Contact Us page + LeadIQ → confirmed TX HQ at 1020 Bay Area Blvd, Houston, founded 2009. Verified WY coverage via `gunlaws.uslawshield.com/state/wyoming/`.
- **Layer 6 negative-space:** "Lockton Affinity NRA sportsman" search surfaced the 2018 NY DFS $7M fine and 2019 NJ DOBI $1M fine for the NRA Carry Guard program → that channel was stale (Carry Guard dead). Pivoted to Lockton Affinity Outdoor (alive, $300/yr instructor product, NSSF + CMP partnerships).
- **Layer 4 cross-source:** ACLDN acquisition by CCW Safe confirmed by three independent sources (Women's Outdoor News, Murray Road Agency, Concealed Carry Inc) → ACLDN dropped from second-wave outreach list because it's no longer independent.
- **Layer 1 primary on Mountain West Farm Bureau:** surfaced the September 2025 acquisition by Idaho Farm Bureau Insurance Holding Company → outreach plan re-timed; consider pitching the 5-state combined entity at the holding-company level instead.
- **Coalition wedge re-check:** original pitch assumed Coalition didn't sell training; their own site shows they have Security Awareness Training (partnered with Curricula). Pitch had to shift from "we provide training" to "we provide proctored certification, more rigorous than your video-based awareness program."

Six load-bearing claims corrected before any of them shipped to a carrier email. Cost: ~15 minutes of parallel WebSearches. Value: avoided cold-emailing wrong addresses; avoided pitching a dead product channel (NRA Carry Guard) to a fined administrator; avoided wasting first contact with a carrier mid-merger; reframed Coalition pitch to a true wedge.

This is what Fravia-style seeking buys: not perfection, but the absence of stupid avoidable errors in the artifact that ships.

## Relationship to adjacent skills

- **search-first** (global rules gate): triggers before *implementation* to check whether an existing library/tool/skill solves the need. Fravia triggers before *assertion* to check whether a claim is verified. Both gate by "search before commit," at different commit moments.
- **verification-loop**: verifies completed work product (tests, builds, runtime behavior). Fravia verifies pre-action information. Use both; they cover different lifecycle stages.
- **deep-research**, **market-research**, **exa-search**: heavier multi-source synthesis skills for open-ended research questions. Fravia is the lightweight per-claim verification habit that should be woven into all of them.

## When to invoke explicitly

You don't need a separate invocation. The Fravia discipline is meant to be applied inline during normal work, the way you would naturally double-check a contact email before clicking send. Explicit invocation is appropriate when:

- The user says "verify," "double-check," "seek," "Fravia," "are you sure," "is that real."
- You catch yourself about to assert a load-bearing fact you don't have a source for.
- You are about to ship an outbound artifact (email, deck, proposal) and have unverified claims in it.
- A prior turn made an inference and you are about to build on it.
