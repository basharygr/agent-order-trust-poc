# 05 — Research conclusions: agent orders, accuracy, enablement, and the serving-layer product

**Date:** 4 November 2026
**Status:** Working conclusions. Synthesizes three research reports (in `reports/`) and the discussion that followed them. Not decisions. Docs 01–04 are the earlier brainstorm; where this doc disagrees with them, this doc is the newer view and 03/04 are due for a redraft.

**Sources this doc rests on**

| Report | Question it answered | Notes folder |
|---|---|---|
| `reports/Shopping agent accuracy trajectory.md` | Will agent accuracy rise enough to remove the merchant-side trust pitch? | `research_notes/Shopping agent accuracy trajectory/` |
| `reports/Shopping agent production data 2026.md` | What do the major shopping agents actually show in production (May–3 Nov 2026)? | `research_notes/Shopping agent production data 2026/` |
| `reports/Agent enablement platform coverage.md` | Which e-commerce platforms cover agent enablement, across browsing and protocol access, and how well? | `research_notes/Agent enablement platform coverage/` |

---

## 1. Conclusions in one page

1. **Do not pivot on accuracy grounds.** Agent accuracy on purchase tasks is rising fast on benchmarks but has a structural floor of roughly 2–5% materially wrong orders per order at a typical small merchant, set by unclear intent, bad merchant data, and humans rubber-stamping confirmations rather than by model reasoning. Better models erode always-on confirmation and standalone detection; they do not erode merchant rules or signed evidence.
2. **Reliability is not accuracy, and nobody tracks it.** Accuracy (pass^1) has risen from roughly 44% to 79% on general agent tasks; consistency (run-to-run) moved only from 0.76 to 0.84. No published series tracks per-order failure probability for a 2025–2026 model.
3. **Production outcome data does not exist.** No operator, card network, processor or fraud vendor has published a wrong-item, cancellation, return or dispute rate for agent orders. The only network data is Forter's, and it is about eligibility failures and approval rates, not accuracy. Chargeback rates are unknown and cannot be inferred from the 1–2% customer-cancellation figure.
4. **The human-signed-cart flow dominates, but its accuracy is unmeasured, not proven good.** Every major agent shipped approval-by-default with caps or hard price ceilings. The absence of disclosed error rates is consistent with nobody checking.
5. **Agents will keep creating merchant-side problems** because intent is non-deterministic, cart construction is a compromise, and human review costs conversion. Adoption volume, standards and policy decide the outcome; merchant protection matters most in the early adoption window.
6. **Discovery is moving to agents regardless of whether checkout does.** Enablement therefore has value now; checkout-in-agent is confined to a few agents and a handful of buyer countries.
7. **Only Shopify does enablement end to end**, default-on at every layer. Wix and BigCommerce expose discovery and carts but never let an agent pay. WooCommerce is an opt-in developer preview with no cart or checkout. Enterprise platforms are absent or enterprise-only. Every platform offers on/off controls only, loses fidelity on shipping, returns, warranty and member pricing, stores no consent evidence, and cannot attribute failed or pixel-less agent checkouts.
8. **No one has measured lift from any serving treatment.** llms.txt, JSON-LD, WebMCP and accessibility fixes have no published completion-rate evidence. The product must generate that evidence itself.
9. **Israel gets discovery, not checkout.** The gate is held by Google, Microsoft, Meta, OpenAI, Stripe and PayPal, not by the platforms. No Israeli PSP supports any agent payment protocol. Wix Payments is not offered in Israel.
10. **The product is an end-to-end serving layer for agents, sold to merchants who are not at the standards table but have the most to lose.** Five layers: Truth, Serve, Decide, Prove/Measure, Guarantee. Shopify is a distribution and learning channel; WooCommerce, Wix, custom carts and non-US merchants are the core. The wedge is "see what agents see," which doubles as the instrument that produces the missing outcome data.
11. **Agent identification is tractable, not a moat and not a blocker.** Mainstream cloud agents are converging on cryptographic signing because platforms pay them to. The product verifies signatures against the registries others run and treats unsigned in-browser agents as page traffic. Identification must be cryptographic; a user-agent string is not identification.

---

## 2. Will accuracy remove the pitch? No.

### Definitions used throughout

- **Accuracy** = pass^1, the share of tasks completed correctly on a single run, averaged across tasks.
- **Reliability** = whether the same task succeeds every time, measured as pass^k or as run-to-run consistency.
- A purchase needs a low **per-order failure probability**. That is neither number directly, and no published series tracks it.

### Benchmark trajectory

| Benchmark | What it measures | Trajectory | Read |
|---|---|---|---|
| tau2-bench Retail | Order-service conversations against a clean API | 74.0% (GPT-4.1, Apr 2025) → 91.9% (Claude Opus 4.6) | Saturated. API-shaped tasks are solved. |
| EComAgentBench | Multi-step shopping tasks | → 57.1% (Opus 4.6) | Half of realistic shopping fails. |
| ShoppingBench | Constraint-heavy intents | 48.2% overall; "Coupon & Budget" 30.4% for every model | Constraints are where agents break. |
| ShopSimulator | Full shopping simulation | 32.7% (GPT-5) | Lowest; richest tasks. |
| Princeton HAL (general agent tasks) | Accuracy vs consistency | Accuracy 44% → 79%; consistency 0.76 → 0.84 | Accuracy outruns reliability by a wide margin. |
| METR horizons | Task length at given success rate | 50% horizon ~12–17 h; 80% horizon ~1.5–3 h; no 99% horizon measurable | The tail is the problem, and purchases live in the tail. |

### Three floors that models cannot lower

1. **Intent.** Humans state purchase intent non-deterministically. Cart construction from signed intent is a fitting exercise with compromises, and the less involved the human, the more doubt in the outcome.
2. **Merchant data.** The agent inherits feed error rates, stale stock, undeclared shipping and returns conditions. OpenAI's March Instant Checkout retreat was attributed to stale stock, delivery and shipping-cost data.
3. **Human attention.** Anthropic's 1,053-tester study found people approve 97% of permission prompts and catch about 13% of planted errors. Confirmation screens become rubber stamps.

**Estimate (ours, not sourced):** by 2028–2029 frontier agents place spec-driven single-item orders correctly 85–95% of the time and multi-constraint carts 70–85%, with a per-order materially-wrong-order floor around 2–5% at a typical small merchant. This is an accuracy estimate and says nothing about whether those orders concentrate in predictable order types (rules can target them) or scatter randomly (only per-order evidence helps). Thin pass^k data suggest a mix.

### Loss math

At the earlier loss model's 26% of order value per mistake, a 2% wrong-order rate costs 52 basis points of agent GMV. Card-not-present fraud runs about 42 bp and sustains a multi-billion-dollar vendor industry. Agent error at its floor is a larger loss line than fraud.

### What erodes and what persists

| Component | As accuracy rises | Why |
|---|---|---|
| Always-on human confirmation of every agent order | Erodes | Pays at today's 40–70% cart-benchmark failure rates; stops paying at 2–5%. Must be routed to the risky minority, as 3DS2 and Waymo route theirs. |
| Standalone agent detection | Erodes | Agents are moving to sign; detection commoditizes into registries run by Cloudflare, Akamai, Shopify, Visa. |
| Merchant rules (eligibility, geography, caps, drift) | Persists | They address the failures actually occurring in production (see §3). |
| Signed intent and dispute evidence | Persists | Standards (AP2 mandates, UCP, Visa TAP, Mastercard Verifiable Intent) write the evidence requirement in; they do not remove it. The merchant is the seller of record in every regime. |

---

## 3. What production shows

### The vacuum

Across OpenAI, Google, Microsoft, Meta, Amazon, Perplexity, xAI, Walmart, Shopify, Visa, Mastercard, PayPal, Stripe and the fraud vendors, not one has published an order-accuracy, wrong-item, cancellation, return or dispute rate for agent orders. The only directly observed count of agent-completed orders in the window is DoorDash's "maybe several hundred" against 993 million quarterly orders. Visa called agentic volume "very small" for FY2027. Every other figure is an engagement metric.

### Forter's network data (23 July–29 Oct 2026), the only outcome data anywhere

- **Mix.** Scraping agents (Muse, Instinct, Grok Bot, ChatGPT Agent) are ~77% of agent orders; Muse alone 49% in the week of 19 October, bought from 74% of stores receiving any agent order. OpenAI, Google and ChatGPT Agent orders combined grew only 4% month over month.
- **Authorization is not the problem.** Protocol agents authorize at 100%, scraping agents at ~98%.
- **Merchant-side risk approval was the problem.** Scraping-agent approval averaged 44% before Forter's 24 September rule changes and 73% after; humans and protocol agents unchanged.
- **Cancellations.** Customer-initiated 1–2% for most agents, ~10% for Google (single-item checkout, baskets 56% below average). Merchant-initiated reach 27% in Asia/Middle East and 37% in Europe for scraping-agent orders, attributed mostly to agents ordering outside the merchant's eligibility (wrong country, currency, unsupported merchant).
- **Withheld.** Fraud rates, dispute rates, network size. Forter promises to address "whether disputes are higher" in its 12 November webinar.

### Reading the ex-US cancellation numbers

They are the closest thing to a measured unfulfillable-order rate, an order of magnitude above the projected accuracy floor, but not comparable to it: they are eligibility mismatches, a transaction-enablement failure the merchant's own rules can prevent. This is direct evidence for the Decide layer (§6) and for enablement before detection.

### Landscape moves in the window

- OpenAI removed ChatGPT agent mode (July–August) and shut the Atlas browser (9 August) with no stated reason; Instant Checkout moved to merchant apps and referral in March after Walmart reported one-third the click-out conversion and CNBC reported item information "often inaccurate."
- Amazon walled off Muse and unidentified agents; its Agent Terms require a named user agent. Amazon alone added unattended Auto-Buy.
- Walmart Agent Profiles (announced 21 October, launch within a month): approval-by-default or customer-set caps of $50/$100/$200; agents never receive card data.
- Google, Meta, Anthropic, Shopify all shipped approval-by-default designs with spend caps or hard price ceilings.

### HP versus NHP, and the cart-review flow

The flow that emerged is human-present: agent reviews intent, builds the cart, shows cart and conditions, human signs, order goes. This is the dominant design, and it is the design the standards reward (AP2 Cart Mandate). But its accuracy is **unmeasured**, not proven good. The 1–2% customer cancellation figure does not imply low chargebacks, because:

1. The orders are weeks old; disputes lag 30–120 days.
2. Cancellation and dispute are different exits for different failures (pre-ship regret vs post-delivery wrong item).
3. The loss model in doc 01 has chargebacks at ~8% of mistakes and cancel-before-ship at ~10%, so a low cancel rate is consistent with an unchanged chargeback share.
4. It is equally consistent with no one checking.
5. Google's 10% is a live counterexample from the narrowest, most controlled checkout.

### Data to watch

| When | What | Why it matters |
|---|---|---|
| 3 Nov (not yet reviewed) | Shopify Q3 | Whether agent-completed orders are distinguished from AI-referred. |
| 4 Nov | Uber, Wayfair, Mercado Libre Q3 | Same. |
| 12 Nov | Forter webinar "The First 60 Days of Agentic Transactions" | Only scheduled disclosure promising a dispute-rate answer. |
| Mid/late Nov | Walmart Q3 FY27 and Agent Profiles launch | First possible retailer statement on third-party agent orders under caps. |

---

## 4. The founder's thesis, evaluated against the data

| Claim | Evidence | Verdict |
|---|---|---|
| Human intent will never be fully clear; language is non-deterministic | Constraint-heavy benchmark intents are the worst-performing class (~30%); intent is the first of three structural floors | Supported |
| Accuracy caps around 92–95% | API-shaped tasks already at 91.9%; realistic shopping 33–57%; our floor estimate 2–5% wrong orders by 2028–29 | Supported in direction; the cap is on realistic carts, not on clean APIs, which already exceed it |
| Reliability will not improve much because errors compound | HAL consistency 0.76 → 0.84 while accuracy nearly doubled; METR 80% horizon an order of magnitude below the 50% horizon | Supported, with the caveat that nobody measures per-order reliability for current models |
| Cart construction is rigorous, complex, compromise-laden | Multi-item and constraint tasks are the failure modes; stale data and variant mapping drive failed sessions | Supported |
| Human review costs conversion | Walmart: in-chat checkout at one-third the click-out conversion; 97% rubber-stamp rate | Supported, and it cuts both ways: review that is kept costs conversion, review that is skipped costs accuracy |
| Agents will challenge merchants' business model; adoption, standards and policy decide | Platforms and networks shipped caps and approval-by-default within one quarter; OpenAI reversed autonomy | Supported |
| Merchant protection and guarantees are critical in early adoption | The merchant is seller of record under every regime; no platform stores consent evidence; ex-US merchant cancellations 27–37% | Supported; the guarantee is a cheap promise early (low volume) and a priced product later |
| Enablement is the larger opportunity because discovery moves first | In-agent checkout confined to a few agents and buyer countries; AI-referred traffic projected up 130% for US holiday; Shopify auto-enrols; Walmart and Target lead earnings with agents | Supported |

---

## 5. Enablement: who covers it, and how well

### Platform tiers

| Tier | Platform | Discovery / catalog | Cart | In-agent checkout | Browser-agent handling | Controls |
|---|---|---|---|---|---|---|
| End to end | **Shopify** | Default-on Catalog (>1B products) to ChatGPT, Copilot, Google, Meta, Shop | UCP/MCP | Google, Copilot, Meta direct checkout; ChatGPT referral-only; Checkout WebMCP for Web Bot Auth-signed Chromium agents with saved Shop Pay cards | Signed agents get higher rate limits and checkout; no documented way to control unsigned browser agents | Master switch, per-channel toggles; no caps, no per-market or per-product rules |
| Exposure without payment | **Wix** | Default-on UCP profile (~6,700 stores); built-in MCP | UCP | None: agent gets `continue_url` to Wix checkout | None | None |
| Exposure without payment | **BigCommerce** | Cloudflare-injected WebMCP on ~100k domains | WebMCP cart | None: checkout link handed back | Cloudflare bot categories | On/off |
| Developer preview | **WooCommerce** | Opt-in MCP | None | None; Stripe Agentic Commerce Suite waitlisted | None | None |
| Enterprise or absent | Adobe, Salesforce, commercetools | Store-management MCPs, enterprise connectors | Varies | Enterprise programs only | Vendor bot management | Varies |

### Access patterns and who uses which

- **Protocol** (UCP, ACP, AP2, feeds): Google, Copilot, Meta direct checkout, ChatGPT merchant apps. Clean, authorizes at 100%, but structurally lossy: shipping rates, returns windows, warranty, member and B2B pricing, subscriptions, bundles and local pickup are excluded or flattened on every protocol.
- **Browsing** (scraping, in-browser): Muse, Instinct, Grok Bot, ChatGPT Agent, Comet, Claude in Chrome, Gemini in Chrome, Edge Copilot. ~77% of agent orders today. Sees whatever the page shows, stalls on theme-level DOM problems, cookie walls, client-rendered prices and hidden variant selectors.

### Where MCP fits

MCP is not a third access pattern. It plays four roles: a transport for UCP; store-management MCPs (Woo, PrestaShop, commercetools, Adobe) that expose admin operations, not shopping; agent-side aggregators (shop.md-style) that wrap many stores; and WebMCP, the W3C draft that lets a page register tools (`search_catalog`, `add_to_cart`, `complete_checkout`) for in-browser agents. WebMCP is the one that matters for the browsing pattern, and it is only live by default on Shopify checkout (signed agents) and BigCommerce (Cloudflare edge).

### Structural gaps, ranked (from the enablement report)

1. Pre-checkout transactability verdict per variant per destination (ships-to at postcode level, landed cost, returns window, promo validity, stock-confidence age). All platforms.
2. Browser-agent handling at SMB checkout: detect, allow verified, challenge unverified, keep humans unaffected. All platforms.
3. Rules beyond on/off: per-product, per-market, per-channel eligibility, quantity and price-drift caps, agent allowlists. All platforms.
4. Attribution that counts failed agent checkouts and pixel-less direct checkouts. All; Shopify partial.
5. Consent and dispute evidence for agent-placed orders. No platform stores a consent artefact; supplemental terms leave every dispute to the merchant.
6. Variant and ID mapping between feed, storefront and checkout. About half of failed UCP sessions.
7. Non-US, non-English, non-USD in-agent checkout. Gated by Google, Microsoft, Meta, OpenAI, Stripe, PayPal.
8. Stale-stock and price-drift guarding with a freshness signal. No feed or Catalog publishes an SLA.
9. Member, loyalty, B2B, subscription, bundle, customizable and local-pickup coverage. Excluded from every direct checkout.
10. Headless, custom and Hydrogen exposure.
11. Theme-level DOM readability for browsing agents.
12. Product-level Q&A (fitment, safety, compatibility).

### Data structuring and page layout: the market as it stands

- **Feed syndicators** (Feedonomics, Productsup, Salsify, Syndigo, Paz.ai, ReFiBuy) structure catalog data for protocol channels. None handles shipping, returns or warranty conditions as first-class data, and none serves the browsing pattern.
- **Checkout connectors** (Firmly, Stripe Agentic Commerce Suite, PayPal Store Sync) wire the merchant to whichever agent admits them. Gated by eligibility and geography.
- **Agent-side buyers** (Rye, Crossmint) buy on the agent's behalf and do nothing for the merchant.
- **Page layout for agents** (ads, heavy UI, server load, latency, DOM readability) has no vendor and no measured lift. Pottery Barn, West Elm and Williams-Sonoma publish llms.txt and still block checkout. Akamai found commerce sites left more than 90% of AI-bot traffic in monitor-only mode.

### Israel

An Israeli merchant can be found by agents on every major platform and cannot be bought from inside any agent on any of them. The exclusion is enforced upstream by Google, Microsoft, Meta, OpenAI, Stripe and PayPal. No Israeli processor (Isracard/PayMe, Pelecard, Tranzila, PayPlus, Grow, iCredit) supports Shared Payment Tokens, Visa TAP, Mastercard Agent Pay or AP2. The agent traffic that actually arrives is unsigned browsing traffic driving a checkout never declared agent-eligible, for destinations the agent never checked. The product cannot lift the gate; it can make whatever arrives correct for the destination and defensible afterwards, and it can carry discovery now and readiness for when the rails open.

---

## 6. Product definition: an end-to-end serving layer for agents

### Why this framing

Merchants face a change in how they expose products and serve customers. They are not part of the decision-making, although they have the most to lose. They must adapt quickly to varying and competing standards. Their platforms do half the job and force merchants to be as narrow as the platform chooses. A serving layer that owns the merchant's side of every agent interaction, across both access patterns, is the counterpart to what CDNs did for the web: one layer between the origin and every consumer, regardless of who the consumer is.

### The five layers

| Layer | What it does | Who does it today | Platform coverage |
|---|---|---|---|
| **Truth** | One canonical, fresh, per-destination record of product, variant, price, stock-confidence, shipping, returns, warranty, eligibility; served identically to feeds, UCP/ACP, WebMCP and the page | Feed syndicators for catalog only; nobody for conditions | Shopify Catalog store-level; others none |
| **Serve** | Expose Truth over every pattern: protocol profiles and endpoints, WebMCP tools, agent-readable page variants, agent-specific rate and latency handling | Platforms per their own protocol choice; Cloudflare/Akamai at the edge for bots generally | Shopify full for its channels; Wix/BigCommerce partial; Woo/custom none |
| **Decide** | Deterministic rules at order time: eligibility, geography, caps, drift, allowlists; route the risky minority to confirmation | Forter-style risk vendors (approval), nobody for merchant rules | On/off only, everywhere |
| **Prove / Measure** | Signed intent and consent artefacts per order; attribution that counts failed and pixel-less checkouts; completion and failure telemetry per agent | Nobody stores consent; attribution partial on Shopify only | Gap 4 and 5 |
| **Guarantee** | Absorb the merchant's loss on qualifying agent orders | Nobody | None |

Enablement (Truth + Serve) is the sale. Decide and Prove are what keep the merchant safe and produce the data. Guarantee is a cheap promise at today's volumes and a priced product once the dispute rate is known.

### Competition by layer

- **Edge framing:** Cloudflare and Akamai are the main competitors if the product is positioned as "serve agents at the edge." They already categorize agent traffic, inject WebMCP (BigCommerce), and run the signature registries. They do not know the merchant's catalog, conditions or rules.
- **Truth:** feed syndicators, for catalog on the protocol side only.
- **Serve (checkout):** Firmly, Stripe, PayPal, and the platforms themselves.
- **Decide:** fraud vendors, at the approval step, not the merchant's eligibility step.
- **Prove, Guarantee:** no incumbent.

### The four hard points, and where the discussion landed

1. **Agent identification.** Resolved as tractable. Mainstream agents do not mask like fraudsters; they fight bot protection to improve their own completion rates. OpenAI's cloud browser, Google's agent subset and Manus sign Web Bot Auth; Meta quietly began signing a share of Muse requests around 12 October; Amazon requires a named user agent; the IETF adopted the draft on 1 September; Shopify rewards signing with higher rate limits and the only browser-agent checkout. The product consumes the registries (Cloudflare, Akamai, Shopify, Visa TAP) and verifies signatures; it does not build fingerprinting. Unsigned in-browser agents (Comet local, Claude in Chrome, Gemini in Chrome, Edge Copilot) are the user's own session, lower risk, and need a readable page, not routing. Identification must be cryptographic; once verified agents get privileged treatment, impersonating one becomes the fraud vector.
2. **Shopify.** A partial offering is acceptable because checkout is not where most of the value is. Shopify leaves open everything before and after checkout: shipping, returns and member pricing in Catalog, failed-checkout attribution, browser-agent control, headless, product-level Q&A. Shopify's terms allow termination without notice, so it is the distribution and learning channel, not the moat. The moat is WooCommerce, Wix, custom carts and every non-US merchant, where nobody does the job.
3. **Scope.** A planning question. Start focused, pick the wedge, evolve. Each stage should fund the evidence for the next.
4. **No measured lift.** Offset by merchant fear of missing the shift: Shopify auto-enrols, Walmart and Target lead earnings calls with agents, Salesforce forecasts a fifth of holiday traffic from agents. Fear sells audits and readiness. Retention needs measured value within a quarter, and the retention metric that exists today is discovery: AI-referred traffic and its conversion. In Israel there is less hype to ride; the pitch there is discovery now and readiness for the rails.

### The wedge: see what agents see

Run every major agent, across both access patterns, against the merchant's store. Show where each one is blocked, stalls, misreads a variant, gets the wrong shipping answer, or orders for a destination the merchant does not serve. This repoints POC-1 (detection) and POC-3 (agent relay) from the earlier brief at merchants rather than at agents, costs the merchant nothing to try, and produces the completion and failure data that no one has published. The Truth and Serve layers that follow are the fixes for what the audit finds; Decide and Prove follow from the failures it records.

### What this changes relative to docs 03 and 04

- The product brief's "Agent Order Trust Layer" becomes the Decide and Prove layers of a larger serving layer; enablement moves from a side benefit to the sale.
- POC-1 (detection) shrinks to signature verification against external registries plus page-level handling of the rest.
- POC-3 (agent relay) becomes the audit instrument.
- POC-2 (passkey), POC-4 (3DS) and POC-5 (platform checkout intervention) are deferred until the dispute rate is known; they address a loss line that has not yet been quantified.
- Israel-first stays, reframed: discovery and readiness, not in-agent checkout.

---

## 7. Open questions

1. Agent-order dispute rate (Forter, 12 November). Decides whether Guarantee is a promise or a product.
2. Whether any retailer or platform distinguishes agent-completed from AI-referred orders in Q3 reporting.
3. Whether the 2–5% floor concentrates in predictable order types or scatters. The audit wedge is the first instrument that can answer this per merchant.
4. Which registries will actually publish key directories (Meta signs but publishes none). Without a directory, a signature cannot be verified.
5. Whether any serving treatment produces measurable completion lift. Must be answered by the product's own telemetry.
6. Pricing of the Guarantee once a dispute rate exists; the 52 bp loss-line estimate is the ceiling of what a merchant would pay for full cover.
7. First Israeli PSP to support any agent payment protocol, which would open in-agent checkout locally.

## 8. Known weaknesses in the underlying reports

- The accuracy report's Muse section asserts that Muse's funnel problems are "site access and habit, not measured wrong orders." That attribution is inferred, not sourced; no source ties Muse's day-14 drop-off to any cause. The surrounding retention figures (73% day 1, 30% day 7, 23% day 14) are sourced and are an app-install cohort metric, not an order-accuracy metric.
- The 2–5% floor and the 2028–29 projection are our estimates, not sourced forecasts.
- Forter does not publish its network size, so its percentages have unknown weight.
- Shopify's Q3 release of 3 November has not been reviewed.
