# Agent Order Trust Layer

Product brief · working name · 1 November 2026

## One line

A merchant-side trust layer for orders placed by AI agents: it tells a small online business which orders came from an agent, decides, by the merchant's own rules, when to go back to the human for a signed approval, keeps the proof, and over time turns that proof into customer retention and, eventually, a guarantee.

## Who it's for

Small and mid-size online merchants outside the enterprise tier, starting with one region and one vertical (Israel, travel and experiences), on platforms where agent checkout is switched on but nothing sits above it: WooCommerce, Wix, custom carts, and Shopify merchants outside the US rollout. The buyer and the agent platform are not customers; they are parties the product has to work with.

## The problem it solves

Agents now place orders. When an agent gets one wrong, the merchant absorbs the return, the refund or the chargeback, has no evidence a human ever approved the purchase, and can't tell a ten-year customer from an anonymous agent session. The platforms give merchants the pipe for agent orders and nothing for the risk, the proof or the relationship.

## What it does, in the order a merchant experiences it

### 1. See
Every order is classified: human, agent via protocol, or agent driving a browser; and for agent orders, whether a human approved the exact cart (HP) or only a standing instruction (NHP). The first thing a merchant gets is a dashboard that shows this, which no one else gives them.

### 2. Decide
The merchant writes rules in plain terms: confirm agent orders over a value, hold first-time customers, re-confirm if the cart changes after approval, always allow registered agents from customers they know. Actions are allow, confirm, hold, decline.

### 3. Confirm
When a rule says confirm, the customer gets a message under the merchant's name (WhatsApp first, push or email where that fits) with the exact cart and a link. The approval is a passkey tap or a bank-app step-up, never the message reply, so an agent holding the customer's phone and inbox can't approve its own order. Each confirmation carries an assurance level the merchant can see.

### 4. Prove
Everything is stored per order: the agent's identity, the intent mandate, the signed cart approval with device and timestamp, the rule that fired. When a dispute comes, it exports as a representment package in the processor's format.

### 5. Recognize
The same enrollment that makes confirmations trustworthy links the customer's identity to the merchant, so the next agent order arrives with the customer's profile, loyalty tier and member terms attached, and the merchant keeps a consented direct channel for tracking, returns and the next offer.

### 6. Measure
Across its merchants the product computes how reliable each one looks to agents: accuracy, substitutions, confirmation speed, dispute rate. First as a mirror for the merchant, later as an attestation agents can read.

### 7. Guarantee
With enough outcome data, a per-order premium covers the merchant against agent mistakes, with a risk partner carrying the book. This is the destination, not a launch feature.

## How it's delivered

As an app in the platform's store: a Shopify app, a WooCommerce plugin, a Stripe app. It reads orders and customers through the platform's APIs, consumes mandates and intent artifacts from the protocols and card networks rather than issuing credentials itself, and sends confirmations through a WhatsApp Business provider. It doesn't replace the merchant's checkout, catalog or fraud tool; it sits above them.

## How it makes money

A per-agent-order fee or a monthly tier by volume for steps 1–4, confirmations at cost plus margin, a retention tier for step 5, and later a premium for step 7. The first pitch is "you're liable and have no evidence"; the lasting contract is "you keep your customers and your record with the agents."

## What it deliberately isn't

- Not a protocol integration layer: the platforms and Stripe do that.
- Not a buyer wallet or spending-control app: the card networks and funded startups own the credential.
- Not a catalog or feed-quality tool.
- Not a fraud engine for stolen cards. Fraud tools decide whether the buyer is real; we prove what the real buyer approved. We consume the merchant's existing fraud score as an input and never produce one.
- Each of those is either already covered or needs scale a newcomer doesn't have.

## What makes it defensible

Being installed early: the identity anchors, the outcome data and the agent-side reputation only accumulate from the first agent orders onward, and they are what the long-term risks (losing the customer, losing standing with the agents) depend on. The confirmation and liability story that opens the door has a short shelf life; the data it produces doesn't.

## What has to be true

- Agent orders reach these merchants at a meaningful rate within 12–18 months.
- At least one mandate provider or agent platform will hand over intent artifacts.
- Customers enroll a passkey at a usable rate during normal checkouts.
- The platforms don't ship a "confirm agent orders" toggle before the product has its first hundred merchants.

## Build difficulty of the wedge (directions 3 + 4)

| Component | Difficulty | Rough effort | Where the risk really is |
|---|---|---|---|
| App shell on Shopify and WooCommerce (install, webhooks, settings UI) | Low | 2–4 weeks each | Shopify app review and its separate approval for protected customer data (PII) |
| Agent-order detection: protocol-declared | Low | 1–2 weeks | Depends on what each platform passes through about order source; verify first |
| Agent-order detection: browser-driven agents | Medium, ongoing | 3–4 weeks, then continuous | False positives on real humans; an arms race |
| HP/NHP classification | Low code, high dependency | 1–2 weeks once data exists | Whether platforms expose the cart mandate or approval flag to a third-party app is the single biggest unknown |
| Rules engine and vertical templates | Low | 2–3 weeks | UX for merchants who won't read docs |
| Confirmation: WhatsApp delivery | Low-medium | 2–3 weeks | Meta template approval, opt-in rules, per-message cost, BSP contract |
| Confirmation: passkey approval page (WebAuthn) | Medium | 3–4 weeks | Enrollment gate; attestation gaps on Apple |
| Confirmation: bank step-up (3DS) as assurance level 2 | High dependency | Unknown | Requires processor support for out-of-checkout step-up; phase two |
| Order hold and release through platform APIs | Low | 1 week | — |
| Enrollment gate (passkey registration in human sessions) | Medium | 3–4 weeks | Shopify checkout extensibility limits; enrollment rate is the business risk |
| Evidence vault and dispute export | Low | 2–3 weeks | Whether evidence moves outcomes is untested |
| Dashboard | Low | 2 weeks | — |
| Security and compliance (DPA, GDPR, eventual SOC 2) | Medium, continuous | Ongoing | Never touch card data; no PCI scope |

Total: a usable MVP on both platforms in roughly three to five months. What is actually hard: data access from the platforms (spike it in week 1), passkey enrollment rate, agent-order volume to test on, and proving the evidence matters in a real dispute.

Build order: platform data-access spike (week 1); detection + dashboard shipped read-only to many merchants to gather agent orders (weeks 2–6); rules, WhatsApp, passkey confirmation and holds (weeks 6–12); enrollment gate and evidence export after; bank step-up only once volume justifies it.


# Competitor analysis

Who already sells something near the trust layer, where they overlap, and where the gap is. Compiled 1 November 2026 from public sources listed at the end; claims about pricing and positioning are as published by the companies or by third parties quoting them.

## 1. Enterprise fraud platforms with chargeback guarantees

The closest incumbents. All three have shipped agent-order detection in 2026, all price as a percentage of approved GMV with custom contracts, and all are built around stolen-card fraud rather than honest agent mistakes.

| Company | What they offer | Target | Overlap with us | Gap we can take |
|---|---|---|---|---|
| Riskified | AI Agent Intelligence: agent detection, Identity Graph signals, AI Agent Policy Builder (build, simulate, deploy, track rules for agent orders); HUMAN AgenticTrust partnership; 100% chargeback guarantee on approved orders | Enterprise and upper mid-market; pricing from ~0.4% of approved transactions, custom | Highest: detection + merchant rules for agent orders is directly our step 1–2 | No intent proof or human confirmation; no evidence package for 'my agent did it'; no identity link or retention; cost and minimums exclude small merchants |
| Forter | Agent detection and agent-type classification, real-time agentic activity dashboard, accepts Skyfire KYA tokens in decisions; optional chargeback guarantee; agentic orders on its network up 4x since Aug 2026 | Enterprise; % of GMV, volume-tiered, minimum commitments | High on detection and dashboard | Same gaps as Riskified; identity-based fraud framing, not intent; enterprise-only |
| Signifyd | Protocol-agnostic agentic order recognition (agent-assisted, agent-actuated, agent-native); Step-Ups route borderline orders to 3DS, review or a direct customer check; Shopify app, free to install, % of approved order value. Two guarantees: Guaranteed Fraud Protection (fraud chargebacks) and Complete Chargeback Protection, which also covers non-fraud codes including unrecognized, canceled merchandise, not-as-described, item not received and subscription canceled | Mid-market down to Shopify SMBs; no published rate; coverage scope is plan-dependent | High: Step-Ups with a direct customer check is the nearest thing to our confirmation step; Complete Chargeback Protection already underwrites the chargeback outcome of agent orders it approves, overlapping direction 7 | Guarantee covers only orders it approves by risk score and only the chargeback (~8% of agent mistakes), not returns, refunds or silent keeps; step-up is a fraud decision, not merchant-rule-driven intent proof; no assurance grading, no anchored approval, no evidence package, no retention. Its non-fraud guarantee also gives it a financial reason to want our evidence: partner or acquirer as much as competitor |
| Wyllo (ex-NoFraud) | Fraud screening with guarantee for small merchants: free up to 100 orders/month, then 1% of revenue with $250/month minimum, up to 1.5% for larger guarantees | Small merchants (card stops at $600K annual revenue) | Low on agents, high as a pricing reference for our segment | No agent-specific capability; shows small merchants will pay ~1% of revenue plus a floor for a guarantee |

## 2. Dispute and chargeback management

They fight disputes after the fact and are the loudest voices on the agent liability gap. None of them create the evidence; we would. Natural partners and a channel.

| Company | What they offer | Target | Overlap with us | Gap we can take |
|---|---|---|---|---|
| Chargeflow | Automated representment; published the agentic-commerce evidence playbook (what to log, which evidence answers each claim, CE 3.0 use) and recommends human confirmation for high-value or unusual orders | SMB and mid-market, pay-on-win | Medium: same evidence story, downstream of us | No order-time controls, no confirmation, no identity; depends on evidence the merchant already has, which is what we produce |
| Chargebacks911 / Justt | Representment and prevention alerts; Chargebacks911 is the main industry voice warning about agent disputes | Mid-market and enterprise | Low to medium | Same: post-hoc, no evidence creation |

## 3. Merchant enablement and agent-payment acceptance

They make merchants reachable and payable by agents. This is the layer we decided not to build. The risk is one of them adding rules and confirmation as a feature.

| Company | What they offer | Target | Overlap with us | Gap we can take |
|---|---|---|---|---|
| Firmly | Firmly Connect: no-code onboarding, protocol abstraction across MCP, AP2, ACP, UCP, A2A and KYA, 45-minute setup; Best Buy and Aurus as reported partners | Enterprise and mid-market merchants | Low today; medium if they add risk controls | No merchant-authored risk rules, no intent confirmation, no dispute evidence |
| Stripe Agentic Commerce Suite / Adyen Agentic / Shopify Agentic Storefronts | Hosted protocol endpoints, catalog sharing, scoped payment tokens; Shopify co-developed UCP including identity linking for its merchants | All merchants on those platforms | Medium: Shopify covers identity linking for its own merchants; Stripe could ship a 'confirm agent orders' toggle | None provide merchant rules, confirmation with assurance levels, or evidence packaging; non-US rollouts lag |
| Nevermined | Merchant-side acceptance of agent payments: delegated virtual cards with spending rules, agent-token acceptance, usage metering, settlement via Stripe; Visa collaboration; Start Path cohort | Digital services, APIs, MCP tools, agent marketplaces | Low: built for programmatic usage, not retail carts | Not a fit for physical or travel merchants; no human confirmation |
| PayOS, SolvaPay, Rye, Wizard | Agent payment execution infrastructure and agent-side checkout; all in Mastercard's Start Path agentic cohort or adjacent | Agent builders and enterprises | Low | Agent-side, not merchant-side |

## 4. Buyer-side credentials and mandates

They issue the credential the agent pays with and record what the user authorized. We consume their mandates rather than compete; the question is whether they will share intent artifacts with a merchant-side layer.

| Company | What they offer | Target | Overlap with us | Gap we can take |
|---|---|---|---|---|
| nekuda | Agentic payments SDK: Secure Agent Wallet and Agentic Mandates; $5M seed led by Madrona with Amex and Visa Ventures; Visa Intelligent Commerce launch partner; open-source ACP checkout gateway | Agent developers | Partner, not competitor | Mandates are buyer-side records; no merchant rules, no confirmation at order time |
| Basis Theory | PCI vault independent of processors; Agentic Commerce Consortium of 20+ companies; $33M Series B (total $50M); 'instruction' objects with spending rules the consumer verifies | Merchants and platforms managing payment data | Partner; slight overlap on instruction verification | Infrastructure, not a merchant-facing risk or retention product |
| Skyfire | Know Your Agent (KYAPay) signed tokens for agent identity and spend authority; $9.5M raised; integrating into Mastercard Agent Pay; Forter consumes its tokens | Agents, merchants, FIs | Partner: KYA is an input to our rules | Identifies the agent, not the human |
| Visa Intelligent Commerce / Mastercard Agent Pay (Verifiable Intent, Agent Pay Trust Framework) | Agentic tokens, intent capture, biometric step before an agent completes a purchase, dispute tooling in progress | Issuers, networks, large merchants | Partner and ceiling: they define what counts as proof | Network-level; nothing merchant-rule-driven, nothing for the long tail, no retention |

## 5. Proof of human and agent trust

The direct neighbors of our identity direction. Each proves 'a human is behind this agent' in a different way; none ties that proof to a merchant's own rules at order time, and none is built on channels small merchants already use.

| Company | What they offer | Target | Overlap with us | Gap we can take |
|---|---|---|---|---|
| World (Tools for Humanity) | AgentKit: users register agents with World ID so sites can verify a distinct, verified human approves the agent's purchases, over x402; proof-of-human tools for agentic commerce announced Oct 2026 | Websites and agent platforms | Medium: solves C4 (human behind the agent) at enrollment | Requires World ID (iris-scan) enrollment and an x402 rail; proves a human exists, not that this human approved this cart; no merchant rules, no evidence package |
| HUMAN Security (Sightline, AgenticTrust) | Bot and agent trust signals; deployed with Riskified | Enterprise merchants via Riskified | Low to medium: agent legitimacy, not human approval | Agent-side classification; sold through an enterprise partner |
| Nava | Escrow that holds funds until an agent's proposed transaction is verified against user intent; $8.3M seed; plans an insurance market for agent actions | Crypto-native agent payments | Low today; conceptually adjacent to our guarantee | On-chain, buyer-side; no merchant integration, no card rails |
| Descope, Stytch and other auth vendors | OAuth and identity-linking infrastructure for agents (UCP identity linking is OAuth 2.0) | Developers | Building block | We would build our identity link on one of these, not against them |

## 6. Catalog quality and GEO

Out of scope by decision; listed so the boundary is explicit. ReFiBuy, Profound, Paz.ai, Salsify and Syndigo optimize how merchants appear to agents. We do not compete here and may integrate.

| Company | What they offer | Target | Overlap with us | Gap we can take |
|---|---|---|---|---|
| ReFiBuy, Profound, Paz.ai, Salsify, Syndigo | Catalog optimization and AI-search visibility | Brands and retailers | None | Not our problem; a merchant with bad data still needs our controls |

## Scale of the incumbents

How big the closest incumbents are. Signifyd and Forter are private, so revenue figures are tracker estimates; Riskified is public (NYSE: RSKD) and is the reliable benchmark for the category.

| Company | Status | Last valuation | Funding | Revenue (est.) | People | Reach |
|---|---|---|---|---|---|---|
| Signifyd | Private; no priced round since April 2021 Series E; secondary estimate ~$0.2B (May 2026), ~85% below last round | $1.3B (2021) | $390M | $150–300M (trackers disagree: ~$141M vs $293M) | ~500, down from ~553 in 2024 | 10,000+ merchants claimed; ~4,400 verified installs; Walmart among customers |
| Riskified | Public (NYSE: RSKD) | Public market | ~$229M raised pre-IPO | ~$318M | ~700 | Enterprise and upper mid-market; processed $89B GMV in 2021 |
| Forter | Private; last priced 2021 | ~$3B (2021) | ~$525M | Not disclosed | ~800 | Processed $250B GMV in 2021, $2T+ to date; 500,000+ businesses on network |

Reading: all three sit in the few-hundred-million-revenue, 500–800-person class, far below the platforms and card networks. Signifyd has run five years without new capital with slightly shrinking headcount and a steep secondary discount: a business that needs a growth story, which explains how hard it is leaning into agentic commerce and why it is as likely to partner or acquire as to build.

## Positioning map

How each group covers the four jobs the product does. 'Partner' means we consume their output.

| | Fraud / dispute defense | Intent proof & confirmation | Identity & retention | Guarantee |
|---|---|---|---|---|
| Riskified / Forter / Signifyd | Strong (fraud-centric) | Partial (Signifyd step-up) | None | Strong (fraud; Signifyd also non-fraud chargebacks via Complete Chargeback Protection) |
| Wyllo and small-merchant fraud tools | Partial | None | None | Partial (fraud only) |
| Chargeflow / Chargebacks911 | Strong (post-hoc) | None | None | None |
| Firmly / Stripe / Adyen / Shopify | None | None | Partial (Shopify identity link) | None |
| nekuda / Basis Theory / Skyfire / networks | None | Partner (mandates, KYA, intent) | None | None |
| World / HUMAN / Nava | None | Partial (proof of human) | None | None (Nava: planned) |
| Us | Partial (agent mistakes, not stolen cards) | Strong (rules + anchored confirmation + evidence) | Strong (identity link, entitlements, channel) | Later (direction 7) |

## What this means for us

- Nobody sells merchant-rule-driven, human-anchored confirmation with an evidence package, and nobody sells it to small merchants. The nearest things are Signifyd's step-up with a direct customer check (fraud framing, Shopify channel) and Riskified's Policy Builder (enterprise). That is the opening, and it is narrower than it looks: Signifyd already reaches Shopify SMBs with a free-to-install app.
- The guarantee model has precedent and price anchors: Riskified, Forter and Signifyd price as a percentage of approved GMV; Wyllo shows small merchants pay about 1% of revenue with a $250 monthly floor. Signifyd's Complete Chargeback Protection already covers the non-fraud codes an agent mistake produces (unrecognized, canceled, not-as-described), so direction 7 overlaps with it for merchants it approves. Our guarantee differs in basis (confirmed intent rather than risk score), in scope (the mistake, including returns and refunds, not only the chargeback) and in segment.
- Proof-of-human is becoming a contested signal (World, HUMAN, the networks' biometric step). Consume it; do not build it. Our identity work is the enrollment gate and the assurance ladder on top of those signals, plus the identity link that nobody provides for non-Shopify merchants.
- Signifyd is the competitor to plan around, not Riskified or Forter: it reaches our segment through a free-to-install Shopify app, it already recognizes agent orders, it has a customer-check step-up it could point at intent, and its non-fraud guarantee already covers the dispute codes agent mistakes produce. It is also the most plausible acquirer: a private company marked far below its 2021 valuation, without new capital, with a financial reason to want signed intent evidence at representment.
- The biggest threats, in order: Signifyd moving its step-ups toward intent proof for SMBs; Shopify shipping a native 'confirm agent orders' setting; Riskified or Forter packaging a down-market tier; Firmly adding rules to its connector. The credential and proof-of-human startups are partners, not threats.
- Chargeflow is the obvious first partner: it already tells merchants to log exactly what we would produce and has the SMB distribution we lack.
- Positioning sentence: fraud tools decide whether the buyer is real; we prove what the real buyer approved, and keep the buyer.

## Why incumbents haven't pivoted here, and what slows them

Nothing structural prevents Riskified, Forter or Signifyd from shipping a "confirm agent orders with the customer" action. What slows them: their business model pays them to approve, not to ask (revenue is a percentage of approved GMV, and confirmation is friction); their asset is a human-session identity graph, not per-order intent proof; their customers are large; retention and identity are outside their remit; and a public company with a guarantee book moves slowly on anything that changes the approval model.

What they would still lack after shipping it: human-anchored enrollment tied to a merchant, the identity link and entitlements, outcome data per vertical and region on agent mistakes, regional channels and anchors, and neutrality as a layer agent platforms and dispute firms read from.

Expect them to do the rules-plus-confirmation part. The moat is being installed early where they aren't, holding the identity anchors and the outcome data, and being the layer others read from. If the product stays only at direction 3, it is a feature and they will take it; if it reaches directions 5 and 6 first, it is a company, and the likely outcome with Riskified or Signifyd is partnership or acquisition. The window is probably 12 to 24 months.

## Sources

- [Riskified – AI Agent Intelligence expansion](https://www.riskified.com/press/riskified-expansion-ai-agent-intelligence-to-secure-native-merchant-ai-shopping-assistants/)
- [Riskified – HUMAN partnership](https://www.riskified.com/press/riskified-joins-forces-with-human-to-help-merchants-embrace-trusted-ai-shopping-agent-commerce/)
- [IT Brief – Forter agentic tools](https://itbrief.co.uk/story/forter-unveils-tools-to-help-retailers-manage-agentic-ai-surge)
- [The Paypers – Skyfire and Forter](https://thepaypers.com/fraud-and-fincrime/news/skyfire-and-forter-partner-to-verify-ai-agents-at-checkout)
- [Signifyd – Summer 2026 Spark release](https://www.signifyd.com/?p=62137)
- [Signifyd – Agentic commerce](https://signifyd.com/blog/agentic-commerce)
- [Redo – Fraud platform pricing](https://redo.com/resources/articles/comparisons/fraud-platform-pricing)
- [Chargeflow – Riskified vs Forter](https://www.chargeflow.io/es/blog/riskified-vs-forter)
- [Chargeflow – Agentic commerce evidence playbook](https://www.chargeflow.io/blog/agentic-commerce-chargebacks-the-evidence-playbook-merchants-need)
- [Firmly Connect launch](https://stellagent.ai/insights/firmly-connect-agentic-commerce-platform)
- [Stripe – Agentic Commerce Suite](https://stripe.com/blog/agentic-commerce-suite)
- [Nevermined](https://nevermined.ai/)
- [Rye – Agentic commerce landscape](https://rye.com/blog/agentic-commerce-startups)
- [Basis Theory – $33M Series B](https://www.digitalcommerce360.com/2025/10/16/basis-theory-raises-33-million-to-build-ai-driven-payments-for-commerce/)
- [Mastercard Start Path – agentic cohort](https://www.mastercard.com/global/en/innovation/partner-with-us/start-path/news.html)
- [Mastercard – Know Your Agent](https://www.mastercard.com/global/en/news-and-trends/stories/2026/agentic-commerce-identity-know-your-agent.html)
- [TechCrunch – World AgentKit](https://techcrunch.com/2026/03/17/world-launches-tool-to-verify-humans-behind-ai-shopping-agents/)
- [Semafor – World proof-of-human tools](https://www.semafor.com/article/10/23/2026/ai-startup-world-launches-proof-of-human-tools-aimed-at-agentic-commerce)
- [Fortune – Nava seed round](https://fortune.com/2026/04/14/nava-seed-funding-ai-financial-agents/)
- [Descope – UCP identity linking](https://www.descope.com/learn/post/ucp)
- [Signifyd – Complete Chargeback Protection product sheet](https://appexchange.salesforce.com/image_host/e4ecdd04-e9d6-4168-9793-50434d0603e3.pdf)
- [Signifyd – Item Not Received coverage policy](https://www.signifyd.com/?p=9846)
- [Signifyd – Shopify app listing](https://apps.shopify.com/signifyd)
- [Recharge – Using Signifyd (GFP vs CCP)](https://support.getrecharge.com/hc/en-us/articles/40886698442391-Using-Signifyd)
- [GetLatka – Signifyd](https://getlatka.com/companies/signifyd)
- [TrueUp – Signifyd](https://partners.trueup.io/co/signifyd)
- [Dealroom – Signifyd](https://dealroom.co/companies/signifyd/)
- [WiserReview – Signifyd alternatives](https://wiserreview.com/blog/signifyd-alternatives/)