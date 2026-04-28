# Clyde Reply — site shipped 28 Apr 2026

LIVE NOW: https://clydereply.co.uk

What works end-to-end:

  https://clydereply.co.uk         200 OK, GitHub Pages, valid LE cert (expires 2026-07-27, auto-renew)
  https://www.clydereply.co.uk     200 OK, same cert
  https://clydereply.com           301 -> https://clydereply.co.uk (Porkbun URL forwarder)
  https://clyde-reply.co.uk        301 -> https://clydereply.co.uk
  https://clyde-reply.com          301 -> https://clydereply.co.uk

Dual Readership Path passes (run the headlines top to bottom — coherent
story: "Be the showroom they drive to → here's what's wrong → here's what
right looks like → 14 days does it → we're new but risk-reversed → here
are your questions → three slots, send the name").

What was created today:

  ./index.html                                                    page (single file, no build step)
  ./fonts/Geist-Variable.woff2                                    self-hosted font (68KB)
  ./DESIGN.md                                                     design system spec
  ./README.md                                                     dev quickstart
  ./CNAME                                                         GitHub Pages custom domain pointer
  /Users/foxy/jordan-platten-study/lp/clydereply-home-v2-uk.md    the LP copy spec
  /Users/foxy/clyde-reply/dns-snapshots/snapshot-*.json           DNS rollback snapshot

Repo: https://github.com/fraser-svg/clyde-reply-site

------------------------------------------------------------------------
WHAT NEEDS YOU (in priority order)
------------------------------------------------------------------------

1. ROTATE PORKBUN API KEYS (5 min) — these were pasted in a chat transcript:
   - https://porkbun.com/account/api → revoke pk1_ce50…2a9a3 → generate new pair
   - Save new pair to your password manager. Don't paste in chat again — give
     me the new one only when we need to script Porkbun changes.

2. SET UP EMAIL FORWARDING for hello@clydereply.co.uk -> fraserklw@gmail.com (5 min):
   - https://improvmx.com → sign in with fraserklw@ Google OAuth
   - Add domain "clydereply.co.uk", alias "hello", forward to fraserklw@gmail.com
   - Their setup wizard will ask you to add 2 MX records + 1 TXT to clydereply.co.uk
     — copy them, give them to me, I'll add them via Porkbun API in 30 seconds
   - OR migrate hosting to Cloudflare Pages later (next priority) and use
     Cloudflare Email Routing instead — same outcome, fewer third parties
   - Until this is done, the "Send the showroom name" mailto button on the LP
     opens an email client with the right To: address, but anyone replying
     will get a bounce. NOT critical today (no cold campaigns running) but
     critical before you send any cold email containing the LP link.

3. ADD CALENDLY URL for the "Or book a 20-minute call" secondary CTA:
   - https://calendly.com → free tier, set up a 20-min "Showroom audit chat"
     event (your fraserklw@ Google Calendar)
   - Send me the public URL (e.g. https://calendly.com/fraserklw/audit-chat)
   - I'll swap the placeholder "#book" anchor for the real Calendly URL
     (one-line edit, push, ~30 seconds)

4. TICK NOMINET INDIVIDUAL OPT-OUT for clydereply.co.uk + clyde-reply.co.uk
   if you registered those as an individual (not a business):
   - Porkbun → domain settings → "Nominet privacy / opt-out"
   - This hides your home address from the public WHOIS for the .co.uk domains
   - .com domains already have WHOIS privacy via Porkbun

------------------------------------------------------------------------
WHAT WE DECIDED ON YOUR BEHALF (override any time)
------------------------------------------------------------------------

a. Primary domain = clydereply.co.uk (the others 301 to it). UK trust signal beats .com for KBB.
b. UK-wide market, not Glasgow-only. H1 reframed from "in Glasgow" to "the showroom that the serious buyers still drive to" — works for any UK independent.
c. Loom-and-Zoom for everyone. The FAQ now says "fully remote — Greater Glasgow showrooms can have in-person kickoff if they want it" rather than promising travel.
d. Phone number OMITTED from page until you wire a routed number. Placeholder 07xxx xxxxxx smelt of rat (soup).
e. Public name "Fraser" everywhere, not "Foxy". You corrected me on this; it's locked in memory.
f. Hosted on GitHub Pages (free, fast, gh CLI ready) instead of Cloudflare Pages (would have needed your browser auth). Recommend migrating to Cloudflare Pages later — better for the audit-form endpoint we'll need, and gives us free email routing.
g. Site is one file (index.html) with inline CSS. No build step. Anyone can edit without npm.

------------------------------------------------------------------------
WHAT'S OUTDATED ELSEWHERE NOW
------------------------------------------------------------------------

The UK-wide pivot creates "Connective Tissue debt" that needs reconciling
before any non-Glasgow prospect is contacted:

- /Users/foxy/jordan-platten-study/customer/cold-email-copy-v1.md
  → still says "Foxy" in signatures, references "this part of Glasgow"
  → needs UK-wide rewrite (next /soup:copy run)
  → soft version bump email body okay, just swap signatures

- /Users/foxy/jordan-platten-study/lp/clydereply-home-v1.md
  → superseded by v2-uk.md but kept for history; do not edit

- /Users/foxy/jordan-platten-study/customer/loom-script.md (TBD)
  → when written, use "Fraser from Clyde Reply, just over in Lenzie" as
    the founder-locality cue; drop prospect-locality entirely

- /Users/foxy/jordan-platten-study/customer/avatar.md
  → Iain is fine as the *primary* archetype, but cold-outreach to
    Newcastle/Manchester/etc will need parallel avatars (Iain has a
    Yorkshire/North-West/Bristol equivalent). Defer until first 30
    Glasgow-area sends prove the offer; then expand.

------------------------------------------------------------------------
DEPLOYING CHANGES
------------------------------------------------------------------------

  cd /Users/foxy/clyde-reply-site
  # edit index.html or DESIGN.md
  git add -A && git commit -m "..." && git push origin main
  # GitHub Pages rebuilds in ~30s, live in ~60s

To preview locally before pushing:
  python3 -m http.server 8765
  open http://localhost:8765

------------------------------------------------------------------------
NEXT-UP CO-FOUNDER QUEUE (when you're ready)
------------------------------------------------------------------------

- The audit form endpoint (someone fills "showroom name" on the LP, we get
  a row in mission control). Currently mailto only. Build when first
  prospect reply lands.
- Cold-email copy v2 (UK-wide, Fraser-signed). Run /soup:copy once cold
  campaign actually starts.
- Companies House registration. £12, 24-hour turnaround. Get a real number
  in the footer.
- ICO registration. £40-60/yr depending on size. Ditto.
- Real hero image once client 1 closes.
- Actual loom recording for the prospect-audit kit (separate workstream
  in /Users/foxy/jordan-platten-study/scripts/audit_kbb.py).
