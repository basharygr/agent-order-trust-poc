# 07 — Pivot 2, the story: an agent serving layer for merchants

**Date:** 4 November 2026
**Status:** The narrative for pivot 2. Docs 05 and 06 are the evidence behind it; this doc is the one to read first. Not a decision record. Section 9 lists options that have not been discussed and bind nothing.

**Terminology.** Commerce, payments, protocol and measurement terms are defined in [../GLOSSARY.md](../GLOSSARY.md).

**How to read the numbers.** Every figure in the body is there because a conclusion rests on it. Appendix A lists each one with its source and how much to trust it. Three traffic vendors see only their own networks and disagree with each other by up to ten times on volume, so treat every traffic and order figure as directional unless it comes from a filing or an earnings call.

---

## 0. The thesis

Merchants are losing control of how their products are found and bought, because the reading is now done by AI agents whose rules are written by Google, OpenAI, Meta, Stripe, PayPal and Shopify, with no merchant in the room. Their platforms cover part of this, a different part on each platform, and nothing on the long tail. We build the layer that answers for the merchant whoever shows up: one record of what the merchant sells and on what conditions, served to every agent over every access pattern, with the merchant's rules enforced at the order and the proof kept afterwards.

---

## 1. What is changing for merchants

Discovery is moving to agents much faster than buying is.

| What | Where it was | Where it is | Source |
|---|---|---|---|
| Share of US retail site visits referred by AI | 2.9% (Q1 2026) | 5.5% (Oct 2026) | FirstPageSage panel |
| Conversion of those visits vs other visits | | 1.6× | FirstPageSage panel |
| AI-referred retail traffic, year on year | | +138%, converts 54% better, 83% more revenue per visit | Adobe Analytics, May 2026 |
| Share of US e-commerce orders completed by an agent | 0.2% (Q1 2026) | 1.5% (Oct 2026) on the panel; "maybe several hundred" of 993M quarterly orders at DoorDash | FirstPageSage; DoorDash Q3 call |
| Share of agent traffic that lands on e-commerce sites | 38–44% (Apr–Jun) | 60% (Sep) | HUMAN |
| What agents do on those sites | | ~95% discovery and lookup; 2.3–2.4% checkout or payment | HUMAN, DataDome |

Three things follow.

- **The merchant's problem today is being read, not being defended.** Nineteen in twenty agent requests are an agent looking at a product, a price, a variant, a shipping rule. One in forty is a checkout, about the rate at which humans convert. Agents are not failing at the checkout form more than humans do. They are misreading the shop before they get there, or being stopped at the door.
- **The buying is small and the reading is not.** At most 1.5% of orders, probably far fewer, are agent-completed. Visits referred by agents are already one in eighteen and doubling in under a year. Whatever a merchant does about agents this year is worth what the visits are worth, not what the orders are worth.
- **The curves decide the rest.** If visits keep doubling and orders keep their trajectory, the lower parts of this product become real inside two years. If orders stall, the upper parts are still a business. Section 8 names the numbers to watch.

---

## 2. Who decides, and who pays

The rules a merchant must follow to be found and bought from by an agent are set by six companies, none of which is the merchant's platform except Shopify.

| Layer of the stack | Who writes it | What the merchant controls |
|---|---|---|
| Whether an agent may read the site | Cloudflare, Akamai, HUMAN, DataDome categories; Web Bot Auth (IETF draft, adopted 1 Sep 2026) | Allow or block, if the platform exposes it |
| How product data reaches the agent | Google Merchant Center, OpenAI ACP feeds, Shopify Catalog, schema.org | Shopify: a master switch and per-channel toggles. Others: whatever the platform syndicates |
| How an agent builds a cart and checks out | UCP (Google, Shopify), ACP (OpenAI, Stripe), proprietary (Amazon, Walmart) | Nothing; the platform picks the protocol |
| Whether checkout can happen inside the agent at all | Google, Microsoft, Meta, OpenAI, Stripe, PayPal, and the merchant's PSP | Nothing; gated by buyer country and PSP support |
| Who pays when the order is wrong | Platform supplemental terms, card network rules | Nothing; the merchant is the seller of record in every regime, and no platform stores a consent record |

**The platforms do half the job, and a different half each.**

| Platform | Stores | Discovery to agents | Cart | In-agent checkout | Conditions (shipping, returns, warranty, member pricing) | Rules | Evidence |
|---|---|---|---|---|---|---|---|
| Shopify | 3.06M (22% of stores, ~14% of US sales) | Default-on, ~3M stores auto-enrolled into ChatGPT, Copilot, Google, Meta | UCP/MCP | Google, Copilot, Meta direct; ChatGPT referral-only; Checkout WebMCP for signed browser agents | Store-level facts only | On/off | None |
| Wix | ~0.7M (5%) | Default-on UCP profile | UCP | None, hands back a checkout link | None | None | None |
| BigCommerce | ~0.03M | Cloudflare-injected WebMCP on ~100k domains | WebMCP | None, hands back a link | None | On/off | None |
| WooCommerce | 4.08M (30% of stores, ~3–5% of sales) | Opt-in developer-preview MCP | None | None | None | None | None |
| Squarespace, PrestaShop, OpenCart, other | ~1M | None | None | None | None | None | None |
| Enterprise custom (Amazon, Walmart, Target…) | ~35% of stores, most of US sales, 45% of the top 10k sites | Own feeds and gateways | Own | Own, behind flags | Own | Own | Own |

So what: **Shopify merchants are served for discovery and checkout and unserved for conditions, rules and evidence. Long-tail merchants, two in five stores, are unserved for everything. Enterprise builds its own.** The gaps that are common to every platform are conditions, order-time rules, attribution of failed and pixel-less agent checkouts, and consent evidence. Those are the merchant's gaps, not the platform's, and they are the same gaps on Shopify as on WooCommerce.

---

## 3. Who shows up

Agent traffic reaches a merchant in three forms, and the merchant cannot tell them apart.

| Class | Agents | Share of agent page traffic (HUMAN, Sep 2026) | Share of agent orders (Forter, Oct 2026) | How it reads the shop | Can the merchant verify it |
|---|---|---|---|---|---|
| **Signed cloud agents** | ChatGPT Agent, Codex, Google's agent subset, Manus | ~40% | Part of the 28% non-Muse scraping share | Cloud browser, scrapes the page | Yes, Web Bot Auth signature against a published key |
| **Unsigned cloud agents** | Meta Muse, Instinct, Grok Bot | ~43% (Muse alone 40%) | **Muse 49%**; Instinct and Grok material | Cloud Chromium from Cloudflare and Fastly IPs, scrapes the page, pays with a Stripe Link virtual card on non-partner stores | No. Muse publishes no user agent and no key directory; partial signing since ~12 Oct, unverifiable |
| **Local browser agents** | Perplexity Comet, Claude in Chrome, Gemini in Chrome, Edge Copilot | ~17% | Small, counted as human | The user's own browser session | No, and no reason to; a human is present |
| **Protocol agents** | Google AI Mode and Gemini (UCP), Copilot (ACP), ChatGPT apps and feeds (ACP) | ~0 page traffic | **23%**, grew 4% month on month | Feeds, cart and checkout APIs; never sees the page | Yes, by construction |

Two measurements say how far identification can go: Arkose finds 97.7% of legitimate agent sessions carry no verified identity; DataDome finds 80% of AI agents do not declare themselves properly.

So what:

- **Browsing is the shop's main agent customer.** Three in four agent orders and nine in ten agent requests come through a browser reading the page. Protocol orders are a quarter, concentrated on Shopify and the big-box feeds, and essentially absent on the long tail, where no protocol path exists.
- **The biggest buyer cannot be verified.** Muse is half of agent orders and signs nothing a merchant can check. Any product that decides on identity is blind to its largest case.
- **Therefore: serve the page for everyone, decide on what the order is rather than on who placed it, and use identity only as a label for attribution.** Mainstream cloud agents are converging on signing because the platforms pay them to with rate limits and checkout access, so the label gets better over time. The decision logic must not wait for it.
- **Design for the pattern, not the agent.** Muse's surge is one product launch with 23% day-14 retention and 39% of Meta's October ad spend. If it fades, the long tail still sees scraping and local browsers.

---

## 4. Where it breaks, along the funnel

### 4.1 Lookup: the agent reads the shop wrong

This is where 95% of agent activity is and where the mistakes originate.

- **Conditions are flattened on every protocol.** Shipping rates, returns windows, warranty, member and B2B pricing, subscriptions, bundles and local pickup are excluded or reduced to a store-level sentence in Shopify Catalog, UCP and ACP alike. No feed syndicator handles them. Shopify's Knowledge Base covers store facts, not "does this fit my model."
- **Variants are mismapped.** About half of failed UCP sessions are variant and ID shape problems between feed, storefront and checkout.
- **Stale data drove the first public retreat.** Walmart found ChatGPT's in-chat checkout converted at one third of the click-out rate and CNBC reported item information "often inaccurate"; OpenAI moved Instant Checkout to merchant apps and referral in March 2026. The post-mortem named stale stock, delivery and shipping-cost data.
- **Structured, correct data is the one thing with lift-type evidence.** Shopify says Catalog-fed search converts at twice the rate of scraped; UCP merchants complete at 2.7 times; Muse with Shop Pay at 3.5 times non-partnered merchants. All three are vendor or panel numbers, none controlled, and the 3.5 confounds partnership with saved cards. They point the same way.
- **Page-side treatments have no lift evidence at all.** llms.txt is published by many sites and no major agent is confirmed to read it; nothing measures whether JSON-LD, DOM readability or WebMCP raises browsing-agent completion. Akamai found commerce sites left more than 90% of AI-bot traffic in monitor-only mode.

So what: **the gap at lookup is real, cross-platform including Shopify, and the only one with directional evidence that fixing it pays.** It is a data problem before it is a protocol problem: the record of what is sold, to where, on what terms, is what every rendering is missing.

### 4.2 The door: the agent is stopped, or has nowhere to pay

- 37 of 51 large US retailers block agents, most by accident of existing bot rules; blocked agent traffic rose from 8% in April to 21% in June. 0 of 51 has a verified-agent route.
- In-agent checkout exists only for buyers in the United States (Copilot, Google), Canada and Australia (Google), and a handful of countries through Meta. Nothing reaches any European state, Israel, or any Israeli payment processor. A non-US merchant can be found by agents on every platform and bought from inside none of them.
- On the long tail, scraping agents walk through the door anyway and pay with a virtual card, and the merchant never knows an agent was there.

So what: **the door is not ours to open.** The gate is held upstream by the agent companies, the PSPs and the card networks. What a merchant-side layer can do is make sure that whatever gets through, by protocol or by browser, is served correctly and recorded.

### 4.3 Order: the order that lands is for the wrong place

The only production outcome data anywhere is Forter's network split, 23 July to 29 October 2026.

| Measure | Protocol agents | Scraping agents | Humans |
|---|---|---|---|
| Card authorization | 100% | 98.8% | |
| Merchant risk approval | unchanged | 44% before Forter's 24 Sep rule changes, 73% after | unchanged |
| Customer-initiated cancellation | 1–2%; 10% on Google's single-item checkout | 1–2% | |
| Merchant-initiated cancellation | lowest (OpenAI) | **27% Asia and Middle East, 37% Europe** | ~10% |

Forter attributes the ex-US merchant cancellations mostly to agents ordering outside the merchant's eligibility: wrong country, wrong currency, unsupported merchant.

So what: **the one confirmed order-time failure is attribute-shaped, not accuracy-shaped.** The merchant already knows where it ships, in which currency, and how many of what. Rules on those attributes prevent the failure without knowing who placed the order, which matters because half of the orders are unsigned. This is also the strongest argument against the earlier identification-first POC: rules need the order, not the agent.

### 4.4 After: nobody knows what the orders cost

- Not one operator, card network, processor or fraud vendor has published a wrong-item, return or dispute rate for agent orders. Forter withholds fraud and dispute rates and has promised to address "whether disputes are higher" in its 12 November webinar.
- The 1–2% customer-cancellation figure does not imply low disputes: the orders are weeks old and disputes lag 30 to 120 days; cancellation and dispute are different exits for different failures; and the figure is equally consistent with nobody checking.
- Our own accuracy research sets a floor the models will not remove: by 2028–2029, a per-order rate of materially wrong orders around 2–5% at a typical small merchant, set by non-deterministic intent, bad merchant data and humans rubber-stamping confirmations (people approve 97% of permission prompts and catch about 13% of planted errors). At the earlier loss model's 26% of order value per mistake, 2% wrong orders cost 52 basis points of agent order value, more than the 42 basis points card-not-present fraud costs today.
- But agent orders are at most 1.5% of orders, so that 52 basis points is under one basis point of a merchant's business today.

So what: **the evidence and guarantee layers are correct in direction and premature in dollars.** Better models do not remove the need, because the floor is set by data, intent and attention rather than reasoning. The volume to protect does not exist yet. These layers wait for the dispute number and for agent share to pass a few percent.

---

## 5. What we build

One record, rendered to everyone, with the merchant's rules and the receipt attached.

**The record.** An offer is a variant for a destination: price, stock with its age, landed cost, ships-to at postcode level, returns window, promo validity, warranty, eligibility. Conditions are structured data on the offer, not prose in a footer. This is the unit no platform models; catalogs stop at the variant and conditions stop at the store.

**The five layers, and what each fixes from section 4.**

| Layer | What it does | Fixes | Needs identity | Deliverable | Who else |
|---|---|---|---|---|---|
| **Truth** | The record above, synced from the store, maintained by the merchant, with a freshness signal | 4.1 conditions, variants, stale data | No | Now, every platform | Botify AgenticCatalog at enterprise (feeds, no conditions); Shopify Catalog (products, store-level facts) |
| **Serve, page** | Render the record into the page for browsing agents: structured data, readable variants, server-rendered prices, no cookie walls; agent-specific load and latency handling | 4.1 for the 90% of traffic that reads the page; 4.2 accidental blocks | No | Now, per platform via plugin, theme or edge | Botify PageWorkers at Edge (enterprise beta); Cloudflare at the edge, without the record |
| **Serve, protocol** | Render the record into feeds, UCP and ACP profiles, MCP and WebMCP tools, well-known discovery | 4.1 for protocol agents | No | Now for discovery and cart; checkout only where the PSP gate is open | Platforms for their own channels; feed syndicators for catalog |
| **Decide** | Rules at the order hook: destination, currency, quantity, price drift, per-product and per-market eligibility, allowlists; route the risky minority to confirmation | 4.3 | No; identity is a label | Now, where the platform exposes an order hook | Forter and Signifyd at the risk-approval step, enterprise only |
| **Prove and measure** | Snapshot of the offer the order was sold against, the consent artefact, the session; attribution that counts failed and pixel-less agent checkouts; completion telemetry per agent | 4.4 evidence; the missing outcome data | Label only | Technically now; commercially unproven | Nobody |
| **Guarantee** | Absorb the merchant's loss on qualifying agent orders | 4.4 | | Not as a business until a dispute rate and a few percent of orders exist | Nobody |

**What we cannot deliver, and must say so.** In-agent checkout for any merchant whose buyers or PSP sit outside the six companies' gates. We can make the order that arrives correct and recorded; we cannot make Google or Meta check out an Israeli or German store.

**Why one product and not five.** Every layer reads or writes the same record. Rules reference offers and conditions. Evidence snapshots them. The page and the feeds render them. A merchant who adopts Truth has, without further work, the input to every other layer. That is the argument for a single vendor, and it is the thing none of the per-layer incumbents has.

---

## 6. Who pays for it, and how much

The market is inverted depending on the unit.

| Segment | Share of stores | Share of US sales | Agent coverage today | What is unserved | Fits per-store pricing | Fits order-value pricing |
|---|---|---|---|---|---|---|
| Long tail (WooCommerce, Wix, Squarespace, other open source) | ~40% (5.7M stores) | ~5% | Nothing beyond Wix and BigCommerce discovery | Everything | Yes | No |
| Shopify | 22% (3.06M) | ~14% | Discovery and checkout, default-on | Conditions, rules, attribution, evidence, browser-agent handling, headless | Yes | Partly |
| Enterprise custom and enterprise platforms | ~35% of stores, 45% of top 10k | ~55–60% plus undisclosed platform GMV | Own gateways, Cloudflare, Akamai, Forter, Botify | Conditions, evidence | No, they build | Yes, and they have vendors |

So what:

- **The long tail is many merchants selling little each.** A product priced as a share of agent order value earns almost nothing there. A product priced per store does. Truth, Serve and Decide sell per store.
- **Shopify is the heaviest cell on the map** (Muse with Shop Pay is the single largest agent-order flow) and the one where what remains unserved is exactly the record and the rules. It is a channel for the same product, not a different product.
- **Enterprise is not a target.** It has vendors for every layer we would sell, builds its own gateways in weeks, and is where Botify and Cloudflare live.
- **The pitch this year is revenue, not loss.** A merchant with 5.5% of visits coming from agents that convert at 1.6 times, and agents that cannot read its shipping rules, loses sales it never sees. That is a number the merchant can feel. The loss line from wrong orders, under one basis point today, is not. Protection becomes the pitch when the dispute rate and the order share say so.

---

## 7. Why it holds, and what could break it

**Why it holds.** The record of offers and conditions is the thing every incumbent lacks and every layer needs. Platforms model products, not conditions. Syndicators model feeds, not destinations. Bot managers model traffic, not the shop. Fraud vendors model risk at approval, not eligibility at order. Whoever holds the merchant's record of what it sells, where, on what terms, holds the input to every agent interaction with that merchant, on any protocol that comes next.

**What could break it.**

1. **Platform absorption.** Shopify shipped five agent features in one Edition and went end to end in two quarters. Conditions in Catalog would take one. The defence is the long tail, where Shopify does not operate, and the cross-platform record, which Shopify has no reason to build.
2. **Botify moving down-market.** It has AgenticCatalog (ACP and UCP feeds from crawl, GA planned second half of 2026), PageWorkers at Edge (open beta, July 2026), a DataDome partnership for agent verification, and a free "readiness audit in 60 seconds." It sells annual enterprise contracts to million-page sites and has no conditions, no order-time rules and no evidence. It stops where the agent has the catalog.
3. **Cloudflare and Akamai at the edge.** They categorize agents, inject WebMCP (BigCommerce, ~100k domains), and run the signature registries. They do not know the merchant's catalog, conditions or rules. If we ever position as "serve agents at the edge," they are the competitor; positioned as the merchant's record, they are infrastructure we consume.
4. **One-agent concentration.** Half of agent orders are one product with 23% day-14 retention. The design must not depend on Muse, and attribute rules plus page rendering do not.
5. **No measured lift on the page side.** The first customers are the experiment. The product has to produce its own completion and failure telemetry, because nobody else has.
6. **Orders stall.** If agent-completed orders stay under 1% of orders, Decide stays small, Prove and Guarantee never arrive, and the company is a merchant data and page product. Still a business, a smaller one.

---

## 8. What we do not know, and what decides it

| Unknown | Why it matters | What resolves it | When |
|---|---|---|---|
| Dispute rate on agent orders versus human orders | Decides whether Prove and Guarantee are products or roadmap | Forter webinar "The First 60 Days of Agentic Transactions" | 12 November 2026 |
| Agent-completed share of orders, by platform | Panel says 1.5%, direct count says far less; nobody publishes it by platform | Shopify Q3 (3 November, not yet reviewed), Walmart Q3 and Agent Profiles launch, our own telemetry once live | November 2026 onward |
| Whether Muse holds its share | Half of agent orders rest on one launch | Meta's next disclosures, HUMAN and DataDome monthly reports | Monthly |
| Whether Meta publishes a key directory | Until it does, the largest agent is unverifiable and Decide must stay attribute-based | Meta, Cloudflare, IETF | Unknown |
| Whether any page-side treatment lifts completion | Decides how much of Serve is product and how much is hygiene | Our own measurement; no external source | After first customers |
| Whether the 2–5% wrong-order floor concentrates or scatters | Concentrated failures can be ruled on; scattered ones need per-order evidence | Per-merchant telemetry | After first customers |
| Which Israeli or European PSP first supports an agent payment protocol | Opens in-agent checkout for non-US merchants | PSPs, Visa TAP, Mastercard Agent Pay, Stripe, PayPal | Unknown |

---

## 9. Options not yet discussed

These are candidates we have raised in conversation and want not to lose. **None has been discussed to a conclusion, none is a decision, and listing one here does not favour it.**

**Wedge candidates**
- "See what agents see": run the major agents (Muse reproduced as headless Chromium from a cloud IP, ChatGPT Agent, Google, Copilot, Comet) against a store and show where each misreads a variant, gets a wrong ship-to or returns answer, is blocked, or orders for an unserved destination. Free, no install. Measures lookup correctness, not checkout reach.
- Truth first: the record and its page rendering as the first paid product, sold on discovery conversion.
- Conditions as system of record: the merchant maintains shipping, returns, warranty and eligibility in our product rather than in prose, and we render them everywhere.
- Decide first: attribute rules at the order hook, sold on the ex-US cancellation problem.

**First-platform candidates**
- WooCommerce: largest by store count, nothing served, Muse buys there with no merchant visibility.
- Shopify sidecar: heaviest agent-order cell, residual gap is conditions, attribution and browser-agent handling; terms allow termination without notice.
- Wix: default UCP profile exists, checkout hands back a link.

**Region candidates**
- Israel: long tail in miniature (about 48% WooCommerce by count), no in-agent checkout, no PSP support, less hype to ride; discovery-first pitch.
- US long tail: where the checkout gates are open and the hype is loudest.
- Europe: 37% merchant cancellations on scraper orders, the sharpest eligibility pain.

**Delivery candidates**
- Hosted sidecar on a merchant subdomain plus a thin per-platform plugin for sync and order hooks.
- Edge proxy in front of the store.
- Platform plugins only.

**Pricing candidates**
- Per store per month, tiered by catalog size or agent sessions.
- Per agent session.
- Per agent order, for Decide and Guarantee only.

**Two-sided option**
- Give agents a reason to prefer the merchant's served record (correct, fresh, signed transactability answers), so that agents route to it to raise their own completion rates.

---

## Appendix A. Number sheet

Confidence: **F** filing or earnings call; **V** vendor network data, self-reported, network size undisclosed; **P** panel or tracker, method published; **E** our estimate.

| # | Figure | Value | Source | Confidence | Used in |
|---|---|---|---|---|---|
| 1 | AI-referred share of US retail visits | 2.88% Q1, 3.79% Q2, 4.73% Q3, 5.46% Oct 2026 | FirstPageSage panel (5,212 shoppers, 46 brands), normalized to Census | P | §1, §6 |
| 2 | Conversion of AI-referred visits vs other | 1.6× | FirstPageSage | P | §1, §6 |
| 3 | AI-referred retail traffic growth | +138% YoY; converts 54% better; 83% more revenue per visit | Adobe Analytics, May 2026 | V | §1 |
| 4 | Agent-completed share of US e-commerce orders | 0.19% Q1, 0.27% Q2, 0.68% Q3, 1.35% Sep, 1.52% Oct 2026 | FirstPageSage | P | §1, §4.4, §6 |
| 5 | Agent-completed orders, directly observed | "maybe several hundred" (Muse, Instinct, Fo) vs 993M Q3 orders | DoorDash Q3 2026 call | F | §1 |
| 6 | Visa on agentic volume | "very small" for FY2027; ~50 AI partners live | Visa Q4 FY2026 call, 27 Oct | F | §1 |
| 7 | Agent traffic landing on e-commerce | 38–44% Apr–Jun, 46% Aug, 60.4% Sep 2026; 76.5% of Muse traffic | HUMAN | V | §1 |
| 8 | What agents do on commerce sites | ~95% discovery and research; 2.3–2.4% checkout or payment; 84% of Muse requests hit product or search pages, 8.9% touch cart, login or payment | HUMAN May–Jun; DataDome Muse sample | V | §1, §4 |
| 9 | Share of agent page traffic by agent, Sep 2026 | Muse 40.3%, ChatGPT Agent 28.6%, Codex 11%, Comet 9.4%, Claude in Chrome 7.1%, Instinct and Grok <3%, others <3% | HUMAN (read from excerpts; primary page blocks fetches) | V | §3 |
| 10 | Share of agent orders by agent, week of 19 Oct 2026 | Muse 49% (bought from 74% of stores receiving any agent order); other scraping agents 28%; protocol agents (Google, Copilot, ChatGPT apps) 23%, +4% MoM | Forter | V | §3 |
| 11 | Scraping vs protocol orders | 77% / 23%; scraping was 1% in late July, grew ~7× in a quarter | Forter | V | §3 |
| 12 | Legitimate agent sessions with no verified identity | 97.7% | Arkose | V | §3 |
| 13 | AI agents that do not declare themselves properly | 80% | DataDome (Galileo) | V | §3 |
| 14 | Muse identification | No user agent, no key directory; 99.3% of requests from Cloudflare/Fastly IPs; partial Web Bot Auth signing from ~12 Oct | DataDome | V | §3 |
| 15 | Muse retention | 73% day 1, 30% day 7, 23% day 14; 39% of Meta's October ad spend | Sensor Tower; ad-spend tracker | P | §3, §7 |
| 16 | Agents signing Web Bot Auth | ChatGPT Agent, Codex, Google's agent subset, Manus; not Muse, Comet, Claude in Chrome, Instinct, Grok | Vendor docs, DataDome, Cloudflare | V | §3 |
| 17 | Live stores by platform | 13.75M total; WooCommerce 4.08M (30%), Shopify 3.06M (22%), Wix ~0.7M, Squarespace ~0.6M, BigCommerce ~0.03M, other OSS ~0.3M | Store Leads Aug 2026; MyLeadFox Oct 2026; HTTP Archive May 2026 | P | §2, §6 |
| 18 | Share of US e-commerce sales | Amazon 36–38%, Shopify ~14%, Walmart 6.4%; long tail ~5% (E); Salesforce 3–5% (E); Adobe 2–4% (E) | Marketplace Pulse; Shopify 10-Q (GMV $216.3B H1 2026, +33%) | F for Shopify and Amazon; E for others | §2, §6 |
| 19 | Share of top-10k e-commerce sites | Custom ~45%, Shopify ~22%, Salesforce ~8%, Adobe ~7%, WooCommerce ~5% | BuiltWith via Taylor Sicard | P | §2, §6 |
| 20 | Shopify auto-enrolment | ~3M stores enrolled by default into ChatGPT, Copilot, Google, Meta | Shopify Help Center | F | §2 |
| 21 | Platform agent features | Shopify Checkout WebMCP 28 Sep 2026 (Web Bot Auth key, Chromium, Shop Pay cards); Wix default UCP profile on ~6,700 stores, `continue_url` checkout; BigCommerce WebMCP via Cloudflare on ~100k domains; WooCommerce MCP opt-in preview | Vendor changelogs and docs | F | §2, §5 |
| 22 | Structured-serving completion multiples | Catalog-fed search converts 2× scraped; UCP merchants complete at 2.7×; Muse with Shop Pay 3.5× non-partnered | Shopify; UCP Checker; Shopify/Meta | V, uncontrolled | §4.1 |
| 23 | Failed UCP sessions due to variant/ID mapping | ~half | UCP Checker, May 2026 | P | §4.1 |
| 24 | OpenAI Instant Checkout retreat | In-chat checkout converted at one third of click-out (Walmart); item information "often inaccurate" (CNBC); moved to apps and referral, March 2026 | Walmart; CNBC | F | §4.1 |
| 25 | Large retailers blocking agents | 37 of 51 block; 0 of 51 have a verified-agent route | Billions Network | P | §4.2 |
| 26 | Blocked share of agent traffic | 8.2% Apr, 21% Jun 2026 | HUMAN | V | §4.2 |
| 27 | In-agent checkout buyer countries | US (Copilot, Google), Canada and Australia (Google), six countries via Meta; none in Europe or Israel | Shopify Help Center | F | §4.2 |
| 28 | Israeli PSP support for agent payment protocols | None (Isracard/PayMe, Pelecard, Tranzila, PayPlus, Grow, iCredit) | Vendor sites, Hebrew and English search | P | §4.2 |
| 29 | Card authorization on agent orders | Protocol 100%; scraping 98.8% | Forter | V | §4.3 |
| 30 | Merchant risk approval on scraping-agent orders | 44% before 24 Sep rule changes, 73% after; Grok Bot +50 points; humans and protocol unchanged | Forter | V | §4.3 |
| 31 | Customer-initiated cancellation | 1–2% most agents; ~10% Google (single-item checkout, baskets 56% below the $227 average) | Forter | V | §4.3, §4.4 |
| 32 | Merchant-initiated cancellation | 27% Asia/Middle East, 37% Europe on scraping-agent orders; ~10% for human orders; attributed to eligibility (country, currency, unsupported merchant) | Forter | V | §4.3 |
| 33 | Dispute, return, wrong-item rates on agent orders | Not published by anyone; Forter to address 12 Nov | All sources | — | §4.4, §8 |
| 34 | Benchmark accuracy | tau2-bench Retail 74.0% (Apr 2025) → 91.9% (Opus 4.6); EComAgentBench 57.1%; ShoppingBench 48.2%, "Coupon & Budget" 30.4%; ShopSimulator 32.7% | Benchmark papers and leaderboards | P | §4.4 |
| 35 | Accuracy vs consistency | HAL accuracy 44% → 79%; consistency 0.76 → 0.84 | Princeton HAL | P | §4.4 |
| 36 | METR horizons | 50% horizon ~12–17 h; 80% horizon ~1.5–3 h; no 99% horizon | METR | P | §4.4 |
| 37 | Human confirmation behaviour | 97% of permission prompts approved; ~13% of planted errors caught | Anthropic study, 1,053 testers | P | §4.4 |
| 38 | Wrong-order floor, 2028–2029 | 2–5% per order at a typical small merchant | Ours | E | §4.4 |
| 39 | Loss per mistake; loss line | 26% of order value per mistake; 2% wrong orders = 52 bp of agent order value; CNP fraud 42 bp | Doc 01 (E); industry CNP rate | E | §4.4 |
| 40 | Botify agentic products | AgenticCatalog (ACP/UCP feeds, tech preview, GA H2 2026); PageWorkers at Edge open beta July 2026; DataDome partnership 5 Mar 2026; quote-based enterprise pricing | Botify site and releases, press | F | §7 |
| 41 | Commerce sites with AI-bot traffic in monitor-only mode | >90% | Akamai | V | §4.1 |

## Appendix B. Where the evidence lives

| Document | What it is for |
|---|---|
| `05-research-conclusions.md` | The conclusions of the three research reports and the discussion that produced this pivot, organized by research question |
| `06-traffic-and-coverage-map.md` | The platform-share, traffic and order numbers, the protocol map, the platform × agent coverage map, and the test of doc 05 against them |
| `reports/Shopping agent accuracy trajectory.md` | Benchmarks, reliability, structural floors, the no-pivot-on-accuracy argument |
| `reports/Shopping agent production data 2026.md` | Agent-by-agent production data, Forter network data, the list of what nobody has disclosed |
| `reports/Agent enablement platform coverage.md` | Platform-by-platform enablement, fidelity loss by protocol, the twelve ranked gaps, Israel |
| `../pivot-1-order-trust-layer/` | The earlier thesis: loss model, directions, brief, POC definitions |
| `../GLOSSARY.md` | Definitions of every commerce, payment, protocol and measurement term used here |
