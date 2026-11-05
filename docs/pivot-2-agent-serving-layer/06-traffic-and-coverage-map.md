# 06 — Traffic and coverage map: where agent traffic lands, and what it says about doc 05

**Date:** 4 November 2026
**Status:** Working analysis. Tests the conclusions of `05-research-conclusions.md` against platform-share, agent-traffic and agent-order data gathered on 4 November 2026. Not decisions. Where this doc disagrees with 05, §7 lists the proposed changes.

**What this doc adds to 05.** Doc 05 established *what* the gaps are. This doc puts weights on them: how many merchants sit on each platform, how much they sell, how much agent traffic reaches them, by which agent and through which channel, and therefore which cells of the platform × agent map carry volume and which are empty.

**Sources.** Vendor traffic reports (HUMAN Security, DataDome, Imperva, Cloudflare Radar), store-count trackers (Store Leads, MyLeadFox, HTTP Archive, BuiltWith), Shopify 10-Q, Marketplace Pulse, Forter's network data, the FirstPageSage panel, plus the `reports/` and `research_notes/` folders. The three traffic vendors each see only their own network, and they disagree with each other by up to 10× on agent order volume. Every figure below is directional unless marked as a filing.

---

## 1. Findings in one page

1. **Agent activity is top of funnel.** ~95% of agent activity on commerce sites is discovery and lookup; checkout and payment are 2.3–2.4% of it, about the rate at which humans convert. Agents are not failing at checkout more than humans; they are stopped before it or complete it on stores that never opted in.
2. **Volume is tiny, growth is fast.** Agent-completed orders are at most 1.5% of US e-commerce orders (panel, October 2026; up from 0.2% in Q1) and probably far less (DoorDash: "several hundred" against 993M quarterly orders). AI-referred visits are 5.5% of US retail visits, up from 2.9% in Q1.
3. **Browsing beats protocols 3:1 in orders and ~10:1 in traffic.** Scraping agents place 77% of agent orders and generate ~90% of agent page traffic; protocol orders (Google UCP, Copilot and ChatGPT ACP) are 23% and grew 4% month on month while scraping grew 7× in a quarter. One product, Meta Muse, is most of that surge.
4. **The largest agent is unsigned.** Muse is 40% of agent traffic and 49% of agent orders, runs from cloud IPs, publishes no user agent and no key directory. Arkose finds 97.7% of legitimate agent sessions carry no verified identity. Doc 05's "verify against registries" covers ChatGPT Agent and Codex, not Muse.
5. **The market is inverted by unit.** By store count the long tail (WooCommerce, Wix, Squarespace) is ~40% of stores; by sales and traffic, custom enterprise stacks plus Shopify are ~75%. Doc 05's "core" (Woo, Wix, custom carts, non-US) is the right market for layers sold per merchant and the wrong market for a layer priced on GMV.
6. **The gap is real at lookup (conditions) and at order eligibility.** It is not yours to close at the checkout gate, it is being closed by platforms at discovery for their own merchants, and it is unmeasurable post-order.
7. **Shopify is the heaviest cell on the map.** Muse × Shop Pay converts at 3.5× non-partnered merchants; Shopify auto-enrolled ~3M stores. The residual Shopify gap is conditions and attribution, as 05 says.
8. **Structured serving has lift evidence; page-side serving does not.** Shopify's Catalog-fed search converts 2× scraped, UCP merchants complete at 2.7×, Muse+Shopify at 3.5×. All vendor or panel numbers, none controlled. llms.txt, DOM readability and WebMCP still have none. Doc 05's conclusion 8 should narrow to the page side.

---

## 2. The protocol map: what a merchant must support to serve agents

Each agent trip passes through six layers. Each layer has competing formats.

| Layer | Protocol | What it does | Status (Nov 2026) |
|---|---|---|---|
| **Access** | robots.txt, Cloudflare Content Signals | Crawl permission per user agent | Honored by fetchers; ignored by browser agents acting as the user |
| | Web Bot Auth (IETF draft) | Agent signs HTTP requests; site verifies key | Adopted by IETF 1 Sep; signed by ChatGPT Agent, Codex, Google's agent subset, Manus; not by Muse, Comet, Claude in Chrome, Instinct, Grok |
| | Bot management (Cloudflare, Akamai, HUMAN, DataDome) | Allow verified, block the rest | How Amazon, Target, Walmart, Kohl's block Muse |
| **Discovery** | schema.org JSON-LD (Product, Offer) | Structured product data in page | Universal on both sides; what agents read when nothing else exists |
| | Merchant feeds (Google Merchant Center, ACP feed, Shopify Catalog) | Bulk catalog to the agent platform | What big-box retailers and Shopify supply |
| | llms.txt | Site summary for LLMs | Published by some sites; no major agent confirmed to read it |
| **Interaction** | Browser automation (DOM, screenshots) | Agent drives a page | Most traffic; no standard |
| | WebMCP (`navigator.modelContext`) | Page exposes tools to in-browser agent | Chrome/Edge origin trial; live by default only on Shopify checkout (signed agents) and BigCommerce (Cloudflare edge) |
| | MCP (hosted server) | Server-to-server tools | Supported by nearly every agent and platform; discovery and cart, not payment |
| | A2A | Agent-to-agent delegation | Gemini, Copilot, Claude; carries AP2 |
| **Checkout** | UCP (Google, Shopify) | Cart, checkout, order | Google AI Mode/Gemini, Copilot, Shopify, Salesforce, commercetools, BigCommerce; Walmart and Target behind flags |
| | ACP (OpenAI, Stripe) | Cart, checkout, feeds | ChatGPT feeds and apps (checkout pulled Mar 2026), Copilot Checkout; most platforms |
| | Proprietary | Vendor account | Amazon Buy for Me, Walmart Agent Gateway, Klarna |
| **Payment** | AP2 (Google → FIDO) | Signed intent and cart mandates | Gemini, ChatGPT; Shopify, Salesforce, Adobe |
| | Visa Trusted Agent Protocol, Mastercard Agent Pay | Agent credentials on card rails, built on Web Bot Auth | Via PSPs (Stripe, Adyen); no Israeli PSP |
| | Stripe Shared Payment Tokens / Link | Scoped single-use tokens or virtual cards | How Muse pays on non-partner stores |
| | x402, MPP | Machine-to-machine stablecoin or HTTP 402 | Dev tooling only |
| **Trust** | Agent identity (Web Bot Auth, TAP, Walmart Agent Profiles), user identity (account linking, passkeys), intent proof (AP2 mandates), risk (Forter, Signifyd, Radar) | Is this order real and authorized | Thinnest layer on both sides |

A merchant that wants full coverage needs three stacks: (a) browser agents via JSON-LD plus bot rules, with WebMCP as upgrade; (b) Google/Shopify surfaces via UCP, Merchant Center and AP2; (c) OpenAI/Microsoft/Stripe surfaces via ACP feeds, MCP and Stripe or PayPal. Amazon and Walmart sit outside all three.

---

## 3. Platforms: merchants, sales, traffic

| Platform | Live stores (share of 13.75M) | Share of US e-commerce sales | Share of top-10k e-commerce sites |
|---|---|---|---|
| **Custom / proprietary** (Amazon, Walmart, Target, eBay, Temu, Apple…) | ~35% (remainder) | Amazon 36–38%, Walmart 6.4%; custom block overall ≈ 55–60% | **~45%** |
| **Shopify** | 3.06M (22%) | **~14%** (10-Q: GMV $216.3B H1 2026, +33%; ~$450B annualized, global) | ~22% |
| Salesforce Commerce Cloud | a few thousand (<0.1%) | undisclosed; est. 3–5% (ours) | ~8% |
| Adobe Commerce / Magento | ~0.1M (0.7%) | undisclosed; est. 2–4% (ours) | ~7% |
| commercetools / SAP | hundreds | commercetools >$100B global run-rate (Q1 2026) | small |
| **WooCommerce** | **4.08M (30%)** | est. 3–5% (ours; no disclosure) | ~5% |
| Wix | ~0.7M (5%) | <1% | ~0 |
| Squarespace | ~0.6M (4%) | <1% | ~0 |
| BigCommerce | ~0.03M (0.2%) | <1% | ~1% |
| PrestaShop, OpenCart, other OSS | ~0.3M (2%) | <1% | ~0 |

Sources: Store Leads (Aug 2026, 13.75M live stores), MyLeadFox (Oct 2026), HTTP Archive May 2026 crawl (all origins: WooCommerce 6.64%, Shopify 4.76%, Wix 1.48%, Squarespace 1.12%, PrestaShop 0.60%, Magento 0.38%, BigCommerce 0.14%; top 1M: Shopify 6.08%, WooCommerce 3.12%), BuiltWith top-10k via Taylor Sicard, Marketplace Pulse (Amazon 35.7%, Shopify 14%, combined 49.7% of $1.2T US), Shopify Q2 2026 10-Q.

**Reading.** The two halves of the market are inverted. By store count the long tail is ~40%; by sales and traffic, custom stacks plus Shopify are ~75%. Enterprise platform GMV is undisclosed anywhere; those rows are estimates.

**Israel** (MyLeadFox, Sep 2026, platform-identifiable stores): WooCommerce ~12.7k (48%), Shopify ~4.0k, Wix ~3.3k. Israel is more long-tail than the global mix. An older AfterShip count gives WooCommerce 46%, Shopify 20%, Wix 14% on a larger base. No current comprehensive Israeli store total exists.

---

## 4. Agent traffic: how much, which agents, which channel

### 4.1 From all traffic down to agent orders

| Layer | Figure | Source |
|---|---|---|
| Automated share of all web traffic | 53% (Imperva 2026 Bad Bot Report, 2025 data); 57.5% of HTML requests (Cloudflare Radar, Jun 2026) | Mostly classic bots and crawlers |
| AI bots (crawlers + agents) share of requests | 1.7% of requests (DataDome, Jun 2026); 13% of sessions (51Degrees, year to May 2026) | Half of DataDome's AI requests are Meta crawlers (49.7%) |
| Agentic (acting for a user) share of AI-bot traffic | 58.2M agentic-browser requests in Sep 2026 vs ~8B AI-agent requests/month on DataDome: **<1%** | DataDome Q3 2026 (25B AI requests in Q3, +41.5% QoQ) |
| Where agent traffic goes | E-commerce **60.4%** of agentic traffic in Sep (46% in Aug; 38–44% Apr–Jun); 76.5% of Muse traffic | HUMAN |
| What it does there | Discovery and research ≈ 95%; **checkout/payment 2.3–2.4%** of agentic activity; 8.9% of Muse requests touch cart/login/payment; 84% hit product/search pages | HUMAN May–Jun; DataDome Muse sample |
| Blocking | 8.2% of agentic traffic blocked in Apr, 21% in Jun; "many still block" Muse by default | HUMAN |
| AI-referred share of US retail visits | 2.88% Q1 → 3.79% Q2 → 4.73% Q3 → **5.46% Oct 2026**; converts 1.6× other visits | FirstPageSage panel (5,212 shoppers, 46 brands), normalized to Census |
| AI-referred retail traffic growth | +138% YoY (May 2026); converts 54% better; 83% more revenue per visit | Adobe Analytics |
| Agent-completed share of US e-commerce orders | 0.19% Q1 → 0.27% Q2 → 0.68% Q3 → 1.35% Sep → **1.52% Oct 2026** | FirstPageSage panel |
| Agent-completed orders, directly observed | DoorDash: "maybe several hundred" from Muse, Instinct, Fo against 993M Q3 orders; Visa: "very small" for FY2027 | Earnings calls |

The panel and the direct count disagree by orders of magnitude. The honest range for agent-completed orders is "well under 1.5% of US orders, possibly under 0.1%."

### 4.2 Per agent, September–October 2026

| Agent | Share of agentic traffic (HUMAN, Sep) | Share of agent orders (Forter, wk of 19 Oct) | Channel | Identifiable |
|---|---|---|---|---|
| **Meta Muse** | **40.3%** (debut; traffic tripled) | **49%**; bought from 74% of stores receiving any agent order | Cloud Chromium, scraping; 99.3% of requests from Cloudflare/Fastly IPs | No UA, no key directory; partial signing from ~12 Oct, unverifiable |
| **ChatGPT Agent** | 28.6% (19.8% Aug) | Largest of the 28% non-Muse scraping share | Cloud browser, scraping | Signs Web Bot Auth |
| Codex (all products) | 11% | ~0 (developer agent) | Cloud browser | Signs |
| Perplexity Comet | 9.4% (40.7% Aug; 47% May–Jul) | Small | Local browser | No |
| Claude in Chrome | 7.1% (22% Aug) | Small | Local browser | No |
| Instinct, Grok Bot | <3% | Material (named in the 28%); Instinct claims $1B annualized GMV | Cloud browser, scraping | No |
| Gemini in Chrome, Edge Copilot, Genspark, Manus, Browserbase | <3% combined | ~0 | Local or cloud browser | Mixed |
| **Google (UCP)**, **Copilot (ACP)**, **ChatGPT apps/feeds (ACP)** | ~0 page traffic (protocol calls and feeds) | **23%**; OpenAI+Google+ChatGPT Agent orders +4% MoM | Protocol | Yes, by construction |

August for comparison: Comet 40.7%, OpenAI products ~31% combined, Claude in Chrome 22%, Codex 7.3%. OpenAI's Atlas browser shut 9 August and its traffic moved into ChatGPT Agent.

### 4.3 By channel

| Channel | Share of agent page traffic | Share of agent orders | Card authorization | Merchant risk approval | Merchant cancellation |
|---|---|---|---|---|---|
| Browser scraping, cloud agents (Muse, ChatGPT Agent, Instinct, Grok) | ~75% | **77%** (1% in late July) | 98.8% | 44% → 73% after Forter relaxed thresholds on 24 Sep | 27% Asia/ME, 37% Europe (eligibility); ~10% for humans |
| Browser, local (Comet, Claude, Gemini in Chrome, Edge Copilot) | ~17% | Small; counted as human | n/a | n/a | n/a |
| Protocol (UCP, ACP, MCP apps) | ~0 | **23%** | 100% | Unchanged | Lowest (OpenAI); customer cancels 10% on Google single-item checkout |
| WebMCP | Unmeasured | ≈0 | — | — | — |

Caveat: Forter's network is enterprise-weighted and its size is undisclosed. The 77/23 split probably understates scraping on the long tail (no protocol path exists there) and overstates it at enterprise partners. The scraping surge is one product's launch: Muse has 23% day-14 retention and was 39% of Meta's October ad spend.

---

## 5. Platform × agent coverage map

Cell = can the agent complete a purchase there, and does the merchant have any control. Column weights are approximate shares of agent orders (Forter mix); the weight of a cell is the column share times how much of that agent's buying lands on that platform, which no one publishes. Shading is inferred from completion multiples and partnerships.

| Platform | Muse (~49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google UCP (~12%) | Copilot / ChatGPT apps ACP (~11%) | Local browser agents (Comet, Claude, Gemini) |
|---|---|---|---|---|---|
| **Custom enterprise** (45% of top-10k; most of US GMV) | Blocked at Amazon, Target, Walmart, Kohl's, Neiman Marcus; works at partners (Wayfair, Best Buy) via Stripe Link or PayPal | Same walls; signed, so allowlistable; 0 of 51 large retailers has a verified-agent route | Walmart, Target, Wayfair live behind flags | Walmart, Target, Best Buy, Home Depot, Sephora feeds | Pass most walls as the user's session |
| **Shopify** (22% of stores, ~14% US GMV) | ✅ Shop Pay partner; **3.5× completion** vs non-partnered; master switch and channel toggles only | ✅ Checkout WebMCP for signed agents; higher rate limits | ✅ default-on | ✅ default-on (ChatGPT referral-only) | Readable page; no controls |
| **Salesforce, Adobe, commercetools** | Vendor bot management decides; no agent checkout path by default | Same | Pilots | Pilots | Pass |
| **WooCommerce** (30% of stores; 48% in Israel) | ✅ scrapes and pays with Link virtual card; **merchant has no visibility or control** | Same | ✗ | ✗ (Stripe suite waitlisted) | Pass |
| **Wix** (5%) | ✅ scrapes; UCP profile exists, checkout handed back as `continue_url` | Same | Discovery only | ✗ | Pass |
| **BigCommerce** | ✅ scrapes; WebMCP cart via Cloudflare, no payment | Same | Partial | Via Stripe | Pass |
| **Any platform, Israel** | ✅ scrapes; no Israeli PSP supports any agent payment protocol | Same | ✗ | ✗ | Pass |

**Heavy cells:** Muse × Shopify, Muse × enterprise partners, Muse × WooCommerce. **Light everywhere:** the protocol columns, except Shopify and the big-box feeds. **Empty:** every in-agent checkout cell for Israel and for Woo/Wix/Squarespace.

Three agent classes fall out of the map, not two as in doc 05 §6.1:

1. **Signed cloud agents** (ChatGPT Agent, Codex, Google's subset, Manus): verifiable against registries.
2. **Unsigned cloud agents** (Muse, Instinct, Grok Bot): the largest class by orders; indistinguishable from a headless browser without the fingerprinting 05 declines to build.
3. **Local browser agents** (Comet, Claude in Chrome, Gemini in Chrome, Edge Copilot): the user's own session; need a readable page, not routing.

---

## 6. The funnel, with doc 05's layers mapped onto it

| Phase | Numbers | Doc 05 layer | What the numbers say about the gap |
|---|---|---|---|
| **Discovery** | AI-referred visits 2.9% → 5.5% in 9 months; 60% of agent traffic lands on e-commerce; Shopify auto-enrolled ~3M stores into every channel; Woo/Wix/Squarespace (40% of stores) have no feed path | Serve | Real but splitting. Closed for Shopify merchants. For the long tail it is page-side only (JSON-LD, readable DOM) and unserved. |
| **Lookup** (variant, price, stock, shipping, returns) | ~95% of agent activity; Catalog-fed search converts 2× scraped; half of failed UCP sessions are variant/ID mapping; Walmart pulled ChatGPT checkout over inaccurate item data; every protocol flattens conditions | Truth | **Strongest gap and the only one with lift-type evidence.** Nobody serves conditions; this is where agents misread and where mistakes originate. Applies to Shopify too. |
| **Cart / checkout** | Checkout is 2.4% of agent activity (≈ human conversion); agents reach it at a normal rate, then hit walls (37 of 51 large retailers block by accident) or PSP gates (Israel, ex-US); on the long tail scrapers pay with virtual cards and the merchant never knows | Serve (checkout), Decide | Gate is upstream; a merchant-side layer cannot open it (05 already says so). What it can do is Decide on the scraped orders that do land. |
| **Order** | Scrapers 77% of orders; authorization 98.8–100%; merchants cancel 27–37% of ex-US scraper orders on eligibility; customer cancels 1–2%, same as humans | Decide | **Confirmed gap, attribute-shaped** (destination, currency, unsupported merchant), not accuracy-shaped. Rules fix it without knowing who the agent is. |
| **Post-order** | No dispute, return or wrong-item rate from anyone; ≤1.5% of US orders agent-completed, likely far less | Prove, Guarantee | Unmeasurable today; the GMV it applies to is under 1 bp of a merchant's business. |

Two readings:

- **The stack is top-heavy in practice.** Truth and Serve sit where 95% of agent activity is and where the only lift evidence exists. Decide has one confirmed failure mode. Prove and Guarantee have no data and almost no dollars. Doc 05 orders the layers this way already; the numbers make the slope steeper.
- **Agents do not fail at checkout more than humans.** They are stopped before it, or they complete it on stores that never opted in. The gap is at the door and at eligibility, not in the form.

### Is the gap real?

| Verdict | Phase and layer | Why |
|---|---|---|
| **Real and unserved** | Conditions at lookup (Truth) | Every protocol flattens shipping, returns, warranty, member pricing; no syndicator handles them; variant and inaccuracy failures originate here. Cross-platform, including Shopify. |
| **Real and unserved** | Eligibility at order (Decide) | 27–37% ex-US merchant cancellations from scrapers buying where they shouldn't. Solvable with rules the merchant already knows, without agent identity. |
| **Real, not yours to close** | Checkout gate | Held by Google, Meta, OpenAI, Stripe, PayPal, local PSPs. |
| **Real, being closed by platforms** | Discovery for platform merchants | Shopify went end to end in two quarters; Cloudflare injects WebMCP on BigCommerce; Walmart built its gateway in weeks. What survives is Serve for the long tail's pages and Truth for everyone. |
| **Unknown** | Prove and Guarantee | No outcome data; the volume to insure is negligible until agent orders reach a few percent of orders. |

---

## 7. Doc 05's conclusions, tested

| # | Doc 05 says | Numbers say | Proposed change |
|---|---|---|---|
| 1–5 | Accuracy floor 2–5%; loss 52 bp of agent GMV | Not tested by traffic data and not yet relevant: 52 bp of ≤1.5% of GMV is <1 bp of the merchant's business. | Keep as the 2028 argument; add the sizing line. |
| 6 | Discovery moves regardless | **Confirmed.** 5.5% of visits vs ≤1.5% of orders, doubling yearly. | None. |
| 7 | Only Shopify end to end | **Confirmed and stronger.** Shopify is the heaviest cell (Muse × Shop Pay, 3.5×). Residual Shopify gap is conditions and attribution. | None. |
| 8 | No measured lift from any serving treatment | **Partly contradicted.** Catalog 2× scraped, UCP 2.7×, Muse+Shopify 3.5× are lift numbers for structured serving (vendor and panel, not controlled). Page-side treatments (llms.txt, DOM, WebMCP) still have none. | Narrow to page-side treatments; cite the structured-serving multiples as directional support for Truth. |
| 9 | Israel gets discovery, not checkout | **Confirmed.** Israel is 48% WooCommerce by count: the long-tail core in miniature. | None. |
| 10 | Core = Woo, Wix, custom carts, non-US | **Depends on the unit.** By store count that core is ~40% of stores and unserved; by GMV it is ~5%. Truth and Serve sell per merchant (count is the right unit); Guarantee prices on GMV (does not fit this core). The five layers do not share a customer. "Custom carts" should exclude enterprise custom (45% of top-10k), which has Cloudflare, Akamai, Forter and builds its own gateways. | State the unit. Split: long tail by count is the Truth/Serve market; enterprise by GMV is not a target. |
| 11 | Identification tractable; unsigned in-browser agents are the user's own session | **Misses the largest agent.** Muse is cloud and unsigned: 40% of traffic, 49% of orders, no key directory; 97.7% of agent sessions unverified (Arkose). Verify-against-registries covers ChatGPT Agent and Codex, not Muse. | Three agent classes (§5). Decide on order attributes, not on agent identity. |
| §5 gaps 1–2 | Transactability verdict first, browser-agent handling at SMB checkout second | Gap 1 confirmed by the 27–37% cancellations. Gap 2 is the Muse problem and is identity-blocked. | Keep ranking; note gap 2 is attribute rules, not detection. |
| §6 wedge | "See what agents see" | **Stronger than 05 puts it.** The numbers name the five agents to run (Muse, ChatGPT Agent, Google, Copilot, Comet), which together are ~90% of orders. Because the largest is unsigned, any headless Chromium from a cloud IP reproduces what the merchant sees. | Name the agent set and the cloud-IP condition. |

### Changes proposed to doc 05

1. Reorder §6: Truth first, Serve for the long tail's page second, Decide as attribute rules third. Prove and Guarantee become a roadmap conditional on the 12 November dispute number and on agent share passing a threshold (3–5% of orders).
2. Rewrite hard point 1 with three agent classes. The audit works for all three precisely because the merchant cannot tell them apart either.
3. Narrow conclusion 8 to page-side treatments.
4. Split conclusion 10 by unit.
5. Add sizing: ≤1.5% of orders today, ~0.2% a year ago, 5.5% of visits. The pitch is growth and discovery, not loss; §6.4 already half-says this.

---

## 8. Open questions added by this doc

1. Agent orders by merchant platform. No one publishes it; the Shopify weighting in §5 is inferred. The audit wedge can measure it per merchant.
2. Whether Muse's share holds. Its traffic is one launch with 23% day-14 retention and heavy ad spend; if it fades, the mix tilts back to Google and Copilot protocol flows, small but steady.
3. Whether Meta publishes a key directory. Until it does, the largest agent is unverifiable and Decide must be attribute-based.
4. Enterprise platform GMV (Salesforce, Adobe). Undisclosed; the estimates in §3 are ours.
5. Israel store totals. No current comprehensive count.

## 9. Known weaknesses in this doc

- Three traffic vendors (HUMAN, DataDome, Forter) each see only their own network and disagree by up to 10× on volume; the panel (FirstPageSage) disagrees with the one direct count (DoorDash) by more.
- Store counts differ by tracker (Store Leads 13.75M, MyLeadFox 11.6M) because they count different things.
- Column weights in §5 come from Forter, whose network is enterprise-weighted and whose size is undisclosed.
- HUMAN's September per-agent table was read from search excerpts and secondary coverage; the primary page blocks fetches.
- GMV shares for Salesforce, Adobe, WooCommerce and Wix are estimates, not disclosures.

---

## Sources

- Store counts: [Store Leads via GravityKit (HTTP Archive May 2026)](https://www.gravitykit.com/ecommerce-platform-market-share-2026/), [MyLeadFox platform statistics](https://myleadfox.com/statistics/ecommerce-platforms), [MyLeadFox Israel WooCommerce](https://myleadfox.com/directory/israel/woocommerce), [Retention Edge US share](https://retentionedge.co/articles/ecommerce-platform-market-share-usa), [Taylor Sicard top-tier shares](https://taylorsicard.com/blog/ecommerce-platform-market-share-north-america-2026), [AfterShip Israel](https://www.aftership.com/ecommerce/statistics/regions/il)
- Sales: [Marketplace Pulse via EcomCrew](https://www.ecomcrew.com/amazon-and-shopify-now-control-nearly-half-of-u-s-e-commerce/), [Shopify Q2 2026 10-Q](https://www.sec.gov/Archives/edgar/data/0001594805/000159480526000047/shop-20260630.htm), [commercetools $100B run-rate](https://www.pulse.bot/ecommerce/news/commercetools-hits-100-b-gmv-runrate-in-q1-2026-fueling-aifirst-commerce-push-6be8227b-efb8-40ab-a347-c7267c51ea49/)
- Traffic: [Imperva Bad Bot Report 2026](https://www.imperva.com/blog/bad-bot-report-2026-bots-agentic-age/), [Cloudflare Radar via Presenc](https://presenc.ai/research/ai-bot-share-web-traffic-2026), [DataDome AI traffic trends](https://datadome.co/resources/bot-and-agent-security-report/ai-traffic-trends/), [DataDome Q3 2026](https://datadome.co/threat-research/ai-traffic-report-q3-2026/), [PPC Land on DataDome Muse](https://ppc.land/metas-muse-gains-a-33-share-of-ai-browser-traffic-in-9-days-datadome-finds/), [HUMAN September 2026](https://www.humansecurity.com/learn/blog/september-2026-agentic-traffic-triples-meta-muse-debuts-at-no-1/), [HUMAN August 2026](https://www.humansecurity.com/learn/blog/state-of-agentic-traffic-august-2026-agentic-traffic-grows-27-reaches-new-high-as-codex-debuts-strongly/), [HUMAN June 2026](https://www.humansecurity.com/learn/blog/state-of-agentic-traffic-june-2026-browser-agent-tooling-for-developers-is-catching-on-fast/), [HUMAN April 2026](https://www.humansecurity.com/learn/blog/state-of-agentic-traffic-april-26/), [PPC Land on HUMAN May](https://ppc.land/ai-agent-traffic-dips-in-may-but-blocking-rates-keep-climbing/), [Arkose via PPC Land](https://ppc.land/97-7-of-legitimate-ai-agent-sessions-carry-no-verified-id-arkose-finds/)
- Orders and referrals: [Forter AI agent order data](https://www.forter.com/blog/ai-agent-order-data/), [FirstPageSage agentic commerce statistics](https://firstpagesage.com/seo-blog/agentic-commerce-statistics), [Adobe via Digital Commerce 360](https://www.digitalcommerce360.com/2026/06/17/adobe-ai-referred-traffic-to-retail-sites-doubles-in-a-year/)
- Protocols and platforms: [AgenticPlug tracker (Oct 2026)](https://agenticplug.ai/current-state-of-agentic-commerce), [Sanbi Q4 checkout tracker](https://sanbi.ai/blog/agentic-commerce-tracker-ai-checkout), [Presenc ACP tracker](https://presenc.ai/research/acp-merchant-adoption-tracker-2026), [TechCrunch: Shopify WebMCP checkout](https://techcrunch.com/2026/09/28/shopify-opens-checkout-to-browser-based-ai-agents/), [Chrome Developers: WebMCP](https://developer.chrome.com/docs/modern-web-guidance/guides/webmcp/webmcp), [Cloudflare: securing agentic commerce](https://blog.cloudflare.com/secure-agentic-commerce/), [FIDO Alliance on AP2](https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/)
- Repo: `reports/` and `research_notes/` (Forter, HUMAN, DataDome, Billions Network, DoorDash, Sensor Tower, Meta Q3 excerpts)
