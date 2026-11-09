# 09 — Opportunity review: the standouts, the step 1 verdict, and what is defensible

**Date:** 9 November 2026
**Status:** Conclusions of the review discussion of doc 08's opportunity sections, held 8 and 9 November 2026. Step 1 was reviewed in depth. Steps 2 to 9 were ranked but not reviewed individually. Numbers come from doc 06, doc 07 Appendix A (A#n), `reports/` and `research_notes/`; the few items sourced from the web during the review are marked.

**What this doc is for.** Doc 08 lists four opportunities per step with a trap, risks and competitors. This doc records what the review made of them: which stand out, why step 1 produced no standalone opportunity, what the conditions problem looks like on the route that carries most orders, whether fitting catalogs per agent is worth anything, and where defensibility could come from at all. It ends with the corrections doc 08 and doc 07 now need and the two pieces of work that settle the open questions.

---

## 1. Standouts across the nine steps

Four opportunities stand out, and three of them are the same product seen from different steps.

1. **Eligibility rules at the order hook (6.1, with the fulfilment hold 7.1 as fallback).** The only opportunity backed by a confirmed production failure: merchants cancel 27% of scraping-agent orders from Asia and the Middle East and 37% from Europe because the agent bought what the merchant does not ship (A#32). Needs no agent cooperation, works on the browser route that carries three in four orders, and WooCommerce exposes the hooks. It fits a non-US merchant on WooCommerce exactly. Weakness: a handful of orders per store per month, so it is bought cheaply or bundled.
2. **The conditions record (1.1).** Not a sale, the spine: six other opportunities are this record rendered elsewhere (transactability at step 3, basket quote at step 4, rules at steps 6 and 7, returns terms at step 8). Zero lift evidence on the page side. Section 4 revises its standing.
3. **Agent analytics with failed checkouts (9.1), with the benchmark (9.3) behind it.** How a merchant finds out the problem exists. Nobody counts failed agent checkouts on any platform and WooCommerce has nothing. The only asset in doc 08 that compounds with customer count, and the measurement layer that proves lift for everything else.
4. **The evidence record (8.1).** Near-zero cost to collect, no competitor captures it at order time, the raw material for representment and any later guarantee. Pays only when disputes arrive, so it rides along until the 12 November dispute number.

**Not standing out:** channel entry for the long tail (2.1, contested by feed managers and free plugins), the self-hosted checkout (5.4, the trap doc 08 names), product Q&A (3.3), cross-sell as data (4.4), the ranking-readiness score (2.4), and the guarantee (8.4, conditional on data that does not exist).

**What to do first is not a product.** The record-quality crawl (1.4, widened in §3.3) and the five-agent before-and-after test from doc 08 §1f together take about two weeks and settle the two unknowns that matter most: how empty the conditions and variant slots really are, and whether filling them changes agent answers.

---

## 2. How agents read a page, by step

The review corrected a conflation in the first draft of the analysis. Three reading modes exist, and they belong to different steps.

| Reader | Examples | What it gets | What it cannot get | Step |
|---|---|---|---|---|
| Offline crawl into an index | Googlebot, OAI-SearchBot, PerplexityBot, meta-webindexer, ClaudeBot | Full HTML as text; closed accordions are present; JSON-LD is available to parse (confirmed for Google, unconfirmed for others) | Anything behind interaction; freshness beyond crawl cadence | 2 (discovery) |
| Live server-side fetch | ChatGPT-User, Claude-User, the user-triggered Perplexity fetcher | Same as the crawl, fetched on demand | Same | 2 and 3 |
| Live browser | Muse (accessibility tree), ChatGPT Agent cloud browser, Comet, Claude in Chrome | Visible text and controls; screenshots at the current scroll position | Muse: JSON-LD, closed accordions, tooltips, unselected tabs; it "can't run its own scripts, and doesn't hover or click to reveal hidden content" (Botify). Any browser: each click and scroll costs a step | 3 to 5 |

**Exception that matters.** Muse's discovery is at least partly a live accessibility-tree session, since the research shows it reaching products by browsing to a search engine. For half of agent orders the live-browser constraints apply from the first step.

**Consequences for anything written for agents at steps 1 and 2.** Presence in the HTML matters more than visibility, except for Muse. Facts must be on the product page itself: retrieval returns the product page as a chunk and a policy page is a separate document with no link back to the product. The text must contain the words a query contains ("ships to Israel", not "international shipping available"). Conditions are stable facts, so index staleness hurts them less than price and stock.

---

## 3. Step 1 in depth

### 3.1 Where conditions live on a typical store, against the live-browser reading modes

| Where the merchant put it | Accessibility tree | Text extraction | Screenshot | What it says |
|---|---|---|---|---|
| "Shipping calculated at checkout" under the price | Visible | Visible | Visible | Nothing usable |
| Announcement bar "Free shipping over $50" | Visible | Visible | Visible | Ambiguous: which countries, which currency |
| "Shipping & Returns" accordion, closed by default (Shopify Dawn, most WooCommerce themes) | Not present | Usually present | Not visible | Often the only place the real terms are |
| Policy pages at /policies/... | Needs navigation | Needs navigation | Needs navigation | Prose, often template text, rarely per destination |
| Cart shipping estimator | Needs country and postcode entry | Same | Same | Accurate, late, several steps |
| Checkout after address | The only authoritative answer | Same | Same | Accurate, too late, and only if zones are configured |
| JSON-LD `shippingDetails` and `hasMerchantReturnPolicy` | Not present | Stripped or ignored, unconfirmed | Not present | Correct and structured, almost never filled |

The row that matters is the accordion. For the largest agent that content does not exist.

**Failures this produces, with the evidence.** Silent ineligibility: nothing visible says where the store ships, the order goes through and is cancelled (Forter's 27–37% ex-US, framed by Forter as geography and eligibility mismatch). Wrong total until the last screen: shipping, duties and VAT appear after address; Shopify's own Checkout WebMCP requires the agent to re-ask if the total changes, and nothing enforces that on browser routes. Returns terms never read (Blue Yonder: 46% of shoppers who bought on an AI recommendation returned an item; human-completed purchases). Conflicting statements: a banner and a policy page both true for different buyers (Product.ai: factual conflicts in 86% of answers across four engines; Wharton: agent choice dominated by whichever context it saw first).

**Cloud-IP effect, new in this review.** Muse makes 99.3% of requests from Cloudflare and Fastly IPs and ChatGPT Agent runs from OpenAI's cloud. Any store that personalises by IP (Shopify Markets geolocation, currency switchers, "ships to your location" widgets) shows those agents the United States version of the store. An Israeli shopper's agent sees USD prices and US shipping terms on an Israeli store. Conditions on the browsing route must be enumerated by destination, never inferred from the visitor. Corollary: an audit run from a cloud IP reproduces what the agent sees.

**Design rules that follow, if a conditions block is ever built.** Server-rendered visible text in the initial HTML; adjacent to price and add-to-cart; enumerated by destination; deterministic sentences in a fixed shape with an as-of date; generated from the same record that configures checkout; JSON-LD alongside for Google and any fetcher that parses it.

### 3.2 The agent product record: what each consumer takes

Step 1 is catalog and merchandising, and conditions turned out to be one group of fields in a larger problem: the merchant's product record is shaped for a theme and a human, and every agent consumer reshapes it by inference.

| Consumer | Identity and variants | Attributes it ranks or filters on | Conditions fields | When the field is empty |
|---|---|---|---|---|
| OpenAI ACP feed | `group_id`, `variant_dict`, `gtin`, `mpn`; variant-specific title, URL and media recommended | `product_category`, `material`, `color`, `size`, `gender`, `age_group`, dimensions, reviews, Q&A | `shipping_price`, shipping tuple, `target_countries`, `store_country`, `accepts_returns`, `return_deadline_in_days`, `accepts_exchanges`; a policy URL does not fill them | Falls back to `id` as the group, normalises colour and size itself, treats the row as parent-level |
| Google Merchant Center and page JSON-LD | `item_group_id`, `variant_option` strongly recommended, GTIN and brand required for most categories; `ProductGroup` with `variesBy` and `hasVariant` | Title structure, category taxonomy, recommended conversational attributes | Per-product shipping and return attributes; `shippingDetails` and `hasMerchantReturnPolicy` | Automatic item updates rewrite from the page; mismatches disapprove; shipping and images were 44% of disapprovals (DataFeedWatch 2022) |
| Stripe ACS feed | `item_group_id`, colour, size, three custom option axes, `inventory_quantity` required | Highlights, Q&A, related products, `product_warning`, `age_restriction` | Shipping per country, region, service, speed and price; exclusions; free threshold; fees by region; tax code; no returns terms | Shipping is required for shippable goods, so the row is rejected |
| Shopify Catalog | Variants with options, `sku`, `requires`; clustering under a Universal Product ID across stores | `metadata.attributes`, `tech_specs`, `top_features`, `unique_selling_points`, all Shopify-generated | `ships_to` filter, `refund_policy` URL, safety warnings since Fall '26 | Shopify infers and clusters; the Olaplex example grouped 3.4 oz with 8.5 oz |
| Meta catalog (web, 8 Nov 2026) | `item_group_id`; colour, size, material, age group, gender, additional variant attributes "required where applicable"; one of brand, GTIN, MPN required | Google or Facebook product category required; `rich_text_description` | `shipping` optional, described as an ads overlay; no return-policy field | Not documented |
| Microsoft, PayPal, Amazon Shop Direct | Google-format feed, Shopify channel, or Feedonomics and Salsify | Whatever the Google-format feed carries | Same | Unknown |
| Browsing agents on the page | Title, option selectors, price as rendered | Visible text in reading order | Visible text | Reads the title "Blue / L" and guesses |

Two facts tie this together. Variant-level attributes are optional in both ACP and Google, so agents routinely see parent-level data (research inference). Attribute mismatch is 81.5% of failed rubrics in EComAgentBench, with accuracy falling from 51.8% to 27.2% as implicit requirements grow from 0–1 to 3+.

**Where small-merchant records fall short.** Variants collapsed into titles (variant shape was about half of failed sessions in the UCP Checker sample). No GTIN on handmade and private-label goods. Attributes in prose, including fitment and safety. Category unmapped. Conditions absent even where ACP and Stripe have the fields. B2B and D2C prices and Markets currencies mixed in one record.

### 3.3 Who maps a product to a global catalog, and how Muse sources products

Three layers do the mapping. Merchant record to channel spec: field renaming and validation by the platform channel, a plugin, or a feed manager; nobody rewrites the description. Channel spec to the operator's global catalog: the operator, by inference (Google's Shopping Graph on GTIN, brand and title; Shopify's Universal Product ID clustering; OpenAI's variant normalisation; Meta's catalog matching); attributes left in prose are extracted by a model. Candidate to recommendation: ranking, which for Muse is prompt behaviour.

**Muse's two paths (Profound via PPC Land, web, 8 Nov 2026; 50 prompts, "a small, exploratory sample").** After a request Muse runs two searches in parallel. The catalog path returned about 60 products from 30 merchant sites per prompt, about 94% of candidates. The browser path visited about 3 domains and returned 4 products from 2 sites. The final six cards were typically 2 from the catalog and 4 from the browser. Meta has not said how it weighs the two.

**Consequences.** Many small merchants, including WooCommerce stores in Israel, already have a Meta catalog because they run Instagram and Facebook ads, so for Muse the feed route is far more populated than any agent-specific feed. Doc 08 treats Muse as page-route only, following Forter's classification; that needs correcting to two routes with Profound's split. A record rendered to Meta's spec makes a product a well-formed candidate; the browser path then decides, so the page render of the same record matters more for Muse than for any other agent. There is no Muse naming standard beyond Meta's catalog spec and the leaked prompt's ranking behaviour (well-known merchants first, listings outside the category's normal price range discarded). Open question: whether Muse uses Commerce Manager shipping settings, which would be the only conditions route to the largest agent.

### 3.4 Is there value in exposing the catalog per agent?

Yes, from three things: being a valid row at all (ACP rejects rows with unknown availability, Stripe rejects shippable goods without shipping, Google disapproved 7% of items in the only large study); saying what the agent would otherwise guess (the fields in §3.2); and conditions where a slot exists (ACP, Stripe, Google; not Meta). The lift evidence is the only lift evidence in the research: Shopify's Catalog-fed search converts at twice the scraped rate, UCP merchants complete at 2.7×, Muse with Shopify at 3.5× (vendor and panel, not controlled). Page-side treatments have none.

Three limits. It buys eligibility and fidelity, not ranking. It does not reach the browser path, which carries three in four orders. Access is gated for the two richest specs (ACP feed onboarding for approved partners, Stripe waitlisted, Amazon via Feedonomics); Google, Microsoft and Meta are open.

Shopify does this with one switch for its stores. Off Shopify nobody does it for a small merchant; enterprise has feed managers; WooCommerce, Wix and Squarespace have one plugin per channel, usually Google and Meta for ads.

### 3.5 "Fitting" catalogs per agent: four meanings

| Meaning | Verdict | Why |
|---|---|---|
| Fields per agent | Yes | §3.2 and §3.4 |
| Content per agent (titles, descriptions, vocabulary tuned to each agent's ranking) | No | Disclosed and leaked signals are not about phrasing: OpenAI ranks on availability, price accuracy, quality, primary-seller status, checkout integration; Muse on merchant fame and price range; Google on attribute completeness and feed–page match. Agent-specific copy is the GEO trap: no evidence, opaque target, moves without notice, and divergent content creates the mismatch Google disapproves. The real differences are mechanical: title limits, taxonomy choice, variant-level rows, which enrichment fields exist |
| Cadence and lifecycle per agent | Yes, pure plumbing | ACP wants a daily snapshot and keeps a missing product up to 14 days; Google may disapprove products whose price or availability changes more than once a day and checks the landing page; Shopify Catalog removes up to 7 days after unpublish with no SLA; Stripe takes separate incremental inventory and price feeds. Stale stock, shipping and price were the proximate cause of Instant Checkout's retreat |
| Eligibility and exposure per agent | Yes, as a merchant control | ACP search, ads and checkout flags per product; Google's checkout-eligibility attribute; Stripe's discovery-only switch; Shopify's per-channel toggles. Scattered across five dashboards today |

The catalog should fit the agent's spec, cadence and switches. The content should fit the shopper. The five-agent test can settle the content question cheaply: hold the record constant, vary only phrasing per agent, and measure presence and position.

### 3.6 Verdict on step 1

**No standalone opportunity.** Catalog structure and feeds are a solved layer for anyone who can pay: Shopify by default for a fifth of stores, feed managers for enterprise, free plugins per channel for everyone else, and the two richest specs gated behind OpenAI and Stripe approval. The buyer left over is a long-tail merchant with a free plugin, no agent analytics, and no way to see the defect being fixed. Content tuning has no evidence. Cadence plumbing is a feature of whoever already moves the feed.

**The precise obstacle on conditions is not an incumbent.** Nobody holds them, nobody renders them, and the schema slot is empty on nearly every store. The obstacle is that the merchant cannot see the pain at this step; it shows up five steps later as a cancelled order, a wrong total, or a return. That confirms doc 08's cross-step ordering: step 1 is the input to the order hook, the evidence record and the analytics, not a sale.

**Dropped:** the agent product record as a product; Meta and ACP render targets as a selling point; per-agent content fitting; the transactability endpoint until an agent calls one.

**Carried forward:** the record as an onboarding input to Decide (a few dozen fields per store), and the two-week test.

---

## 4. Defensibility

**The conditions record is not defensible, and doc 08 should stop claiming it is "defensible as the whole".** The source data is the merchant's own and already sits structured in platform settings. Shopify could emit the schema blocks and a conditions paragraph from shipping zones and return rules in one theme release, and it already drafts store facts for merchant approval in its Knowledge Base. A WooCommerce plugin author could do the same quickly. The normalisation is a few dozen fields in five groups. No network effect, no proprietary data, no switching cost. A bundle of absorbable features does not hold against platform defaults.

**What could be defensible in this space.** Only what an incumbent does not see.

- **Outcome data across platforms.** Which agent orders got cancelled, failed, disputed or returned, by platform, agent and vertical, for small merchants. Shopify sees it for its own stores and only on protocol routes. Forter sees it for enterprise and withholds disputes. Nobody sees it for a WooCommerce store in Israel. This is the benchmark (9.3) and the evidence corpus (8.1).
- **Position in the order path.** A hook that decides or holds orders accumulates the outcomes above as a by-product. The decision is copyable; the history is not.

Both are thin while agent orders are a fraction of a percent of volume, and Shopify and Forter will each see more agent orders than a new entrant would for years. That is the honest state of defensibility for the whole pivot, not only for conditions. The record becomes an onboarding input and should not appear as a Truth-layer product anywhere in the docs.

---

## 5. What happens next

1. **The two-week test.** A crawl of a few thousand Shopify and WooCommerce stores scoring the whole record: variant axes explicit or in the title, GTIN present, `ProductGroup` used, attributes structured or prose, shipping and returns present and whether visible without interaction. Then a few dozen stores get a conditions block and the schema blocks from a hand-built record, and five agents (Muse, ChatGPT Agent, Google, Copilot, Comet) are run with destination-laden and attribute-laden questions before and after. One extra probe: ask Muse and ChatGPT Agent for price and shipping on a Markets-enabled Israeli store and record which country they report.
2. **Review step 6 against one question.** Does the order hook produce data nobody else has, and how long until it is enough to matter. If no, the pivot has no defensible core.
3. **Review steps 8 and 9 next**, which §1 ranks highest, before steps 2 to 5, which share step 1's weakness: discovery is contested by trackers and feed managers, lookup and cart belong to the protocols, and the checkout door is not ours.
4. **Wait for the 12 November dispute number** before touching anything at step 8 beyond evidence capture.

---

## 6. Corrections owed to docs 07 and 08

- Doc 08 §1f: rewrite to "no standalone opportunity, two inputs carried forward, one test"; delete "defensible as the whole"; rank render targets by confirmed reader; shrink 1.2 to a pointer to 6.1; add the UCP-shape note to 1.3; widen 1.4 to the record-quality crawl; add the feed–page drift risk (Google automatic item updates).
- Doc 08 steps 1 and 2: Muse has two routes, catalog and browser, with Profound's split; add Meta's catalog as a feed route with wide adoption through ads.
- Doc 08 step 1 reading: add the cloud-IP effect and the product-page-not-policy-page point.
- Doc 08 "Across the nine steps": strike the Truth-layer framing; keep "the same record feeds steps 3, 4, 6, 7, 8" as an input relationship, not a product claim.
- Doc 08 §3: move the live-browser reading-mode table there from the step 1 analysis.
- Doc 07: the Truth layer should be described as an onboarding input to Decide, not a product; §9 should record the step 1 verdict and the defensibility conclusion as undiscussed-with-others positions.
- Glossary: no change needed; "Conditions" and "Merchant-added rule" stand.

---

## 7. Numbers used in this doc

| Number | Value | Source |
|---|---|---|
| Merchant cancellations on scraping-agent orders | 27% Asia and Middle East, 37% Europe; ~10% humans | A#32; `research_notes/Agent enablement platform coverage/browsing_pattern.md` (Forter, 2 Nov 2026) |
| Muse share of agent orders | 49% (Forter, wk of 19 Oct); bought from 74% of stores receiving any agentic order | doc 06; `reports/Agent enablement platform coverage.md` |
| Muse request origin | 99.3% from Cloudflare and Fastly IPs | `reports/Agent enablement platform coverage.md` browsing table |
| Muse reading mode | Accessibility tree; cannot run scripts, hover or click to reveal hidden content | `browsing_pattern.md` (Botify, 21 Oct 2026) |
| Muse two discovery paths | Catalog ~60 products from 30 sites (~94% of candidates), browser ~4 from 2 sites; final cards 2 catalog, 4 browser; 50 prompts | Web, 8 Nov 2026: PPC Land on Profound. Not in the research folder |
| Meta catalog spec fields | Required id, title, description, availability, condition, price, link, image_link, one of brand/GTIN/MPN; category required; `shipping` optional; no return-policy field | Web, 8 Nov 2026: developers.facebook.com catalog reference. Not in the research folder |
| Attribute mismatch share of agent failures | 81.5% of 12,467 failed rubrics; accuracy 51.8% → 27.2% with 3+ implicit requirements | `research_notes/Shopping agent accuracy trajectory/benchmark_trajectory.md` (EComAgentBench) |
| Variant-shape share of failed live sessions | About half | doc 07 Appendix A (UCP Checker playground) |
| Feed disapprovals | 7% of items; shipping and images 44% of disapprovals (2022, 4.5M items, 15k stores) | `structural_error_sources.md` (DataFeedWatch) |
| Google automatic item updates | Products changing price or availability more than once a day "may be disapproved rather than updated" (27 Sept 2026) | `structural_error_sources.md` |
| ACP feed lifecycle | Daily snapshot recommended; missing product retained up to 14 days; feed onboarding for approved partners | `protocol_pattern.md` (OpenAI commerce docs) |
| Shopify Catalog lifecycle | Removal up to 7 days after unpublish; no freshness SLA | doc 07 Appendix A; `shopify.md` |
| Structured-serving lift | Catalog-fed search 2× scraped; UCP 2.7×; Muse+Shopify 3.5× (vendor and panel, not controlled) | doc 06 §7 row 8; `shopify.md`; `protocol_pattern.md` (FirstPageSage) |
| Returns after AI recommendation | 46% of US shoppers returned ≥1 item (human-completed purchases) | `cross_agent_telemetry.md` (Blue Yonder, 2 Nov 2026) |
| Factual conflicts across engines | 86% of 220 shopping questions | `cross_agent_telemetry.md` (Product.ai, Sept 2026) |
| Context-order effect | A single source shifted choice up to +99 pp; source order switched >50% of runs for one model | `cross_agent_telemetry.md` (Wharton, 27 Aug 2026) |
| Instant Checkout retreat cause | Stale stock, shipping and price; ~3× lower conversion than redirect | `research_notes/Shopping agent accuracy trajectory/production_evidence.md` |

**Caveats.** The Profound sample is fifty prompts and the authors call it exploratory. Meta's spec was read from the developer reference, not from any Muse-specific documentation, which does not exist publicly. Whether Muse uses Commerce Manager shipping settings is unverified. The web-sourced rows should be moved into the research folder if they are kept.
