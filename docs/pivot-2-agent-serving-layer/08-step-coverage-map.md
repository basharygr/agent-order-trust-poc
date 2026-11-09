# 08 — Step coverage map: per step, which agent reaches which merchant, by which route, and what it gets

**Date:** 5 November 2026
**Status:** Draft, all nine steps. Step 1 was reviewed in discussion; steps 2 to 9 were written in one pass against the same structure and await review. Numbers come from doc 06 (traffic and platform shares) and doc 07 Appendix A (A#n); platform facts from `reports/Agent enablement platform coverage.md`. Terms are in `../GLOSSARY.md`; the steps are the nine in `../ECOMMERCE-FLOW.md`.

**What this doc is for.** The flow doc says what each step is and how agents change it. Doc 06 says how much agent traffic there is and where it lands. This doc joins the two: for each step, which agent reaches a merchant on which platform, by which route, with what coverage, weighted by how much traffic and how many orders that agent carries. The point is to see, per step, which cells are heavy, which are served, and which are empty.

---

## 0. The three axes and the weights

**Platforms** (rows). Grouped by what a merchant on them can do, with store and sales share from doc 06 §3.

| Platform | Stores | US sales | Note |
|---|---|---|---|
| Shopify | 22% | ~14% | Default-on everywhere; the heaviest cell on every map |
| WooCommerce | 30% | ~3–5% (E) | 48% of Israeli stores; everything is opt-in or plugin |
| Wix, Squarespace | 9% | <2% | Wix publishes UCP by default; Squarespace has almost nothing documented |
| BigCommerce | 0.2% | <1% | Small by count, the best-served non-Shopify SMB platform |
| Enterprise custom (Salesforce, Adobe, commercetools, self-built) | ~35% | 55–60% incl. Amazon and Walmart | Builds its own; not a target, kept for completeness |

**Agents** (columns). Named, with class and weight. Traffic share is HUMAN September 2026 (share of agentic page traffic); order share is Forter, week of 19 October 2026 (doc 06 §4.2). Protocol agents generate no page traffic by construction.

| Agent | Class | Agent page traffic | Agent orders | Verifiable |
|---|---|---|---|---|
| Google AI Mode, Gemini | Protocol (UCP) | ~0 | ~12% | Yes |
| ChatGPT apps and feeds | Protocol (ACP), referral-only since Mar 2026 | ~0 | part of ~11% | Yes |
| Microsoft Copilot | Protocol (ACP, UCP) | ~0 | part of ~11% | Yes |
| ChatGPT Agent | Signed cloud browser | 28.6% | largest of the ~28% non-Muse scraping share | Yes (Web Bot Auth) |
| Meta Muse | Unsigned cloud browser | 40.3% | 49% | No |
| Instinct, Grok Bot | Unsigned cloud browser | <3% | material, inside the 28% | No |
| Amazon Buy for Me | Proprietary cloud browser | not in HUMAN's table | not in Forter's table | No |
| Perplexity, Comet | Local browser (Comet); own index (Perplexity) | 9.4% | small | No |
| Claude in Chrome | Local browser | 7.1% | small | No |
| Gemini in Chrome, Edge Copilot | Local browser | <3% | ~0 | No |

**Routes** (how the step's data or action travels between merchant and agent). Doc 06 calls these channels; this doc says route, because the glossary uses channel for Google, Meta and the other destinations.

| Route | What travels | Who controls freshness |
|---|---|---|
| Platform catalog push | The platform sends its own copy of the catalog to agent operators (Shopify Catalog) | Platform |
| Channel feed | The merchant, or a plugin, sends a feed to one channel (Google Merchant Center, Microsoft Merchant Center, Meta Catalog, ACP feed, Stripe ACS feed, PayPal Store Sync) | Merchant or plugin for the first hop, channel after |
| Protocol endpoint | The store publishes a well-known profile and an MCP or REST endpoint the agent queries live (UCP, Storefront MCP) | Live |
| Page | The agent's browser or crawler reads the storefront: JSON-LD, DOM, and the platform's public JSON (Shopify `products.json`, WooCommerce Store API) | Live |
| Aggregator | A syndicator or metadata provider holds the merchant's feed and licenses it onward (Feedonomics, affiliate networks, OpenAI's "metadata providers") | Aggregator |

**Per step, seven parts.** (a) per agent: where it gets the step's data; (b) per platform: which routes the merchant's data leaves by; (c) the platform × agent map; (d) reading; (e) numbers and sources; (f) opportunities, with the trap and the risks as discussed; (g) key competitors for those opportunities. Opportunities and competitors are discussion notes, not decisions; the wedge is unsettled (doc 07 §9).

**How to read a cell.** Each per-step map has one cell per platform × agent. A cell says the route the agent uses on that platform, whether it is on by default, and what it gets. Weight of a cell is the agent's column share times the unknown share of that agent's activity that lands on the platform; nobody publishes the second factor, so weights are directional.

---

## Step 1. Catalog and merchandising

**What the step is.** The merchant creates the record: variants, prices, stock, images, descriptions, GTIN, brand, category, and the conditions (shipping zones and rates, delivery promise, returns, warranty, member pricing). Flow doc step 1. At this step nothing is bought; the question is which agents the record reaches, by which route, and how much of it arrives.

### 1a. Per agent: where the agent gets the merchant's record

| Agent | Primary source of the record | Also reads | Conditions it receives | Freshness |
|---|---|---|---|---|
| Google AI Mode, Gemini | Merchant Center feed into the Shopping Graph (~50B listings, web 5 Nov, not in research folder); Shopify Catalog | Page structured data, used as the referee for the feed (automatic item updates) | Shipping and returns from the merchant's Merchant Center account settings, if set; nothing per product | Feed processing in minutes, no SLA; Shopping Graph refresh roughly daily per listing; page crawl for corrections |
| ChatGPT apps and feeds | ACP feed (approved partners only), Shopify Catalog, Stripe ACS feed, Feedonomics exports, metadata providers | Product pages (still pulled more often than feeds: ~20% of offers from feeds per Profound, web 5 Nov, not in research folder) | Shopify Catalog: `ships_to` filter and a refund-policy URL; ACP and Stripe feeds: shipping per country and promotions, no returns field | ACP push at most every 15 min; page crawl otherwise |
| ChatGPT Agent | The page, in a cloud browser | Nothing else | Whatever the page shows at the moment it reads it | Live |
| Microsoft Copilot | Microsoft Merchant Center feed; Shopify Catalog; Feedonomics and PayPal Store Sync exports | Bing crawl of the page | As Google's: account-level, if set | Feed cadence; crawl |
| Meta Muse | The page, in a cloud Chromium from Cloudflare and Fastly IPs; Shopify Catalog on Shopify | Meta's own catalog from ad feeds, unconfirmed as a shopping source | Page prose; Shopify Catalog fields on Shopify | Live on page |
| Instinct, Grok Bot | The page | Shop Pay on Shopify | Page prose | Live |
| Amazon Buy for Me | Amazon's own crawl of brand sites (Shop Direct: 400k+ merchants, 100M+ products, listed without consent, opt-out by email); merchant feeds accepted via Feedonomics, Salsify and CedCommerce since Mar 2026 | The page at purchase time | Page prose | Amazon's cadence |
| Perplexity, Comet | Perplexity's own index; PayPal Store Sync feeds; Shopify integration | Comet reads the page live | Page prose; Store Sync tax and shipping via the merchant's API | Index cadence; live in Comet |
| Claude in Chrome, Claude | Brave index for search; the page in the user's browser | Nothing else | Page prose | Live |
| Gemini in Chrome, Edge Copilot | The page in the user's browser, plus the vendor's index above | — | Page prose | Live |

Reading: every browsing agent, signed or not, cloud or local, gets the record from the page. Only the three protocol agents and Muse-on-Shopify get it from a feed or catalog. No feed or catalog route carries per-product conditions; the page route has a schema.org slot for shipping and returns that Google reads and that almost no store fills (see 1d).

### 1b. Per platform: which routes the merchant's record leaves by

| Platform | Platform catalog push | Channel feeds | Protocol endpoint | Page | Default or opt-in |
|---|---|---|---|---|---|
| Shopify | Catalog to ChatGPT, Copilot, Google, Meta, Shop, "Other"; title, description, options, images, price, availability incl. `running_low`, `ships_to`, refund-policy URL; no shipping rates, returns text, promotions, member pricing; 7-day removal lag | Google and Meta channels in admin | `/.well-known/ucp`, Global Catalog MCP, Storefront MCP, `agents.md`, `llms.txt` on Liquid stores | JSON-LD in standard themes; `products.json` public | All default-on; master switch and per-channel toggles |
| WooCommerce | None | Google for WooCommerce (free listings and ads, syncs on save); Facebook for WooCommerce; third-party feed plugins to anything; Stripe ACS waitlisted | Core MCP (products and orders only, flag off by default); community UCP plugins (~20 verified stores) | JSON-LD only via an SEO plugin; Store API public with price and stock status | Everything opt-in; most long-tail stores have one Google feed or none |
| Wix | None | Google and Meta integrations; Stripe ACS where eligible; PayPal Store Sync | UCP profile default-on with catalog, cart, checkout, fulfilment, discount (~6,700 verified) | Product, Offer and MerchantReturnPolicy markup by default | UCP default-on; feeds opt-in |
| Squarespace | None | Stripe ACS partner, otherwise undocumented | None | Basic markup | Undocumented |
| BigCommerce | None | Merchant Center feed with shipping, returns policy and support contact; Feedonomics Agentic Catalog Exports to OpenAI, Gemini, Copilot, PayPal, Stripe | MCP for all stores; UCP early access; Cloudflare edge WebMCP default-on | Live prices and stock via WebMCP | Mixed; the most for an SMB off Shopify |
| Enterprise custom | None | Own Merchant Center, Microsoft, Meta feeds; ACP feeds as approved partners (Walmart, Target, Best Buy, Home Depot, Sephora); Salesforce Merchant Center feed GA | Self-built UCP and ACP endpoints; 7% of custom profiles valid in May 2026 | Own | Built per retailer |

### 1c. The map: platform × agent, how the record arrives

Legend: **F** feed or catalog, on by default; **f** feed possible but opt-in or plugin; **E** live endpoint; **P** page only; **—** no route. Agents are ordered by order share.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google UCP (~12%) | Copilot, ChatGPT feeds (~11%) | Comet, Claude, Gemini, Edge (local) | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | **F** Catalog + P | P + E (UCP, MCP) | **F** Catalog + Merchant Center + P referee | **F** Catalog + P | P | P (Shop Direct listing) |
| WooCommerce | P (+ f Meta ad catalog, unconfirmed as source) | P | f Google plugin + P | f via Stripe ACS waitlist or third-party feed; else P | P | P |
| Wix | P | P + E (UCP) | f + E (UCP) + P markup | f via Stripe ACS; else P | P | P |
| Squarespace | P | P | f unknown; P | — / P | P | P |
| BigCommerce | P | P + E (MCP, WebMCP) | **F** Merchant Center + Feedonomics | **F** Feedonomics + PayPal Store Sync | P | P |
| Enterprise custom | P where not blocked | P where not blocked, E where self-built | F own feeds | F own feeds and ACP | P | P or blocked |

### 1d. Reading step 1

- **The page is the record for most of the volume.** Muse, ChatGPT Agent, Instinct, Grok, Comet, Claude and Amazon all read the page. By order share that is three in four agent orders, and on WooCommerce, Wix and Squarespace it is every order. The feed and catalog routes carry a quarter of orders and exist by default only on Shopify and BigCommerce.
- **Feeds reach only the channel they were sent to.** A Google feed serves Google. Copilot needs Microsoft's, ChatGPT needs ACP or Shopify or an aggregator. A long-tail merchant with one Google feed is served for one protocol agent and depends on the page for the other nine.
- **Conditions have one standard slot, on the page, and almost nothing fills it.** The feed and catalog routes carry at most a ships-to filter, a refund-policy URL, or account-level shipping settings for one channel; ACP and Shopify's Catalog have no returns field. The page route is different: schema.org gives Offer a `shippingDetails` block (rates, destinations, handling and transit time, free-shipping threshold) and a `hasMerchantReturnPolicy` block (days, fees, method, country), and Google reads both for merchant listings and flags their absence in the Rich Results Test. But Shopify's standard themes and WooCommerce core emit neither; Wix emits the return policy; adding them means editing theme code or installing an SEO app. Only Google is confirmed to consume them; browsing agents read the rendered page, where conditions are prose, and no vendor says it parses these blocks. Warranty and member pricing have no equivalent slot that any agent reads. So the route exists and is empty on nearly every store, which is a Serve problem on top of the Truth problem.
- **Freshness is live only on the routes the merchant does not manage.** The page and the endpoints are live; the feeds and catalogs run on the channel's cadence with no SLA. For the browsing agents the merchant's page accuracy is the freshness contract.
- **Heavy cells:** Muse × Shopify (catalog plus page, Shop Pay), Muse × WooCommerce (page only, no merchant visibility), ChatGPT Agent × everything (page). **Served cells:** Shopify × protocol agents, BigCommerce × protocol agents. **Empty cells:** every protocol column on WooCommerce, Wix and Squarespace without a plugin, which is 40% of stores and 48% of Israel.
- **The record is wrong often enough to matter, before any agent touches it.** DataFeedWatch's 2022 sample of 4.5M Google Shopping items across 15,000 stores found 7% disapproved for data errors. A July 2026 audit of five live Merchant Center accounts (11,107 products) found only 0.19% disapproved but 7.6% carrying Google-flagged data defects while still live. Google's automatic item updates page, revised 27 Sept 2026, says items whose price or availability changes more than once a day may be disapproved rather than updated. On the protocol side, UCP Checker's 1,252 agent sessions across 105 stores and 16 models reached checkout 37.9% of the time, stopped at search 27.4%, failed outright 20.8%, with variant-shape problems about half of the failures; only 7% of custom and headless UCP profiles were valid in May 2026. In the benchmarks, 81.5% of EComAgentBench errors are attribute or variant mismatch. All of these are record problems that start at this step.
- **Protocol endpoints are a Shopify and Wix phenomenon.** UCP Checker's 30 Oct 2026 census: more than 19,000 valid profiles, of which Shopify ~10,950 and Wix ~6,700; BigCommerce 65, Shopware 38, WooCommerce ~20. The endpoint route barely exists on the largest platform by store count.
- **The only lift number for this step is Shopify's own.** Catalog-powered AI search converts at 2× scraped data (Q2 2026 call, no methodology, unverified); its CFO said LLM-started searches are 2.5× more likely to land on a product page with about 80% conversion uplift (Sept 2026, secondary summary). Directional support that a structured record beats a scraped one; nothing comparable exists for the page route.
- **What the merchant can act on at this step:** JSON-LD and public JSON on the page (reaches every agent), one canonical Google-format feed (reaches Google, and via plugins or aggregators the rest), and shipping and returns in the page's schema.org slots (reaches Google today, and any browsing agent that chooses to parse them; the feeds cannot carry them). Warranty, member pricing and delivery promise per destination have no slot and need an endpoint.

---

### 1e. Numbers used in step 1 and where they come from

| Figure | Source in this repo | Origin |
|---|---|---|
| Agent traffic and order shares per agent | doc 06 §4.2; `research_notes/Shopping agent production data 2026/cross_agent_telemetry.md` | HUMAN Sep 2026; Forter wk of 19 Oct 2026 |
| Platform store and sales shares | doc 06 §3 | Store Leads, Marketplace Pulse, Shopify 10-Q, estimates (E) |
| Shopify Catalog: >1B products, fields, `ships_to`, `running_low`, refund-policy URL only, no rates or promotions, 7-day removal lag, no SLA, 1,000-result cap | `research_notes/Agent enablement platform coverage/shopify.md` lines 8, 19, 82–83, 210; report table row Shopify | Shopify docs; Retailgentic |
| Catalog converts 2× scraped; 2.5× product-page landing, ~80% uplift | `research_notes/Agent enablement platform coverage/shopify.md` line 163; `protocol_pattern.md` line 129 | Shopify Q2 2026 call; Goldman Sachs conference 10 Sept 2026 (secondary) |
| Platform routes (Woo MCP flag, Stripe ACS waitlist, Wix UCP default-on, BigCommerce Feedonomics and WebMCP, Squarespace unknown) | `reports/Agent enablement platform coverage.md` platform table; `research_notes/Agent enablement platform coverage/other_platforms.md` | Platform docs and blogs, Oct–Nov 2026 |
| UCP census: >19,000 valid profiles; Shopify ~10,950, Wix ~6,700, BigCommerce 65, Shopware 38, Woo ~20; custom 7% valid (May 2026) | `research_notes/Agent enablement platform coverage/other_platforms.md` line 10; `protocol_pattern.md` line 122 | UCP Checker 30 Oct 2026 and 12 May 2026 |
| UCP playground: 1,252 sessions, 105 stores, 16 models; 37.9% / 27.4% / 20.8%; variant-shape ≈ half of failures | `reports/Agent enablement platform coverage.md` line 71 | UCP Checker |
| Feed quality: 7% disapproved (2022, 4.5M items, 15k stores); 0.19% disapproved and 7.6% defective (Jul 2026, 11,107 items); >1/day changes may be disapproved (Google, 27 Sept 2026) | `research_notes/Shopping agent accuracy trajectory/structural_error_sources.md` lines 115–120 | DataFeedWatch; agency audit; Google help page |
| 81.5% of EComAgentBench errors are attribute or variant mismatch | `reports/Shopping agent accuracy trajectory.md` line 11 | EComAgentBench |
| Amazon Shop Direct: 400k+ merchants, 100M+ products; feeds via Feedonomics, Salsify, CedCommerce | `research_notes/Shopping agent production data 2026/google_amazon.md` lines 66–69 | About Amazon, 11 Mar 2026 |
| Shopping Graph ~50B listings; ChatGPT ~20% of offers from feeds | Not in the research folder; web search 5 Nov 2026 (PPC Land; Profound via Relevant Audience) | Vendor figures, unverified |

### 1f. Opportunities at step 1

Four, one trap, two risks discussed to a position.

1. **The conditions record.** Nobody holds a merchant's conditions as data; they sit in shipping settings, tax settings, customer groups, the promo engine and prose policy pages. Extract them once, normalise into the five groups (glossary: Conditions), and render to every route that has a consumer today: the page's schema.org shipping and returns blocks (Google reads them now and reports on them in Merchant Center; nearly every store leaves them empty, so the before-and-after is measurable); Merchant Center and Microsoft Merchant Center shipping and returns attributes, per product; Stripe's feed fields for shipping per country, fees, tax codes and promotions; and a deterministic conditions paragraph in the product page text beside the price, because browsing agents carry three in four orders and read rendered text. This is the Truth layer for conditions, the one gap doc 06 marks real and unserved on every platform including Shopify, and it is the merchant's own data.
2. **Eligibility rules at order time.** Merchants cancel 27–37% of scraped orders from outside the US because the agent bought what the merchant does not ship (A#32, doc 06 §4.3). The only confirmed order-time failure in production, and it is a conditions check: destination, currency, quantity, promo. The same record answers it without knowing who the agent is. Decide, needing no agent cooperation. Belongs to step 6 in the flow but is fed from here.
3. **The transactability endpoint.** Warranty, member pricing, quantity limits, buyer eligibility and a delivery date per destination have no slot in any feed or page block; they are answered per basket. An endpoint that takes basket plus destination and returns landed cost, eligibility, delivery date and return terms, signed and timestamped. The merchant-side half of the intent contract; UCP's fulfilment and discount capabilities point this way and Wix already publishes them. No agent calls such an endpoint today, so this is a bet on where the protocols go, not a sale this quarter.
4. **The number nobody has.** No measurement exists of how many stores publish shipping or returns markup. A crawl of a few thousand Shopify and WooCommerce stores produces it in days: a research asset, a marketing asset for the audit wedge, and the sizing for opportunity 1.

**The trap.** Filling the schema.org blocks alone is a feature, not a company. Shopify SEO apps and AIOSEO on WooCommerce already offer it, and Shopify could emit the blocks from its return rules and shipping zones in one theme release. The defensible unit is the record plus multi-route rendering plus order-time rules plus the endpoint. Any one piece alone gets absorbed.

**Risks, as discussed.**

- *Google trusts only Merchant Center and ignores page markup.* Not a risk once the record renders to every route: Merchant Center accepts per-product shipping and return attributes, so we render there too. Google does not need any one merchant's data, it needs the class of data, and it consumes whichever route carries it. Residual: the Google route needs a Merchant Center account and feed, which part of the long tail lacks; that is onboarding work.
- *Browsing agents never parse structured blocks.* A bet worth taking: agents read product pages more than feeds, browsing agents read rendered text and nothing else, and OpenAI already ranks on availability and price accuracy, so a readable paragraph is read like any other text. What survives: **placement** (cloud agents trim pages to a token budget; a block beside add-to-cart is read, a footer or policy page often is not; a design rule); **reading is not trusting** (an agent may still verify by building a cart; the paragraph wins only if it matches checkout every time, which is why both must render from one record; trust without a cart needs the signed endpoint, opportunity 3); **ranking is separate** (Muse's leaked instructions rank well-known merchants first; clear conditions make a merchant answerable, not visible; a step 2 problem).
- *The real risk is merchant-side.* Agents reading the data costs nothing to bet on; merchants paying before lift evidence is where it fails, and page-side treatments have no lift evidence at all (doc 06 §7 row 8). **The test:** a few dozen stores, add the conditions block and the schema blocks from a hand-built record, run the five agents (Muse, ChatGPT Agent, Google, Copilot, Comet) with condition-laden questions before and after, score the answers. The audit wedge pointed at one question; the first page-side lift number anyone would have; a week of work.

### 1g. Key competitors at step 1

| Who | What they do at this step | Overlap with the opportunities | Gap they leave |
|---|---|---|---|
| Platform SEO and JSON-LD apps (Shopify: JSON-LD for SEO, Smart SEO, Schema Plus; WooCommerce: AIOSEO, Yoast WooCommerce SEO, Rank Math) | Emit Product and Offer markup; some add shipping and returns blocks from a form the merchant fills | The trap: opportunity 1's page render, as a feature | Hand-entered, one route, no record, no feed or endpoint, nothing at order time |
| Feed managers and syndicators (Feedonomics, Productsup, Channable, DataFeedWatch) | Transform one catalog into many channel feeds; Feedonomics exports to OpenAI, Gemini, Copilot, PayPal, Stripe | Opportunity 1's feed routes; the aggregator route in §0 | Catalog, price, stock only; no syndicator handles conditions (doc 06 §6); enterprise and mid-market pricing |
| Stripe Agentic Commerce Suite, PayPal Store Sync | Hosted feed plus managed checkout for merchants on their rail; Stripe's feed carries shipping per country, fees, tax codes, promotions; PayPal calls the merchant's API for tax and shipping | The nearest thing to a conditions route and to opportunity 3 | Only for their PSP's merchants, waitlisted, US and select countries, no returns field, no Israeli PSP |
| Shopify | Catalog with `ships_to` and a policy URL; return rules object; could emit the schema blocks natively | Could absorb the page render for 22% of stores overnight | Has not in years; no shipping rates, returns text, promotions or member pricing in Catalog; nothing for the other 78% |
| Google Merchant Center | Per-product shipping and return attributes; automatic item updates from page markup; AI readiness insights (May 2026) | Owns the Google consumer of opportunity 1 | One channel; the merchant still has to produce the data |
| Botify (AgenticCatalog, PageWorkers) | Crawl-based agent feeds and edge page rewrites for enterprise sites | Multi-route rendering, at the top of the market | Enterprise only, crawl cadence, no conditions, no long tail |
| PIM vendors (Salsify, Akeneo) | Product record of truth for brands and large retailers | The record idea, for catalog not conditions | Enterprise; product attributes, not terms of sale |
| Readiness checkers and GEO agencies (UCP Checker, Paz.ai, agencies selling "AI visibility") | Audit a store's agent readiness; sell optimisation | Opportunity 4 and the audit wedge | Measure protocols and markup presence, not conditions or outcomes; no product behind the audit |

Reading: the page render is contested by cheap apps, the feed route by well-funded syndicators, and the rail-based route by Stripe and PayPal for their own merchants. Nobody holds the record, nobody renders it to all routes, and nobody serves the long tail off Shopify. That is the space, and it is only defensible as the whole.

---

## Step 2. Discovery

**What the step is.** The shopper, or the agent, decides which merchants become candidates. Flow doc step 2. The question here is which agents can find a merchant on which platform, how they rank it, and what the merchant can see of its own position.

### 2a. Per agent: how it finds and ranks merchants

| Agent | Where candidates come from | What is known about ranking | Ads | Merchant visibility into rank |
|---|---|---|---|---|
| Google AI Mode, Gemini | Shopping Graph (Merchant Center feeds, page markup), organic index | Merchant Center quality signals; "majority of AI Mode queries are incremental" and upper-funnel (Schindler, Q3 call); one unverified study says AI Mode showed prices 21.6% higher than Search | Shopping ads in AI Mode | Merchant Center AI performance insights (27 May 2026): share of voice and attribute completeness; no per-SKU answer report |
| ChatGPT apps and feeds | Own product index (Labrador, reported) from ACP feeds, Shopify Catalog, metadata providers and crawl | Stated signals for merchants selling the same item: availability, price, quality, maker or primary seller, checkout integration enabled; position-1 card takes 43% of picks (Clickstream); ~80% of AI product recommendations in one eight-category sample (Jellyfish) | Ads in 89 countries, 100,000+ advertisers | None; tracking parameters sometimes missing on desktop links (Retailgentic, Oct 2026) |
| ChatGPT Agent | Whatever ChatGPT search returns, then browses | As above | As above | None |
| Microsoft Copilot | Bing index, Microsoft Merchant Center, Shopify Catalog (500,000+ merchants in Copilot Checkout's catalog) | Undisclosed | Microsoft Advertising | Clarity AI Visibility: citations and share of authority, no orders |
| Meta Muse | Scrape plus Shopify Catalog | Leaked system prompt (Profound, 25 Sept): rank well-known merchants first, told three times; first instruction is to save the shopper money; Shop Pay partners complete at 3.5× | Meta's ad business; no Muse ads documented | None |
| Instinct, Grok Bot | Scrape; unknown index | Undisclosed | None known | None |
| Amazon Buy for Me | Shop Direct listing, built by Amazon without merchant consent | Amazon's own | Amazon's own | None; opt-out by email |
| Perplexity, Comet | Own index; PayPal Store Sync and merchant program feeds | Undisclosed | Sponsored answers | None |
| Claude, Claude in Chrome | Brave index | Undisclosed | None | None |
| Gemini in Chrome, Edge Copilot | The vendor's index above, in the user's browser | As vendor | As vendor | None |

Reading: nine of ten agents publish no ranking method and no rank report. What is disclosed favours integration (checkout enabled, Shop Pay partner) and reputation (well-known merchants first). Two vendor studies disagree on concentration: Lightspeed found a national retailer chosen up to 94% of the time; Recomaze found the top brand at 1.3%. The same query produces a different shelf per assistant and per run (WARC).

### 2b. Per platform: which agents' indexes the merchant is in

| Platform | In the protocol agents' indexes | In the crawl-based indexes | Blocking posture | Default or opt-in |
|---|---|---|---|---|
| Shopify | ~3M stores auto-enrolled into ChatGPT, Copilot, Google, Meta (A#20); Perplexity via Shopify integration | Crawlable; `agents.md`, `llms.txt`, UCP profile on Liquid stores | None by default; rate limits for unsigned traffic since May 2026 | Default-on |
| WooCommerce | Google via the Google plugin, if installed; Meta via the Facebook plugin, if installed; nothing for ChatGPT or Copilot without a feed plugin or Stripe ACS | Crawlable unless a security plugin blocks bots | Varies; many security plugins block unknown bots | Opt-in |
| Wix | Google and Meta integrations, opt-in; UCP discovery default-on (~6,700 verified profiles) | Crawlable; Product, Offer, MerchantReturnPolicy markup | Wix-managed | Mixed |
| Squarespace | Undocumented | Crawlable | Squarespace-managed | Unknown |
| BigCommerce | Google, Copilot, OpenAI, PayPal, Stripe via Feedonomics exports | Crawlable; WebMCP on ~100k domains | Cloudflare at the edge | Opt-in, well tooled |
| Enterprise custom | Own feeds; ACP as approved partners | 37 of 51 large retailers block agents, most by legacy robots rules; 0 of 51 has a verified-agent route (A#25); blocked share of agent traffic 8.2% Apr to 21% Jun (A#26); >90% of commerce AI-bot traffic in monitor-only mode at Akamai (A#41) | Deliberate or accidental | Built per retailer |

### 2c. The map: platform × agent, can the agent find the merchant

Legend: **F** in the agent's index by feed or catalog, default; **f** by feed, opt-in; **C** crawl only, found if crawled; **B** commonly blocked; **—** no path.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google (~12%) | Copilot, ChatGPT feeds (~11%) | Local browser agents | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | **F** + C | C (ChatGPT Agent via ChatGPT's index, which has the Catalog) | **F** | **F** | C | C (Shop Direct) |
| WooCommerce | C | C | f + C | f or C | C | C |
| Wix | C | C | f + C, UCP discovery | f or C | C | C |
| Squarespace | C | C | C | C | C | C |
| BigCommerce | C | C | **F** (Feedonomics) | **F** (Feedonomics, PayPal) | C | C |
| Enterprise custom | C or B | C or B | F | F | C | C or B |

### 2d. Reading step 2

- **Discovery is the step agents have already taken.** AI-referred visits went from 2.9% to 5.5% of US retail visits in nine months and convert 1.6× (A#1, A#2); AI referrals worldwide averaged 770.7M visits a month, up 117% year on year (Similarweb); 61% of AI shoppers research on one platform and buy on another (CivicScience). The volume is at the top of the funnel and it is here.
- **Channel entry is solved for Shopify and open for everyone else.** Shopify put three million stores into four agent indexes with one switch. A WooCommerce store is in Google's index if it installed the plugin and in nobody else's without more work. Rank is unknown on both.
- **Ranking favours what Truth and Serve produce.** OpenAI's disclosed signals are availability, price, quality, primary seller and checkout integration. Three of the five are data accuracy; the fourth is GTIN and brand ownership; the fifth is the platform's. Muse's instructions favour known merchants, which a small store cannot change, and saving money, which it can.
- **The shelf is short, winner-take-most, and different each run.** Position 1 takes 43% of picks in ChatGPT. No merchant can test its rank reproducibly.
- **Sponsored bias is arriving.** ChatGPT sells ads in 89 countries. Most of 23 models tested favoured pricier sponsored options over the best deal in a booking task (Princeton and UW).
- **Accidental blocking is an enterprise problem now and a long-tail problem by plugin.** 37 of 51 large retailers block by legacy rules. WordPress security plugins do the same by default on many small stores; unmeasured.
- **The merchant sees nothing.** Referral clicks, sometimes without tracking parameters, and no impressions, position or share of answers. Step 9.
- **Heavy cells:** Muse × Shopify (catalog and crawl), ChatGPT × Shopify (Catalog in the index), every agent × WooCommerce by crawl. **Empty:** protocol columns on WooCommerce, Wix and Squarespace without a plugin.

### 2e. Numbers used in step 2

| Figure | Source in this repo | Origin |
|---|---|---|
| AI-referred visits 2.9% → 5.5%, 1.6× conversion; +138% YoY | doc 07 A#1–3 | FirstPageSage; Adobe |
| Shopify ~3M auto-enrolled | A#20 | Shopify Help Center |
| 37 of 51 block, 0 verified route; 8.2% → 21% blocked; >90% monitor-only | A#25, A#26, A#41; `research_notes/Agent enablement platform coverage/browsing_pattern.md` lines 58, 64 | Billions Network; HUMAN; Akamai |
| OpenAI ranking signals; ads in 89 countries, 100k advertisers; position 1 takes 43% of picks; missing tracking parameters | flow doc step 2; `research_notes/Shopping agent production data 2026/openai_microsoft.md` lines 16, 18, 23, 28 | PPC Land; OpenAI; Clickstream Solutions; Retailgentic |
| Muse prompt: well-known merchants first, save money | `research_notes/Shopping agent production data 2026/perplexity_meta_others.md` line 45 | Profound leak, reported 8 Oct 2026 |
| Copilot Checkout catalog 500,000+ merchants | `openai_microsoft.md` line 102 | Microsoft, Apr 2026 |
| Lightspeed 94%, Recomaze 1.3%, Jellyfish 80%, WARC different shelves, Princeton/UW sponsored bias | flow doc step 2 links | Vendor and academic studies |
| Similarweb 770.7M AI referral visits/month; CivicScience 61% research-here-buy-there; AI Mode prices +21.6% (unverified) | `openai_microsoft.md` lines 19, 139; `google_amazon.md` line 47 | Similarweb; CivicScience; unnamed study |

### 2f. Opportunities at step 2

1. **Channel entry for the long tail.** One canonical Google-format feed, generated from the Truth record, pushed to Google, Microsoft, Meta, Stripe and PayPal, and to an aggregator for ChatGPT. This is what Shopify did for its merchants with one switch and nobody does for the other 78% of stores. The enablement report's step order for a non-Shopify merchant starts exactly here.
2. **Where do I stand.** Run the five agents that carry ~90% of orders (Muse, ChatGPT Agent, Google, Copilot, Comet) against a merchant's query set and report presence, position and which competitor won, per run, over time. The audit wedge. Because the largest agent is unsigned, any headless Chromium from a cloud IP reproduces what the merchant sees. No rank report exists from any agent.
3. **Reachability audit.** Check that each agent's fetcher and browser can actually reach the product pages: robots rules, security plugins, edge bot rules, rate limits. Cheap, unique for small stores, and a prerequisite for everything else.
4. **Ranking-signal readiness.** The disclosed signals are data accuracy, primary-seller status and checkout integration. Truth produces the first; GTIN ownership through the brand's GS1 prefix proves the second; the platform decides the third. A readiness score against the disclosed signals, with the fix for each, is the sales motion for opportunity 1 of step 1.

**The trap.** Rank tracking alone is a crowded SaaS category already (2g), and GEO content agencies sell optimisation against a ranking nobody can see. The defensible version is the one where the audit is the front end of the record and the rendering.

**Risks.** Ranking is opaque and moves; a readiness score against disclosed signals can be wrong the week OpenAI changes them. Ads may crowd out organic placement faster than on Google, which would turn discovery into a bidding problem the merchant pays the agent for, not us. Shopify owns channel entry for a fifth of stores and could extend its switch to more channels, which would not touch the long tail.

### 2g. Key competitors at step 2

| Who | What they do | Overlap | Gap they leave |
|---|---|---|---|
| AI visibility trackers (Profound, Peec AI, Scrunch, Semrush and Ahrefs AI toolkits, Microsoft Clarity AI Visibility, Wix AI Visibility Overview) | Track citations and share of voice across assistants for brands | Opportunity 2 | Built for brands and enterprise marketing, priced that way; measure mentions, not whether an agent can buy; no fix behind the report |
| GEO and AEO agencies | Rewrite content for AI answers | Opportunity 4, as services | No data layer; cannot change availability, price accuracy or checkout integration, which are the disclosed signals |
| Feed managers (Feedonomics, Productsup, Channable, DataFeedWatch) and the Google and Meta plugins for WooCommerce | Get the catalog into channels | Opportunity 1 | One channel per plugin; no conditions; enterprise pricing for the managers |
| Shopify | Auto-enrolment; agentic analytics | Owns channel entry for 22% of stores | Nothing for the rest; no rank |
| Google Merchant Center AI insights | Share of voice and attribute completeness for Google surfaces | Opportunity 2 and 4, for one agent | One agent; no fix |
| Cloudflare, Akamai, DataDome, HUMAN | Bot rules and agent categories at the edge | Opportunity 3 from the blocking side | Enterprise; they decide who gets in, they do not tell the merchant who it is missing |
| Botify | Enterprise crawl optimisation and agent feeds | Opportunities 1 and 2 at the top of the market | Enterprise only |

---

## Step 3. Evaluation (lookup)

**What the step is.** The agent reads the candidate's product and asks the questions that decide the order: variant, price, stock, ships-to, landed cost, delivery date, returns, fit and compatibility. Flow doc step 3. About 95% of agent activity on commerce sites happens here (A#8).

### 3a. Per agent: how it reads the product

| Agent | What it reads | Structured lookup available | What it cannot get | Load it puts on the store |
|---|---|---|---|---|
| Google AI Mode, Gemini | Shopping Graph record; page as referee; UCP `get_product` on Shopify and Wix | Yes on UCP stores, with buyer context (country, postal code) | Per-product shipping rates, returns text, delivery date, warranty, member pricing | Crawl |
| ChatGPT apps and feeds | Product index record; product page | ACP product data where fed | Same | Crawl (OAI-SearchBot) |
| ChatGPT Agent | The page DOM in a cloud browser | None | Anything JavaScript hides; conditions in prose only | Page fetches at machine rate |
| Microsoft Copilot | Merchant Center record; Shopify Catalog; Bing crawl | Via Shopify | Same as Google | Crawl |
| Meta Muse | The accessibility tree of the page only (Botify); Shopify Catalog on Shopify | Catalog on Shopify | Anything not in the accessibility tree: images, JavaScript-rendered prices, hidden variant selectors | 84% of its requests hit product or search pages; 99.3% from Cloudflare and Fastly IPs |
| Instinct, Grok Bot | Page DOM | None | Same as ChatGPT Agent | Machine-rate fetches |
| Amazon Buy for Me | Page at purchase time | None | Same | Unknown |
| Perplexity, Comet | Index record; page live in Comet | Store Sync merchant API for tax and shipping | Conditions beyond Store Sync | Human-rate in Comet |
| Claude in Chrome | Page in the user's browser | None | Same | Human-rate |
| Gemini in Chrome, Edge Copilot | Page in the user's browser | None | Same | Human-rate |

Reading: the structured lookup route exists only on Shopify, Wix and self-built endpoints, and only for Google and Copilot in practice. Everyone else reads the page, and the largest reads only its accessibility tree.

### 3b. Per platform: what the merchant exposes at lookup

| Platform | Structured lookup | Store facts | Page readability | Rate limits |
|---|---|---|---|---|
| Shopify | Global Catalog MCP: `search_catalog`, `lookup_catalog`, `get_product` with buyer context; returns `price`, `list_price`, `availability` with `running_low`, `eligible.native_checkout`, `ships_to`; 1,000-result cap, estimate-only counts | Knowledge Base and `/ask`: store-level facts (return window, contact), not product-level | Standard themes readable; hidden variant stock and client-rendered prices reported by merchants | Tightened for unsigned traffic May 2026; higher tiers for Web Bot Auth signers |
| WooCommerce | Store API: products, prices, stock status, public; core MCP products only, flag off | None | Depends on theme and plugins; unaudited | Host-dependent; shared hosting slows under machine-rate fetches |
| Wix | UCP catalog over `wixapis.com` and `/_api/mcp`: titles, variants, prices, stock status | None | Markup by default | Wix-managed |
| Squarespace | None | None | Basic | Managed |
| BigCommerce | MCP for all stores; WebMCP live products, prices, stock, sizes | None | Edge-injected tools for in-browser agents | Cloudflare |
| Enterprise custom | Self-built; 7% valid | Own | Own; Mastercard's L1 guide prescribes readability, nobody enforces | Own |

### 3c. The map: platform × agent, how the product is read

Legend: **S** structured lookup with buyer context; **s** structured without conditions; **P** page, full DOM; **A** accessibility tree only; **R** rate-limited or blocked; **—** no path.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google (~12%) | Copilot, ChatGPT feeds (~11%) | Local browser agents | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | s (Catalog) + A | P, R for unsigned | S (UCP, `ships_to`) | s | P | P |
| WooCommerce | A | P | s (Store API, if crawled) | P | P | P |
| Wix | A | P | S (UCP catalog) | P | P | P |
| Squarespace | A | P | P | P | P | P |
| BigCommerce | A, WebMCP if it uses it | P, WebMCP | s (MCP) | s | P, WebMCP | P |
| Enterprise custom | A or R | P or R | own | own | P | P or R |

### 3d. Reading step 3

- **This is where the activity is and where the mistakes originate.** 95% of agent activity; Catalog-fed search converts 2× scraped (A#22); half of failed UCP sessions are variant and ID shape (A#23); the first public retreat, OpenAI's Instant Checkout, was over stale and inaccurate item data (A#24); 81.5% of EComAgentBench errors are attribute or variant mismatch.
- **Conditions are the deciding factor and nobody serves them.** Two merchants show the same item; the one that answers "ships to Israel, 14-day returns" as data gets recommended. The enablement report ranks the pre-checkout transactability verdict as gap 1 across every platform and every feed.
- **The largest agent reads the least.** Muse reads the accessibility tree. A price rendered by JavaScript, a variant selector built as styled divs, a cookie wall, a pop-up, all stop it. Nobody audits themes for this (gap 11).
- **Product-level questions have no source.** Fitment, compatibility, safety. Shopify's Knowledge Base covers store facts only (gap 12).
- **Freshness has no contract at the one moment it matters.** Step 1 covered the feeds; here the page is read live, and a page that is true at read time is the only freshness contract a merchant can offer (gap 8).
- **Load is real on the long tail.** Machine-rate fetches from cloud agents slow a shared-hosting store; Shopify rate-limits unsigned traffic, WooCommerce hosts do whatever they do.
- **Heavy cells:** Muse × everything by accessibility tree; ChatGPT Agent × everything by DOM. **Served:** Google × Shopify and Wix by UCP lookup with buyer context, the only cells where a destination question gets a structured answer. **Empty:** structured lookup for every browsing agent on every platform.

### 3e. Numbers used in step 3

| Figure | Source in this repo | Origin |
|---|---|---|
| 95% discovery and research; 84% of Muse requests on product or search pages; 99.3% from Cloudflare/Fastly | A#8, A#14 | HUMAN; DataDome |
| Catalog 2× scraped; UCP 2.7×; Muse+Shop Pay 3.5× | A#22 | Shopify; UCP Checker; Shopify/Meta, uncontrolled |
| Half of failed UCP sessions variant/ID | A#23 | UCP Checker May 2026 |
| Instant Checkout retreat; "often inaccurate" | A#24 | Walmart; CNBC |
| Shopify Catalog fields and caps; Knowledge Base store facts; rate limits | `research_notes/Agent enablement platform coverage/shopify.md` lines 19, 82–83 | Shopify docs; Retailgentic |
| Muse reads accessibility tree only | `reports/Agent enablement platform coverage.md` gap 11 | Botify |
| Gaps 1, 8, 11, 12 | `reports/Agent enablement platform coverage.md` ranked gaps table | Report |
| 81.5% attribute/variant errors | `reports/Shopping agent accuracy trajectory.md` line 11 | EComAgentBench |
| >90% monitor-only | A#41 | Akamai |

### 3f. Opportunities at step 3

1. **The transactability verdict.** Per variant, per destination: ships-to at postcode level, landed cost, delivery date, returns window, promo validity, stock-confidence age. Gap 1 in the enablement report, shared by every platform and every feed. Served two ways from the one record: as a signed endpoint for agents that call endpoints, and as the conditions block on the page for the browsing agents that carry three in four orders. This is opportunity 3 of step 1 seen from the agent's side.
2. **Theme readability audit and fix.** Run Muse's view (accessibility tree) and ChatGPT Agent's view (DOM) of each product page, list what each cannot see, and fix the theme: server-rendered prices, real select elements for variants, no cookie wall for verified fetchers. Gap 11; no platform audits it; Mastercard prescribes it. Measurable with the same five-agent harness.
3. **Product-level answers.** A per-product Q&A block built from the merchant's own knowledge (fitment tables, compatibility lists, safety notes) rendered as text and as structured data. Gap 12; the merchant has this knowledge and nowhere to put it.
4. **The freshness signal.** An "as of" timestamp and a stock-confidence bucket on the page and in the endpoint, so an agent can tell a live answer from a cached one. Gap 8; protocols rely on the live endpoint to absorb staleness and no feed publishes an SLA.

**The trap.** "Make your site AI-readable" is already a service category with no evidence behind it, and llms.txt is its emblem (no major agent reads it). The defensible version is measured: before-and-after agent answers, not a checklist.

**Risks.** Shopify's Knowledge Base and inferred fields could grow into product-level facts for its stores, taking opportunity 3 for a fifth of the market. Browsing agents could get better at reading bad pages faster than merchants fix them, which shrinks opportunity 2 but not 1. And the endpoint half of opportunity 1 has no caller today; the page half does.

### 3g. Key competitors at step 3

| Who | What they do | Overlap | Gap they leave |
|---|---|---|---|
| Shopify (Global Catalog MCP, Knowledge Base, `/ask`, inferred fields) | Structured lookup with buyer context; store-level facts | Opportunities 1 and 3 for Shopify stores | No rates, returns text, promotions, member pricing; store facts only; nothing off Shopify |
| Botify PageWorkers at Edge | Rewrites pages at the edge for crawlers and agents | Opportunity 2, enterprise | Enterprise; crawl cadence; no conditions |
| Cloudflare WebMCP injection (BigCommerce) | Exposes live products, prices, stock to in-browser agents | Opportunities 1 and 2 at the edge | Platform-specific; no conditions; no payment |
| Firmly Connect | No-code connection of a merchant to agents and agentic channels | Opportunity 1, as a connector | Enterprise US retailers; pricing unpublished |
| Accessibility and site-audit tools (Lighthouse, axe, Siteimprove) | Audit DOM accessibility | Opportunity 2's measurement | Built for humans with assistive tech, not agents; no commerce semantics |
| Storefront chatbots (Gorgias, Tidio, Shopify Sidekick) | Answer shopper questions on the page | Opportunity 3's content | Answer humans in a widget; invisible to agents |
| Product enrichment (Lily AI, Salsify, Akeneo) | Enrich attributes for retail | Opportunity 3's data | Enterprise brands; attributes, not transactability |

---

## Step 4. Cart

**What the step is.** Items, quantities, a subtotal, a promo field, sometimes a shipping estimate. Flow doc step 4. For an agent the cart is where intent meets availability and the compromise is made.

### 4a. Per agent: how it builds the cart

| Agent | How the cart is built | Promo and discount | Shipping before address | What the merchant sees |
|---|---|---|---|---|
| Google AI Mode, Gemini | UCP cart via MCP on Shopify, Wix, BigCommerce; Universal Cart across merchants (US, announced May 2026) | UCP discount capability where declared | Only after address in the checkout session | A cart session on UCP stores; nothing elsewhere |
| ChatGPT apps and feeds | ACP checkout session with line items, since March 2026 handed to the merchant's checkout by referral | Merchant's | Merchant's checkout | A referral |
| ChatGPT Agent | Adds to cart through the DOM like a shopper | Types codes if told | Reads what the cart page says | A cart from a cloud IP |
| Microsoft Copilot | ACP checkout session via PayPal, Stripe or Shopify | Merchant's | After address | Session |
| Meta Muse | Adds to cart through the accessibility tree; 8.9% of its requests touch cart, login or payment | Unknown | Reads the page | A cart from a Cloudflare or Fastly IP, no user agent |
| Instinct, Grok Bot | DOM | Unknown | Page | Cloud-IP cart |
| Amazon Buy for Me | DOM at purchase time | None | Page | A cart that becomes an order with a relay email |
| Comet, Claude in Chrome, Gemini in Chrome, Edge Copilot | DOM in the user's browser, user watching | User's | Page | A human-looking cart |

### 4b. Per platform: the cart surfaces

| Platform | Protocol cart | Public cart API | Basket rules the merchant can set | Cross-sell and bundles |
|---|---|---|---|---|
| Shopify | Cart and Checkout MCP; discounts, shipping options, Shop Pay | Storefront API | Shopify Functions (developer); minimum and maximum quantity via apps | Theme and apps; invisible to protocol agents |
| WooCommerce | Community UCP plugins only | Store API cart, public, used by scrapers | Plugins, mostly page-side (step 5) | Plugins; page-side |
| Wix | UCP cart and discount capabilities, default-on | Via UCP | Wix settings | Page-side |
| Squarespace | None | None | Settings | Page-side |
| BigCommerce | WebMCP cart via Cloudflare, no payment; UCP early access | Storefront API | Settings and apps | Page-side |
| Enterprise custom | Self-built | Own | Own | Own |

### 4c. The map: platform × agent, how the cart is made

Legend: **C** protocol cart with discounts and shipping options; **c** protocol cart without payment; **D** DOM cart in a cloud browser; **A** accessibility-tree cart; **U** user's own session; **—** no path.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google (~12%) | Copilot, ChatGPT feeds (~11%) | Local browser agents | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | A (Shop Pay partner) | D | **C** | **C** / referral | U | D |
| WooCommerce | A | D | — (plugin only) | — | U | D |
| Wix | A | D | C (discount declared) | — | U | D |
| Squarespace | A | D | — | — | U | D |
| BigCommerce | A, c via WebMCP | D, c | c (early access) | via Stripe or PayPal | U, c | D |
| Enterprise custom | A or blocked | D or blocked | own | own | U | D or blocked |

### 4d. Reading step 4

- **The cart is built by software from language.** Multi-constraint intents ("under $200, by Friday, blue if possible") are where agents fail most: 30.4% on the constraint-heavy benchmark split and 48.2% overall on ShoppingBench (A#34). The merchant did not participate and the human has not yet looked.
- **Single-item carts are a symptom.** Google's single-item checkout has 10% customer cancellations against 1–2% elsewhere, with baskets 56% below the $227 agent average (A#31). An agent that cannot assemble the basket in one shop makes a smaller, worse cart.
- **No shipping until address, on every route.** Protocol sessions return shipping options after the address; cart pages say "calculated at checkout." The landed cost that decides the order is unknown at the step where the basket is assembled. The free-shipping threshold, the merchant's main basket lever, is prose.
- **Price and promo drift.** The cart holds a price read earlier; checkout re-prices; no freshness token tells either side. Google hard-fails a purchase on any price increase.
- **The merchant's cart logic is lost.** Upsell, bundles and quantity rules live on the cart page. Protocol agents never load it; browsing agents ignore it.
- **Abandonment is invisible.** 2.3–2.4% of agent activity reaches checkout, about the human conversion rate (A#8); an agent that gives up leaves no email and no pixel.
- **Heavy cells:** Muse × Shopify by accessibility tree with Shop Pay; browsing carts everywhere. **Served:** Google × Shopify and Wix with discounts and shipping options after address. **Empty:** protocol carts on WooCommerce and Squarespace.

### 4e. Numbers used in step 4

| Figure | Source in this repo | Origin |
|---|---|---|
| ShoppingBench 48.2%, Coupon & Budget 30.4%; EComAgentBench 57.1% | A#34 | Benchmark papers |
| Customer cancellation 1–2%, Google 10%, baskets 56% below $227 | A#31 | Forter |
| 2.3–2.4% of agent activity at checkout; 8.9% of Muse requests at cart, login or payment | A#8 | HUMAN; DataDome |
| Google hard-fails on price increase; merchant responsible for wrong items | `research_notes/Shopping agent production data 2026/google_amazon.md` line 51 | Google UCP terms |
| Universal Cart announced May 2026 | `google_amazon.md` line 20 | Google I/O |
| Platform cart surfaces | `reports/Agent enablement platform coverage.md` platform table | Platform docs |

### 4f. Opportunities at step 4

1. **The basket quote.** Given a basket and a destination, return landed cost, eligibility, delivery date, return terms, and the threshold hint ("add $12 for free shipping"). The transactability verdict of step 3 applied to a basket instead of an item. Signed and timestamped so checkout can verify the cart was priced on fresh data. The merchant-side half of the structured intent contract.
2. **Basket policy as data.** Minimum order, quantity limits per item and per order, split-shipment rules, bundle definitions, promo rules with their exclusions. Today prose and cart-page scripts; as data they reach protocol carts and browsing agents alike, and they are the same rules Decide enforces at step 6.
3. **The freshness token.** A price-as-of and stock-as-of carried from lookup through cart to checkout, so a protocol session or an order hook can tell drift from error. Gap 8 again, at the point where it causes cancellations.
4. **Cross-sell as data.** Bundles and "goes with" relationships in the record so an agent assembling a multi-item intent can complete it in one shop. Small today; it is the agent-era version of the merchant's cart-page upsell.

**The trap.** Cart-abandonment tooling (emails, exit pop-ups, retargeting) has no agent-era equivalent and should not be rebuilt; an agent leaves nothing to recover. The value is before the cart, in the quote, not after it.

**Risks.** Protocols define the cart and may add quotes themselves: UCP already returns shipping options after address, and a pre-address quote is a small step for Google or Shopify to take. Agents may not call an extra endpoint; the page render of the policy is the fallback that always reaches browsing agents.

### 4g. Key competitors at step 4

| Who | What they do | Overlap | Gap they leave |
|---|---|---|---|
| Google Universal Cart, UCP cart and discount | Cross-merchant cart and per-merchant cart sessions | Opportunity 1 for Google's surfaces | US; shipping after address; no pre-quote; Google's surfaces only |
| Shopify Cart and Checkout MCP | Cart with discounts and shipping options for agents | Opportunities 1 and 2 on Shopify | After address; Shopify only; policies via Functions need a developer |
| Stripe ACS, PayPal Store Sync | Hosted checkout sessions that compute tax and shipping | Opportunity 1 on their rails | Their merchants; after address; no returns |
| Upsell and bundle apps (Rebuy, Bold, Frequently Bought Together) | Cart-page cross-sell | Opportunity 4's content | Page widgets; invisible to agents |
| Shipping-rate and threshold apps (ShipperHQ, Shopify Shipping) | Compute rates by zone and basket | Opportunity 1's rate engine | Run inside checkout; no agent-facing surface |
| Firmly, Feedonomics | Connect carts and catalogs to agents | Opportunity 1, enterprise | Enterprise |

---

## Step 5. Checkout

**What the step is.** Identity, address, shipping method, payment, tax, total, confirm. Flow doc step 5. For agents the question is whether there is a door at all, who holds it, and what identity comes through it.

### 5a. Per agent: how it checks out

| Agent | Checkout route | Buyer countries | Payment instrument | Identity the merchant receives | Who approves |
|---|---|---|---|---|---|
| Google AI Mode, Gemini | UCP checkout on enrolled stores; AP2 mandates | US, Canada, Australia (A#27) | Google Pay | Platform-supplied buyer details | Shopper in Google's surface |
| ChatGPT apps and feeds | Referral to the merchant's own checkout since March 2026 (A#24); merchant apps | Anywhere the merchant ships | Merchant's | Guest or the shopper's own account | Shopper on the merchant's page |
| ChatGPT Agent | Fills the merchant's checkout form in a cloud browser; Shopify Checkout WebMCP for signed agents with Shop Pay cards (A#21) | Anywhere | Whatever the user gave it, often a virtual card | Whatever the user gave it; cloud IP | Shopper in ChatGPT, sometimes takes over for payment |
| Microsoft Copilot | ACP checkout via PayPal, Stripe or Shopify | US | PayPal, Stripe, Shop Pay | Platform-supplied | Shopper in Copilot |
| Meta Muse | Shop Pay on partner stores (3.5× completion, A#22); fills the form elsewhere and pays with a Stripe Link virtual card; PayPal for PayPal merchants | US, CA, MX, BR, JP, SG for direct; anywhere by browser | Shop Pay or Link virtual card | Shop Pay customer on partners; otherwise guest from a Cloudflare or Fastly IP with no user agent | Shopper in Muse; spending limits in the user's hands |
| Instinct, Grok Bot | Fills the form; Shop Pay in agents from Fall '26 | Anywhere | Virtual cards; Shop Pay | Guest, cloud IP | Shopper in the app |
| Amazon Buy for Me | Fills the form | Anywhere Amazon offers it | Amazon's encrypted payment | Name, address, phone, per-order relay email | Shopper in Amazon's app |
| Perplexity, Comet | Comet fills the form in the user's browser; PayPal for Store Sync merchants (US, USD) | Anywhere | User's | User's own session | User watching |
| Claude in Chrome, Gemini in Chrome, Edge Copilot | Fill the form in the user's browser | Anywhere | User's | User's own session | User watching |

Reading: in-agent checkout exists for buyers in three countries plus six through Meta, and for merchants on the platforms and PSPs admitted to each program. Everyone else reaches the merchant's checkout by browser, where the agent becomes the customer (flow doc step 5).

### 5b. Per platform: the doors the merchant has

| Platform | Protocol checkout | Browser-agent checkout | Controls | Israel and non-US |
|---|---|---|---|---|
| Shopify | UCP and ACP via Checkout MCP: Google (US, CA, AU), Copilot (US), Meta (six countries); ChatGPT referral-only | Checkout WebMCP for signed agents, 28 Sep 2026; unsigned agents fill the form like anyone; "no documented way to turn off browser agents on your checkout" | Master switch, per-channel toggles; nothing per product beyond Unlisted | Discovery yes, in-agent checkout no; Shop Pay in agents not country-documented |
| WooCommerce | None native; Stripe ACS waitlisted since Dec 2025; "Enable agentic commerce" toggle with a warning to check payment-method support | Scrapers fill the form and pay with virtual cards; merchant-added rules on the page do not run for protocol orders and may not for browsers | REST API scopes | Nothing; no Israeli PSP supports any agent payment protocol (A#28) |
| Wix | UCP with empty `payment_handlers`; agent gets `continue_url` to Wix checkout; Stripe ACS where eligible | Fill the form | None documented | Wix Payments not offered in Israel |
| Squarespace | Stripe ACS partner, undocumented | Fill the form | Unknown | Nothing |
| BigCommerce | UCP early access Q4 2026; WebMCP hands a checkout link; Stripe ACS, PayPal | Fill the form | Settings menu for AI connections; per-SKU `native_commerce` | Google product eligibility US, CA, AU |
| Enterprise custom | Self-built UCP and ACP; Walmart, Target, Wayfair behind flags | 37 of 51 block, often by accident; partners (Wayfair, Best Buy) via Stripe Link or PayPal | Own bot management | Own |

### 5c. The map: platform × agent, can the agent complete a purchase

This is doc 06 §5 with the step's legend. **N** native in-agent checkout; **W** WebMCP-assisted browser checkout; **B** browser fills the form, merchant unaware; **R** referral to the merchant's checkout; **X** blocked; **—** no path.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google (~12%) | Copilot, ChatGPT feeds (~11%) | Local browser agents | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | N (Shop Pay) | W signed, B unsigned | **N** (US, CA, AU) | **N** Copilot (US); R ChatGPT | B (as the user) | B |
| WooCommerce | B (Link virtual card) | B | — | — (Stripe ACS waitlist) | B | B |
| Wix | B | B | R (`continue_url`) | — | B | B |
| Squarespace | B | B | — | — | B | B |
| BigCommerce | B, W | B, W | N early access | via Stripe or PayPal | B, W | B |
| Enterprise custom | X at most, N at partners | X at most, B where allowed | N behind flags | N for feed partners | B | X or B |
| Any platform, Israel | B | B | — | — | B | B |

### 5d. Reading step 5

- **Agents reach checkout at the human rate and then hit a wall or walk through it unseen.** Checkout and payment are 2.3–2.4% of agent activity (A#8). On enrolled Shopify stores the protocol door is open for three countries; on the long tail there is no door, and the browsing agents walk through the human one with a virtual card and the merchant never knows.
- **The gate is upstream and not ours.** Held by Google, Meta, OpenAI, Microsoft, Stripe, PayPal and the card networks (doc 07 §4.2). A merchant cannot opt in; it is admitted. No Israeli PSP supports any agent payment protocol; an Israeli merchant can be found on every platform and bought from inside none.
- **The agent becomes the customer.** Guest by construction on every route; relay email and platform payment on Amazon; single-use tokens on ACP; no account, email, loyalty or repeat-purchase signal; fraud screening loses its history signal. Only local browser agents preserve the relationship.
- **The merchant's own rules do not run.** Platform rules run server-side on protocol paths; merchant-added rules (quantity, PO box, age, terms) are theme scripts and page apps, never executed by a protocol agent. Off Shopify, integrations that create orders through an orders API skip the pipeline entirely.
- **3DS reaches a cloud browser.** A challenge meant for a human either fails the sale or needs the user's device in the loop, which is not standard.
- **Consent is not recorded.** AP2 mandates exist and about 40 stores declare them; Checkout WebMCP confirms with a status and stores nothing; the Supplemental Terms leave every dispute to the merchant (gap 5).
- **Heavy cells:** Muse × Shopify (Shop Pay, 3.5×), Muse × WooCommerce (virtual card, unseen), Google × Shopify. **Empty:** every native cell for Israel, WooCommerce, Wix and Squarespace.

### 5e. Numbers used in step 5

| Figure | Source in this repo | Origin |
|---|---|---|
| Buyer countries; Israeli PSPs | A#27, A#28 | Shopify Help Center; vendor sites |
| Checkout WebMCP 28 Sep 2026; Wix `continue_url`; BigCommerce WebMCP | A#21 | Vendor docs |
| Muse+Shop Pay 3.5×; UCP 2.7× | A#22 | Shopify/Meta; UCP Checker |
| 37 of 51 block; 0 verified route | A#25 | Billions Network |
| Checkout 2.3–2.4% of activity | A#8 | HUMAN |
| Instant Checkout retreat | A#24 | Walmart; CNBC |
| "No documented way to turn off browser agents"; AP2 ~40 stores; WebMCP stores no consent | `reports/Agent enablement platform coverage.md` gaps 2 and 5 | Witscode; UCP Checker; Shopify Dev |
| Amazon Buy for Me identity fields; ACP buyer and token flow | flow doc step 5; web 5 Nov 2026 (About Amazon; OpenAI commerce docs) | Vendor docs |

### 5f. Opportunities at step 5

The door is not ours to open. What is left is what happens at the door the merchant already has.

1. **Checkout hardening for browsing agents.** Move the merchant-added rules from theme scripts into server-side hooks (Shopify Functions, WooCommerce hooks) so they run for every route; make the form readable for a machine (step 3's audit applied to checkout); handle the identity gap by labelling, not blocking. Gap 2 in the enablement report: no small-merchant platform documents a browser-agent checkout control.
2. **Consent and evidence capture.** At the merchant's own checkout, record what the agent was shown and what was confirmed: the conditions snapshot, the as-of, the basket quote, the mandate hash where AP2 is present. Gap 5; nobody stores it; it is the raw material of step 8. Cheap on the browser route because the merchant renders the page.
3. **Relationship recovery.** The order arrives with a relay email or a platform identity. A post-order flow (packing slip, delivery message, warranty registration) that offers the shopper a direct relationship is the only way to get the customer back. Small, but every other retention tool is dead on these orders.
4. **Self-hosted protocol checkout against a local PSP.** For non-US merchants the enablement report says a third party can only "declare eligibility, self-host UCP checkout against a local PSP, and harden the storefront." A UCP checkout endpoint wired to an Israeli PSP would be the first in-agent door for Israeli merchants. A large build, dependent on Google admitting it, and on the PSP; recorded, not recommended.

**The trap.** Building a checkout competes with Shopify, Stripe and PayPal on their own ground, and the admission to agent programs is theirs to give. Opportunity 4 is the trap unless an agent operator wants it.

**Risks.** Country expansion (Google adds Canada and Australia, PayPal the Eurozone) will slowly shrink the non-US gap, by the platforms' schedule. Shopify could ship browser-agent checkout controls, which an agency already claims for Fall '26, and take opportunity 1 for its stores. Opportunity 2 depends on the merchant's checkout being rendered, which is true for browser routes and false for protocol routes, where only the platform could store the record.

### 5g. Key competitors at step 5

| Who | What they do | Overlap | Gap they leave |
|---|---|---|---|
| Shopify Checkout, Checkout MCP, Checkout WebMCP, Shop Pay | The protocol door and the signed-browser door for 22% of stores | Everything on Shopify | Nothing per product, no browser-agent control, nothing off Shopify |
| Stripe Agentic Commerce Suite, PayPal Agent Ready | Hosted checkout and tokens on their rails for ChatGPT, Google, Copilot, Perplexity, Meta | The door for their merchants | Their PSP only; US and select countries; waitlists; no Israeli PSP |
| Google UCP, AP2 | The protocol and the mandates | Opportunity 2's mandate hash | Google's surfaces; ~40 stores declare mandates |
| Firmly Connect, Adyen Agentic, Klarna | Enterprise connectors and rails | Opportunity 4 for large US merchants | Enterprise; unpublished pricing |
| Cloudflare, DataDome, HUMAN, Fingerprint | Agent categories, signed-agent programs, labels | Opportunity 1's identity labelling | Enterprise; label, not merchant rules |
| Custom checkout builders (Bold, CheckoutChamp) | Replace the platform checkout | Opportunity 1 on custom carts | Human-facing; no agent features |
| Local PSPs (Isracard, Pelecard, Tranzila, PayPlus, Grow, iCredit) | Israeli card acceptance | Opportunity 4's dependency | None supports any agent payment protocol |

---

## Step 6. Payment authorization and risk approval

**What the step is.** The issuer says yes or no; then the fraud vendor or the merchant's rules decide whether to accept the authorized order. Flow doc step 6. For agents, authorization works and approval did not.

### 6a. Per agent: what the fraud stack sees

| Agent | Authorization | Risk approval | Verifiable identity | Card-rail agent credentials | What it looks like to a fraud model |
|---|---|---|---|---|---|
| Google, Copilot, ChatGPT feeds (protocol) | 100% (A#29) | Unchanged from human (A#30); lowest merchant cancellations (OpenAI) | Yes, by construction | AP2 mandates; Visa Trusted Agent Protocol and Mastercard Agent Pay through PSPs | A normal order from a known platform |
| ChatGPT Agent, Codex | 98.8% (scraping class) | 44% before 24 Sep, 73% after | Web Bot Auth signature against a published key (A#16) | Via PSPs that support TAP or Agent Pay | A cloud IP with a signature, if anyone checks |
| Meta Muse | 98.8% | 44% → 73% | No user agent, no key directory; partial signing from ~12 Oct, unverifiable (A#14) | Stripe Link virtual cards on non-partner stores; Shop Pay on partners | Cloud IP, headless Chromium, single-use card, no history: every bot signal at once |
| Instinct, Grok Bot | 98.8% | Grok +50 points after rule changes | No | Virtual cards | Same |
| Amazon Buy for Me | Not in Forter's data | Unknown; one merchant reported a 28% chargeback rate on about a dozen Amazon-originated orders (Jan 2026 anecdote) | No | Amazon's encrypted payment | A guest with a relay email and Amazon's card |
| Local browser agents | As human | As human | No, and a human is present | User's card | Human |

Across all of them: 97.7% of legitimate agent sessions carry no verified identity (A#12); 80% of AI agents do not declare themselves properly (A#13).

### 6b. Per platform: the risk stack the merchant has

| Platform | Default fraud screening | Agent-aware options | Merchant rules | Eligibility in the loop |
|---|---|---|---|---|
| Shopify | Shopify's risk analysis; Shopify Protect on Shop Pay orders | Web Bot Auth rate tiers; Fall '26 visibility controls claimed by an agency, unverified | Flow automations; Functions | No; risk asks "is this fraud," not "do I ship there" |
| WooCommerce | PSP's (Stripe Radar, PayPal); Signifyd and similar plugins | None | Order hooks, many | No |
| Wix | Wix Payments risk | None | Settings | No |
| BigCommerce | Signifyd, Forter integrations | Via vendors | Settings | No |
| Enterprise custom | Forter, Signifyd, Riskified | Forter Agentic Commerce Engine; Adyen Agentic; DataDome labels | Own | Own |
| Israel | Local PSP risk | None supports TAP or Agent Pay (A#28) | Platform's | No |

### 6c. The map: platform × agent, what decides the order

Legend: **V** verified identity available to the decision; **S** signature exists, platform may check; **U** unverifiable, decided on attributes or not at all; **H** human; **A** Amazon-paid.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google (~12%) | Copilot, ChatGPT feeds (~11%) | Local browser agents | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | U (V on Shop Pay) | S for ChatGPT Agent; U for Instinct, Grok | V | V | H | A |
| WooCommerce | U | S / U, nobody checks | — | — | H | A |
| Wix | U | S / U | — | — | H | A |
| Squarespace | U | S / U | — | — | H | A |
| BigCommerce | U | S / U | V | V | H | A |
| Enterprise custom | U or blocked | S checked by edge vendors | V | V | H | A or blocked |

### 6d. Reading step 6

- **Authorization is not the problem.** 98.8 to 100%. The issuer sees a valid card.
- **Approval was, and the fix was to loosen.** Fraud models trained on humans declined 56% of scraping-agent orders until Forter relaxed thresholds on 24 September; approval rose to 73%, Grok Bot by 50 points. Once "looks like an agent" means "approve," imitating an agent is the attack. Visa saw a 450% rise in dark-web posts mentioning AI agents over six months (threat chatter, not a fraud rate).
- **The decision is identity-shaped and identity is unavailable for the largest buyer.** Web Bot Auth covers ChatGPT Agent, Codex and Google's subset. Muse, half of orders, signs nothing verifiable. Any product that decides on identity is blind to its largest case (doc 07 §3).
- **The merchant's eligibility is not in the loop.** Risk approval asks whether the order is fraud. The confirmed failure is that the order is for a place the merchant does not ship (step 7). The merchant already knows its countries, currencies and quantity limits; nobody asks it here.
- **Heavy cells:** Muse × everything, unverifiable. **Served:** protocol × Shopify and BigCommerce, verified by construction and approved at human rates. **Empty:** any agent-aware control on WooCommerce, Wix, Squarespace or in Israel.

### 6e. Numbers used in step 6

| Figure | Source in this repo | Origin |
|---|---|---|
| Authorization 100% / 98.8%; approval 44% → 73%; Grok +50 | A#29, A#30 | Forter, through 29 Oct 2026 |
| 97.7% unverified sessions; 80% undeclared | A#12, A#13 | Arkose; DataDome |
| Muse identification; signers list | A#14, A#16 | DataDome; vendor docs |
| Israeli PSPs | A#28 | Vendor sites |
| 28% chargeback anecdote on Buy for Me orders | `research_notes/Shopping agent production data 2026/google_amazon.md` lines 74, 156 | Value Added Resource; Modern Retail, Jan 2026 |
| Visa dark-web +450% | `research_notes/Shopping agent production data 2026/cross_agent_telemetry.md` line 83 | Visa PERC via Chargeflow |
| Forter withholds fraud and dispute rates; 12 Nov webinar | `cross_agent_telemetry.md` lines 126, 138 | Forter |

### 6f. Opportunities at step 6

1. **Eligibility rules in the approval loop.** Destination, currency, quantity, promo validity, product-level restrictions, checked at the order hook against the conditions record, before the order is accepted. The confirmed failure (A#32) is attribute-shaped; the rules need the order, not the agent; the merchant already knows the answers. Decide, as doc 06 reframed it. WooCommerce exposes the hooks; Shopify needs Flow or Functions.
2. **Price-drift and quantity caps.** Reject or hold an order whose price differs from the as-of price beyond a tolerance, or whose quantity exceeds the basket policy. The freshness token of step 4 consumed here.
3. **Identity as a label, not a gate.** Verify Web Bot Auth where present, classify cloud-IP and virtual-card orders where not, and pass the label to the fraud vendor and to analytics (step 9). Mainstream agents are converging on signing because platforms pay them to; the label improves over time without the decision waiting for it.
4. **Agent context for the fraud vendor.** Feed the fraud model the signals it lacks: signed or unsigned, platform of origin, basket-quote match, eligibility result. The vendors loosened blind; a merchant-side context makes the loosening selective.

**The trap.** Competing with Forter, Signifyd and Riskified on fraud detection, or building the fingerprinting doc 05 declined. The merchant-side product decides on what the order is; the vendors decide on whether it is fraud.

**Risks.** Forter and Signifyd could add eligibility rules to their agent engines, which would close opportunity 1 for enterprise and leave the long tail, where they do not sell. Shopify could ship per-market rules, which an agency claims for Fall '26. And if Meta publishes a key directory, identity-first products become viable again and the label gets cheap.

### 6g. Key competitors at step 6

| Who | What they do | Overlap | Gap they leave |
|---|---|---|---|
| Forter (Agentic Commerce Engine), Signifyd, Riskified | Fraud decisions with chargeback guarantees; Forter publishes the only agent-order network data | Opportunities 1 and 4, enterprise | Fraud, not eligibility; enterprise-weighted; loosened thresholds blind |
| Stripe Radar, Adyen Agentic, PayPal | PSP-side risk; TAP and Agent Pay support | Opportunity 3's credentials | PSP's merchants; no Israeli PSP |
| Cloudflare, DataDome, HUMAN, Arkose, Fingerprint | Bot and agent classification | Opportunity 3's label | Enterprise; classification, not merchant rules |
| Shopify risk analysis, Shopify Protect, Flow | Platform risk and automations | Opportunities 1 and 2 on Shopify, if the merchant builds them | Binary toggles; nothing per market yet; nothing off Shopify |
| WooCommerce anti-fraud plugins | Rule engines on order hooks | Opportunity 1's mechanism | Human-era rules; no agent context |
| Visa TAP, Mastercard Agent Pay, Amex | Agent credentials on card rails | Opportunity 3 upstream | Via PSPs; outcome data unpublished |

---

## Step 7. Fulfilment

**What the step is.** Pick, pack, ship, track; cancellations by shopper or merchant before shipment. Flow doc step 7. The one step with a confirmed production failure.

### 7a. Per agent: what lands in the order queue

| Agent | Merchant-initiated cancellation | Customer-initiated cancellation | How the order is identified in the queue | Tracking and communication |
|---|---|---|---|---|
| Google, Copilot, ChatGPT feeds (protocol) | Lowest (OpenAI); Google makes the merchant responsible for wrong items, cancellations and returns and hard-fails on any price increase | 1–2%; 10% on Google's single-item checkout (A#31) | Channel-tagged on Shopify; UCP order and fulfilment capabilities where declared | Through the protocol's order status |
| ChatGPT Agent, Instinct, Grok (scraping) | 27% Asia and Middle East, 37% Europe, ~10% for humans (A#32); attributed to wrong country, wrong currency, unsupported merchant | 1–2% | A guest order from a cloud IP with a virtual card; untagged unless the merchant infers it | Merchant's normal emails to whatever address the agent gave |
| Meta Muse | Same 27–37% class | 1–2% | Shop Pay order on partners; untagged guest elsewhere | Same |
| Amazon Buy for Me | Anecdotes only: one Shopify merchant, 8 orders a month, half calling for tracking; 16 cancelled at another | Unknown | Relay email per order; Amazon's payment | Through the relay; Amazon's A-to-Z does not apply |
| Local browser agents | As human | As human | Looks human | Normal |

### 7b. Per platform: what the merchant can see and do in the queue

| Platform | Agent orders identifiable | Pre-fulfilment hold on eligibility | Cancellation feedback to the agent | Fulfilment data back to the agent |
|---|---|---|---|---|
| Shopify | Protocol orders carry the agentic-storefront channel; browser-agent orders do not | Flow or Functions, if built | None documented | UCP fulfilment on declared stores; Shop app tracking for Shop Pay |
| WooCommerce | No | Order hooks, if built | None | None |
| Wix | UCP orders via Wix; browser orders no | Settings | None | UCP fulfilment capability declared |
| Squarespace | No | No | None | None |
| BigCommerce | UCP early access | Settings | None | Via UCP |
| Enterprise custom | Own | Own | Own | Own |

### 7c. The map: platform × agent, what the queue knows

Legend: **T** tagged as an agent order with the agent named; **t** tagged as protocol, agent not always named; **U** untagged, indistinguishable from a human guest; **R** relay identity; **H** human.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google (~12%) | Copilot, ChatGPT feeds (~11%) | Local browser agents | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | U (Shop Pay order on partners) | U | **T** | **T** | H | R |
| WooCommerce | U | U | — | — | H | R |
| Wix | U | U | t | — | H | R |
| Squarespace | U | U | — | — | H | R |
| BigCommerce | U | U | t | t | H | R |
| Enterprise custom | U or blocked | U | T | T | H | R or blocked |

### 7d. Reading step 7

- **The confirmed failure is here and it is eligibility.** A quarter to a third of scraping-agent orders outside the US are cancelled by the merchant for being somewhere it does not ship or in a currency it does not take. Humans run about 10%. The human would have seen "we don't ship there" at checkout; the agent paid, and the merchant found out in the queue.
- **Cancellation costs more than it did.** Refund, lost authorization, and the shopper's agent records a failed merchant, which feeds ranking at step 2.
- **The queue cannot tell.** Browser-agent orders, three in four, arrive as guest orders from cloud IPs; no platform tags them. Only protocol orders carry a channel.
- **Communication goes to the agent's address.** Relay emails on Amazon; whatever the agent entered elsewhere. Tracking requests come back as support tickets.
- **Nothing flows back.** No route tells the agent why an order was cancelled, so the agent cannot learn the merchant's eligibility and will make the same order again.
- **Heavy cells:** scraping agents × every platform, untagged, with the ex-US cancellation rate. **Served:** protocol × Shopify, tagged and lowest cancellation. **Empty:** any feedback channel from merchant to agent.

### 7e. Numbers used in step 7

| Figure | Source in this repo | Origin |
|---|---|---|
| Merchant cancellations 27% / 37% vs ~10%; attribution to eligibility | A#32 | Forter |
| Customer cancellations 1–2%; Google 10%, baskets 56% below $227 | A#31 | Forter |
| Google: merchant responsible for wrong items, cancellations, returns; hard-fail on price increase | `research_notes/Shopping agent production data 2026/google_amazon.md` line 51 | Google terms |
| Buy for Me anecdotes: 8 orders/month, half asking for tracking; 16 cancelled | `google_amazon.md` lines 74, 156 | Value Added Resource; Modern Retail, Jan 2026 |
| Shopify channel tagging; headless excluded | `reports/Agent enablement platform coverage.md` gap 4 | Shopify Help Center |
| Failed sessions: 37.9% reached checkout | `reports/Agent enablement platform coverage.md` line 71 | UCP Checker |

### 7f. Opportunities at step 7

1. **Eligibility hold before pick.** The same rules as step 6, applied as a hold in the queue for platforms where the order hook is weak: catch the order before the label prints, cancel with a reason, refund automatically. On Shopify this is where Decide lands if Functions are not used.
2. **Agent-order tagging.** Infer the agent class from what the order carries: cloud IP ranges and ASNs, virtual-card BIN ranges (Stripe Link), relay email domains, Web Bot Auth signatures where present, missing session history. A label for operations and for step 9, not a decision. Three in four agent orders are untagged today.
3. **Cancel with a reason the agent can read.** Return a structured cancellation reason (destination not served, currency, quantity) through whatever channel exists: UCP order status on protocol orders, a machine-readable email or page for browser orders. The only way the agent learns the merchant's eligibility and stops repeating the order. No agent consumes this today; it is cheap to emit and it is the merchant's interest to be learned correctly.
4. **Tracking to the right place.** Push tracking into the protocol's fulfilment capability where declared, and through the relay where Amazon is the channel, so "where is my order" stops arriving as a ticket.

**The trap.** Becoming an order-management system. The opportunity is a hold and a label inside the merchant's existing queue, not a new queue.

**Risks.** Shopify tags protocol orders already and could infer browser-agent orders for its stores. Forter sees cancellation reasons across its network and could sell the hold to its enterprise customers. Neither reaches WooCommerce, where the failure rate is unmeasured because nobody tags anything.

### 7g. Key competitors at step 7

| Who | What they do | Overlap | Gap they leave |
|---|---|---|---|
| Shopify order admin, Flow, agentic-storefront channel tags | Tags protocol orders; automations | Opportunities 1 and 2 on Shopify | Browser-agent orders untagged; nothing off Shopify |
| OMS and 3PL software (ShipStation, ShipBob, Linnworks) | Pick, pack, ship, carrier rates | The queue opportunity 1 sits in | No agent concept; no eligibility hold |
| Forter, Signifyd | See cancellation outcomes across their networks | Opportunity 1's data, enterprise | Decide fraud, not fulfilment; enterprise |
| Amazon Shop Direct merchant portal | Order and relay management for Buy for Me | Opportunity 4 for one channel | One channel; merchant never opted in |
| UCP fulfilment extension | Order status and tracking to Google's surfaces | Opportunities 3 and 4 on protocol orders | Google only; declared by few stores |
| Address validation services (Loqate, Smarty) | Validate addresses at checkout | Part of opportunity 1 | Human checkout only; no eligibility semantics |

---

## Step 8. Post-purchase

**What the step is.** Delivery, returns, warranty, support, and disputes that arrive 30 to 120 days later. Flow doc step 8. For agent orders, nobody has published an outcome.

### 8a. Per agent: who carries the outcome

| Agent | Who is seller of record | Consent artefact stored | Returns | Disputes | Outcome data published |
|---|---|---|---|---|---|
| Google, Copilot, ChatGPT feeds (protocol) | Merchant, on every protocol's terms | AP2 mandates bind a merchant-signed checkout hash; ~40 stores declare them; nothing stored for the merchant | Merchant's policy; Google makes the merchant responsible | Standard chargeback process (Mastercard L1 guide); agent company not a party | None (A#33) |
| ChatGPT Agent, Instinct, Grok | Merchant | None | Merchant's | Standard | None |
| Meta Muse | Merchant | None; Shopify Checkout WebMCP confirms with `status: completed` and stores no consent record | Merchant's | Standard | None; Meta discloses set-ups and average purchase >$200, not outcomes |
| Amazon Buy for Me | Merchant; Amazon is explicitly not the seller | None | Merchant's; Amazon's A-to-Z guarantee does not apply | One merchant reported 28% chargebacks on about a dozen Amazon-originated orders (Jan 2026) | None |
| Local browser agents | Merchant | The human saw the page | Merchant's | Standard | n/a |

What exists instead of outcome data: 46% of US shoppers who bought on an AI recommendation returned at least one such item in the past year, top reasons "different than described" and wrong size or fit (Blue Yonder; human-completed purchases); successful dispute-challenge rate 62% in 2026 versus 43% the year before (industry survey); Chargebacks911 launched merchant-side indemnification for AI-agent purchases on 26–27 Oct 2026 with no pricing or caps; Forter's 12 November webinar promises to say whether disputes are higher.

### 8b. Per platform: what the merchant has after the order

| Platform | Returns tooling | Evidence the merchant can produce for an agent order | Protocol coverage of post-order | Dispute handling |
|---|---|---|---|---|
| Shopify | Return rules, Shop app returns, apps (Loop, Happy Returns) | Order record, tracking; no record of what the agent was shown or what the human approved | UCP order status; ACP order webhooks; returns outside every protocol | Platform's dispute tools; Shopify Protect on Shop Pay |
| WooCommerce | Plugins | Order record, tracking | None | PSP's |
| Wix | Settings | Order record | UCP order | Wix Payments |
| Squarespace | Settings | Order record | None | PSP's |
| BigCommerce | Apps | Order record | UCP early access | Vendors |
| Enterprise custom | Own | Own | Own | Forter, Signifyd guarantees |

### 8c. The map: platform × agent, what the merchant holds if it is disputed

Legend: **E** a consent artefact exists upstream (AP2), not held by the merchant; **O** order record and tracking only; **H** human saw the page; **R** relay identity, no direct contact.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google (~12%) | Copilot, ChatGPT feeds (~11%) | Local browser agents | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | O | O | E / O | O | H | R / O |
| WooCommerce | O | O | — | — | H | R / O |
| Wix | O | O | E / O | — | H | R / O |
| Squarespace | O | O | — | — | H | R / O |
| BigCommerce | O | O | E / O | O | H | R / O |
| Enterprise custom | O | O | E / O | O | H | R / O |

### 8d. Reading step 8

- **Liability is settled and it is the merchant's.** Every protocol's terms, Amazon's structure, and the card networks' guidance put returns, support and disputes on the merchant. The agent company is not a party.
- **The evidence does not exist.** For a human order the merchant shows the checkout page the shopper saw. For an agent order there is no record of what the agent was shown or what the human approved (gap 5). The merchant will lose disputes it should win.
- **Returns policies were not read.** If the agent did not expose the window at lookup, the shopper learns it at the return. The 46% figure is on AI advice, not agent orders, and it is the only outcome-type number anyone has.
- **The floor is structural and the dollars are not there yet.** Our estimate puts materially wrong orders at 2–5% per order by 2028–2029, set by intent, data and rubber-stamped confirmations (97% of prompts approved, 13% of planted errors caught; A#37, A#38). At 26% of order value per mistake, 2% wrong is 52 bp of agent order value, above the 42 bp of card-not-present fraud (A#39). But agent orders are at most 1.5% of orders, so this is under one basis point of a merchant's business today (A#4).
- **The market is pricing the risk before measuring it.** Paid indemnification has appeared on both sides (Chargebacks911 for merchants, Amex for cardholders). That is a signal that the risk is perceived as material and unallocated, not a measurement.
- **Heavy cells:** none by dollars. **Served:** nothing. **Empty:** evidence on every cell except local browsers.

### 8e. Numbers used in step 8

| Figure | Source in this repo | Origin |
|---|---|---|
| No dispute, return or wrong-item rate from anyone; 12 Nov | A#33; `research_notes/Shopping agent production data 2026/cross_agent_telemetry.md` lines 117, 138, 159, 162 | All vendors, networks, processors |
| 46% returned an AI-recommended item | `cross_agent_telemetry.md` line 207 | Blue Yonder, Nov 2026 |
| 28% chargebacks on ~12 Buy for Me orders | `google_amazon.md` lines 74, 156 | Merchant anecdote, Jan 2026 |
| Chargebacks911 indemnification 26–27 Oct; Amex cardholder-side | `cross_agent_telemetry.md` lines 108–113 | The Paypers |
| Dispute-challenge success 62% vs 43% | `cross_agent_telemetry.md` line 145 | Industry survey of 1,504 fraud professionals |
| AP2 ~40 stores; WebMCP stores no consent; Mastercard standard chargebacks | `reports/Agent enablement platform coverage.md` gap 5 | Shopify Dev; Mastercard L1 guide |
| Floor 2–5%; 97% / 13%; 52 bp vs 42 bp; ≤1.5% of orders | A#37–39, A#4 | Anthropic study; ours; FirstPageSage |
| Meta: >$200 average purchase, 25% of weekly actives shop | `perplexity_meta_others.md` line 35 | Meta Q3 call |

### 8f. Opportunities at step 8

1. **The evidence record.** For every agent order, store what was shown and what was confirmed: the conditions snapshot with its as-of, the basket quote, the variant identifiers, the mandate hash where AP2 is present, the agent label from step 7, and the page as rendered on browser routes. Gap 5. Produced at step 5 on browser routes by the merchant's own page, and at step 4 by the basket quote on any route. This is Prove, and it is the one layer whose cost is near zero to collect and whose value arrives 30 to 120 days later.
2. **Returns terms in the record, so they are read before the order.** The step 1 record again: returns window, fees, method, exclusions as data on the page and in the quote. The cheapest way to cut "different than described" returns is to have the agent show the terms at lookup.
3. **Representment for agent orders.** A dispute package assembled from the evidence record: what the agent saw, what the human approved, the eligibility check that passed, the tracking. Dispute challenges succeed 62% of the time for human orders with evidence; agent orders currently have none.
4. **Guarantee, later.** Indemnification priced on the evidence record, once the dispute number exists and agent share passes a few percent of orders. Chargebacks911 has entered without pricing; doc 06 keeps this as a roadmap item conditional on 12 November and on volume.

**The trap.** Insurance before measurement. Pricing a guarantee on a risk no one has quantified, for a volume under one basis point of the merchant's business, is the earlier pivot's mistake in a new coat.

**Risks.** The 12 November number could show disputes at human rates, which would make Prove a compliance nicety rather than a loss saver, though the evidence record still wins the disputes that do come. Platforms could store consent artefacts themselves on protocol routes (Shopify, Google), which would cover a quarter of orders and leave the browser three-quarters to the merchant. Volume may stay too small for years, which the pricing has to survive.

### 8g. Key competitors at step 8

| Who | What they do | Overlap | Gap they leave |
|---|---|---|---|
| Chargebacks911, Chargeflow | Dispute management and representment; Chargebacks911 now indemnifies agent purchases | Opportunities 3 and 4 | Work from the merchant's existing records, which for agent orders are empty; no evidence capture at order time |
| Forter, Signifyd, Riskified chargeback guarantees | Guarantee approved orders against fraud chargebacks | Opportunity 4 for fraud disputes | Fraud reason codes only; not "not as described"; enterprise; Forter withholds agent dispute data |
| Returns platforms (Loop, Narvar, Happy Returns, AfterShip) | Returns portals and logistics | Opportunity 2's downstream | Human shoppers; no agent exposure of terms |
| AP2 and Google, Shopify Checkout WebMCP | Consent artefacts and completion status upstream | Opportunity 1 on protocol routes | ~40 stores; nothing stored for the merchant; nothing on browser routes |
| Card networks (Visa rules, Mastercard Agent Pay, Amex) | Dispute frameworks for agent transactions | The rules opportunity 3 plays by | No outcome data; apply standard chargebacks |
| Amex cardholder-side protection | Protects the buyer on agent purchases | The other side of opportunity 4 | Raises merchant exposure, does not reduce it |

---

## Step 9. Measurement and retention

**What the step is.** Attribute the sale to a channel, compute conversion and return on spend, feed email, loyalty and retargeting. Flow doc step 9. For agents, the merchant cannot see the channel.

### 9a. Per agent: what it leaves behind

| Agent | Referral trace on the merchant's site | Order trace | Tracking parameters | Can the merchant count failed attempts | Retention signal |
|---|---|---|---|---|---|
| Google AI Mode, Gemini | Click-outs look like Google organic; UCP orders never load the site | Agentic-storefront channel on Shopify; Google UCP reporting in alpha | None distinguishing AI Mode from Search | No | None on protocol orders |
| ChatGPT apps and feeds | Referral with a chatgpt.com source, sometimes with tracking parameters missing (Retailgentic, Oct 2026); orders arrive with no referrer (Shopify community) | Shopify channel tag; otherwise a guest order | Inconsistent | No | Guest |
| ChatGPT Agent | Cloud IP, signed requests; appears as direct traffic unless the signature is read | Guest order from a cloud IP | None | No | Guest |
| Microsoft Copilot | None on protocol orders; Clarity AI Visibility reports citations | Shopify channel tag | None | No | Guest |
| Meta Muse | Cloud IP, no user agent; looks like direct traffic or a bot | Shop Pay order on partners; guest elsewhere | None | No | Shop Pay customer on partners, else none |
| Instinct, Grok Bot | Cloud IP | Guest | None | No | None |
| Amazon Buy for Me | None | Relay email, Amazon payment | None | No | Relay only |
| Comet, Claude in Chrome, Gemini in Chrome, Edge Copilot | Indistinguishable from the user; "most looked human" in a 23-agent test | Normal | None | n/a | Normal |

The only transaction-level academic dataset (Kaiser and Schulze, Oct 2025, background): 973 e-commerce sites, >50,000 ChatGPT-referred transactions over 12 months, built from referrer data that is now less reliable than it was.

### 9b. Per platform: what the merchant's reports show

| Platform | Agent reporting | Covers | Does not cover |
|---|---|---|---|
| Shopify | Agentic-storefront per-channel reports; Q2 2026 call: agentic traffic and orders 3× YoY, new-buyer orders at ~2× other channels, 75% of AI-attributed orders outside the top 100 categories | Protocol and channel orders | Headless stores; browser-agent sessions and orders; failed checkouts |
| WooCommerce | None | — | Everything |
| Wix | AI Visibility Overview: citations | Mentions | Orders, sessions, failures |
| Squarespace | None | — | Everything |
| BigCommerce | None documented | — | Everything |
| Enterprise custom | Own; Clarity AI Visibility; GA4 channel groups by referrer | Referral clicks | Protocol orders without pixels; cloud-IP sessions; failures |
| Any | Google Merchant Center AI performance insights (share of voice, attribute completeness) | Google surfaces | Orders |

### 9c. The map: platform × agent, what the merchant can attribute

Legend: **T** attributed by channel tag; **r** attributed by referrer, partially; **D** appears as direct or bot traffic; **H** looks human; **—** nothing.

| Platform | Muse (49%) | ChatGPT Agent, Instinct, Grok (~28%) | Google (~12%) | Copilot, ChatGPT feeds (~11%) | Local browser agents | Amazon Buy for Me |
|---|---|---|---|---|---|---|
| Shopify | D (T on Shop Pay partner orders) | D | **T** | **T** / r | H | — |
| WooCommerce | D | D | — | r | H | — |
| Wix | D | D | T partial | r | H | — |
| Squarespace | D | D | — | r | H | — |
| BigCommerce | D | D | T early access | r | H | — |
| Enterprise custom | D or blocked | D | T, alpha | T / r | H | — |

### 9d. Reading step 9

- **The merchant cannot see the channel that is growing fastest.** AI referrals are 5.5% of visits and converting 1.6× (A#1, A#2), and most of that is invisible: Google's AI surfaces hide inside Google, protocol orders never fire a pixel, cloud agents show as direct traffic, ChatGPT links sometimes drop their tags.
- **Failed agent checkouts are invisible everywhere.** An agent that reached checkout and was blocked, declined or cancelled leaves nothing the merchant can count. Nobody, including Shopify, counts them (gap 4).
- **Analytics are polluted both ways.** Agent sessions inflate traffic and deflate conversion unless separated, and the merchant cannot separate them because identity is unavailable (step 6).
- **No benchmark exists.** No one publishes agent conversion by platform or vertical; Shopify's multiples are self-reported without baselines; the three traffic vendors disagree by 10× (doc 06 §9).
- **Retention is gone on the orders that matter.** Guest by construction, relay emails, platform payment: the merchant cannot email, enrol, or retarget the agent-era customer. Shop Pay partner orders on Shopify are the exception.
- **Heavy cells:** Muse and the scraping agents everywhere, appearing as direct or bot traffic. **Served:** protocol × Shopify, by channel tag. **Empty:** WooCommerce, Squarespace, BigCommerce entirely; failed attempts everywhere.

### 9e. Numbers used in step 9

| Figure | Source in this repo | Origin |
|---|---|---|
| AI-referred 5.5%, 1.6×; +138% YoY | A#1–3 | FirstPageSage; Adobe |
| Shopify Q2: 3× traffic and orders, 2× new-buyer pace, 75% outside top-100 categories | `reports/Shopping agent production data 2026.md` line 31 | Shopify Q2 2026 call |
| Missing tracking parameters; no referrer on ChatGPT orders; GA pixels do not fire in direct checkout; UCP reporting alpha | `research_notes/Shopping agent production data 2026/openai_microsoft.md` line 23; `reports/Agent enablement platform coverage.md` gap 4 | Retailgentic; Shopify community and Help Center |
| 23 agents, most looked human | `reports/Agent enablement platform coverage.md` gap 2 | dev.to, 27 Oct 2026 |
| Kaiser and Schulze: 973 sites, >50k ChatGPT-referred transactions | `openai_microsoft.md` line 24 | Working paper, Oct 2025 (background) |
| Clarity AI Visibility; Merchant Center AI insights | `openai_microsoft.md` line 117; web 5 Nov (Merchant Center insights, 27 May 2026) | Microsoft; vendor blogs |
| Vendors disagree by 10× | doc 06 §9 | HUMAN, DataDome, Forter, FirstPageSage, DoorDash |

### 9f. Opportunities at step 9

1. **Agent analytics per store.** Label sessions and orders by agent class using what step 7 infers (signatures, cloud-IP ranges, virtual-card BINs, relay domains, channel tags, referrers), and report: agent visits, lookups, carts, checkouts reached, checkouts failed and why, orders, cancellations. The first report that counts failed agent checkouts on any platform. Gap 4; nobody does it; WooCommerce has nothing at all.
2. **Share of answer, per query set.** The step 2 audit as a recurring metric beside the analytics: presence, position, which competitor won, per agent, per run. Together with 1 it is the only end-to-end view of the agent channel a merchant can get.
3. **The benchmark.** Across customers, agent conversion, cancellation and failure rates by platform, vertical and agent. A data asset no one has and the thing every merchant asks first: is my number good. It compounds with customer count and it is what the traffic vendors cannot produce because they see only their own network.
4. **Relationship recovery.** Step 5's post-order flow measured here: how many relay and guest customers converted to a direct relationship. Small, and the only retention lever left.

**The trap.** Analytics-only SaaS. A dashboard that shows the merchant it is invisible, without the record and the rendering that fix it, is a complaint generator. The analytics are the measurement layer of the product, the proof of lift for everything upstream, and the wedge's recurring form.

**Risks.** Shopify's agentic analytics will keep improving for protocol orders, and GA4 could add AI channel groupings by referrer, both covering the easy quarter and not the browser three-quarters. The labelling in opportunity 1 depends on inference for Muse until Meta publishes a key directory; it will have an error rate, and it must be reported as a label with confidence, not as a count.

### 9g. Key competitors at step 9

| Who | What they do | Overlap | Gap they leave |
|---|---|---|---|
| Shopify agentic analytics | Per-channel reports for protocol and channel orders | Opportunity 1 on Shopify, protocol only | Browser agents, headless, failed checkouts; nothing off Shopify |
| GA4, Adobe Analytics | Referrer-based channel attribution | Opportunity 1's referral slice | Pixels do not fire on protocol surfaces; cloud agents look direct; no agent class |
| AI visibility trackers (Profound, Peec, Scrunch, Semrush, Clarity AI Visibility, Wix AI Visibility) | Citations and share of voice | Opportunity 2 | Mentions, not orders or failures; brand and enterprise pricing |
| Attribution platforms (Triple Whale, Northbeam, Rockerbox) | Multi-touch attribution for DTC brands | Opportunity 1's reporting layer | Built on pixels and ad platforms; no agent model |
| Bot analytics (DataDome, HUMAN, Cloudflare Radar) | Classify and count bot and agent traffic | Opportunity 1's labels, enterprise | Each sees its own network; sold to security teams, not merchants; no order join |
| Retention platforms (Klaviyo, Yotpo, Smile) | Email, loyalty, reviews | Opportunity 4's tooling | Need an email the merchant does not have on agent orders |
| Forter | Network-level agent-order data | Opportunity 3's nearest thing | Enterprise-weighted, undisclosed size, withholds disputes |

---

## Across the nine steps

| Step | Heaviest route today | Served cells | Empty cells | Opportunity that leads | Competitor that matters most |
|---|---|---|---|---|---|
| 1 Catalog | Page, for three in four orders | Shopify and BigCommerce × protocol agents | Protocol columns on Woo, Wix, Squarespace; conditions on every route | The conditions record, rendered everywhere | SEO apps (the trap), Stripe and PayPal rails, Shopify |
| 2 Discovery | Crawl, for everyone off Shopify | Shopify × four channels, rank unknown | Long-tail feeds; any rank report | Channel entry plus "where do I stand" | AI visibility trackers, feed managers |
| 3 Lookup | Page and accessibility tree | Google × Shopify and Wix by UCP lookup | Structured lookup for browsing agents; conditions | Transactability verdict; theme readability | Shopify Catalog and Knowledge Base, Botify |
| 4 Cart | DOM and accessibility-tree carts | Google × Shopify and Wix after address | Pre-address quote anywhere | Basket quote; basket policy as data | UCP cart, Shopify Checkout MCP |
| 5 Checkout | Browser form-fill, merchant unaware | Shopify × Google, Copilot, Meta; three countries | Every native cell off Shopify and outside the US | Hardening; evidence capture; the door is not ours | Shopify, Stripe, PayPal, local PSPs |
| 6 Approval | Unverifiable cloud agents | Protocol × Shopify and BigCommerce | Eligibility in the loop, anywhere | Eligibility rules at the order hook | Forter, Signifyd; Shopify Flow |
| 7 Fulfilment | Untagged guest orders with virtual cards | Protocol × Shopify, tagged | Any feedback to the agent | Eligibility hold; tagging; cancel with reason | Shopify admin, OMS vendors |
| 8 Post-purchase | Merchant carries everything | Nothing | Evidence on every cell | The evidence record; guarantee later | Chargebacks911, Forter guarantees |
| 9 Measurement | Direct and bot traffic | Protocol × Shopify by channel tag | Failed checkouts everywhere; Woo entirely | Agent analytics with failures; the benchmark | Shopify analytics, visibility trackers |

Three things repeat on every row. The page route carries most of the volume on every step and is served by nobody. The protocol route is served on Shopify and nearly empty elsewhere. And the same record, the conditions and basket policy created at step 1, is the input to the opportunity on steps 3, 4, 6, 7 and 8. That is the argument for one product rather than nine.

**Assumptions carried through this doc, open for correction.** Agents are named rather than classed, which keeps the columns wide. Amazon Buy for Me is included with unknown weight because no traffic vendor measures it. Cell weights are directional: column shares are Forter's and the platform split per agent is unpublished. Figures marked web, 5 Nov 2026 are not in the research folder and should be moved there if they are kept.

