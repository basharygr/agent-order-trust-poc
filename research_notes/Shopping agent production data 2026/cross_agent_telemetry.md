# Cross-cutting, agent-agnostic telemetry on AI shopping-agent activity and outcomes (May–3 Nov 2026)

Scope note: every figure below is dated. Items older than May 2026 are marked **[background]**. "Vendor" = the publisher sells into the market it is measuring; "independent" = no obvious commercial stake. Research was conducted 3 Nov 2026; Shopify's Q3 2026 results (scheduled for the morning of 3 Nov) were not yet indexed anywhere I could reach. Several primary pages (Riskified press, Adobe blog, Forbes, Salesforce transcript mirrors, HUMAN blog) returned HTTP 403; where that happened I relied on named secondary coverage and flag it. The "2.4x dispute rate" figure from the TrustSphere blog was excluded per instructions and nothing found elsewhere corroborates it.

---

## 1. Web telemetry: agent traffic share, growth, and how far agent sessions get (cart / checkout / order)

### Takeaway
The only large cross-site session study remains Microsoft Clarity's 5.2M-session report (window 13 Aug–9 Sep 2026, published 29 Oct 2026); no later Clarity update exists. It shows checkout events in ~25% of agent-involved sessions vs ~15% of human sessions but explicitly cannot attribute completion to the agent, and PPC Land found an internal base-count inconsistency. Independent network-level measurement (Cloudflare, HUMAN) puts agent *checkout* activity at ~2–5% of agent requests and agent-completed orders remain well under 2% of US e-commerce even on the most generous vendor estimate (FirstPageSage 1.52% in Oct 2026). Nobody publishes a measured Web Bot Auth adoption rate.

### Cited Findings

**Microsoft Clarity (independent of commerce vendors, but Microsoft sells Copilot Checkout)**
- Report "What AI Agents Do on Websites: Insights From 5.2 Million Sessions" published 29 Oct 2026; window 13 Aug–9 Sep 2026 (UTC); >5.2M agentic-classified sessions across 230,511 Clarity projects; agent ecosystems detected: OpenAI, Google, Anthropic (Meta Muse not named; window closed a day after Muse's US launch) — [PPC Land](https://ppc.land/checkout-appears-in-25-of-ai-agent-sessions-vs-15-for-humans-clarity-finds/)
- Checkout smart-event present in 25.21% of agentic sessions (306,621 sessions) vs 14.65% of human sessions; Add-to-cart 2.33% agentic vs 3.31% human; "Order success" is in the human top-10 (1.79%) but not in the agentic top-10; "Begin checkout" human 1.62%, not in agentic top-10; Login 6.83% vs 8.84%; Outbound click 14.28% vs 26.99% — [PPC Land](https://ppc.land/checkout-appears-in-25-of-ai-agent-sessions-vs-15-for-humans-clarity-finds/)
- Microsoft reports agentic session volume up 88% "in just three weeks" (no baseline given) and made its Agent Activity Dashboard generally available the same day — [PPC Land](https://ppc.land/checkout-appears-in-25-of-ai-agent-sessions-vs-15-for-humans-clarity-finds/); [Relevant Audience](https://www.relevantaudience.com/analytics/microsoft-clarity-ai-agent-sessions-checkout-study/)
- Caveats stated by Microsoft: it cannot separate agent and human actions within a session (sessions may include human handoff); events "should not be interpreted as steps in a single conversion funnel or proof that an agent independently completed a task"; no breakdown by ecosystem, industry, country; agent share of total traffic not reported — [PPC Land](https://ppc.land/checkout-appears-in-25-of-ai-agent-sessions-vs-15-for-humans-clarity-finds/)
- PPC Land's own arithmetic: 306,621 / 25.21% implies a base of ~1.22M sessions, not 5.2M; on a 5.2M base checkout would be ~5.9% of agentic sessions; the ten published agentic event counts sum to 1,124,872, so at least 78% of agentic sessions recorded none of the top ten actions; checkout-to-add-to-cart ratio ~10.8:1 for agents vs ~4.4:1 for humans — [PPC Land](https://ppc.land/checkout-appears-in-25-of-ai-agent-sessions-vs-15-for-humans-clarity-finds/)
- No Clarity update after the 29 Oct 2026 publication was found as of 3 Nov 2026.

**Cloudflare Radar (independent network measurement; ~20% of web traffic)**
- Early June 2026: Cloudflare said agentic/automated bots were 57.4–57.5% of HTML web requests vs 42.6% human; CEO Matthew Prince called the classification "a bit messy" — [TechTimes, 5 Jun 2026](https://www.techtimes.com/articles/317877/20260605/bot-traffic-passes-humans-online-cloudflare-says-agentic-ai-drove-575-share.htm); [Let's Data Science](https://letsdatascience.com/news/cloudflare-reports-agentic-ai-traffic-surpasses-human-traffi-49fa9037)
- Radar AI Insights "crawl purpose" series (share of AI-bot requests declared as User Action, i.e., fetch-on-behalf-of-a-user): Q2 2026 2.45%; Jul 2026 2.66%; Aug 2026 4.80%; Sep 2026 3.11% (provisional); Q3 2026 3.66%. Bot share of all HTTP requests 34.75% (Jul 2026). Claude-User (fetch-on-demand agent) rose to 8.31% of bot traffic in Jul 2026 (#2 bot). AI_ASSISTANT category 11.64% (Jun) / 9.01% (Jul) of bot traffic, then retired in Aug reclassification. — secondary compilation of Radar data, [TechnologyChecker (updated 1 Oct 2026)](https://technologychecker.io/blog/ai-adaption-trends-data-insights); primary: [radar.cloudflare.com/ai-insights](https://radar.cloudflare.com/ai-insights)
- Another secondary compilation: Q3 2026 AI bots = 32.49% of verified-bot requests (crawlers 22.12%, search fetchers 7.03%, assistants 3.35%); search-purpose crawls 8.26% of AI-bot requests in Q3 2026 vs 4.05% in Q2 — [TechnologyChecker crawler stats](https://technologychecker.io/blog/ai-crawler-statistics); [Digital Applied](https://www.digitalapplied.com/blog/ai-crawler-bot-traffic-statistics-2026-data-reference)
- Radar added an "adoption of AI agent standards" widget (Web Bot Auth, Content Signals, robots.txt AI rules) in April 2026, but no measured share of signed (Web Bot Auth) requests has been published — [PPC Land Radar explainer](https://ppc.land/cloudflare-radar/); Web Bot Auth mechanism and Mastercard Agent Pay integration **[background, 2025]** — [Cloudflare blog](https://blog.cloudflare.com/signed-agents/); [Cloudflare press](https://www.cloudflare.com/press/press-releases/2025/cloudflare-collaborates-with-leading-payments-companies-to-secure-and-enable-agentic-commerce/)

**HUMAN Security (vendor: bot defense)**
- 2026 State of AI Traffic & Cyberthreat Benchmark (press release 26 Mar 2026; 2025 data) **[background]**: agentic AI traffic +7,851% YoY; checkout pages = 2.31% of agentic activity, product/search pages 77%, account pages 8.82%, auth flows 4.95%; agent vertical mix retail/e-commerce 46.6%, streaming/media 28.5%, travel 19.2%; OpenAI bots ~69% of AI-driven traffic, Meta-ExternalAgent ~16%, Anthropic ~11%; only ~0.5 pt separates benign from malicious automation — [PPC Land, 2 Jun 2026](https://ppc.land/ai-agents-are-now-buying-things-and-fraud-looks-identical/); [HUMAN press release](https://www.humansecurity.com/newsroom/2026-state-of-ai-traffic-cyberthreat-benchmark-report/)
- Monthly "State of Agentic Traffic" series: May 2026 agentic traffic −4.3% MoM, Comet 47% and Atlas 20.3% of observed agentic traffic — [PPC Land](https://ppc.land/ai-agent-traffic-dips-in-may-but-blocking-rates-keep-climbing/); June 2026 edition: checkout and payment flows held at 2.34% of agent activity — [HUMAN June 2026](https://www.humansecurity.com/learn/blog/state-of-agentic-traffic-june-2026-browser-agent-tooling-for-developers-is-catching-on-fast/); July 2026 edition (dated 6 Aug): publishers claim highest share — [HUMAN July 2026](https://www.humansecurity.com/learn/blog/state-of-agentic-traffic-july-2026-publishers-claim-highest-share-of-agentic-traffic/); August 2026 edition: "agentic traffic grows 27%, reaches new high as Codex debuts strongly" — [HUMAN Aug 2026](https://www.humansecurity.com/learn/blog/state-of-agentic-traffic-august-2026-agentic-traffic-grows-27-reaches-new-high-as-codex-debuts-strongly/) (page returned 403; title only). No September or October 2026 edition was found.

**Adobe Analytics / Adobe Digital Insights (vendor: analytics; measures AI-*referred* visits, i.e., clicks from AI chat/browser services, not agent-completed checkouts)**
- September 2026 (published 29 Oct 2026; based on >1 trillion visits to US retail sites, 100M SKUs, 18 categories): AI traffic to US retail sites +71% YoY; +1,772% since Oct 2024; AI-referred visitors generated 83% more revenue per visit and converted at a 90% higher rate than non-AI visitors; 73% more time on site, 25% more pages, 53% higher add-to-cart rate. Trend: Aug 2025 AI visits converted 9% *worse* than non-AI — [Adobe blog](https://business.adobe.com/blog/ai-traffic-higher-revenue-per-visit-retail-holiday) (403 on fetch); [Retail TouchPoints](https://www.retailtouchpoints.com/?p=621887); [Search Engine Land](https://searchengineland.com/ai-traffic-drives-more-revenue-per-visit-adobe-499713); [Chain Store Age](https://chainstoreage.com/adobe-ai-e-commerce-traffic-soars-generates-sales-september)
- Earlier 2026 months: Q1 2026 AI traffic +393% YoY; May 2026 retail +138% YoY; July 2026 +62% YoY with conversion 60% higher than non-AI; August 2026 reported as +127% YoY by one outlet (conflicts with the Sept +71% run-rate; unresolved) — [Adobe Q2 report PDF](https://business.adobe.com/resources/sdk/.2026-q2-ai-traffic-report/q2-2026-adi-ai-sourced-traffic-insights.pdf); [Adobe Q3 report PDF](https://business.adobe.com/resources/sdk/.q3-ai-traffic-trends-report/q3-2026-ai-sourced-traffic-insights.pdf); [Adobe Aug 2026 PDF](https://business.adobe.com/assets/pdfs/resources/sdk/ai-traffic-trends-report-august-2026/ai-traffic-trends-report-2026-08-19.pdf); [Small Biz Trends](https://smallbiztrends.com/ai-driven-retail-traffic-surges-71-converts-90-better)
- Adobe does not publish AI-referred traffic as a share of total retail traffic; it describes it as "modest" vs other channels — [Search Engine Watch](https://searchenginewatch.com/adobe-expects-ai-shopping-referrals-to-more-than-double-this-holiday-season/)

**Salesforce Shopping Index (vendor: commerce platform; 1.5B+ shoppers on Salesforce clouds)**
- Q2 2026 Shopping Index: global digital traffic +18% YoY, orders +1% — [Salesforce blog, 20 Jul 2026](https://www.salesforce.com/blog/holiday-retail-predictions-2026/)
- No Q3 2026 Shopping Index release was found as of 3 Nov 2026.
- Consumer-side (Salesforce survey, sample/dates not given): 50% of shoppers use an AI assistant somewhere in the journey (+67% YoY); reliance on AI assistants as first stop +200% May 2025→May 2026; 21% engaged a third-party AI chat assistant in past 12 months, 12% a brand-owned one — [Salesforce blog](https://www.salesforce.com/blog/holiday-retail-predictions-2026/)

**Shopify (platform; self-reported)**
- Q2 2026 (reported 5 Aug 2026): AI-driven traffic and orders to Shopify stores tripled YoY; new-buyer orders from AI channels grew at ~2x the pace of other channels; Q2 GMV $115.6B — [Seeking Alpha](https://seekingalpha.com/news/4626580-shopify-expects-q3-revenue-growth-in-the-low-30s-as-it-expands-agentic-commerce); [Shopify Q2 8-K](https://www.sec.gov/Archives/edgar/data/0001594805/000159480526000046/exhibit991pressreleaseq220.htm)
- Q3 2026 results were scheduled for 3 Nov 2026 pre-market; as of research time no release was indexed and Shopify's investor press page showed only the 6 Oct 2026 scheduling notice — [Shopify IR press releases](https://www.shopify.com/investors/press-releases); [GlobeNewswire notice](https://www.globenewswire.com/news-release/2026/10/06/3375308/0/en/shopify-to-announce-third-quarter-2026-financial-results.html)
- Third-party preview claims (not company-reported): 75% of AI-attributed orders from outside top-100 categories; Q3 "AI-driven orders +3x YoY" guidance framing — [Hudson Labs preview](https://www.hudson-labs.com/research/shopify-q3-2026-earnings-preview-shop-results-preview)
- Meta Muse / Shop Pay partnership announced Sept 2026 — [Seeking Alpha](https://seekingalpha.com/news/4662392-shopify-q3-earnings-preview-eps-revenue-growth-and-ai-commerce-push-in-focus)

**FirstPageSage (vendor: SEO agency; own panel, methodology opaque — treat as estimate)**
- Study dated Nov 2026, window 13 Jul–26 Oct 2026, panel 5,212 US online shoppers, 46 brands, 19,850 shopping prompts, normalized to US Census e-commerce totals: AI-referred share of US retail site visits Q1 2026 ~2.88%, Q2 ~3.79%, Q3 ~4.73%, Oct 2026 5.46%; agent-completed share of US e-commerce orders Q1 ~0.19%, Q2 ~0.27%, Q3 0.68%, Sept 1.35%, Oct 1.52%; AI-referred visits converted at 1.6x other traffic; Meta Muse accounted for 71% of the Q3 increase in agent-completed orders and 32.6% of agent-completed purchases in Oct 2026 on 7.9% of AI-assisted sessions; merchants inside an agent checkout partnership had 1.4x–3.9x the agent purchase completion rate of non-participants — [FirstPageSage](https://firstpagesage.com/seo-blog/agentic-commerce-statistics)

**Similarweb / Akamai**
- No October 2026 Similarweb retail agent-traffic figures found. Similarweb's 2026 Generative AI Landscape: ChatGPT share of gen-AI site visits fell from ~76% (Jun 2025) to ~53% (May 2026); marketplaces lead AI referral traffic at 46.8M avg monthly visits (Jun 2025–May 2026); beauty +312.5% YoY — [Similarweb AI referral by industry](https://aisearch.similarweb.com/blog/ai-referral-traffic-by-industry/). Walmart/Etsy ~20% of referral traffic from ChatGPT **[background, Aug 2025]** — [Modern Retail](https://www.modernretail.co/technology/chatgpt-is-now-20-of-walmarts-referral-traffic-while-amazon-wards-off-ai-shopping-agents/)
- Akamai: no 2026 agent-commerce traffic publication surfaced in any search.

### Inferences
- Across independent measurement (Cloudflare user-action ~3–5% of AI-bot requests; HUMAN checkout ~2.3% of agent activity) and the most aggressive vendor estimate (FirstPageSage 1.52% of US orders in Oct 2026), agent-*completed* commerce is still a low-single-digit-percent phenomenon at most, while agent-*referred* traffic is growing 70–130% YoY off a small base.
- Clarity's 25% "checkout" figure is best read as "agent-involved sessions skew toward checkout pages," not as a conversion rate; the absence of "order success" from the agentic top-10 and the implied 1.22M base both argue against using it as completion evidence.
- Adobe's conversion advantage (90% in Sept 2026, 60% in July, −9% in Aug 2025) is a referral effect (users clicking out of chat after research), so it measures human-completed purchases following AI research, not agent-completed orders.
- The Meta Muse launch (8 Sept 2026) is the single biggest driver of agent-order growth in every Q3/Q4 dataset (Forter, FirstPageSage); any "agent order" trend line that spans September is dominated by one new entrant.

### Gaps
- No Clarity update after 29 Oct 2026; Clarity does not publish agent share of total sessions, order-success rate for agents, or per-ecosystem splits.
- No published measured adoption rate of Web Bot Auth / signed agent requests on Cloudflare Radar or elsewhere.
- No Cloudflare Radar Q3 2026 report specifically on commerce; Cloudflare "agentic commerce" page is product marketing — [Cloudflare](https://www.cloudflare.com/solutions/agentic-commerce/)
- HUMAN's Aug 2026 monthly post could not be fetched (403); no Sept/Oct 2026 editions found.
- No Salesforce Q3 2026 Shopping Index; no Shopify Q3 2026 disclosure yet (released the day of research).
- No Akamai agentic-commerce data at all.

---

## 2. Payments networks and processors: agent transaction volumes, merchant counts, authorization and dispute rates

### Takeaway
Through the Q3 2026 earnings cycle (Visa 27 Oct, Mastercard 29 Oct) neither network disclosed any count, volume, or dispute statistic for agent-initiated transactions; both characterized volumes as small but growing at "impressive weekly" rates. No processor (Stripe, Adyen, Checkout.com, PayPal, Worldpay) has published authorization or dispute rates for agent orders; the only Stripe growth figure in circulation (38x MoM in Sept 2026) is unverified secondary reporting. Amex has published no Agent Purchase Protection claims data.

### Cited Findings

**Visa (Q4 FY2026 call, 27 Oct 2026)**
- CEO Ryan McInerney: "nearly 50 AI partners live and in production" (incl. Shopify's Shop Pay), expected to double "in the coming months"; "dozens of partners" announced on Visa Intelligent Commerce with "many dozens more" engaged — [Motley Fool transcript](https://www.fool.com/earnings/call-transcripts/2026/10/28/visa-v-q4-2026-earnings-call-transcript/)
- Agentic transactions described as "albeit limited," "still very early," "a very small amount" of the business; for FY2027 "remains very small"; no agentic volume, revenue or growth figures; CFO did not quantify agentic in guidance — [Motley Fool transcript](https://www.fool.com/earnings/call-transcripts/2026/10/28/visa-v-q4-2026-earnings-call-transcript/)
- "In agentic, trust is the biggest barrier to scale"; Visa rules define dispute roles/processes; Trusted Agent Protocol + Agent Directory for agent identity; consumer authenticated intent capture for transactions; B2B use cases (Ramp bill pay GA, BILL piloting, Billtrust, Airwallex) — [Motley Fool transcript](https://www.fool.com/earnings/call-transcripts/2026/10/28/visa-v-q4-2026-earnings-call-transcript/); [AIM Media House](https://aimmediahouse.com/banking/visa-says-agentic-commerce-has-hit-a-tipping-point)
- Cross-border e-commerce +17% YoY constant-dollar "primarily driven by retail, including spending on AI platforms" (not separated) — [Motley Fool transcript](https://www.fool.com/earnings/call-transcripts/2026/10/28/visa-v-q4-2026-earnings-call-transcript/)
- Visa PERC: >450% increase in dark-web posts mentioning AI agents over six months (threat chatter, not fraud rate) — via [Chargeflow](https://www.chargeflow.io/blog/agentic-commerce-and-its-impact-on-fraud-and-chargebacks) (vendor)

**Mastercard (Q3 2026 call, 29 Oct 2026)**
- CEO Michael Miebach: "agentic transactions are ramping up at impressive weekly growth rates" (no rate given); "still early days, and the numbers are not yet significant in the grand scheme"; expects trend "to build as holiday shopping ramps up" — [AOL transcript](https://www.aol.com/articles/mastercard-ma-q3-2026-earnings-191318000.html); [Benzinga](https://www.benzinga.com/news/26/10/62699593/mastercard-q3-2026-earnings-call-transcript)
- Agent Pay verifies agents and buyer authorization; Shop Pay "already available on multiple agentic platforms"; OpenAI strategic partnership "includes the use of Agent Pay and goes well beyond"; "Agentic Trust Intelligence" launched to help banks/merchants evaluate agent-initiated transactions; Agent Connect for merchant discoverability; Mastercard "not supporting VCN as a solution in between" — [AOL transcript](https://www.aol.com/articles/mastercard-ma-q3-2026-earnings-191318000.html)
- Q2 2026 (secondary): Adyen and Checkout.com named among ~30 partners on Agent Pay for Machines — [Axis Intelligence](https://axis-intelligence.com/agentic-payments-statistics/)
- No agentic transaction count, volume, merchant count, or dispute figure disclosed in either quarter.

**Stripe**
- John Collison (podcast, 12 Oct 2026): agentic commerce "only as of September basically started going vertical"; he did not show numbers — [Forkast](https://forkast.news/?p=132104)
- Secondary, unverified ("Stripe data," no primary cited): agent-driven Link transactions +38x MoM in Sept 2026, payment volume +20x, avg transaction ~$100, retail #1 category; Link connected to ~200 AI agents (up from ~70 in Sept) and ~300M registered users (Dan Hill, Stripe) — [Winzheng](https://www.winzheng.com/en/article/stripe-ai-agent-38x-payment-surge-september-2026)
- Stripe president quoted (Payments Dive, Oct 2026) that agentic activity began rising sharply in September 2026, no figure — via [FirstPageSage](https://firstpagesage.com/seo-blog/agentic-commerce-statistics)
- Original OpenAI/Stripe Instant Checkout (Sept 2025 launch, ~a dozen Shopify merchants) reportedly had near-zero sales and was taken offline in March 2026; OpenAI shifted to retailer apps inside ChatGPT — [Winzheng](https://www.winzheng.com/en/article/stripe-ai-agent-38x-payment-surge-september-2026); [Idukki](https://idukki.io/blog/agentic-checkout-explained-acp-and-instant-checkout)

**Adyen, Checkout.com, PayPal, Worldpay**
- No published agent-initiated transaction counts, authorization rates, or dispute rates from any of them. One aggregator states explicitly that no network, issuer or processor has disclosed transaction counts/values for agent-initiated payments — [Axis Intelligence](https://axis-intelligence.com/agentic-payments-statistics/)
- Status only: Adyen "limited availability" for US enterprise; Checkout.com on Visa TAP partner list and first-wave Mastercard Agent Pay acquirer; Worldpay on Google AP2 and Visa TAP lists with live ACP docs; PayPal exploring agent purchases under spend limits (analyst blog) — [Agentic Commerce Report](https://agenticcommerce.report/topics/payments/); [Clinkbill](https://clinkbill.com/blog/agentic-commerce-merchant-stack-psp)
- Checkout.com surpassed $300B annual volume and returned to full-year profitability (no agentic split) — [FF News](https://ffnews.com/news/checkout-com-returns-to-full-year-profitability-and-surpasses-300b-in-volume-as-it-positions-for-the-era-of-agentic-commerce)

**American Express**
- Agent Purchase Protection announced 14 Apr 2026 with ACE developer kit; covers charges from registered-agent errors when cardholder authorized the agent and agent sent authenticated intent — [PYMNTS](https://www.pymnts.com/artificial-intelligence-2/2026/american-express-to-back-purchases-made-by-customers-ai-agents/)
- "Intent Intelligence" framework introduced 13 Oct 2026 to capture/authenticate purchase intent before an agent transacts — [TechCrunch](https://techcrunch.com/2026/10/13/amex-introduces-a-framework-to-prevent-ai-agents-from-making-unauthorized-purchases/)
- As of Oct 2026 terms, conditions and registration criteria reported as unfinalized, US-only — [El Fondo](https://www.el-fondo.com/en/news/american-express-plans-purchase-protection-for-ai-shopping-agents)
- No claims counts or claim-rate data published.

**Chargebacks911 (dispute vendor) merchant-side indemnification**
- Launched indemnification for merchants against qualifying chargebacks on AI-agent purchases, 26–27 Oct 2026; pricing, eligibility, caps, regions not disclosed — [The Paypers](https://thepaypers.com/fraud-and-fincrime/news/chargebacks911-launches-indemnification-for-ai-agent-purchases); [Digital Transactions brief](https://digitaltransactions.net/chargebacks911s-ai-chargeback-tool-and-other-digital-transactions-news-briefs-from-10-26-26)

### Inferences
- Both networks now have formal dispute/liability frameworks (Visa rules + TAP; Mastercard Agent Pay + Agentic Trust Intelligence; Amex APP) but neither has released outcome data, implying volumes are too small or too new for stable dispute statistics as of Q3 2026.
- The emergence of paid indemnification products (Chargebacks911 merchant-side, Amex cardholder-side) is itself a signal that agent-order dispute risk is perceived as material and unallocated, even though no vendor will quantify it.
- Stripe's "38x" figure, if real, is consistent with Forter's 7.2x since early August and FirstPageSage's Sept jump, all driven by Muse; but it should be cited only as an unverified secondary claim.

### Gaps
- No agent transaction counts, authorization rates, or dispute rates from Visa, Mastercard, Stripe, Adyen, Checkout.com, PayPal, Worldpay, or Amex as of 3 Nov 2026.
- No Amex Agent Purchase Protection claims data.
- Mastercard Q3 8-K text was table fragments; agentic statements come from call transcripts only.

---

## 3. Fraud and dispute vendor network data: agent-order volume, fraud, dispute, cancellation, approval rates, and merchant identification

### Takeaway
Forter is the only vendor publishing network-level agent-order data (now updated 2 Nov 2026 with data through 29 Oct): agentic orders up 7.2x since the first week of August, Muse ~49% of agent orders by late October, 100% authorization for protocol agents, customer cancellations 1–2% (Google 10%), merchant cancellations on scraping-agent orders 27–37% in Asia/ME and Europe. Forter explicitly withholds fraud and dispute rates (promised in a 12 Nov webinar). Signifyd, Riskified, Chargeflow, Sift, Kount, Ravelin publish no agent-specific rates.

### Cited Findings

**Forter (vendor: fraud prevention; network of its own merchants)**
- Post "AI Agent Order Data: What's Actually Happening at Checkout" updated 2 Nov 2026; data 23 Jul–29 Oct 2026. Definitions: protocol agents = OpenAI (ChatGPT Instant Checkout) and Google; scraping agents = Meta Muse, Instinct, Grok Bot, ChatGPT Agent; "humans" = other orders at sites with ≥1 agentic order in prior 14 days; detection dates may lag actual first orders — [Forter](https://www.forter.com/blog/ai-agent-order-data/)
- Growth: agentic orders +24% MoM Aug→Sep (+48% incl. Instinct and Grok Bot, first detected 1 Sept); Muse detection began 21 Sept, that week agentic orders +229% MoM; since Muse launch all agentic orders +41% WoW and 7.2x since first week of August; Muse placed 7.3x as many orders in its first 39 days as Google did in 13 weeks; OpenAI/Google/ChatGPT Agent orders only +4% MoM (Muse growth "additive") — [Forter](https://www.forter.com/blog/ai-agent-order-data/)
- Earlier snapshot (window 1 Aug–27 Sep 2026): agentic orders reached 4.4x early-August level; within 8 days of 21 Sept Muse placed two-thirds as many orders as all agents combined had in the prior month — [Opus Research, 7 Oct 2026](https://opusresearch.net/2026/10/07/agentic-commerce-is-starting-to-leave-a-trail/)
- Mix: OpenAI 80–92% of agentic orders each week in August; Muse >50% in its first detection week, 49% in week of 19 Oct; scraping agents 1% of agentic orders in late July → 77% in week of 19 Oct; Muse bought from 74% of stores receiving any agentic order (through 7 Oct), ChatGPT Agent 45% — [Forter](https://www.forter.com/blog/ai-agent-order-data/)
- Order profile: avg agentic order $227; 27% of agentic orders ≤$50; Google spends 56% less than average and supports single-item checkout only; ChatGPT Agent has largest share of orders >$500; nearly a quarter of ChatGPT Agent orders outside US; Google/OpenAI pilots US-only, Muse US+Canada — [Forter](https://www.forter.com/blog/ai-agent-order-data/)
- Authorization: protocol agents 100%, humans 98.9%, scraping agents 98.8%. Approval (merchant/Forter risk approval): scraping agents' average approval rate rose from 44% to 73% after 24 Sept adjustments; Grok Bot +50 pts; no material change for humans or protocol agents — [Forter](https://www.forter.com/blog/ai-agent-order-data/)
- Cancellations: customer-initiated typically 1–2%, Google 10%; merchants cancel scraping-agent orders more often (ChatGPT Agent and Instinct most, OpenAI least), human orders ~10%; scraping-agent orders canceled 27% in Asia/Middle East and 37% in Europe; human travel orders canceled at a rate 10 pts lower than agentic travel — [Forter](https://www.forter.com/blog/ai-agent-order-data/)
- Not reported: fraud rates, dispute/chargeback rates, total network size. Webinar "The First 60 Days of Agentic Transactions" (12 Nov 2026) promises to address whether disputes are higher — [Forter](https://www.forter.com/blog/ai-agent-order-data/)
- Merchant identification: Forter states merchants "cannot see who the real buyer is" behind an agent; its product claim of linking ~98% of agentic orders to a known person is marketing without published methodology — [Forter](https://www.forter.com/blog/ai-agent-order-data/); [Market & Tech, 14 Oct 2026](https://marketandtech.com/en/news/2026-10-14-ecom-forter-agentic-commerce-engine/)

**Signifyd (vendor)**
- 2026 State of Fraud (Aug 2026): e-commerce fraud pressure +33% YoY in first four months of 2026; ATO attacks +78%; card-testing +175%; characterizes agentic fraud as "clean, fast, successful" transactions; no agent-specific rate — [Signifyd release](https://www.signifyd.com/news-releases/state-of-fraud-report/); [DevPro Journal](https://www.devprojournal.com/technology-trends/ai/agentic-ai/agentic-commerce-fraud-looks-clean-which-is-the-problem/)

**Ravelin (vendor)**
- Global Fraud Trends 2026: survey of 1,504 fraud professionals in 10 countries; successful dispute-challenge rate 62% in 2026 vs 43% prior year; agentic content is directional only — [The Paypers](https://thepaypers.com/fraud-and-fincrime/thought-leader-insights/ravelin-report-key-takeaways-global-fraud-trends-2026); [Ravelin blog](https://www.ravelin.com/blog/the-agentic-commerce-gold-rush-risk)

**Chargeflow (vendor; marketing)**
- Projects ~286M chargebacks in 2026 rising to 359M by 2029; friendly fraud ~8 in 10 disputes across its merchant base; argues legitimate agent orders produce disputes from "forgotten approvals and delegated mistakes"; no agent-order rate — [Chargeflow](https://www.chargeflow.io/blog/agentic-commerce-and-its-impact-on-fraud-and-chargebacks)

**Riskified (vendor)** — publishes consumer surveys (Section 4) but no network data on agent orders, fraud, or disputes was found.

**Sift, Kount** — no 2026 agent-order disclosures found; Sift's only figure is non-agent and 2025 **[background]** (loyalty points fraud attack rate 6.19% Q1 2025).

**HUMAN Security (vendor)** — carding: blocked checkout interactions +20% vs 2024, +250% since 2022; one observed agent session with 11 card-add and 6 payment attempts pivoting to loyalty redemption (2025 data) **[background]** — [PPC Land](https://ppc.land/ai-agents-are-now-buying-things-and-fraud-looks-identical/)

### Inferences
- Forter's data implies the *approval* problem is on the merchant/risk side (44%→73% for scraping agents after rule changes) rather than issuer authorization (≈99–100%), i.e., agent orders are being declined by fraud rules, not by banks.
- Forter's high merchant cancellation rates for scraping-agent orders in Europe (37%) and Asia/ME (27%) are the closest thing to a measured "wrong/unfulfillable order" rate in production; but Forter attributes much of it to agents ordering outside supported geographies, not to item errors.
- The absence of any published agent dispute rate from eight vendors, two networks and a dozen processors as of 3 Nov 2026 should itself be reported; any specific "agent dispute multiple" circulating (e.g., the excluded 2.4x) lacks a traceable source.

### Gaps
- No agent-order fraud rate, chargeback rate, or dispute rate from any vendor. Forter's 12 Nov 2026 webinar is the next scheduled disclosure.
- No vendor publishes its network's total agent-order count or share of e-commerce.
- No independent validation of Forter's "98% linkage to a person" product claim.

---

## 4. Consumer surveys (May–Nov 2026) measuring actual use: completed an agent purchase, wrong purchase, abandonment after error, by region

### Takeaway
Across independent-ish samples the share of consumers who have *actually completed* a purchase inside or via an AI agent clusters at 8–13% (Riskified 11% across five markets, Sept 2026; NMI 11% US, Aug 2026; Product.ai 8% of US shoppers; Riskified Oct 2025 13% background), while 7–16% say they would let an agent buy without review. No survey directly measures the share who *experienced* a wrong agent purchase; the closest proxies are Blue Yonder's 46% of US AI-recommendation buyers who returned an item (Nov 2026) and ACI's 60% of UK adults who would stop after one mistake (June 2026). No Israel-specific data exists.

### Cited Findings

**Riskified Agentic Commerce Pulse, release 27 Oct 2026 (URL slug says "November 2026"); fielded Sept 2026; 5,100 consumers 18+; US, UK, Japan, Mexico, Brazil** (vendor)
- 80% have used AI in some part of shopping; **11% have completed a purchase inside an AI tool**; 7% comfortable allowing an agent to buy without approval; half are more comfortable than six months ago with an agent completing a purchase without immediate approval; 74% would allow agentic purchases in some form; 68% likely to use AI for holiday shopping; share who would trust *no* company with AI-led shopping fell from 47% (Q2 survey) to 17%; top concerns: payment-credential security and AI choosing the wrong product — [Riskified press (403 on fetch; via search excerpt)](https://www.riskified.com/press/agentic-commerce-pulse-consumer-survey-november-2026/); [Agentic Commerce Frontier newsletter](https://agentcommerce.substack.com/p/the-agentic-commerce-frontier-october-a4f)
- Same wave reported on Riskified blog for holiday: 57% would let AI compare products/prices; 7% would let it buy gifts without further review — [Riskified blog](https://www.riskified.com/blog/)
- No per-country breakdown was retrievable.

**Riskified Q1 2026 wave (released 27 Apr 2026; 2,000 consumers, 1,000 US + 1,000 UK)** (vendor, edge of window)
- 61.5% used AI tools to discover products; 55% not comfortable with agents purchasing on their behalf (vs 70% at least somewhat comfortable in Q4 2025); liability for unauthorized purchases: 50.8% AI platform, 23.2% retailer/brand, 18.7% consumer; 53.9% think AI could increase fraud risk; 73.9% expect biometrics/OTP — [Riskified press](https://www.riskified.com/press/riskified-agentic-commerce-pulse/); [Digital Transactions](https://www.digitaltransactions.net/many-consumers-say-no-thanks-to-agent-based-payments/)
- **[background]** Oct 2025 wave (5,400 consumers, Sept–Oct 2025): 13% said an AI completed a purchase after guiding them; 70% at least somewhat comfortable — [CX Dive](https://www.customerexperiencedive.com/news/consumers-ai-research-few-purchase/803156/)

**NMI "Embedded Payments: Agentic Commerce Reality Check", 26 Aug 2026; 1,000 US adults** (vendor: payments gateway)
- **11% have used AI to complete a transaction** (parents 19% vs non-parents 7%); **58% worry an agent could buy the wrong item, size, color or brand**; 60% worry it could overspend; 76% uncomfortable with AI completing purchases; 10% would give full control; 70% must be able to review/override; 84% never uploaded payment details to an AI tool; blame if AI bought before ready: 62% AI provider, 34% retailer, 24% payments provider (multi-select) — [NMI](https://www.nmi.com/about-us/news/nmi-research-consumers-want-ai-to-help-them-shop-not-control-their-spending/)

**ACI Worldwide / YouGov UK, 28 Jun 2026; 2,080 UK adults 18+, fielded 19–22 Jun 2026, weighted** (vendor)
- **60% would stop using an AI shopping agent after one mistake**; 69% do not trust AI even when it follows rules they set; 7 in 10 put off by purchases made without asking; 61% balk at linking an agent to their bank account; refund responsibility when transactions fail: 54% AI providers, 9% retailers, 3% banks/issuers; 50% trust AI to find best price, 43% to follow spend limits — [ACI Worldwide IR](https://investor.aciworldwide.com/news-releases/news-release-details/six-ten-uk-consumers-would-stop-using-ai-shopping-agent-after); [Business Wire](https://secure.businesswire.com/news/home/20260628382944/en/Six-in-Ten-UK-Consumers-Would-Stop-Using-an-AI-Shopping-Agent-After-One-Mistake-ACI-Survey-Finds)
- ACI/YouGov fashion wave, 16 Sept 2026; 3,328 adults 18–65 (UK 2,172, US 1,156), fielded June 2026: 53% uncomfortable letting AI buy on their behalf; 20% accept recommendations only; 14% require manual approval per purchase; 7% would permit autonomous purchasing under predefined conditions; after a payment failure 39% abandoned, 22% switched retailer, 53% tried another method — [Finviz/Business Wire](https://finviz.com/news/392490/only-7-of-fashion-shoppers-trust-ai-to-buy-for-them-today-showing-merchants-must-build-confidence-in-agentic-commerce)

**Global Payments / The Lantern, 23 Sept 2026; wave 2 = 8,027 consumers, May 2026; US, UK, France, Brazil, China, Singapore, Australia** (vendor)
- Consumers expect AI to make 15% of their purchases within five years (9% a year earlier); 45% of Americans have used *or would consider* an AI shopping agent (past use not separated); **42% worry AI could make wrong purchasing decisions**; 33% want to approve every transaction; 50% concerned about payment security; comfort letting AI spend up to $50 on cinema tickets 32%→82% YoY — [Business Wire via Finviz](https://finviz.com/news/394701/consumers-expect-ai-to-make-15-of-their-purchases-within-five-years-global-payments-research-finds)

**Deloitte 2026 Holiday Retail Survey; 4,000 US consumers, fielded Aug–Sep 2026, published ~15 Oct 2026** (independent)
- 39% say generative AI is their primary tool for at least one holiday shopping task; **16% trust / would let AI agents complete a purchase for them**; 37% more comfortable using agents than six months ago; 66% worry about data use — [Adweek](https://www.adweek.com/commerce/consumers-arent-ready-to-let-ai-do-their-holiday-shopping-yet/); [Agile Brand Guide](https://agilebrandguide.com/spending-more-buying-less-our-2026-holiday-predictions/)

**Bain Consumer Lab Holiday Survey 2026 (with ROI Rocket); US n=1,105, fielded Aug 2026; published 3 Sept 2026** (independent)
- 24% of online shoppers say they will *begin* holiday shopping on gen-AI platforms (ChatGPT, Claude, Gemini), up from 17% in Oct 2025 (multi-select); an additional 13% plan to use a retailer's own agent (e.g., Walmart Sparky); no actual-purchase measure; shoppers 3x more likely to trust retailer-built AI than general platforms with the full journey — [Bain](https://www.bain.com/insights/2026-holiday-shopping-outlook-infographic/)

**Accenture Consumer Pulse 2026; 25,590 consumers, 16 countries, Jan 2026; published 3 Jun 2026** (independent-ish consultancy)
- 74% would trust a personal AI agent more than their best friend to buy on their behalf; 32% would let an agent decide what to buy if they confirm payment; **9% open to fully autonomous purchasing incl. payment**; 12% open to autonomy at the payment stage (another reading) — [The Diplomat (RO)](https://www.thediplomat.ro/2026/06/09/accenture-study-74-of-consumers-are-ready-to-let-an-ai-agent-shop-on-their-behalf/); [AI News](https://www.artificialintelligence-news.com/news/ai-shopping-agents-consumer-trust-accenture-report/)

**Amex Trendex, Aug 2026; 2,005 US adults** (vendor)
- 52% comfortable letting an agent buy everyday low-cost items; 15% comfortable for high-value items such as electronics — [Business Wire index](https://www.businesswire.com/news/home/20260224711754/en) (via search summary)

**Blue Yonder 2026 Consumer Retail Returns Survey, 2 Nov 2026; >5,000 consumers incl. 2,000 US; US, AU, FR, DE, UK** (vendor: supply chain)
- **46% of US shoppers who bought an item based on an AI recommendation returned at least one such item in the past year**; top reasons: different than described, wrong size/fit; 20% trust AI to recommend right size/fit — [Retail Customer Experience](https://www.retailcustomerexperience.com/news/ai-data-issue-driving-product-returns/)

**Product.ai (vendor: product-data) — US, 1,493 shoppers, self-reported, 2026**
- 21.6% of AI users (= 8.0% of all shoppers surveyed) have completed a purchase through an AI tool; separate April 2026 figure: 14% trust agents to purchase without human confirmation — [Product.ai trust report](https://product.ai/research/trust-in-ai-commerce-report/); via [Winzheng](https://www.winzheng.com/en/article/stripe-ai-agent-38x-payment-surge-september-2026)

**PYMNTS Intelligence / Visa Acceptance GDSI (vendor-sponsored)**
- June 2026 index: nearly half of online shoppers used AI during their latest purchase (research/assist, not agent checkout); Brazil edition (Aug 2026): 53% used AI in most recent purchase, 70% expect to use a shopping agent within two years; UAE playbook (Sept 2026): 72% used AI in most recent purchase, 71% expect an agent within two years; "Agentic Deep Dive" (Jul 2026; US/Brazil/UAE surveys fielded early 2026) — [PYMNTS agentic deep dive](https://www.pymnts.com/study/global-digital-shopping-index-agentic-commerce-deep-dive/); [PYMNTS June 2026 PDF](https://www.pymnts.com/wp-content/uploads/2026/06/PYMNTS-Intelligence-Global-Digital-Shopping-Index-June-2026.pdf); agentic-edition PDF could not be text-extracted — [Visa Acceptance PDF](https://www.visaacceptance.com/content/dam/documents/campaign/shopping-index/2026-global-digital-shopping-index-agentic-edition.pdf)
- PYMNTS: only 23% of US consumers trust GenAI to handle payments on their behalf; 39% used AI for at least one payment-related activity in the three months to April 2026 — [The Outpost](https://theoutpost.ai/news-story/ai-payments-hit-trust-barrier-72-use-ai-assistants-but-only-23-trust-them-with-money-30658/); [Agile Brand Guide chart](https://research.agilebrandguide.com/chart/39-of-us-consumers-used-ai-for-at-least-one-payment-related-activity-in-the-thre)
- GDSI Merchant Edition (Sept 2026): 93% of merchants believe the AI/agent provider should bear financial loss for incorrect agent purchases — via [Forkast](https://forkast.news/the-agentic-commerce-liability-gap-who-pays-when-the-agent-buys-wrong/) (secondary; primary not retrieved)

**Checkout.com "Agentic Commerce 2026" (vendor); >12,000 consumers in six countries + 400 UK payments executives; Q1 2026, reported June 2026**
- 24% flatly refuse to delegate purchases to AI, 8% unsure; 25% would stop using an agent if a purchase was difficult to dispute — [Checkout.com report page](https://www.checkout.com/guides-and-reports/agentic-commerce-2026); [Fintech News SG](https://fintechnews.sg/137054/ai/trust-and-control-emerge-as-key-barriers-to-ai-enabled-commerce/)

**Other**
- Alipay+ / S&P Global: only 26% of consumers interested in using agentic AI for transactions — [Fintech News SG](https://fintechnews.sg/137054/ai/trust-and-control-emerge-as-key-barriers-to-ai-enabled-commerce/)
- Cordial (2026 holiday): 13% would trust an agent to complete purchases without review; 34% comfortable with agent completing purchases within preferences/budget — [Retail Customer Experience](https://www.retailcustomerexperience.com/news/holiday-shoppers-embracing-ai-helpers/)
- Forbes (Jordan McKee, 1 Oct 2026): "more than two in five" would stop using agents for a task after an agent error; "more than one-third" would contact their bank/issuer after an agent purchasing error — [Forbes](https://www.forbes.com/sites/jordanmckee/2026/10/01/agentic-commerce-has-a-consumer-trust-problem/) (403 on fetch; underlying survey and sample not confirmed — likely S&P Global 451 Research but unverified)
- Bluestone PIM (201 consumers, Mar 2026; tiny sample): a quarter would blame the retailer if AI helped them buy the wrong product — [Bluestone PIM](https://www.bluestonepim.com/blog/are-consumers-ready-to-shop-with-ai)
- Merchant side: 63% of e-commerce leaders say their checkout cannot currently be completed by an AI agent (Blend Commerce audit/survey, 200 UK/US retailers) — [Blend Commerce](https://blendcommerce.com/blogs/shopify/ai-shopping-audit-2026)

### Inferences
- "Completed a purchase via AI" is consistently ~8–13% in mid/late-2026 US/UK/multi-market samples regardless of sponsor, and has not moved materially since Riskified's 13% in Oct 2025, even as comfort/intent metrics rose sharply (distrust-of-all-companies 47%→17%). Intent is running far ahead of behavior.
- Willingness for *unsupervised* agent purchases is a stable 7–16% band (Riskified 7%, ACI 7%, Accenture 9%, NMI 10%, Cordial 13%, Deloitte 16%).
- Wrong-purchase *fear* (NMI 58%, Global Payments 42%) is measured; wrong-purchase *experience* is not. Blue Yonder's 46% return rate among AI-recommendation buyers is the only outcome-type number and it concerns human-completed purchases on AI advice.
- Liability expectation is consistent: consumers (ACI 54%, NMI 62%, Riskified 50.8%) and merchants (PYMNTS 93%) both point at the AI provider.

### Gaps
- No survey measures the share who *experienced* a wrong agent purchase or who abandoned agents *after* an actual error (ACI/Forbes figures are hypothetical).
- No regional breakdowns retrievable for Riskified's five-market Sept 2026 wave (press page 403).
- No Israel-specific agentic commerce survey found.
- Klarna, Capgemini, McKinsey (beyond a 749-person European AI-use survey), Bain actual-use, and Salesforce sample sizes: nothing with actual-purchase measures found.
- Visa/PYMNTS agentic-edition PDF is image-based; US agent-purchase share not extracted.

---

## 5. Holiday 2026 forecasts for agent-driven sales (Adobe, Salesforce, Bain, Deloitte) and their methodology

### Takeaway
None of the four forecasters projects a dollar figure for *agent-completed* holiday orders in 2026. Adobe forecasts AI-*referred* traffic +130% YoY (season) on $275.1B US online spend; Salesforce forecasts 20% of holiday e-commerce *traffic* from AI chat agents (a definition that includes scrapers and backend agents); Bain and Deloitte provide survey-based intent shares only. The only agent-order share forecasts are from vendors/aggregators ("low single digits", "<3%").

### Cited Findings
- **Adobe** (forecast published 28 Sept 2026): US online holiday spend 1 Nov–31 Dec 2026 = $275.1B (+6.7%); AI-referred traffic +130% YoY for the season, +159% Thanksgiving, +95% Black Friday, +88% Cyber Monday; AI referral defined as clicks on links from AI chat/browser services; methodology: >1 trillion visits to US retail sites, 100M SKUs, 18 categories, direct transaction data; no agent-completed purchase forecast — [Relevant Audience](https://www.relevantaudience.com/ecommerce-marketing/adobe-2026-holiday-forecast-275-billion-ai-traffic-130-percent/); [Adobe forecast PDF](https://business.adobe.com/assets/resources/holiday-shopping-report/2026/pdf/2026-adobe-digital-insights-holiday-forecast.pdf); [Search Engine Watch](https://searchenginewatch.com/adobe-expects-ai-shopping-referrals-to-more-than-double-this-holiday-season/)
- **Salesforce** (20 Jul 2026): 20% of all 2026 holiday e-commerce traffic will originate from AI chat agents, explicitly including consumer-facing bots, autonomous backend agents and competitor price-scrapers; 1 in 3 e-commerce sites will have a site-specific shopper agent live by Cyber Week; 2025 actual: brands with branded agents grew holiday sales 6.2% vs 3.9%; forecast model not described; based on Salesforce Commerce/Marketing/Service Cloud data (1.5B+ shoppers) plus unspecified survey — [Salesforce](https://www.salesforce.com/blog/holiday-retail-predictions-2026/). **[background]** Salesforce's 2025-season forecast of 21% of holiday orders / $263B influenced by AI and agents is from Sept 2025 and should not be cited as 2026 — [Salesforce 2025 release](https://www.salesforce.com/news/stories/emerging-llm-search-for-holidays/)
- **Bain** (3 Sept 2026): US Nov–Dec retail sales +4.5% YoY, >$1T; survey (US n=1,105, Aug 2026, with ROI Rocket): 24% will begin shopping on gen-AI platforms, 13% plan to use a retailer agent; no agent sales-share forecast — [Bain](https://www.bain.com/insights/2026-holiday-shopping-outlook-infographic/)
- **Deloitte** (Oct 2026): e-commerce holiday sales $316.1–318.9B for Nov 2026–Jan 2027 (+7.5–8.4%); survey (4,000 US, Aug–Sep 2026): 39% use gen AI as primary tool for ≥1 task, 16% would let it complete a purchase; no agent sales-share forecast — [MarketScale](https://www.marketscale.com/industries/retail/holiday-e-commerce-will-reach-up-to-319-billion-this-season-deloitte-forecasts.md); [Adweek](https://www.adweek.com/commerce/consumers-arent-ready-to-let-ai-do-their-holiday-shopping-yet/)
- Vendor/aggregator agent-order predictions (not from the four firms): "agent-completed purchases likely to stay in the low single digits as a share of holiday e-commerce" — [Shopappy](https://shopappy.com/payments/card-networks/agentic-checkout-holiday-2026); "AI agents will complete fewer than 3% of holiday online orders" — [Elite Automation](https://elite-automation.com/ai-shopping-agents-in-2026/); an analysis of why four holiday AI reports disagree — [Matt Britton](https://www.mattbritton.com/blog-posts/ai-holiday-shopping-trends-2026-cmo-divide)
- Gartner (Emily Weiss, Oct 2026): holiday shoppers use gen AI for inspiration, "still, by and large, want to complete that purchase themselves" — [Retail TouchPoints](https://www.retailtouchpoints.com/?p=621821)

### Inferences
- Adobe's +130% and Salesforce's 20% are not comparable: one is YoY growth of referral clicks, the other a share of all traffic including non-shopping bots. Neither supports a claim about agent-completed order share.
- Given Forter's 7.2x growth since August and FirstPageSage's 1.5% Oct estimate, a 2026 holiday agent-completed order share in the 1–3% range is plausible but no credible forecaster has committed to a number.

### Gaps
- No Adobe, Salesforce, Bain or Deloitte forecast of agent-completed holiday order share or dollars for 2026.
- Salesforce's 20% derivation and site count are undocumented.

---

## 6. Field evidence on real agent order outcomes (retailer disclosures, academic studies with real purchases)

### Takeaway
No retailer has disclosed an error rate on agent-placed orders, and no academic study with real purchases exists; the only production outcome data is Forter's cancellation breakdown (customer 1–2%, Google 10%; merchant-initiated 27–37% on scraping-agent orders in Europe/Asia-ME). Wharton's August 2026 technical report is a simulated-environment study showing agent choices are highly contingent on context (up to +99 pp shift from a single source) but reports no accuracy metric.

### Cited Findings
- Forter production cancellations (see Section 3 for full detail): customer cancellations typically 1–2% (Google 10%); merchant cancellations higher on scraping-agent orders, ChatGPT Agent and Instinct most, humans ~10%; scraping-agent orders canceled 27% Asia/Middle East, 37% Europe; agentic travel canceled 10 pts more than human travel; digital goods and travel highest — [Forter, 2 Nov 2026](https://www.forter.com/blog/ai-agent-order-data/)
- Wharton Generative AI Labs, "Agentic Shopping is Complicated and Contingent" (27 Aug 2026; Kumar, Meincke, Shapiro, Mollick, Puntoni, Mollick): simulated purchases in ACES (Agentic e-CommercE Simulator); six models (Claude Haiku 4.5, Claude Opus 4.8, GPT-5 Mini, GPT-5.5, Gemini 3.1 Flash Lite, Gemini 3.5 Flash); ~26,000 runs, 200 per condition; a single external source (Wirecutter) shifted product choice up to +99 pp (Gemini 3.5 Flash); source order changed outcomes (Gemini 3.1 Flash Lite switched >50% of runs when Reddit shown first); an injected one-sentence user "memory" redirected most models away from an objectively superior $29.99 product (Claude Opus 4.8 switched ~75% of runs); bundled vs separate tool calls shifted GPT-5.5 +53 pp; no purchase-accuracy or error-rate metric reported — [Wharton GAIL](https://gail.wharton.upenn.edu/research-and-insights/technical-report-agentic-shopping/); [The Decoder](https://the-decoder.com/ai-shopping-agents-arent-ready-to-buy-on-your-behalf-study-finds/)
- arXiv "When Agents Shop for You: Role Coherence in AI-Mediated Markets" (Apr 2026) — simulation/theory, no real purchases — [arXiv](https://arxiv.org/pdf/2604.26220)
- Product.ai (Sept 2026, vendor): four AI engines asked 220 shopping questions; 86% of responses contained a factual conflict — [Product.ai](https://product.ai/research/ai-shopping-divergence-study/)
- Blend Commerce audit of 200 UK/US retailers: tested whether agents could complete checkout (not correctness); 63% of e-commerce leaders say checkout cannot currently be completed by an agent — [Blend Commerce](https://blendcommerce.com/blogs/shopify/ai-shopping-audit-2026)
- Blue Yonder (2 Nov 2026): 46% of US shoppers who bought on an AI recommendation returned ≥1 item in past year (human-completed purchases) — [Retail Customer Experience](https://www.retailcustomerexperience.com/news/ai-data-issue-driving-product-returns/)
- Meta Muse scale context: >10M users per Meta Q3 2026 call (TechCrunch via FirstPageSage); 2.18M DAU as of 5 Oct (Collison, via Winzheng) — [FirstPageSage](https://firstpagesage.com/seo-blog/agentic-commerce-statistics); [Winzheng](https://www.winzheng.com/en/article/stripe-ai-agent-38x-payment-surge-september-2026)
- Merchant identification of agent orders: Forter says merchants "cannot see who the real buyer is"; agent-driven orders often appear as direct traffic, generic referrals or API calls so retailers undercount the channel — [Forter](https://www.forter.com/blog/ai-agent-order-data/); [Idukki](https://idukki.io/blog/agentic-checkout-explained-acp-and-instant-checkout); Clarity's Agent Activity Dashboard (GA 29 Oct 2026) is the first mass-market merchant-side tool for identifying agent sessions — [PPC Land](https://ppc.land/checkout-appears-in-25-of-ai-agent-sessions-vs-15-for-humans-clarity-finds/)

### Inferences
- The best available production "error" proxy is Forter's merchant-cancellation rate on scraping-agent orders (27–37% ex-US), but Forter frames it largely as geography/eligibility mismatch rather than wrong-item selection, so it overstates item-level error.
- Wharton's finding that agent choice is dominated by whatever context it saw first implies that "accuracy" in production will depend heavily on merchant data quality and injected context, consistent with Blue Yonder's "it's a data problem" framing.

### Gaps
- No retailer-published agent-order error, return, or dispute rate.
- No academic study using real purchases with real money.
- No merchant survey quantifying what share of merchants can distinguish agent orders from human orders.
