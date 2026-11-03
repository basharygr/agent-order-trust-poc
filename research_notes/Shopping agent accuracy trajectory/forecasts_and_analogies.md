# Shopping-agent accuracy trajectory: forecasts, rate-of-improvement evidence, and historical analogies for verification layers

As-of date: 3 November 2026. Every figure is dated where the source dates it. "Vendor" marks claims by a company selling into the market; "independent" marks academic, regulator, or non-vendor analyst work. Secondary citations (a blog restating an analyst number) are flagged as such; the report writer should treat them as weaker than primary.

Framing used throughout: a merchant-side "confirm with the human and keep the proof" product has two separable parts — (1) a **confirmation step** (friction before the act) and (2) an **evidence/audit layer** (zero-friction proof after the act). The analogies below consistently treat these two parts differently, so the notes keep them apart.

---

## Key Question 1: What do analysts, platforms and networks forecast for agent-completed purchases (share of e-commerce by 2027/2028/2030), and what accuracy do they assume or require?

### Takeaway
Forecasts for the US converge on a 10–25% share of online retail being agent-initiated or agent-completed by 2030 (Bain 15–25%, Morgan Stanley 10–20%, Gartner 20% of digital commerce "executed through AI platforms", J.P. Morgan up to 25%), with headline global numbers ranging from $144B (eMarketer) to $3–5T (McKinsey) depending on whether "influenced" purchases count. None of the forecasts I found states a required agent accuracy level; the stated gating factor is uniformly "consumer trust" (and, in the only hard post-mortem available, OpenAI's Instant Checkout, "errors" and stale product data), with no forecaster quantifying trust as an error rate.

### Cited Findings

**Gartner (analyst, independent of merchants but sells research)**
- Gartner projected at its October 2025 IT Symposium that 90% of B2B purchases will be handled by AI agents "within three years", moving more than $15 trillion through automated exchanges (reported 28 Nov 2025). — [Digital Commerce 360](https://www.digitalcommerce360.com/2025/11/28/gartner-ai-agents-15-trillion-in-b2b-purchases-by-2028/); the primary Gartner page is paywalled/403 — [Gartner Strategic Predictions for 2026](https://www.gartner.com/en/articles/strategic-predictions-for-2026)
- Gartner: "20% of digital commerce transactions will be executed through AI platforms, either via on-platform checkout or via AI agents by 2030" (secondary citation, commercetools blog dated 8 Oct 2026; vendor blog restating Gartner). — [commercetools](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)
- Gartner, 28 July 2026 press release: AI agents will outnumber sellers 10 to 1 by 2028, "yet fewer than 40% of sellers will say agents improved productivity" (title only; page returned 403 on fetch). — [Gartner newsroom](https://www.gartner.com/en/newsroom/press-releases/2026-07-28-gartner-predicts-ai-agents-will-outnumber-sellers-10-to-1-by-2028-yet-fewer-than-40-percent-of-sellers-will-say-agents-improved-productivity)
- Gartner's Bob Hetu on OpenAI's March 2026 retreat from Instant Checkout: "OpenAI underestimated how difficult the enablement of transactions was going to be." — [CNBC, 20 Mar 2026](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html)

**Bain (independent analyst)**
- Bain (reported 22 Dec 2025): US agentic commerce could reach $300–500B by 2030, equal to 15–25% of US online retail sales. Definition: transactions that autonomous agents "start, shape, or complete", excluding AI used only for search/recommendation. — [Digital Commerce 360, 22 Dec 2025](https://www.digitalcommerce360.com/2025/12/22/bain-agentic-ai-us-ecommerce-sales-2030/)
- Bain's stated gating assumptions: "Most consumers are not yet comfortable with AI handling an end-to-end transaction without human involvement, particularly for higher-consideration purchases"; specification-driven categories (household essentials, commodities) shift first; discretionary categories (apparel, travel) later. The article contains no mention of accuracy, error rates, or reliability as a gate. — [Digital Commerce 360, 22 Dec 2025](https://www.digitalcommerce360.com/2025/12/22/bain-agentic-ai-us-ecommerce-sales-2030/)
- Bain estimates 30–45% of US consumers already use generative AI to research or compare products (Dec 2025). — [Digital Commerce 360](https://www.digitalcommerce360.com/2025/12/22/bain-agentic-ai-us-ecommerce-sales-2030/)

**Morgan Stanley (independent sell-side)**
- Morgan Stanley estimates agentic shoppers could reach $190–385B of US e-commerce spend by 2030, capturing 10–20% market share. (Primary page timed out twice on fetch; figure confirmed by two secondary citations.) — [Morgan Stanley](https://www.morganstanley.com/insights/articles/agentic-commerce-market-impact-outlook); restated by [commercetools, 8 Oct 2026](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)

**McKinsey, BCG/Stripe, J.P. Morgan, Juniper, eMarketer (all via secondary sources)**
- McKinsey (with ICSC): $1T of US retail revenue "orchestrated" by AI agents by 2030; global estimates cited as $3–5T. One aggregator notes "orchestrated" includes purchases influenced, not only executed, by agents. — [stellagent.ai (secondary)](https://stellagent.ai/insights/agentic-commerce-market-size-forecast-2030); [agentic-commerce.de (secondary)](https://agentic-commerce.de/en/marktprognosen/)
- Stripe and BCG: 55% of US e-commerce spend "agent-assisted" and 18% "fully autonomous" (~$1.3T impacted, ~$400B mostly autonomous); horizon not specified in the secondary source. — [commercetools (secondary)](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)
- J.P. Morgan: up to 25% of US online sales by 2030, concentrated in groceries and subscriptions. — [commercetools (secondary)](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)
- Juniper Research: global agentic transaction value $8B in 2026 growing to $3.5T by 2031. — [commercetools (secondary)](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)
- Spread of forecasts: $144B (eMarketer) to $5T (McKinsey); aggregator attributes the spread to definitional differences, and notes US B2C forecasts converge on 10–25% of online sales by 2030. — [stellagent.ai (secondary)](https://stellagent.ai/insights/agentic-commerce-market-size-forecast-2030)

**Forrester (independent analyst)**
- Forrester projects US e-commerce of $1.8T by 2030 (29% of retail) and names agentic commerce as one of several growth drivers; no isolated agent-share figure found. — [Retail Dive](https://www.retaildive.com/news/online-retail-sales-ecommerce-forecast-2030/812833/)
- Forrester's Emily Pfeiffer on Instant Checkout (Mar 2026): only ~30 Shopify merchants live as of Feb 2026; the process was "prone to errors"; stock status, delivery estimates and shipping costs could be inaccurate because data was scraped: "Crawling and scraping is inadequate to get the full breadth of product data." — [CNBC, 20 Mar 2026](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html)

**Visa / Mastercard / Amex (vendors)**
- Visa press release: "hundreds" of agent-initiated transactions completed with partners, "setting the stage for mainstream adoption in 2026". — [Visa newsroom](https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.21961.html); [Digital Transactions](https://www.digitaltransactions.net/visa-predicts-agentic-commerce-will-be-mainstream-in-2026-bigcommerce-adds-stripes-agentic-commerce-suite/)
- Visa and 10+ partners launched Trusted Agent Protocol in October 2025 (with Cloudflare), signing agent identity into HTTP headers checked against Visa's directory. — [Digital Transactions](https://www.digitaltransactions.net/visa-predicts-agentic-commerce-will-be-mainstream-in-2026-bigcommerce-adds-stripes-agentic-commerce-suite/)
- Four agent protocols shipped between April 2025 and March 2026: Visa TAP, Mastercard Agent Pay, Google AP2, Stripe/Tempo Machine Payments Protocol (opinion piece, independent outlet, secondhand sourcing). — [Banks & Bankers](https://banksandbankers.com/agentic-commerce-liability-gap-fraud-vs-agent-error/)
- Visa Intelligent Commerce Connect launched April 2026 in pilot; Amex launched Agent Purchase Protection in April 2026, which covers the cardholder, not the merchant's exposure to agent-behaviour disputes. — [Worldpay (vendor), 2026](https://worldpay.com/en/insights/articles/agentic-commerce-liability-is-still-being-written)
- A vendor benchmark claims only 17% of brands accept any major agent payment protocol as of mid-2026 (unverified). — [presenc.ai (vendor)](https://presenc.ai/research/agentic-commerce-adoption-benchmarks-2026)

**Salesforce / Adobe (vendors; measurement, not forecast)**
- Salesforce pre-season forecast: AI and agents to drive 21% of global holiday 2025 orders, $263B. — [Salesforce blog](https://www.salesforce.com/blog/holiday-predictions-2025/)
- Salesforce Cyber Week 2025 reporting: AI agents "influenced" $67B, 20% of orders (vendor, counts influence not completion; a commentator argues the figure counts agent activity rather than human clicks). — [paz.ai (secondary)](https://paz.ai/blog/salesforce-confirms-ai-agents-hit-20-of-cyber-week-orders-the-tipping-point-has-arrived); [Digital Commerce 360, 13 Jan 2026](https://www.digitalcommerce360.com/2026/01/13/generative-ai-online-holiday-shopping-traffic-2025/)
- Adobe Analytics: generative-AI-referred traffic to US retail sites up 693% YoY in holiday 2025; AI referrals converted 31% better than other traffic; Adobe does not publish AI traffic as a share of visits. — [Digital Commerce 360](https://www.digitalcommerce360.com/article/online-holiday-sales/)
- Digiday: AI sessions below 0.2% of total e-commerce traffic (via commercetools, undated). — [commercetools (secondary)](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)

**Consumer-trust surveys (the proxy forecasters use instead of accuracy)**
- YouGov 2025: ~65% of Americans trust AI to compare prices; only 14% trust AI to place orders for them (Gen Z 29%, millennials 30%). — [commercetools (secondary)](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)
- Worldpay Agentic Commerce Report (vendor, 2026): 45% of consumers ready to let an agent buy on their behalf (54% Gen Z); 95% report at least one concern; 50% say fraud protections would make them trust it more. — [Worldpay](https://worldpay.com/en/insights/articles/agentic-commerce-liability-is-still-being-written)
- Accenture: 87% of CTOs/payments heads see trust as the biggest barrier to agentic payments. — [commercetools (secondary)](https://commercetools.com/blog/agentic-commerce-stats-enterprise-guide)
- Semrush survey of 1,000+ US consumers (reported Mar 2026): 22% had bought a product inside an AI tool; half had purchased after using AI for research. — [CNBC](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html)

**The one real-world post-mortem: OpenAI Instant Checkout (Sept 2025 – Mar 2026)**
- OpenAI deprioritised Instant Checkout in March 2026, moving purchases to retailer-built ChatGPT apps and merchants' own checkouts. Walmart found in-chat conversion was three times lower than sending users to its own site; Walmart's Daniel Danker called Instant Checkout "a very temporary moment in time" (4 Mar 2026). — [CNBC, 20 Mar 2026](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html)
- Google's update loading real-time product data "to prevent out-of-stocks and pricing errors" addresses the same data-accuracy issue. — [CNBC](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html)

### Inferences
- No forecaster models accuracy explicitly; "trust" is the stated gate, and the only measured trust number for autonomous ordering (YouGov 14%, 2025) is far below the 45% "ready" figure from a vendor survey. The forecasts are therefore conditional on trust rising, not on a measured error threshold.
- The Instant Checkout episode shows the first failure mode was not agent reasoning but merchant-data freshness and transaction enablement — i.e., exactly the layer a merchant-side confirmation/evidence product addresses. The platforms' response (push checkout back onto the merchant's own site) moves the error and liability surface to the merchant, which strengthens rather than weakens the case for merchant-side proof.
- Headline growth figures (Salesforce 20%, Adobe +693%) measure influence/traffic, not completed autonomous orders. Digiday's <0.2% of sessions is the only absolute base-rate figure found and suggests autonomous completion is still a tiny fraction of orders as of 2026.

### Gaps
- No Gartner, Forrester, McKinsey, Bain, BCG, Visa or Mastercard document found that states a required agent accuracy (e.g., "X% task success") for mainstream adoption. Searched directly; nothing surfaced.
- Morgan Stanley's primary page timed out on two fetch attempts; figures are from secondary restatements.
- McKinsey/BCG/J.P. Morgan primary reports not retrieved; all via secondary blogs.
- No Sequoia or a16z forecast with a share-of-commerce number was found.
- Salesforce "20% of orders" methodology (influenced vs completed) not published in the sources found.

---

## Key Question 2: What does METR's time-horizon trend (and other rate-of-improvement evidence) say about 50% vs 80% reliability, and what does it imply for "complete a multi-step purchase correctly"?

### Takeaway
As of mid-2026 the frontier 50% horizon is ~12–17 hours while the 80% horizon is ~1.5–3 hours, a roughly 5–8x gap; METR says the 50% and 80% trends are "very similar" (i.e., parallel exponentials with doubling every ~3–4.5 months post-2024, ~7 months long-run), so the 80% horizon lags the 50% horizon by a roughly stable factor. But METR measures software/research tasks, never measured a 99% horizon, and independent work (Princeton/Narayanan, Feb 2026; AI Index 2026; International AI Safety Report 2026) finds that capability gains have produced only small reliability gains and agents still fail ~1 in 3 structured tasks — so the horizon trend does not translate into the 99%+ per-order reliability a purchasing use case needs.

### Cited Findings

**METR time-horizon data (independent nonprofit)**
- Original method (19 Mar 2025): frontier 50% time horizon doubling roughly every 7 months 2019–2025; fitting only 2024–2025 data shortens the arrival date for month-long tasks by ~2.5 years; "external validity concerns ... are responsible for the majority of our uncertainty"; absolute measurements being off by 10x shifts forecast arrival by only ~2 years. — [METR, 19 Mar 2025](https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks/)
- Time Horizon 1.1 (29 Jan 2026): doubling time 196.5 days for all models 2019–2025; 130.8 days [95% CI 107–161] for models since 2023 (vs 165.3 under TH1.0, ~20% faster); 88.6 days for models since 2024. 50% horizons: Claude Opus 4.5 320 min [170–729]; GPT-5 214 min; o3 121 min. Suite is 228 tasks "designed to capture skills required for research or software engineering"; only 5 of 31 tasks over 8 hours have measured human baselines; CIs "still very wide". — [METR TH1.1](https://metr.org/blog/2026-01-29-time-horizon-1-1/)
- METR's time-horizons page: "the trends for 50%- and 80%-time horizons are very similar"; "we haven't seen any evidence of the exponential growth in time horizon slowing down"; "Measurements above 16 hrs are unreliable with our current task suite" (added 8 May 2026); page "no longer actively updated" as of 8 Sep 2026; 99% horizon not measured "because it would need many more and shorter tasks"; results "don't directly generalize to all jobs or to messier, holistically scored work". — [METR time horizons](https://metr.org/time-horizons/)
- 80% horizons: baseline 1.5 hours (Gemini 3.1 Pro) at survey time (Apr–May 2026); Claude Mythos Preview 3h 06m [95% CI 1h 37m – 6h 39m] added 8 May 2026. — [Forecasting Research Institute LEAP](https://leap.forecastingresearch.org/forecasts/80-8-hour-task-horizon-1)
- Mythos Preview 50% horizon reported as ~17 hours; METR's Frontier Risk Report (May 2026, covering Feb–Mar 2026) put the public frontier at a 50% horizon of ~12 hours (5–61h) against an 80% horizon of ~1.5 hours (secondary summaries of METR). — [agitracker.io (secondary)](https://agitracker.io/blog/metr-time-horizons-2026/); [agentmarketcap.ai (secondary)](https://agentmarketcap.ai/blog/2026/04/24/metr-50-minute-agent-reliability-horizon-enterprise-task-design)
- Expert forecasts for the longest 80% horizon on 31 Dec 2026 (collected 20 Apr – 11 May 2026): experts median 3.4h, superforecasters 3.5h, public 3h; 25th/75th percentiles ~2.1h/5.5h; the Mythos result already sits at the median. — [LEAP](https://leap.forecastingresearch.org/forecasts/80-8-hour-task-horizon-1)
- AI Futures Project (AI 2027 authors) shortened its doubling-time assumption to 4 months (Kokotajlo) and 4.5 months (Lifland); a regression on the 21 models since 2024 gives ~3.5 months [3.2–3.8] (secondary tracker). — [AI 2027 Tracker (secondary)](https://ai2027-tracker.com/predictions/metr-doubling/)
- Critique noted in secondary coverage: the data "cannot tell exponential growth from faster-than-exponential growth", and 80% figures may overstate current capability. — [agitracker.io](https://agitracker.io/blog/metr-time-horizons-2026/)

**Reliability-specific evidence (independent)**
- Rabanser, Kapoor, Kirgis, Liu, Utpala, Narayanan, "Towards a Science of AI Agent Reliability" (arXiv 18 Feb 2026, v3 2 Jun 2026, ICML 2026): 12 metrics across consistency, robustness, predictability, safety; 15 models on two benchmarks; "recent capability gains have only yielded small improvements in reliability"; a single success metric "obscures critical operational flaws". — [arXiv 2602.16666](https://arxiv.org/abs/2602.16666)
- Stanford AI Index 2026, Technical Performance chapter: agents "still fail roughly one in three attempts on structured benchmarks"; OSWorld accuracy rose from ~5% (2024) to 66.3% (2025) vs human baseline 72.35%; evaluations "are being outpaced by the progress they were built to measure". — [AI Index 2026 ch. 2](https://hai.stanford.edu/assets/files/ai_index_report_2026_chapter_2_technical.pdf)
- International AI Safety Report 2026: "even leading AI agents are still sufficiently unreliable to pose risks and hamper deployment in many contexts". — [arXiv 2602.21012](https://arxiv.org/pdf/2602.21012)

**Shopping-specific benchmarks**
- WebArena leaderboard (2026): top overall score 74.3% (WebTactix / DeepSeek V3.2) vs human 78.24%; scores vary with scaffolding, observation mode and retry budget; the shopping subset shows best rows at 38.9–42.9% (May 2026 update). — [steel.dev WebArena leaderboard](https://leaderboard.steel.dev/leaderboards/webarena/)
- WebShop (purchase-specific): metric is "fraction of instructions where the agent buys a fully matching product"; human experts 59%, best trained agent 29% (older generation agents; imported May 2026). — [benchmarklist.com WebShop](https://benchmarklist.com/benchmarks/webshop/)
- A 2026 arXiv planning paper reports WebArena baselines around 38%, illustrating setup sensitivity. — [arXiv 2603.12710](https://arxiv.org/pdf/2603.12710)

**Compounding-error arithmetic (practitioner blogs, no primary study)**
- Widely repeated: 95% per-step success over 10 sequential steps gives ~60% end-to-end, ~36% at 20 steps; errors are correlated across LLMs so independence is optimistic. No primary study found; the "63% of multi-step tasks fail" statistic has no traceable source. — [tianpan.co](https://tianpan.co/blog/2026/04/20/compound-accuracy-multi-step-agent-pipelines); [lenshq.io](https://lenshq.io/blog/ai-agent-compounding-errors-math)

### Inferences
- Ratio: from published pairs (Mythos ~17h/3.1h ≈ 5.5x; Feb–Mar 2026 frontier 12h/1.5h = 8x) the 80% horizon lags the 50% horizon by roughly 5–8x, i.e., 2.5–3 doublings or ~8–14 months at post-2024 doubling rates. METR's statement that the trends are "very similar" supports treating the factor as roughly stable.
- A human completes a routine multi-step purchase in ~2–10 minutes; the 80% horizon already exceeds that by >10x, so on METR's task distribution an agent "should" complete a purchase with >80% reliability today. That is consistent with WebArena shopping subsets in the 40% range only if (a) web/commerce tasks are messier than METR's software tasks, and (b) the binding constraint is the tail, not the median. Both are what METR's own caveats and the Rabanser et al. finding predict.
- Purchasing needs something like a 99%+ per-order horizon, which METR explicitly did not measure. If the 99% horizon lags the 80% horizon by a similar multiplicative factor, the tail reliability for even short tasks is years behind the headline numbers, and there is no published trend to anchor on.
- The headline "no slowing" claim is about the 50% horizon of software tasks; the independent reliability literature (Feb–Jun 2026) says consistency/robustness improves slowly with capability. These are not contradictory: a model can do longer tasks half the time while still failing short tasks a few percent of the time.

### Gaps
- METR has not published a 99% horizon or an explicit 50%/80% ratio statement with CIs; the ratio above is computed from secondary summaries of two data points.
- Epoch AI: no Epoch-specific agent-reliability or horizon figures surfaced in searches; nothing cited.
- No benchmark measuring live-site checkout accuracy (right item/variant/quantity/address/price) was found; WebArena/WebShop are sandboxed.
- Quantified results of Rabanser et al. (specific metric values) are on the paper's dashboard, not the abstract page fetched.

---

## Key Question 3: Is there evidence of long-tail reliability plateaus, and how did companies handle residual error?

### Takeaway
The best-documented automation (robotaxis) shows continued improvement, not a hard plateau (Waymo ~19,000 miles per disengagement in 2025, up on 2024; Zoox ~60,000), but with improvement rates described as "logarithmic, not exponential" and with a permanent human-assistance layer (Waymo: ~70 remote agents on duty for ~3,000 vehicles, ~1:43) that handles the residual tail and whose intervention frequency the company declines to disclose. Across RPA, self-checkout and agentic checkout, residual error was handled by adding human review, attendants and receipt checks, or by pulling the automation back behind a human-confirmed step, not by removing oversight.

### Cited Findings

**Autonomous vehicles**
- California DMV disengagement data for Dec 2024–Dec 2025 (reported 25 Feb 2026): Waymo 19,234 miles per disengagement (improved vs 2024); Zoox 60,682 (improved); Nuro 646 (worse than 2,044 in 2024). — [Consumer Watchdog, 25 Feb 2026](https://consumerwatchdog.org/accountability/new-ca-dmv-numbers-show-uber-robotaxi-partner-nuro-could-not-go-700-miles-without-human-intervention/)
- A 2026 risk-assessment paper reports year-over-year improvement factors of 1.45x–2.79x and describes them as "logarithmic, not exponential"; it cites Waymo at one disengagement per 60,000 miles with wide confidence intervals. — [arXiv 2606.06396](https://arxiv.org/pdf/2606.06396)
- Mileage-normalised DMV analysis: disengagements per 1,000 km fell from 0.12 (2018) to 0.04 (2022), ~16.7% reduction per year of cumulative testing; disengagement reports are inconsistent across manufacturers and may fall as safety drivers trust the system more "even if the underlying system has not changed". — [ResearchGate: Trends in AV Performance](https://www.researchgate.net/publication/390364925_Trends_in_Autonomous_Vehicle_Performance_A_Comprehensive_Study_of_Disengagements_and_Mileage)
- A 2026 study of DMV reports finds a significant declining trend in ADS-initiated disengagements; software/hardware failures remain the main cause of system-initiated takeovers. — [Traffic Injury Prevention, 2026](https://www.tandfonline.com/doi/full/10.1080/15389588.2026.2656715)
- Waymo's 17 Feb 2026 response to Sen. Markey: ~70 remote assistance agents on duty at any time for a fleet of ~3,000 vehicles (~1:40–1:43); roughly half located in the Philippines; a US-only Event Response Team handles collisions and law-enforcement interactions; remote operators "provide guidance ... but do not directly control, steer or drive"; the vehicle calls for help "in an ambiguous situation in which it may benefit from more context, even if the ADS can confidently proceed". — [Waymo letter to Sen. Markey, 17 Feb 2026](https://assets.ctfassets.net/7ijaobx36mtm/7E5uOzS5F7Z1yuFoz27BIc/680a27f89a3aae48977db655a5f45005/Sen._Markey_RA_Letter_Waymo__Response.pdf); [Forbes, 17 Feb 2026](https://www.forbes.com/sites/bradtempleton/2026/02/17/waymo-overseas-human-assist-wasnt-secret-but-is-it-secure/)
- Waymo and Tesla did not answer how frequently remote operators step in per mile or per trip; Waymo says it keeps internal metrics. — [Markey investigation report](https://www.markey.senate.gov/imo/media/doc/remote_assistance_investigation_report.pdf); [Austin American-Statesman](https://www.statesman.com/business/article/tesla-waymo-remote-operators-self-driving-cars-22192151.php)
- NTSB: in January 2026 a Waymo in Austin asked a remote agent whether a school bus was loading and the remote worker wrongly told it to proceed — the human confirmation layer itself erred. — [TechCrunch, 25 Mar 2026](https://techcrunch.com/2026/03/25/waymo-robotaxi-roadside-assistance-emergency-first-responders)

**RPA**
- Rule-of-thumb only: vendors say an exception rate above ~20% means the bot "will need constant supervision" / costs more than it saves; 30–50% of RPA programs underperform (EY 2016, still the most-cited figure as of 2026); a practitioner estimate that automation effort is "20% happy-flow and 80% exception handling". No peer-reviewed per-run exception-rate study found. — [Moxo (vendor)](https://www.moxo.com/blog/human-in-the-loop-rpa); [Flobotics (vendor)](https://www.flobotics.io/blog/rpa-successs-metrics); [Medium practitioner post](https://medium.com/intelligent-automation-journal/why-so-many-rpa-projects-fail-8db4dff5633b)
- One vendor case study: RPA bots at a 15% exception rate vs AI agents at 4% on healthcare claims (illustrative, unverified). — [AI Crescent (vendor)](https://www.ai-crescent.com/blog/ai-agents-vs-diy-rpa-2025)

**Self-checkout**
- Dollar General pulled self-checkout from almost 10,000 stores (Mar 2024) to reduce theft; Walmart removed it at some locations and limited it at others; Target capped express self-checkout at 10 items (2024) and later disputed reports that it changed policy over shrink; Costco added staff to self-checkout areas; Safeway added receipt-scanning gates in California. — [Fortune, 15 Mar 2024](https://fortune.com/2024/03/15/dollar-general-self-checkout); [CBS News](https://www.cbsnews.com/news/walmart-self-checkout-target-dollar-general-costco/)
- Self-checkout use among small and midsize retailers fell from 43% to 36%, with efficiency and theft cited. — [Yahoo/AT&T Currently](https://currently.att.yahoo.com/att/stores-reversing-course-self-checkout-150636102.html)
- A widely repeated "3.5% shrink at self-checkout vs 0.21% at staffed lanes" figure could not be traced to a primary source. — [The Coconut Mama (unreliable)](https://thecoconutmama.com/self-checkouts-collapse-accelerates-as-10-major-chains-rip-out-kiosks/)

**Agentic checkout**
- OpenAI's March 2026 retreat: Instant Checkout "prone to errors", stale stock/price/shipping data, 3x lower conversion than redirect; resolution was to push checkout back to the merchant's site. — [CNBC, 20 Mar 2026](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html)

### Inferences
- The AV data is the best natural experiment: a decade of improvement, roughly 19k–60k miles between disengagements, and yet the operator maintains a standing human layer that is consulted even when "the ADS can confidently proceed". The human layer did not disappear as the error rate fell by orders of magnitude; it changed shape (safety driver in seat → remote fleet response at ~1:43) and moved off the critical path for routine cases.
- Self-checkout is the clearest case of a verification layer being added after deployment because residual error (here, theft) was discovered to be costly; retailers did not remove automation, they added attendants, item caps and receipt gates.
- For agents, the only live post-mortem shows the error mode was data/transaction enablement, which platforms solved by handing the final step back to the merchant — a human-adjacent confirmation point, not a removal of oversight.

### Gaps
- No published Waymo remote-assistance request rate per mile/trip (explicitly withheld, per the Markey investigation).
- No rigorous, independent RPA exception-rate dataset; only vendor rules of thumb.
- No primary self-checkout shrink-rate data; retailer statements are qualitative.
- OCR/speech last-mile plateau evidence was not researched (out of call budget); not cited.

---

## Key Question 4: In the analogies, did the verification/evidence layer disappear when the underlying error rate dropped?

### Takeaway
In every analogy found, when the underlying error or fraud rate fell, the **confirmation** component became risk-based and mostly invisible (3DS2 frictionless flows and SCA exemptions; Waymo calling for help only when ambiguous) while the **evidence/audit** component persisted or became the core product (e-signature Certificates of Completion; SEC Rule 15c3-5 pre-trade controls still enforced in 2025; CNP fraud tools that grew after EMV cut counterfeit fraud by ~76%; chargeback-guarantee fraud vendors whose revenue kept growing, if slowly). The pattern is "confirmation shrinks to exceptions, proof becomes a compliance/insurance norm" — not "the layer disappears". Agentic-commerce rulemaking is already following this pattern: networks solved agent fraud with tokens/identity, left agent-error liability unresolved, and advise merchants to keep signed intent records.

### Cited Findings

**EMV chip migration and card-not-present fraud tools**
- Visa (Mar 2018 update): merchants that completed the chip upgrade saw a 76% decline in counterfeit fraud dollars Dec 2015–Dec 2017; a separate Visa figure of 70% through Sept 2017. — [TechTarget](https://www.techtarget.com/cybersecurity/news/252435920/Visa-reports-EMV-chip-cards-thwart-fraud-but-criminals-adapting); [Commerce Bank](https://www.commercebank.com/business/trends-and-insights/2019/emv-cards-are-resulting-in-a-76-percent-drop-in-card-present-fraud)
- Federal Reserve Bank of Kansas City (Fumiko Hayashi, 21 May 2025, independent): US CNP debit fraud rates (dual-message networks) 14.3 bp (2013) → 16.9 bp (2015), rising <2 bp 2015–17 and >2 bp 2017–19; the US did not see the CNP surge other countries saw after EMV, partly because networks, issuers and merchants "had already adopted CNP mitigation tools"; Pulse industry average debit CNP rate 26.1 bp (2019) → 41.6 bp (2023). Australia and France saw CNP fraud rise after EMV and stayed above 20 bp until 2019/2020, reduced via strong customer authentication, tokenization, ML fraud models and real-time monitoring. — [KC Fed, 21 May 2025](https://www.kansascityfed.org/research/payments-system-research-briefings/card-not-present-fraud-rates-in-the-united-states-after-the-migration-to-chip-cards/)
- KC Fed earlier briefing: counterfeit fraud rate declined 2015–2021 only on single-message networks; cardholder fraud loss rates show "an upward trend ... for both card-present and card-not-present transactions". — [KC Fed](https://www.kansascityfed.org/research/payments-system-research-briefings/new-data-on-card-present-and-card-not-present-fraud-rates-in-the-united-states/)

**3DS2 / SCA: did authentication shrink or expand?**
- Ravelin (vendor, 19 Mar 2026, 36–37 countries): frictionless rates declined in 76% of countries 2025→2026, 28 of 37 "declining or plateauing"; global challenge success rate "remained stable"; US 3DS success rate rose 47% YoY to 88% in 2026; UK 95%, Brazil 36%; 78% of merchants now use SCA exemptions, up YoY; 51% of British merchants would like PSD2 SCA abolished. — [Ravelin 3DS rates 2026](https://www.ravelin.com/blog/3d-secure-rates-2026)
- Ravelin Jan–Jul 2025: global challenge success 76%, frictionless 58%; Europe 75%/62%; North America 85%/54%. — [Ravelin country comparison](https://www.ravelin.com/blog/global-3d-secure-rates-country-data-comparison)
- Stripe (vendor marketing): "20% of challenge flows lead to abandonment"; Stripe's analytics define a failed challenge as customer abandonment or bank rejection. — [Stripe authentication](https://stripe.com/payments/authentication); [Stripe docs](https://docs.stripe.com/payments/analytics/authentication)
- Mastercard projection via Checkout.com (vendor): ~90% of 3DS2 transactions should proceed without a challenge under risk-based authentication. — [Checkout.com](https://www.checkout.com/blog/3-d-secure-2-0-explained)
- Vendor benchmark, untraceable to primary: abandonment "dropped from 15–25% in 3DS1 to 2–5% in 3DS2"; challenges "reduce checkout conversion by 3–8% relative to frictionless". — [gr4vy (vendor)](https://gr4vy.com/posts/what-is-3d-secure-3ds2-a-complete-merchant-guide-for-2026/); [2accept (vendor)](https://www.2accept.net/blog/challenge-vs-frictionless-3ds2)

**Fraud-prevention vendors (Signifyd, Riskified, Forter) as fraud rates changed**
- Riskified: founded 2012; IPO 28 Jul 2021 at $4.3B; 2023 revenue $297.6M; backs approved transactions with a "100% money-back guarantee" (insurance model). — [Wikipedia](https://en.wikipedia.org/wiki/Riskified)
- Riskified FY2025 revenue $344.64M, +5.23%; Q1 2026 +7.14% (third-party analysis, not the 20-F). — [Koala Gains (secondary)](https://koalagains.com/stocks/NYSE/RSKD/future-performance)
- Forter: no published revenue; estimates conflict (GetLatka ~$103M 2024 vs Sacra ~$200M 2021); last valued at $3B; PayPal partnership Mar 2025; Signifyd–Experian joint product May 2025. — [GetLatka (estimate)](https://getlatka.com/companies/forter#competitors); [Sacra](https://sacra.com/c/forter/); [OpenPR market note](https://www.openpr.com/news/3818772/e-commerce-fraud-prevention-tools-market-growing-trends)
- Market-size estimates diverge by scope: $11.17B (2025, 10.6% CAGR to 2035) vs $73.5B (2025, 20.7% CAGR to 2030); both are low-quality market-research-mill figures. — [WiseGuy Reports](https://www.wiseguyreports.com/reports/ecommerce-fraud-prevention-tools-market); [TBRC](https://www.thebusinessresearchcompany.com/report/ecommerce-fraud-detection-and-prevention-global-market-report)

**E-signature audit trails**
- DocuSign (vendor): "In any legal dispute, the Certificate of Completion is the audit trail that would be required in court"; records signer email, timestamps, IP; under E-SIGN, UETA and eIDAS, e-signatures backed by audit trails are admissible; the certificate does not prove who was physically at the device. — [DocuSign blog](https://www.docusign.com/blog/are-electronic-signatures-admissible-in-court); [DOCUJET (vendor)](https://docujet.it/en/blog/electronic-signature-audit-trail)
- For a simple electronic signature "the audit trail is often the main evidence of who signed"; a plain signature image without a trail is weak evidence. — [Loresign (vendor)](https://loresign.com/blogs/e-signature-audit-trail-guide)

**2FA / passkeys**
- 2025 passkey adoption: 69% of users have at least one passkey (Authsignal, vendor); Microsoft made passkeys default in May 2025. — [Authsignal](https://www.authsignal.com/blog/articles/passwordless-authentication-in-2025-the-year-passkeys-went-mainstream)
- Descope (vendor) expects passkeys to be enabled "alongside passwords and MFA paths rather than completely replace" them, with risk-based signals layered on top "for step-up scenarios, like sensitive transactions and new devices". — [Descope](https://www.descope.com/blog/post/passkey-trends)

**Algorithmic trading kill switches / pre-trade controls**
- SEC Rule 15c3-5 (adopted 2010) remains in force and enforced: a 2025 SEC order found Liquidnet lacked adequate market-access risk controls; FINRA's 2025 oversight report lists findings against firms without pre-trade order limits or capital thresholds; a 2025 NYSE filing states exchange kill-switch and pre-trade controls "supplement, and not replace" Rule 15c3-5 obligations. — [SEC order 2025](https://www.sec.gov/files/litigation/admin/2025/33-11351.pdf); [FINRA 2025 report](https://www.finra.org/rules-guidance/guidance/reports/2025-finra-annual-regulatory-oversight-report/market-access-rule); [SEC/NYSE filing 2025](https://sec.gov/files/rules/sro/nyse/2025/34-102753.pdf)

**Agentic commerce: what the networks did and did not settle (2025–2026)**
- Worldpay (vendor, 2026): when an agent authenticates and a token is issued correctly, existing tokenized-transaction rules apply (issuer carries fraud risk; chargeback rights intact); for non-fraud disputes ("agent misread an instruction, bought the wrong item, or exceeded its authority") "no clean answer exists yet"; Reg E "assumes a transaction was either authorized or not, with no framework for an agent that buys the wrong thing"; EMVCo's task force is exploring changes. Merchants should keep a cryptographic record of what the shopper asked (Verifiable Intent, AP2), agent identity, and instruction linkage; without a signed mandate an agent-vs-shopper dispute is "a coin flip" and issuers tend to side with the cardholder; merchant-set rules such as a spend cap triggering a human step-up are the only confirmation example given. — [Worldpay](https://worldpay.com/en/insights/articles/agentic-commerce-liability-is-still-being-written)
- Banks & Bankers (independent opinion, ~Aug 2026): Visa's zero-liability guarantee reportedly extends to AI-initiated transactions; Mastercard issuer carries fraud liability when an Agentic Token is validly issued; "Neither network has published a binding chargeback rule for agent-error disputes"; one unnamed specification reportedly assigns such losses to the consumer by default. — [Banks & Bankers](https://banksandbankers.com/agentic-commerce-liability-gap-fraud-vs-agent-error/)
- AP2 spec: Cart Mandate for human-present purchases (merchant signs, user signs); Intent Mandate for human-not-present (user pre-approves conditions); in autonomous mode the shopping agent "MAY now sign it using its Agent Key instead of getting that approval"; the Payment Mandate carries a human-present/not-present flag so the issuer can see whether a person was involved; mandates are "secure, immutable JSON blobs which are proof of what was agreed to" and are sent to the dispute adjudicator with the cart hash. — [AP2 specification](https://ap2-protocol.org/ap2/specification/); [AP2 core concepts](https://ap2-protocol.net/en/topics/core-concepts); [Cloud Security Alliance, 6 Oct 2025](https://cloudsecurityalliance.org/blog/2025/10/06/secure-use-of-the-agent-payments-protocol-ap2-a-framework-for-trustworthy-ai-driven-transactions)
- Critique: AP2 has no in-protocol way for a user to revoke an Intent Mandate before TTL expiry; one analyst argues mandates "do not survive beyond the transaction and agent that created them". — [AURA Labs (independent)](https://marcmassar.substack.com/p/agentic-protocol-fragility)

### Inferences
- EMV is the cleanest "accuracy improved, verification layer grew" case: counterfeit fraud fell ~76% at upgraded merchants, fraud migrated to CNP, and the CNP verification/scoring industry (3DS2, tokenization, ML scoring, chargeback-guarantee vendors) expanded rather than shrank. The KC Fed notes the US avoided the post-EMV CNP spike precisely because CNP tools were already deployed — the verification layer was the reason the error rate stayed low, not something made redundant by it.
- 3DS2 shows the shape confirmation takes at maturity: the challenge is retained as a legal/liability instrument (SCA mandate, liability shift) but routed to a minority of transactions by risk; and Ravelin's 2026 data shows frictionless shares falling again, i.e., issuers re-expanding challenges when risk rises. Confirmation is a dial, not a stage that gets removed.
- Fraud-vendor growth (Riskified +5–7% in 2025–26) shows the insurance/guarantee layer persisting with slow growth once fraud rates stabilise — a mature compliance line item rather than a disappearing one.
- E-signature and 15c3-5 show that where an action is cheap and automated, the evidence/control layer becomes the regulated or litigated artifact; it does not become less important as signing/trading got easier.
- Agentic-commerce rules as of 2026 already encode this split: fraud (identity) is solved by tokens; agent error (judgment) is left to merchants, who are told to keep signed intent records. That is a direct statement by a major acquirer that the evidence layer is where merchant value lives.

### Gaps
- Stripe Radar growth as fraud rates fell: no data found.
- Signifyd revenue: no reliable figure (GetLatka $292.8M is an unverified estimate).
- AV insurance treatment (how insurers price robotaxi liability vs human-driver confirmation): not found within call budget.
- Whether any agent-error chargeback reason code has been published by Visa/Mastercard as of Nov 2026: both sources say no, but both are secondary/opinion; not verified against network rulebooks.
- OCR/speech and MiFID II kill-switch specifics not researched.

---

## Key Question 5: What is the economic threshold — at what per-order error rate does a confirmation step cost more in lost conversion than it saves?

### Takeaway
No published model of a break-even agent error rate exists in the sources found. The only usable inputs are checkout-friction figures of mixed quality: Stripe's marketing claim that 20% of 3DS challenge flows are abandoned, vendor estimates that challenges cut conversion 3–8% versus frictionless, Ravelin's measured 24% global challenge failure (Jan–Jul 2025, includes bank rejections), Walmart's finding that in-chat checkout converted 3x worse than redirect, and Baymard-derived figures that 19–24% of abandoners cite forced account creation. Using these as the cost of a confirmation step, a simple break-even (below, labelled as inference) implies confirmation on every order only pays when the per-order error rate is in the low single digits or the cost of an erroneous order is many multiples of margin — which is why mature systems (3DS2, Waymo) route confirmation to a risk-selected minority of cases and keep the evidence layer on all of them.

### Cited Findings
- Baymard running average cart abandonment ~70% (70.19–70.22% across 49–50 studies, updated Sep 2025); 19% (one restatement) or 24% (another) of abandoners cite forced account creation; 19% abandoned in the past 3 months because they did not trust the site with card details. All figures are secondary restatements of Baymard. — [Growthegy (secondary)](https://www.growthegy.com/2026/05/26/cart-abandonment-science-why-customers-leave-checkout/); [Baymard checkout friction audit](https://baymard.com/blog/audit-checkout-flow-hidden-friction)
- Zuko Shopify checkout research (Jul 2025–Mar 2026): password fields carry 10.5% abandonment; a failed discount code drops checkout completion from 94% to 35%. — [Zuko](https://www.zuko.io/blog/25-conversion-rate-statistics-you-need)
- "4.1% conversion drop per additional form field" is attributed to a 2024 HubSpot study and could not be verified; one form-analytics provider found no relationship between field count and completion. — [blog.mean.ceo (secondary)](https://blog.mean.ceo/checkout-form-field-statistics/); [Zuko](https://www.zuko.io/blog/25-conversion-rate-statistics-you-need)
- Stripe: average 2024 checkout had 11.3 form fields; most purchases can be completed with 8 (no per-field conversion figure). — [Stripe guide](https://stripe.com/en-hu/resources/more/streamlined-checkout-processes-how-to-boost-conversions-with-an-easier-checkout-flow)
- Stripe marketing: 20% of 3DS challenge flows lead to abandonment. — [Stripe authentication](https://stripe.com/payments/authentication)
- Ravelin: global challenge success 76% (Jan–Jul 2025) → 24% of challenges fail (abandonment or bank rejection, not separated). — [Ravelin](https://www.ravelin.com/blog/global-3d-secure-rates-country-data-comparison)
- Vendor estimates: challenges reduce conversion 3–8% vs frictionless; 3DS2 abandonment 2–5% vs 15–25% for 3DS1 (untraceable to primary). — [2accept (vendor)](https://www.2accept.net/blog/challenge-vs-frictionless-3ds2); [gr4vy (vendor)](https://gr4vy.com/posts/what-is-3d-secure-3ds2-a-complete-merchant-guide-for-2026/)
- Walmart: conversion for products sold directly in ChatGPT was three times lower than sending users to Walmart's site (reported Mar 2026). — [CNBC](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html)
- Worldpay: without a signed intent record an agent-vs-shopper dispute is "a coin flip" and issuers tend to side with the cardholder; the cost of the dispute lands on the merchant. — [Worldpay](https://worldpay.com/en/insights/articles/agentic-commerce-liability-is-still-being-written)
- Vendor guide: after a wrong agent order "the cost stays with the store that sold the item" (exchanges, support). — [eesel.ai (vendor)](https://www.eesel.ai/blog/chatgpt-instant-checkout-2026)

### Inferences
- Break-even model (my construction, not from a source). Let *e* = per-order probability the agent places a materially wrong order; *L* = merchant loss per wrong order (refund/return shipping/support/chargeback fee/lost margin); *c* = fractional conversion loss caused by inserting a confirmation step; *m* = contribution margin per completed order; *r* = fraction of wrong orders the confirmation actually catches. Confirming every order pays when *e × L × r > c × m*, i.e., break-even error rate *e\* = c × m / (L × r)*. Illustration using cited inputs only: if *c* = 3–8% (vendor 3DS challenge estimate) or 20% (Stripe challenge-abandonment claim), *r* ≈ 0.8, and *L* is 2–5x margin (a returned, restocked, refunded order plus support), then *e\** ≈ 0.75–5% at *c* = 3–8% and ≈ 5–12% at *c* = 20%. Current shopping-subset benchmark failure rates (~55–60% on WebArena shopping; ~1 in 3 on structured benchmarks per AI Index) are far above any of these thresholds; the thresholds would be crossed only once live error rates fall to low single digits.
- The same arithmetic favours risk-based rather than universal confirmation: if the merchant can route confirmation to the top-decile risk orders (as 3DS2 and Waymo do), the conversion cost *c* applies to ~10% of orders while most of the error mass is still intercepted, which keeps the step economic at much lower *e*.
- The evidence layer has *c* ≈ 0 (it is invisible to the shopper), so its break-even is set only by dispute economics; Worldpay's "coin flip" characterisation implies each disputed wrong order without proof costs the merchant roughly half the dispute value plus fees, so the proof layer pays at any non-zero error rate as long as disputes occur. That is consistent with every analogy in Question 4.
- Walmart's 3x conversion gap for in-chat checkout suggests the binding friction today is the agent surface itself, not a confirmation step; a merchant-side confirmation that redirects to a trusted surface may currently *raise* conversion relative to an opaque in-agent purchase.

### Gaps
- No published break-even or ROI model for human confirmation on agent purchases found from any analyst, PSP, or academic source.
- No measured OTP/one-time-code abandonment rate at checkout found (searches returned only generic form-friction figures).
- No primary Baymard, Stripe "State of checkouts", or Dynamic Yield per-step conversion data retrieved; all per-step figures are secondary or vendor claims.
- No merchant-reported cost per erroneous agent order (returns, support, chargeback fees) found; *L* in the model above is assumed, not sourced.
- 3DS challenge-abandonment data separated from bank rejections is not public (Ravelin measures success, not abandonment).
