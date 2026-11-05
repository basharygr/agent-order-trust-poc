# Glossary

Terms as they are used in these docs. Where a term has a broader meaning elsewhere, the definition here is the one that matters for this project.

## Commerce and payments

**GMV (gross merchandise value).** The total value of everything sold through a store or platform in a period, before returns, cancellations, fees or discounts are subtracted. Not revenue: Shopify's revenue is a few percent of its GMV. Used here for sizing (Shopify is ~14% of US sales by GMV) and for pricing (a fee charged as a share of GMV earns little on the long tail).

**Basis point (bp).** One hundredth of one percent. 52 bp = 0.52%. Used for loss lines and fraud rates because they are small fractions of sales.

**Channel.** Any place outside the merchant's own site where its products are shown and from which a sale originates: search and shopping ads (Google), social (Meta, TikTok), marketplaces (Amazon, eBay), affiliates and comparison sites. The merchant does not own it, usually pays for it, and formats products to its rules, typically via a feed. Email and SMS are the owned exception. Agents are becoming a channel: they decide which merchants a shopper sees, take a feed, sell ads, and may complete checkout inside their own surface. Shopify uses the word literally: ChatGPT, Copilot, Google and Meta are "sales channels" in its admin, hence "per-channel toggles" and "per-channel reports."

**PSP (payment service provider).** The company that takes a card payment on the merchant's behalf: Stripe, PayPal, Adyen globally; Isracard/PayMe, Pelecard, Tranzila, PayPlus, Grow, iCredit in Israel. Agent payment protocols are implemented by the PSP, so the PSP decides whether an agent can pay a merchant inside the agent.

**Card networks.** Visa, Mastercard, American Express. They set the rules under which a payment is authorized and under which a dispute is decided, and they are publishing their own agent protocols (Visa TAP, Mastercard Agent Pay).

**Issuer.** The bank that issued the shopper's card. It authorizes or declines a payment.

**Authorization.** The issuer's yes or no to a payment. Agent orders authorize at 98.8–100%, so authorization is not where agent orders fail.

**Risk approval.** The merchant's or its fraud vendor's decision to accept an authorized order, made after authorization. Forter's scraping-agent approval rate moved from 44% to 73% when it loosened rules. This is a separate gate from authorization.

**CNP (card-not-present) fraud.** Fraud on payments where the card is not physically presented, i.e. all e-commerce. Runs about 42 bp of sales and is the benchmark the docs compare agent error losses against.

**Chargeback / dispute.** The cardholder asks their bank to reverse a payment. The merchant loses the money, usually the goods, and pays a fee. Disputes lag the order by 30 to 120 days, which is why a low cancellation rate on weeks-old agent orders says nothing about disputes.

**Cancellation, customer-initiated vs merchant-initiated.** A customer cancellation is the shopper backing out before shipment. A merchant cancellation is the merchant refusing to fulfil, typically because the order cannot be served (wrong country, wrong currency, unsupported destination). Forter reports 1–2% customer and 27–37% merchant cancellations on ex-US scraping-agent orders.

**Seller of record.** The party legally selling the goods and therefore liable for the order, the dispute and the tax. In every agent protocol published so far, the merchant is seller of record; the agent company is not.

**3DS / 3DS2 (3-D Secure).** The card networks' step-up authentication at checkout. 3DS2 routes only risky transactions to a challenge. Used in the docs as the model for routing only the risky minority of agent orders to confirmation.

**Passkey.** A cryptographic login credential bound to a device. Appears in pivot 1 as a way to prove the human was present at checkout.

**Conversion / conversion rate.** The share of visits that end in an order. Human e-commerce conversion is roughly 2–3%. Agent checkout is 2.3–2.4% of agent activity, about the same.

**Click-out.** The agent or search result sends the shopper to the merchant's own site to finish buying, as opposed to buying inside the agent. Walmart found in-agent checkout converted at one third of click-out.

**Discovery.** The finding step: which products and merchants become candidates. Happens off the merchant's site, in search, feeds, ads, marketplaces, and now inside agents. Step 2 of the flow. Not to be confused with lookup.

**Lookup / evaluation.** The checking step: variant, price, stock, delivery, returns and warranty for this item at this merchant. "Evaluation" when a human does it on the page (step 3 of the flow); "lookup" when an agent does it. For browsing agents lookup is the merchant's page; for protocol agents it is the channel's copy of the merchant's feed, so the merchant's site is never touched. About 95% of agent activity.

**Transactability.** Whether this item can actually be bought by this buyer: ships to this address, in this currency, at this landed cost, by this date, under these returns terms. Neither the feed nor the page answers it today; agents resolve it by building a cart and reaching checkout, which is where the ex-US eligibility cancellations come from.

**Landed cost.** The total the buyer pays to have the item arrive: price plus shipping, duties and taxes for that destination.

**Eligibility.** Whether the merchant will actually serve an order: ships to that country, accepts that currency, allows that quantity, honours that promo. Eligibility failures are the one confirmed order-time failure in production.

**GTIN (Global Trade Item Number).** The number in a product's barcode: UPC (12 digits, North America), EAN (13 digits, Europe and Israel), ISBN for books. One GTIN identifies one sellable variant worldwide. Channels (Google, Amazon, Meta) match listings to their own product records by GTIN; listings without one are demoted or rejected. The one identifier that can anchor variant mapping across feed, storefront and checkout.

**PDP / PLP.** Product detail page (one product) and product listing page (a category or search result). The pages browsing agents read most.

**SEO.** Search engine optimization: making pages rank in Google. The twenty-year-old discipline that agent discovery has no equivalent of yet.

**Pixel / UTM.** A pixel is a channel's tracking script on the merchant's page that reports a visit or a sale back to the channel. UTM parameters are tags on an inbound link naming its source. Together they are how attribution works; neither fires when checkout happens inside an agent.

**ROAS (return on ad spend).** Revenue attributed to a channel divided by what was spent on it. Unmeasurable for a channel whose sales are not attributed.

**3PL.** Third-party logistics: a warehouse that stores, picks, packs and ships for the merchant.

**False decline.** A legitimate order refused by risk approval. Scraping-agent orders were declined at 56% until Forter loosened its rules.

**Friendly fraud.** A shopper disputing a charge they actually authorized. Expected to be a question for agent orders, because the shopper may not remember what the agent did.

**Oversell.** Accepting an order for stock that is not there. A stale-data failure that agents trigger more often because they act on cached data.

**Variant / SKU.** A specific sellable version of a product (size, colour, pack). SKU is the merchant's stock-keeping identifier for it. Variant mapping between feed, storefront and checkout is about half of failed UCP sessions.

**Conditions.** Shorthand in these docs for the terms attached to an offer: shipping rates and destinations, returns window, warranty, member or B2B pricing, promo validity. Every protocol flattens them.

**Catalog.** The description of the items a merchant sells: name, brand, category, attributes, images, GTIN. Answers "what is this thing," changes rarely, and is the same whichever merchant sells it. Distinct from **inventory** (how many, where; the merchant's, changes hourly) and from the **offer** (at what price, to which destinations, on what shipping, returns and warranty terms, right now; the merchant's, changes hourly). In agent commerce the word is stretched to mean the feed, which carries catalog plus price and stock: Shopify Catalog, Google Merchant Center feeds and Botify's AgenticCatalog are all feeds. Where these docs mean the merchant's terms, the right word is offer.

**Offer.** In these docs, a variant for a destination with its price, stock and conditions. The unit the product's record is built on; no platform models it.

**Long tail.** The millions of small stores on WooCommerce, Wix, Squarespace and other open-source carts. About 40% of stores and about 5% of sales.

**Enterprise custom.** Large retailers running their own stack (Amazon, Walmart, Target). About 45% of the top 10,000 e-commerce sites and most US sales.

**Headless.** A store whose front end is custom-built and talks to the platform through APIs rather than using the platform's themes. Shopify's Hydrogen is its headless framework. Headless stores miss most default-on agent features.

**Theme.** The platform-supplied templates that render a store's pages. Theme-level DOM problems (hidden variant selectors, client-rendered prices, cookie walls) are what browsing agents stall on.

**Order hook.** A point in the platform's code where a plugin can inspect or stop an order before it is accepted. WooCommerce exposes many; Shopify exposes fewer. Where the Decide layer runs.

**App proxy.** A Shopify mechanism that lets an app serve content under the store's own domain. One way to add agent-facing pages to a Shopify store.

**Feed.** A bulk file of product data a merchant sends to a channel (Google Merchant Center, OpenAI ACP feed, Meta). Products only; conditions are not in it.

**Feed syndicator.** A vendor that builds and distributes feeds to many channels (Feedonomics, Productsup, Salsify, Syndigo). Handles catalog, not conditions, not the page.

**Fraud vendor.** A service that scores orders for risk after authorization (Forter, Signifyd, Stripe Radar). Forter's network data is the only production outcome data on agent orders.

**Bot management.** Services that classify and allow or block automated traffic (Cloudflare, Akamai, HUMAN, DataDome, Vercel BotID). They now carry categories for AI agents and run the signature registries.

**Edge / CDN.** Servers between the shopper and the merchant's origin that cache and modify responses (Cloudflare, Akamai, Fastly, CloudFront). Where bot management and some agent-serving products (Botify SpeedWorkers, BigCommerce's WebMCP) run.

## Agents and access patterns

**Shopping agent.** Software that searches, compares, builds a cart or buys on a person's behalf: ChatGPT, Meta Muse, Google AI Mode and Gemini, Microsoft Copilot, Perplexity Comet, Amazon Buy for Me, Walmart Sparky, and others.

**Access pattern.** How an agent reaches a store. Two in these docs: **browsing** (the agent drives a web page like a person) and **protocol** (the agent calls feeds and checkout APIs and never sees the page).

**Scraping agent.** A browsing agent run from a cloud browser that reads the merchant's pages and, if it buys, fills the checkout form itself. Muse, Instinct, Grok Bot, ChatGPT Agent. 77% of agent orders.

**Protocol agent.** An agent that buys through UCP, ACP or a proprietary API. Google, Copilot, ChatGPT apps and feeds. 23% of agent orders, nearly none on the long tail.

**Cloud agent vs local browser agent.** A cloud agent runs in the agent company's data centre (Muse, ChatGPT Agent). A local browser agent runs inside the user's own browser (Comet, Claude in Chrome, Gemini in Chrome, Edge Copilot) and is indistinguishable from the user.

**Signed vs unsigned agent.** A signed agent attaches a cryptographic signature (Web Bot Auth) to its requests that a site can verify against a published key. ChatGPT Agent, Codex, Google's agent subset and Manus sign. Muse, Instinct, Grok and all local browser agents do not.

**HP / NHP (human-present / not-human-present).** Whether a person is at the keyboard when the order is placed. In AP2 terms, HP orders carry a Cart Mandate the human signed; NHP orders run on an Intent Mandate signed earlier.

**Cart Mandate / Intent Mandate.** AP2's two signed artefacts. An Intent Mandate records what the user asked for; a Cart Mandate records the exact cart the user approved. Together they are the "signed intent" the docs refer to.

**Approval by default.** The design most agents shipped in 2026: the agent proposes, the human approves each order, optionally within a spending cap (Walmart Agent Profiles: $50/$100/$200).

**Auto-Buy.** Amazon's unattended purchase mode, the exception to approval by default.

**Agent mode / Atlas.** OpenAI's autonomous browsing agent and browser, removed in July–August 2026.

**Instant Checkout.** OpenAI's in-chat checkout, retired for merchants in March 2026 in favour of merchant apps and referral.

**Agent Profiles.** Walmart's per-agent account inside a customer's Walmart account, with spending caps. Announced 21 October 2026.

**Agent Gateway.** Walmart's proprietary API for third-party agents.

**Buy for Me.** Amazon's agent that buys from third-party sites on the shopper's behalf.

**Shop Pay.** Shopify's saved-card wallet. Required for Checkout WebMCP; Muse orders on Shop Pay stores complete at 3.5× non-partnered.

**Link virtual card.** A single-use card number Stripe Link issues so an agent can pay on a store that has no agent protocol. How Muse pays on non-partner stores; to the merchant it looks like an ordinary card.

**Shared Payment Tokens.** Stripe's scoped, single-use payment credentials for agents under ACP.

## Protocols and standards

**UCP (Universal Commerce Protocol).** Google and Shopify's protocol for catalog, cart, checkout and order, carried over REST, MCP, A2A or embedded. Fields include `requires_escalation`, `requires_buyer_review`, `continue_url`. Discovered at `/.well-known/ucp`.

**ACP (Agentic Commerce Protocol).** OpenAI and Stripe's protocol for feeds, cart and checkout. Fields include `not_ready_for_payment`, `manual_review`. Used by ChatGPT and Copilot.

**AP2 (Agent Payments Protocol).** Google's protocol for signed payment mandates, now moving to the FIDO Alliance. Defines Intent and Cart Mandates and a `checkout_hash`.

**MCP (Model Context Protocol).** A general protocol for giving an AI model tools. In commerce it plays four roles: a transport for UCP; store-management servers (WooCommerce, PrestaShop, commercetools, Adobe); agent-side aggregators that wrap many stores; and the base of WebMCP.

**WebMCP.** A W3C draft letting a web page register tools (`search_catalog`, `add_to_cart`, `complete_checkout`) for an agent running in the browser, via `navigator.modelContext`. Live by default only on Shopify checkout (signed agents) and BigCommerce (via Cloudflare).

**A2A (Agent-to-Agent).** Google's protocol for one agent delegating to another. Carries AP2.

**Web Bot Auth.** An IETF draft (adopted 1 September 2026) under which an agent signs HTTP requests with an Ed25519 key (RFC 9421 message signatures) and the site verifies against a key directory. The basis of Visa TAP and of Shopify's signed-agent features.

**Visa TAP (Trusted Agent Protocol).** Visa's agent-identification scheme on card rails, built on Web Bot Auth. Delivered through PSPs.

**Mastercard Agent Pay / Verifiable Intent / Agentic Trust Intelligence.** Mastercard's equivalents: agent credentials, signed intent, and risk signals.

**robots.txt / Cloudflare Content Signals.** Files that tell crawlers what they may read. Honoured by crawlers, ignored by browser agents acting as the user.

**llms.txt / agents.md / shop.md.** Proposed site-summary files for AI models and agents. Published by some sites; no major agent is confirmed to read llms.txt.

**schema.org JSON-LD (Product, Offer, MerchantReturnPolicy).** Structured data embedded in a page. What agents read when nothing else exists; universal on both sides.

**Google Merchant Center.** Google's feed intake for product data; also the source for Google's agent surfaces.

**Shopify Catalog.** Shopify's default-on product database syndicated to ChatGPT, Copilot, Google and Meta. Products and store-level facts; no conditions, no contractual freshness.

**Knowledge Base (Shopify).** Store-level facts (hours, policies) Shopify exposes to agents. Not product-level.

**x402 / MPP.** Machine-to-machine payment proposals (HTTP 402, stablecoins). Developer tooling, not commerce yet.

## Measurement

**Accuracy (pass^1).** The share of tasks an agent completes correctly on one attempt, averaged across tasks.

**Reliability (pass^k).** Whether the same task succeeds on all k attempts. **Consistency** is the run-to-run version. Accuracy has risen fast; consistency barely.

**METR time horizon.** The length of task an agent completes at a given success rate. The 50% horizon is ~12–17 hours; the 80% horizon ~1.5–3 hours; no 99% horizon exists.

**Benchmarks named.** tau2-bench Retail (order-service conversations against a clean API, saturated at 91.9%); EComAgentBench, ShoppingBench, ShopSimulator, Amazon-Bench, WebShop, WebArena, OSWorld (shopping and web tasks of increasing realism, 33–57%).

**Princeton HAL.** An independent leaderboard that reports accuracy, reliability and consistency as separate columns.

**AI-referred visit.** A visit to a merchant's site that arrived from an AI assistant (a click-out). Counted by panels and Adobe. Distinct from an agent-completed order.

**Agent-completed order.** An order the agent placed itself, by protocol or by filling the form. Panels estimate ≤1.5% of US orders; direct counts say far less.

**Agentic traffic.** Requests made by an agent acting for a user, as opposed to crawlers gathering training or search data. Under 1% of AI-bot traffic.

**Day-14 retention.** The share of app installers still active two weeks later. Muse: 23%. An app metric, not an order-accuracy metric.

**Panel.** A sample of shoppers or brands whose behaviour is scaled up to the market (FirstPageSage: 5,212 shoppers, 46 brands). Method published; sample small.

**Vendor network data.** Figures a traffic or fraud vendor reports from its own customers (Forter, HUMAN, DataDome). Network size undisclosed; vendors disagree by up to 10×.

**Store trackers.** Services that count live stores per platform by crawling (Store Leads, MyLeadFox, BuiltWith, HTTP Archive). They count different things and differ by millions.

**Lift.** A measured increase in a conversion or completion rate caused by a treatment. "No measured lift" means no controlled comparison exists, not that the treatment does nothing.

**Loss model.** Doc 01's estimate that a wrong agent order costs the merchant about 26% of the order's value, with chargebacks ~8% and cancel-before-ship ~10% of mistakes.

## Agents named in the docs

| Agent | Company | Class |
|---|---|---|
| ChatGPT shopping, merchant apps, ChatGPT Agent, Codex | OpenAI | Protocol (apps, feeds) and signed cloud browser |
| Muse | Meta | Unsigned cloud browser; largest by orders |
| Google AI Mode, Gemini, Gemini in Chrome | Google | Protocol (UCP) and local browser |
| Copilot, Edge Copilot | Microsoft | Protocol (ACP) and local browser |
| Comet | Perplexity | Local browser |
| Claude in Chrome | Anthropic | Local browser |
| Instinct | Instinct | Unsigned cloud browser |
| Grok Bot | xAI | Unsigned cloud browser |
| Manus, Genspark, Browserbase | various | Cloud browser; Manus signs |
| Buy for Me, Alexa for Shopping, Auto-Buy | Amazon | Proprietary |
| Sparky, Agent Gateway, Agent Profiles | Walmart | Proprietary |
| Fo | DoorDash | Proprietary |
