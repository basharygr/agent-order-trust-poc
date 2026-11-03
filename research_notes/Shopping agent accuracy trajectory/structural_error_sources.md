# Structural vs. model-reducible sources of AI shopping-agent purchase errors

As-of date: 3 November 2026. Every figure is dated to its source. "Measured" = reported by the study/company; "author estimate/inference" is labelled as such. Where a figure comes only from secondary coverage, that is flagged.

Working taxonomy used throughout: (1) model-capability errors; (2) intent-ambiguity errors; (3) merchant-data errors; (4) human-behaviour errors; (5) adversarial errors.

---

## KQ1. What do agent benchmarks and simulation studies say about the breakdown of failures by cause? Does any paper estimate the fraction due to ambiguous instructions vs model error?

### Takeaway
Every shopping-agent failure analysis published between 2024 and Sept 2026 attributes the majority of failures to model capability (attribute mismatch, unverified attributes, buying without confirming details, poor search), with the share attributable to under-specified user intent ranging from ~10% (benchmark authors' own labelling of tau-bench: 4 of 40 failures) to 33% (independent AgentRx re-labelling of tau-bench). The model-capability share is visibly shrinking across model generations (e.g. ShoppingBench 36.6% -> 48.2% GPT-4o -> GPT-4.1; attribute-hallucination rate ~0.25 on older open-weight models vs ~0.055 on newer ones), whereas multi-turn clarification performance is stuck around 35% even after training.

### Cited Findings

**ShoppingBench (arXiv 2508.04266, v2 Aug 2025; submitted to AAAI 2026)**
- Benchmark: 3,310 user instructions, sandbox of 2,746,368 unique products; 900-instruction test set — [ShoppingBench HTML](https://arxiv.org/html/2508.04266v2)
- Absolute success rates (domain-weighted, Aug 2025): GPT-4.1 48.2%; DeepSeek-R1 and o3-mini 39.2%; Claude-4-Sonnet 39.0%; GPT-4o 36.6%; Qwen2.5-max 35.9%; Gemini-2.5-Flash 35.4%; GPT-4o-mini 20.0%; authors' SFT+RL Qwen3-4B 48.7% — [ShoppingBench HTML](https://arxiv.org/html/2508.04266v2)
- Failure analysis sampled 60 failed GPT-4.1 trajectories; five categories: Attribute Mismatch, Metric Issue, Product Missing, Constraint Not Satisfied, Knowledge Error. Paper states "The largest proportion of failures is due to missing or mismatched product attributes." Exact counts are only in Figure 4 (not extractable as text) — [ShoppingBench HTML](https://arxiv.org/html/2508.04266v2)
- "Metric Issue" is explicitly an evaluation artefact: the evaluator rejects a correct product because its label is too strict (e.g. "high waist" vs "high"). A case study counts the agent substituting black for an unavailable "twist" colour as a failure, i.e. the benchmark penalises substitution rather than treating it as merchant-stock error — [ShoppingBench HTML](https://arxiv.org/html/2508.04266v2)
- Paper does not discuss ambiguous/underspecified intent as a failure cause — [ShoppingBench HTML](https://arxiv.org/html/2508.04266v2)

**ShopSimulator (arXiv 2601.18225, Jan 2026; accepted ACL 2026)**
- 28K tasks (25K train / 2.8K eval), 1.34M-product catalogue, 4,726 personalised user profiles — [ShopSimulator HTML](https://arxiv.org/html/2601.18225)
- Full-success rate (Jan 2026): GPT-5 32.65% overall (Single-Turn 40.78%, Single-Turn+Personalisation 36.41%, Multi-Turn 29.29%, Multi-Turn+Pers 24.13%); Claude-4-Sonnet 31.52% overall, best Multi-Turn at 38.25% — [ShopSimulator HTML](https://arxiv.org/html/2601.18225)
- Error analysis of Claude-4-Sonnet failures: BuyNow errors — "nearly 80% of errors resulting from purchases without confirming details" (incl. purchasing after user rejection); AskShopper — "failing to inquire when information is missing (9.40%)"; Click — dominant error is violating explicit user constraints; Search — ignored key attributes, abandoned high-match results, repeated queries — [ShopSimulator HTML](https://arxiv.org/html/2601.18225)
- Personalisation failures: partial omission of personalisation info 55.82%; over-interpretation 35.71%; confusing immediate needs with long-term preferences 8.79% — [ShopSimulator HTML](https://arxiv.org/html/2601.18225)
- Simulated-shopper-side errors (user-simulator noise): less than 8% of failures — [ShopSimulator HTML](https://arxiv.org/html/2601.18225)
- Multi-turn tasks start with a deliberately vague goal revealed only on questioning; "sustained clarification is harder for current LLMs than interpreting static preference constraints"; multi-turn success "stays around 35% even after training" — [ShopSimulator HTML](https://arxiv.org/html/2601.18225)

**Shopping Companion (arXiv 2603.14864, Mar 2026)**
- 1,000 instructions (800 train / 200 test); preliminary zero-shot GPT-5 on 200 instances: 75.0% single-product, 54.0% add-on deals, 64.5% average; 69 of 200 trajectories failed — [Shopping Companion HTML](https://arxiv.org/html/2603.14864)
- "two largest categories are Unverified Attributes and Same-Shop Alignment"; Unverified Attributes = recommending products "without verifying their key attributes or seller information against user requirements"; also "Preference Hallucination" and "insufficient memory inspection". Counts are in Appendix F.2 (not extracted) — [Shopping Companion HTML](https://arxiv.org/html/2603.14864)

**Evaluating Open-Weight E-Commerce Agents with Environment-Grounded Verification (arXiv 2609.16093, submitted 14 Sep 2026; NeurIPS 2026 listing)**
- 8 open-weight agents (20B–35B), 160 trials each (100 shopping, 50 grocery, 10 refusal probes) = 1,280 trials; 44 metrics — [arXiv 2609.16093 HTML](https://arxiv.org/html/2609.16093); [NeurIPS 2026 listing](https://neurips.cc/virtual/2026/173062)
- "Exact-cart success ranges from 0.30 to 0.57" (Sep 2026) — [arXiv 2609.16093 HTML](https://arxiv.org/html/2609.16093)
- Per-agent diagnostic rates (Table 3, Sep 2026): attribute hallucination 0.236 (GPT-OSS-20B), 0.252 (Qwen3-30B) vs ~0.050–0.063 for the other six (Gemma4-26B/31B, Qwen3.5-35B/27B, Qwen3.6-35B/27B); phantom actions 0.050–0.158; numeric hallucination 0.011–0.045; bad-search 0.291–0.434 — [arXiv 2609.16093 HTML](https://arxiv.org/html/2609.16093)
- Under-action: Qwen3-30B exhausts turn budget in 63% of trials; over-purchase example: 28 distinct products added for a 15-SKU target (precision 0.344); paper does not report an over-purchase rate — [arXiv 2609.16093 HTML](https://arxiv.org/html/2609.16093)
- Attribution: failures attributed to agent behaviour ("Catalog search returned candidates for every query"); authors caveat results "characterize controlled, tool-mediated catalog shopping rather than performance on live retail traffic" and the simulator "can therefore rescue a poor search" — [arXiv 2609.16093 HTML](https://arxiv.org/html/2609.16093)

**tau-bench (arXiv 2406.12045, Jun 2024)**
- Retail failure analysis: 115 sampled gpt-4o trajectories; 40 failed; 4 of the 40 attributed to "user instruction typo or ambiguity (and then fixed)" = 10% of failures were instruction defects; remaining 36 agent failures: ~55% "wrong argument or information provided", 25% "incorrect decision-making", 19% "partial resolution of compound requests" — [tau-bench HTML](https://arxiv.org/html/2406.12045)
- Reliability: gpt-4o retail pass^1 61.2; "pass^8 drops to <25%" (Jun 2024) — [tau-bench HTML](https://arxiv.org/html/2406.12045)
- Authors list three user-simulator limitations: instruction typos/ambiguities, missing domain knowledge, simulator LM reasoning/memory limits — [tau-bench HTML](https://arxiv.org/html/2406.12045)
- Atla's independent tau-retail analysis: "Wrong Information" given to user is the most common failure mode; workflow errors dominate — [Atla tau-bench post](https://atla-ai.com/post/t-bench)

**AgentRx (arXiv 2602.02475, Feb 2026) — independent root-cause labelling**
- Nine root-cause categories incl. "Under-specified User Intent" ("The agent was unable to complete the task due to lack of complete information at that point in the trajectory") — [AgentRx HTML](https://arxiv.org/html/2602.02475)
- Share of critical failures labelled Under-specified User Intent (Feb 2026): tau-bench 33.3% (largest category there; ~13 of 39), Flash 19%, Magentic-One 0%, RelWork 0%. Other tau-bench shares: Instruction/Plan Adherence 20.5%, Misinterpretation of Tool Output 20.5%, Intent–Plan Misalignment 15.4% — [AgentRx HTML](https://arxiv.org/html/2602.02475)
- Example: "The task instruction did not specify the type of black lamp and since there are multiple black lamps available" — [AgentRx HTML](https://arxiv.org/html/2602.02475)

**WebShop and derivatives**
- WebShop (arXiv 2207.01206, 2022): 1.18M real products, 12,087 instructions; best agent 29% task success vs human expert 59%, rule-based 9.6% — [WebShop abstract](https://arxiv.org/abs/2207.01206)
- HarnessX 100-session WebShop run (arXiv 2606.14249, Jun 2026): largest failure cluster "wrong product" 46%; colour matching 17%; attribute checks 17% (one system, not a benchmark-standard breakdown) — [HarnessX PDF](https://arxiv.org/pdf/2606.14249)
- WebShop-Clarification (arXiv 2606.19559, Jun 2026) deliberately underspecifies 50% of tasks to test whether agents trigger clarification — [Uncertainty Decomposition for Clarification Seeking](https://arxiv.org/pdf/2606.19559)
- ChatShop (arXiv 2404.09911): argues WebShop "gives the shopper's full goal at the start, which obscures the need for strategic information seeking" — [ChatShop](https://arxiv.org/pdf/2404.09911)
- LHAW (arXiv 2602.10525, Feb 2026, general not shopping): clarification "markedly improves results but rarely restores baseline performance"; performance degrades with increased underspecification — [LHAW](https://arxiv.org/pdf/2602.10525)

**ShoppingComp (arXiv 2511.22978, Nov 2025, open-web)**
- 145 instances / 558 scenarios; GPT-5.2 product-retrieval F1 17.76%; safety pass rate: ChatGPT DeepResearch 37.86%, GPT-5.2 35.42%, human experts 77.08% — [ShoppingComp HTML](https://arxiv.org/html/2511.22978v2)
- Identifies "implicit requirement inference as a dominant and previously underexplored failure mode"; user requests are "goal-oriented intents rather than explicit attribute constraints" — [ShoppingComp HTML](https://arxiv.org/html/2511.22978v2)

**Marketplace/choice-bias studies**
- Microsoft/ASU Magentic Marketplace (MSR-TR-2025-50, Oct 2025): "all models exhibit severe first-proposal bias, creating 10-30x advantages for response speed over quality"; "Almost 80 to 100% of the agents accepted the first proposal they received" — [Microsoft Research](https://www.microsoft.com/en-us/research/publication/magentic-marketplace-an-open-source-environment-for-studying-agentic-markets/)
- ACES (arXiv 2508.02630, Aug 2025): "agents exhibit strong position biases -- varying across providers and model versions, and persisting even in text-only interfaces"; agents "penalize sponsored tags while rewarding platform endorsements"; a seller-side agent making "simple, query-conditional description tweaks can drive significant gains in market share" — [ACES abstract](https://arxiv.org/abs/2508.02630)

### Inferences
- (Author inference) Across benchmarks the model-capability bucket is 55–90% of measured failures: tau-bench authors' labelling puts agent errors at 36/40 = 90%; AgentRx's stricter relabelling still leaves ~67% agent-side; ShopSimulator's <8% simulator noise and ~9% "failed to ask" leave >80% agent-side.
- (Author inference) The intent-ambiguity share is sensitive to who labels: 10% (tau-bench authors, who then fixed the instructions) vs 33% (AgentRx, external). Real-world instructions are not annotator-cleaned, so the real share is probably closer to the upper bound.
- (Author inference) Model-capability errors are shrinking with model generation: ShoppingBench GPT-4o-mini 20.0% -> GPT-4o 36.6% -> GPT-4.1 48.2% (Aug 2025); Shopping Companion GPT-5 64.5% (Mar 2026); newer open-weight models' attribute-hallucination rate (~0.055) is ~4x lower than older ones (~0.25). ShopSimulator's note that multi-turn clarification "stays around 35% even after training" suggests the ambiguity bucket is not shrinking at the same rate.
- (Author inference) "Buying without confirming details" (~80% of BuyNow errors in ShopSimulator) is the agent-side mirror of human rubber-stamping, and is exactly what the protocol confirmation gates in KQ5 are designed to catch.

### Gaps
- No paper gives a single clean "% of shopping-agent failures caused by ambiguous instructions"; the closest are tau-bench's 4/40 and AgentRx's 33.3%.
- ShoppingBench Figure 4 and Shopping Companion Appendix F.2 category counts could not be extracted from the HTML/PDF.
- No benchmark separates merchant-data errors (stale stock/price) as a failure category; all treat the catalogue as ground truth.

---

## KQ2. Human baseline: error, return and "ordered wrong thing" rates for human online shopping

### Takeaway
US online return rates are ~19% of online sales (NRF, Oct 2025) and fit/size/colour is the single largest return reason (~42%, Narvar; 72.6% in a Dec 2025 German sample), while merchant-side "item differed from listing/wrong item shipped" accounts for roughly a fifth to a third of returns depending on the survey. There is no modern dataset that cleanly splits returns into customer error vs merchant error; the only such split (Voxware, 2015) is a decade old and internally contradictory in press coverage.

### Cited Findings
- NRF/Happy Returns 2025 Retail Returns Landscape (released 15 Oct 2025): ~$849.9B expected returns = 15.8% of total annual sales (2024: 16.9%, $890B); online return rate 19.3% of online sales; 9% of all returns estimated fraudulent; 17% of holiday sales expected to be returned — [NRF press release](https://nrf.com/media-center/press-releases/consumers-expected-to-return-nearly-850-billion-in-merchandise-in-2025)
- Coresight estimate: US online apparel return rate 23.4% in 2025 (secondary citation) — [Branvas apparel returns report](https://branvas.com/blogs/news/apparel-returns-fit-issue-report)
- Narvar: fit, size or colour is the "number one driver" at 42% of e-commerce returns (secondary coverage; date of underlying Narvar study not confirmed) — [Cahoot summary of Narvar](https://www.cahoot.ai/top-return-reasons-and-solutions/)
- Narvar study: 58% of consumers intentionally buy more goods than they intend to keep (bracketing) (press coverage; date not confirmed) — [SalesTechStar](https://salestechstar.com/sales-marketing/narvar-study-finds-58-of-consumers-intentionally-buy-more-goods-than-they-intend-to-keep/)
- KPMG Germany (Dec 2025): "Wrong size: 72.6 percent" among online shoppers' top return reasons — [KPMG press release](https://kpmg.com/de/en/media/press-releases/2025/12/retouren-als-chance-kundenerwartungen-verstehen-huerden-verringern.html)
- Signifyd/Talker Research 2024 (US): wrong size or wrong item was the second most common refund-request reason — [Statista summary](https://www.statista.com/statistics/1611397/reasons-for-online-purchase-refund-request)
- Rithum 2025 Global Returns & Profit Impact Report: 33% of respondents returned a product because it didn't match its online photos or description; Syndigo 2025 State of Product Experience: 21% returned a product that didn't meet expectations set by product content (both secondary summaries) — [Chain Store Age](https://chainstoreage.com/inaccurate-product-information-hurts-online-sales)
- Voxware survey (Jan 2015, 500 consumers): 42% said size or colour was incorrect, 15% received the wrong product altogether; press framed this as "57% returned due to retailer error" but another outlet attributes the 57% to "item is correct, but they did not like the item" — contradictory coverage — [Retail TouchPoints](https://retailtouchpoints.com/topics/customer-experience/57-of-online-purchases-are-returned-due-to-retailer-error); contradicted by [SDCExec](https://www.sdcexec.com/warehousing/news/12039135/voxware-inc-most-often-the-reason-customers-return-items-bought-online-or-by-phone-is-because-the-item-received-is-incorrect)
- Fulfilment accuracy (vendor benchmarks, undated/2026 guides, not audited): whole-order accuracy 96–98% for high performers; pick accuracy targets 99.5%+ (top performers 99.9%+); mispick rate above 0.5% treated as a process problem — [Racklify order-accuracy benchmarks](https://racklify.com/encyclopedia/order-accuracy-benchmarks-what-good-looks-like-for-e-commerce-and-b2b-fulfillment/); [Racklify mispick](https://racklify.com/encyclopedia/what-is-a-mispick-causes-and-operational-impact/)
- WebShop human-expert task success 59% (2022) is the only "human doing the agent's job" baseline in a benchmark setting — [WebShop abstract](https://arxiv.org/abs/2207.01206)
- ShoppingComp human-expert safety pass rate 77.08% vs best model 37.86% (Nov 2025) — [ShoppingComp HTML](https://arxiv.org/html/2511.22978v2)

### Inferences
- (Author inference) "As accurate as a human" is a low bar on the ordering step itself: humans return ~1 in 5 online orders, mostly for fit/size (which an agent can at best match, not beat, given it cannot try things on), and ~20–33% of returns are caused by the listing not matching the product — a merchant-data failure that will hit agents identically.
- (Author inference) The relevant "human error" comparator for agent mis-orders is not the return rate but the wrong-variant/wrong-item selection rate, which no public dataset isolates; the Voxware 42% "incorrect size/colour" figure conflates customer selection error and retailer picking error.
- (Author inference) Human fulfilment mispicks (~0.5–4% of orders depending on metric) set a floor for order "wrongness" that no agent accuracy can remove.

### Gaps
- No 2024–2026 report splits returns into customer-error vs merchant-error; NRF/Happy Returns full report not accessed.
- Narvar's 42% and 58% figures were only found in secondary coverage; underlying report dates unconfirmed.
- Salsify 2025 "71% returned because item differed from page" could not be verified (PDF not text-extractable).
- No measured rate of humans accidentally selecting the wrong variant at checkout.

---

## KQ3. Merchant-side data quality: wrong price/stock in feeds; grocery substitution rates

### Takeaway
The only sizeable public feed-quality dataset (DataFeedWatch, 2022, ~4.5M products / 15,000 stores) found 7% of Google Shopping items disapproved for data errors; Google itself now warns (Sept 2026) that products whose price or availability changes more than once a day may simply be disapproved, and its agentic checkout refuses to buy if the final price exceeds what the user confirmed — evidence that price/stock drift is treated as a structural, merchant-side problem by the platform. Instacart does not publish its absolute found/fill rate, only that its "perfect order fill rate" rose 5 points YoY in 2025.

### Cited Findings
- DataFeedWatch Feed Marketing Report 2022: 7% of Google Shopping feed items rejected because of data errors; sample of >15,000 stores across 20+ sectors (~4.5M products); shipping settings and images responsible for 44% of disapprovals; Shopify stores 5.47% disapproved vs Magento ~10% — [Webtonic summary of DataFeedWatch 2022](https://www.webtonic.io/blog/google-shopping-and-feed-management-statistics); [Search Engine Land](https://searchengineland.com/top-data-errors-that-cause-google-shopping-to-disapprove-products-433993)
- Small July 2026 agency audit of five live Merchant Center accounts (11,107 products): 21 disapproved (~0.19%) but 848 products carried Google-flagged data defects while still being served (single-agency sample, per search summary) — [Limelight Marketing](https://limelightmarketing.com/blogs/product-feed-optimization/)
- Google's automatic item updates help page, revised 27 Sept 2026: products whose prices or availability change more than once a day "may be disapproved rather than updated"; Google may stop automatic updates if mismatches get too high (no threshold given) — [PPC News Feed](https://ppcnewsfeed.com/ppc-news/2026-10/google-warns-fast-changing-products-may-be-disapproved/); [Google help: automatic item updates](https://support.google.com/merchants/answer/6098372?hl=en)
- Google help: "If too many of your products appear to be inaccurate, automatic item updates will stop working and your entire account will be at risk of suspension" — [Google help 6101167](https://support.google.com/merchants/answer/6101167?hl=en-GB)
- Google price-mismatch policy: a product may be disapproved when the landing-page price at load time differs from the feed price — [Google help 12159029](https://support.google.com/merchants/answer/12159029?hl=en)
- Google preemptive-disapproval troubleshooter: Google "needs to regularly check the price and availability on your landing pages"; limited crawl rate can prevent this — [Google help 7334523](https://support.google.com/merchants/answer/7334523?hl=en)
- Feed vendors (Productsup 2026, Webtonic, AdNabu) describe price/availability mismatch as the most common disapproval type but give no measured percentage — [Productsup top-5 2026](https://www.productsup.com/blog/top-5-reasons-for-google-merchant-center-disapprovals-and-how-to-fix-them-in-2026/)
- Google Pay agentic checkout (help page, live Nov 2025 onward): "Agentic checkout is designed to buy the product if the final price is the same as, or lower than the total price you confirmed... If the price goes up, the purchase won't go through, and you won't be charged"; acknowledges a price change can occur "between the time you confirm and when Agentic checkout tries to buy it" — [Google Pay help 16421147](https://support.google.com/googlepay/answer/16421147)
- Instacart FY2025 shareholder letter (8-K dated 12 Feb 2026): "our perfect order fill rate – meaning every item a customer requested was found or successfully replaced – improved year-over-year by 5 percentage points"; no absolute level disclosed — [Maplebear 8-K via StockTitan](https://stocktitan.net/sec-filings/CART/8-k-maplebear-inc-reports-material-event-71ab76f22a2b.html)
- Instacart Q1 2025 letter: perfect order fill rate up 15 percentage points vs three years earlier; company says found and fill rates improved YoY for 10 consecutive quarters — [Maplebear 8-K Q4 2024](https://www.sec.gov/Archives/edgar/data/1579091/000157909125000009/cartfourthquarter2024sha.htm)
- Instacart availability model: predicts availability for 200M+ items every 30 minutes; retailers send inventory updates about once a day; during the 2020 pandemic a found-rate metric fell from 93% to 61% before model retraining — [Instacart tech blog](https://tech.instacart.com/predicting-real-time-availability-of-200-million-grocery-items-in-us-canada-stores-61f43a16eafe); [Fortune, Jun 2020](https://fortune.com/2020/06/09/instacart-coronavirus-artificial-intelligence)
- Instacart experiment: flagging low-stock items produced "a 3.29% decrease in the proportion of items replaced, and a 2.6% decrease in the proportion of items refunded" — [Instacart: predicting the shop](https://tech.instacart.com/the-story-behind-an-instacart-order-part-3-predicting-the-shop-d1a3bb9f5d98)
- Instacart: about 5% of customers express dissatisfaction with replacement items (Instacart statement, via secondary coverage, date unclear) — [Power Commerce summary](https://powercommerce.com/blogs/ecommerce-hub/instacart-introduces-new-tools-to-address-out-of-stocks)
- Instacart order flow: substitution offered first; refund if no agreement — [Instacart Connect docs](https://docs.instacart.com/connect/recommendations_guide/concepts/replacement_items)
- NYU Stern study: personalised substitution suggestions raised acceptance by 26–32% over a retailer's existing ML system — [RetailWire](https://retailwire.com/discussion/substitutions-improved-online-grocery)

### Inferences
- (Author inference) Price/availability mismatch between feed and landing page is a structural error source independent of agent model quality; Google's own policy response (disapprove fast-changing products; price-ceiling rule at checkout) is a platform-side mitigation, not a model-side one.
- (Author inference) Grocery substitution is inherent (daily inventory syncs vs intra-day sellouts); an agent inherits Instacart's substitution/refund rates regardless of its reasoning quality. Instacart's "+5pp YoY" shows the merchant side is improving slowly, driven by inventory prediction, not by the buyer's agent.

### Gaps
- No post-2022 large-sample feed disapproval study; Feedonomics/Salsify publish no comparable rates.
- No measured share of disapprovals caused by price/availability mismatch.
- Instacart's absolute found rate, fill rate, substitution rate and refund rate are not public.

---

## KQ4. Prompt injection and adversarial product pages: attack success rates and whether defenses are closing the gap

### Takeaway
Published browser-agent injection success rates have fallen from 17–86% intermediate / 0–17% end-to-end (WASP, 2025) and 23.6% (Claude for Chrome, Aug 2025) to 0–3.8% without extra safeguards and 0–0.3% with classifiers on Anthropic's 2026 models, and an independent June 2026 reproduction found 0/140 multi-step browser attacks succeeded on Claude Sonnet 4.6 and GPT-5.4. But OpenAI states prompt injection is "unlikely to ever be fully 'solved'", the same June 2026 paper found coding-agent injection still succeeds "at up to 100%", and marketplace studies show models remain manipulable via ranking position, fake reviews and seller-written descriptions — so the direct-injection bucket is shrinking while the softer adversarial-listing bucket is not.

### Cited Findings
**Direct/indirect prompt injection on web agents**
- WASP (Meta FAIR, NeurIPS 2025): intermediate attack success 17%–86% across models (o1 highest at 85.7%); end-to-end attacker-goal completion 0%–17%; authors call it "security by incompetence" — [WASP overview](https://www.alphaxiv.org/overview/2504.18575v1); [NeurIPS 2025 paper](https://papers.nips.cc/paper_files/paper/2025/hash/1c9818387f5dd0a0bc151214660f059d-Abstract-Datasets_and_Benchmarks_Track.html)
- Anthropic Claude for Chrome pilot (25 Aug 2025): 123 test cases, 29 attack scenarios; attack success 23.6% without mitigations -> 11.2% with mitigations in autonomous mode; browser-specific challenge set 35.7% -> 0%; mitigations included "Action confirmations, which ask before high-risk actions" and "Claude asks users before taking high-risk actions like publishing, purchasing, or sharing personal data" — [Claude for Chrome blog](https://claude.com/blog/claude-for-chrome)
- Anthropic Claude in Chrome GA post (26 Aug 2026): attacks reaching the model succeeded against Opus 4.5 17.6% of the time without additional safeguards (16.7% with Nov-2025 probes); Opus 5 3.8% without safeguards; with probes plus safety classifier "no attacks succeeded against Claude Sonnet 5, Claude Opus 5, or Claude Mythos 5"; 0.3% against Fable 5; a classifier now auto-checks actions against the user's request and "You can switch this off in your settings" to keep approving manually — [Claude in Chrome GA blog](https://claude.com/blog/claude-in-chrome-generally-available)
- Anthropic (prompt-injection defenses post): "a 1% attack success rate—while a significant improvement—still represents meaningful risk" — [Anthropic prompt injection defenses](https://www.anthropic.com/news/prompt-injection-defenses)
- Domain-Conditioned Safety in Frontier CUAs (arXiv 2606.05233, Jun 2026): CUA-HandCrafted benchmark, 793 episodes, 24 web tasks, 56 attack templates; "0/140 multi-step attack success (Clopper-Pearson 95% upper bound 2.60%)" on Claude Sonnet 4.6 and GPT-5.4; same weights fall to coding-skill injection "at up to 100%"; high published ASRs "cluster on retired models and on the most-vulnerable model in each paper's panel"; resistance is "domain-conditioned, specific to the heavily-targeted browser surface" — [arXiv 2606.05233](https://arxiv.org/abs/2606.05233)
- Task-universal injection study (arXiv 2606.10525, Jun 2026): attacks optimised on open models "do not transfer to frontier models like GPT-5"; cross-family transfer to GPT-5 / Claude Sonnet 4.5 "ASR < 2%"; TAP ~5% ASR vs GPT-5 (secondary summary) — [EmergentMind summary](https://www.emergentmind.com/papers/2606.10525)
- AutoInject (arXiv 2602.05746, Feb 2026) claims to compromise GPT-5 Nano, Claude Sonnet 3.5, Gemini 2.5 Flash on AgentDojo with a 1.5B suffix generator (rates not verified) — [AutoInject](https://arxiv.org/html/2602.05746)
- OpenAI Atlas hardening post (22/23 Dec 2025): "Prompt injection, much like scams and social engineering on the web, is unlikely to ever be fully 'solved'"; agent mode "expands the security threat surface"; RL-trained automated attacker "can steer an agent into executing sophisticated, long-horizon harmful workflows"; no attack success rates disclosed; "watch mode" requires explicit confirmation for sensitive actions "such as sending messages or making payments" — [Fortune, 23 Dec 2025](https://www.fortune.com/2025/12/23/openai-ai-browser-prompt-injections-cybersecurity-hackers); [Towards AI](https://pub.towardsai.net/can-ai-browsers-ever-be-made-safe-from-prompt-injection-d958e3c10e6c)
- OpenAI "Designing agents to resist prompt injection" (11 Mar 2026): most effective attacks "leveraged social engineering tactics"; cites a 2025 email-based attack on ChatGPT that worked 50% of the time in testing; argues defence must limit what a successful manipulation can do — [OpenAI](https://openai.com/index/designing-agents-to-resist-prompt-injection/)
- OpenAI Operator (Jan 2025) / o3 CUA system card: "Confirmations, Watch Mode, and refusals for high-risk tasks serve as base layers of defense against prompt injections"; Operator "asks for confirmations before finalizing transactions"; screenshot-based prompt-injection monitor is the primary defence layer — [o3 CUA system card PDF](https://cdn.openai.com/pdf/4375e605-f9a6-438d-bcc8-190599c183a6/o3_cua_system_card.pdf); [Introducing Operator](https://openai.com/index/introducing-operator/)

**Adversarial listings / marketplace manipulation**
- Microsoft/ASU Magentic Marketplace (Oct–Nov 2025): tested fake reviews, fake awards and prompt injection; for compromised models "All payments were redirected to the attackers"; "Some frontier models resisted everything, but others were completely compromised" (Decrypt names GPT-4o and GPT-OSS-20b as fully compromised and Claude Sonnet 4 as resistant — secondary) — [Microsoft Research](https://www.microsoft.com/en-us/research/publication/magentic-marketplace-an-open-source-environment-for-studying-agentic-markets/); [Decrypt](https://decrypt.co/347709/microsoft-ai-agents-fake-money-buy-online-they-spent-scams)
- ACES (Aug 2025): strong position bias; seller description tweaks shift share; cited in a Jan 2026 collusion paper as Claude Sonnet 4 choosing top-half listings 77% vs 23% (secondary) — [ACES](https://arxiv.org/abs/2508.02630); [Vertical tacit collusion, arXiv 2601.03061](https://arxiv.org/html/2601.03061v1)
- HBS (Lakkaraju & Kumar): appending a short crafted text sequence to a product description shifts LLM ranking in a 10-machine test — [HBS Working Knowledge](https://www.library.hbs.edu/working-knowledge/gen-ai-marketing-how-some-gibberish-code-can-give-products-an-edge)
- Image-embedding perturbation paper (arXiv 2605.04261, May 2026): ChatGPT Atlas recommended a perturbed product as "more premium" — [arXiv 2605.04261](https://arxiv.org/pdf/2605.04261)
- Stakeholder-centric injection benchmark for real-world web agents (arXiv 2606.13385, Jun 2026) exists; numbers not extracted — [arXiv 2606.13385](https://arxiv.org/pdf/2606.13385)

### Inferences
- (Author inference) The classic "hidden instruction on a product page" vector is being closed by model training plus classifiers (23.6% -> ~0–4% in 12 months on Anthropic's own evals; 0/140 independent). This bucket is model-reducible.
- (Author inference) The "persuasive but legal" vector — seller-optimised descriptions, position, fake social proof — is not a prompt-injection problem and is not addressed by injection classifiers; it is structurally equivalent to SEO/ad manipulation and will persist. First-proposal bias (80–100% acceptance) is a model-reducible flaw but one no vendor has published progress on.
- (Author inference) All vendor-reported ASRs are self-evaluated; the only independent frontier-model reproduction (2606.05233) agrees for browsers but warns the hardening is surface-specific.

### Gaps
- OpenAI has published no numeric attack success rates for Operator, ChatGPT agent or Atlas.
- No study measures real-world (non-benchmark) injection incidence on shopping agents.
- Microsoft's per-model manipulation counts could not be extracted from the PDF.

---

## KQ5. Confirmation / human-in-the-loop requirements built into protocols and platforms

### Takeaway
As of Nov 2026 every consumer-facing shopping agent (ChatGPT Instant Checkout, Google agentic checkout, Amazon Buy for Me, Perplexity Comet, Claude in Chrome, OpenAI Operator/Atlas) requires the human to confirm the order before payment, and UCP's checkout spec mandates hand-off to a "trusted and deterministic UI for the user to review" unless the AP2 Mandates extension is used. The loosening path is already written into the specs: AP2 "open" mandates, UCP's AP2-mandates exception, Mastercard Verifiable Intent's "delegated" three-layer flow, Google's price-ceiling post-confirmation purchase, and Anthropic's classifier-based auto-approval all allow purchases without the user seeing the final cart, bounded by pre-signed constraints.

### Cited Findings
**AP2 (Google, Agent Payments Protocol)**
- AP2 docs: Open Checkout Mandate "Captures the user's constraints and goals for the transaction before a specific cart is finalized for autonomous execution"; Closed Checkout Mandate "Captures the user's (or agent's) authorization for a specific, finalized checkout" and is shared with the merchant; Open Payment Mandate "Captures the user's constraints on payment (e.g., budget, allowed instruments) for autonomous execution"; Closed Payment Mandate "Captures the authorization for a specific transaction amount bound to a finalized checkout" — [AP2 docs/index.md](https://raw.githubusercontent.com/google-agentic-commerce/AP2/main/docs/index.md)
- AP2: the VDC chain gives "a complete, verifiable audit trail for both human-present and human-not-present transactions"; "AP2 provides a non-repudiable, cryptographic audit trail for every transaction, aiding in dispute resolution"; accountability is posed as an open question ("If a fraudulent or incorrect transaction occurs, who is accountable") with user, developer, merchant, issuer, PSP and orchestration layer listed as candidates — [AP2 docs/index.md](https://raw.githubusercontent.com/google-agentic-commerce/AP2/main/docs/index.md)
- Secondary description of the earlier Intent/Cart/Payment naming: Cart Mandate is "the primary VDC for human-present transactions. It captures explicit user authorization for a specific set of items at specific prices"; the merchant endpoint creates and signs the Cart Mandate after receiving an Intent Mandate; in human-not-present flows a detailed Intent Mandate is signed in advance and the Cart Mandate is generated automatically once conditions are met (third-party summaries) — [skills.sh AP2 cart mandate](https://www.skills.sh/orcaqubits/agentic-commerce-claude-plugins/ap2-cart-mandate); [Stellagent on Universal Cart](https://stellagent.ai/ja/insights/google-universal-cart-agentic-commerce)

**OpenAI Agentic Commerce Protocol (ACP) / Instant Checkout**
- ACP checkout spec: checkout session status enum "not_ready_for_payment, ready_for_payment, completed, canceled"; order lifecycle states "created, manual_review, confirmed, canceled, shipped, fulfilled"; "Once the customer clicks 'Buy', the checkout session is completed with a selected payment method"; the merchant "confirm[s] order creation and return[s] the final cart and order identifiers". No `requires_escalation` state exists in ACP; the spec does not explicitly require the buyer to see the final total before payment, though totals are returned "to be displayed to the user" — [ACP checkout spec](https://developers.openai.com/commerce/specs/checkout)
- Instant Checkout (launched 29 Sept 2025): user taps "Buy", then confirms order, shipping and payment details in-chat; "users explicitly confirm each step before any action is taken"; launch limited to single-item purchases from US Etsy sellers; merchant remains merchant of record — [VentureBeat](https://venturebeat.com/ai/openai-debuts-new-chatgpt-buy-button-and-open-source-agentic-commerce); [Search Engine Land](https://searchengineland.com/instant-checkout-chatgpt-agentic-commerce-463222)
- Stripe: the agent "sends the checkout request to the business when the buyer confirms" — [Stripe blog](https://stripe.com/se/blog/developing-an-open-standard-for-agentic-commerce)
- ACP Delegate Payment API lets merchants set spending limits and time-based controls (secondary) — [DEV Community ACP/UCP](https://dev.to/sraveend/agentic-commerce-dive-into-acp-ucp-protocols-4ahj)

**Google Universal Commerce Protocol (UCP)**
- UCP checkout status enum: incomplete, requires_escalation, ready_for_complete, complete_in_progress, completed, canceled; requires_escalation = needed information cannot be given via API or buyer must provide input; "Businesses MUST provide continue_url when returning status = requires_escalation"; ready_for_complete = platform can finalize without the buyer — [UCP checkout spec](https://ucp.dev/specification/shopping/checkout/)
- UCP: checkout "has to be finalized manually by the user through a trusted UI unless the AP2 Mandates extension is supported"; "the agent must hand over the checkout session to a trusted and deterministic UI for the user to review"; `requires_buyer_review` warning for cases where policy/regulatory/entitlement rules "require buyer authorization before order placement"; disclosure warnings "MUST" be displayed to the buyer and not auto-dismissed; businesses "MUST send a confirmation email" after completion — [UCP checkout spec](https://ucp.dev/specification/shopping/checkout/)
- UCP home: "every authorization backed by cryptographic proof of user consent"; AP2 handles payment mandates — [ucp.dev](https://ucp.dev/)
- Google agentic checkout (13 Nov 2025): Google "will always ask your permission first, and will have you confirm your purchase and shipping details"; user taps "buy for me" after confirming; purchase completes with Google Pay — [Google blog](https://blog.google/products-and-platforms/products/shopping/agentic-checkout-holiday-ai-shopping/); [TechCrunch](https://techcrunch.com/2025/11/13/google-expands-ai-shopping-with-conversational-search-agentic-checkout-and-an-ai-that-calls-stores-for-you)
- Google Pay rule: buys only if final price is "the same as, or lower than the total price you confirmed" — a bounded post-confirmation autonomous purchase — [Google Pay help](https://support.google.com/googlepay/answer/16421147)
- BigCommerce guide says a UCP-powered version (Jan 2026) completes the purchase inside AI Mode/Gemini rather than on the merchant site (secondary, unverified date) — [BigCommerce](https://www.bigcommerce.com/articles/ecommerce/google-agentic-checkout/)

**Visa Trusted Agent Protocol (TAP) / Intelligent Commerce**
- Visa developer docs: TAP is "a verifiable, cryptographic standard to help merchants differentiate legitimate agents from nefarious bots"; uses RFC 9421 HTTP message signatures; covers a browsing step ("agent has been initiated by a verified consumer with commerce intent") and a payment step; "describes signatures to enable consumer recognition and payment"; no user-confirmation or final-cart requirement is stated — [Visa TAP specifications](https://developer.visa.com/capabilities/trusted-agent-protocol/trusted-agent-protocol-specifications); [Visa TAP getting started](https://developer.visa.com/capabilities/trusted-agent-protocol/docs-getting-started)
- TAP announced Oct 2025 — [FStech](https://fstech.co.uk/fst/Visa_Launches_Protocol_For_Agentic_Commerce.php)

**Mastercard Agent Pay / Verifiable Intent**
- Mastercard Agent Pay infrastructure launched at NRF, 11 Jan 2026; Verifiable Intent announced 5 Mar 2026, co-developed with Google, "aligned with" AP2 and UCP, to be "integrated directly into Mastercard Agent Pay's intent APIs in the coming months"; links "the identity of the cardholder authorizing the AI agent, the consumer's specific instructions, and the interaction between the agent and the retailer" — [ppc.land](https://ppc.land/mastercard-and-googles-new-trust-layer-could-reshape-how-ai-buys-for-you/); [Mastercard story page (US)](https://www.mastercard.com/us/en/news-and-trends/stories/2026/verifiable-intent.html)
- Mastercard: "in more autonomous scenarios, Verifiable Intent provides transparency that allows merchants to assess whether additional confirmation is needed before completing an agentic transaction" (via search summary of the Mastercard page) — [Mastercard story page (US)](https://www.mastercard.com/us/en/news-and-trends/stories/2026/verifiable-intent.html)
- verifiableintent.dev (Draft v0.1, 2026): "Direct: Buyer authorizes and pays in one step"; "Delegated: Buyer delegates authority before the agent acts — consent and action are separated in time"; "Agent may present options or decide autonomously — buyer may never see the merchant"; 8 constraint types incl. amount bounds, merchant allowlists, budget caps, recurrence terms; maps to AP2, ACP, UCP — [verifiableintent.dev](https://verifiableintent.dev/)
- Technical deep-dive (15 Mar 2026): immediate (human-present) flow = "The user signs final checkout and payment values"; autonomous flow = user signs constraints up front and binds an agent key; agent later produces fulfillment credentials within bounds — [Fintech Wrap Up](https://www.fintechwrapup.com/p/deep-dive-mastercard-verifiable-intent)
- As of 3 Nov 2026 no source confirms the Agent Pay intent-API integration or developer specs have shipped — [ppc.land](https://ppc.land/mastercard-and-googles-new-trust-layer-could-reshape-how-ai-buys-for-you/)

**Platforms**
- Amazon Buy for Me (beta, 10 Apr 2025): after the customer confirms shipping address and payment method, Amazon's agent "navigates to the brand's website, adds the item to cart, enters shipping and payment details, and completes the checkout"; brand handles delivery, returns, service — [Retail Dive](https://www.retaildive.com/news/amazon-buy-for-me-agentic-ai-thirdparty-sites/745047/); [Shopifreaks](https://www.shopifreaks.com/amazon-launches-an-agentic-buy-for-me-feature/)
- Amazon sued Perplexity over Comet making purchases "without authorization"; a US District Court granted Amazon a preliminary injunction in March 2026 (secondary; current status unverified) — [The Decoder](https://the-decoder.com/a-court-battle-over-perplexitys-comet-agent-could-define-how-ai-is-allowed-to-shop-online-for-users/); [eWeek](https://www.eweek.com/de/news/amazon-sues-perplexity-comet-ai-shopping-agent/)
- Perplexity Comet guidance: agent adds to cart and pre-fills shipping/payment, then the user "review[s] the order details and click[s] buy to complete checkout yourself" — [ecommerceguide Comet](https://ecommerceguide.com/agents/comet-2/)
- Anthropic Claude for Chrome (Aug 2025): "Claude asks users before taking high-risk actions like publishing, purchasing, or sharing personal data"; blocks financial-services sites — [Claude for Chrome blog](https://claude.com/blog/claude-for-chrome)
- Anthropic Claude in Chrome GA (26 Aug 2026): a classifier checks actions against the user's request and blocks mismatches; manual per-action approval is now opt-in ("You can switch this off in your settings") — [Claude in Chrome GA](https://claude.com/blog/claude-in-chrome-generally-available)
- OpenAI Operator (Jan 2025): "ask for approval before finalizing any significant action, such as submitting an order"; Operator folded into ChatGPT agent 17 Jul 2025 — [Introducing Operator](https://openai.com/index/introducing-operator/)
- OpenAI Atlas (Dec 2025): watch mode requires explicit confirmation for "making payments" — [Fortune](https://www.fortune.com/2025/12/23/openai-ai-browser-prompt-injections-cybersecurity-hackers)

### Inferences
- (Author inference) Final-cart-before-payment is universal today for consumer agents; the only live exception is Google's price-ceiling rule, which lets the purchase execute after confirmation if the price has not risen.
- (Author inference) The specs have already pre-built the loosening path (AP2 open mandates; UCP's "unless the AP2 Mandates extension is supported"; Verifiable Intent delegated flow; Anthropic classifier auto-approval). No vendor has announced a date or accuracy threshold for enabling unattended consumer purchases; the stated gating factor is cryptographic proof of bounded consent, not model accuracy.
- (Author inference) ACP has no escalation state and relies on ChatGPT's own UI for confirmation, whereas UCP encodes escalation and trusted-UI hand-off in the protocol — so ACP merchants depend on OpenAI's product policy rather than the spec for the human gate.

### Gaps
- Could not retrieve the AP2 specification page (ap2-protocol.org returned 404 for the specification and core-concepts paths); mandate naming (Intent/Cart vs Open/Closed Checkout) differs between the live docs and secondary sources — the live docs/index.md is authoritative.
- Mastercard's US story page and OpenAI's Operator system card returned 403; quotes come from regional/secondary coverage.
- Visa TAP's JSON-schema fields for consumer intent were not inspected (GitHub repo not opened).
- No public statement from any vendor tying confirmation loosening to a measured accuracy threshold.

---

## KQ6. Evidence on rubber-stamping: how carefully users review AI-proposed actions before approving

### Takeaway
The strongest 2026 evidence (Anthropic's 1,053-tester controlled study) found humans caught only 13.6% of dangerous agent commands while approving 97% of permission prompts, with catch rate decaying from ~17% early in a session to ~5% after 50 prompts; a 113-participant academic study found most agent overreach actions "went through after users approved them"; and a 40,000-play game found reviewers missed ~1 in 3 threats. Users nonetheless say they want confirmation for irreversible, externally visible actions (purchases) and experience "delegation regret" when no preview is shown — so the gate is wanted but not reliably used.

### Cited Findings
- Anthropic, "Auto mode is now the default in Claude Code" (7 Aug 2026, updated 22 Oct 2026): "users approve 97% of permission prompts in Claude Code"; controlled study with 1,053 paid testers: "human review caught just 13.6% of dangerous commands, while auto mode caught 89%"; human catch rate fell from ~17% early in a session to ~5% after 50+ prior prompts; "many users are clicking through reflexively rather than reviewing each command"; manually-approved sessions contained serious unintended harm more than twice as often as auto-mode sessions — [Claude Code auto mode blog](https://claude.com/blog/auto-mode-default-in-claude-code)
- Earlier (Mar 2026) coverage of the auto-mode launch cited a 93% approval figure — [Shelly Palmer, Mar 2026](https://shellypalmer.com/2026/03/claude-code-auto-mode/)
- Yan, "Do User-Authored Permission Policies Improve Protection Against AI Agent Overreach?" (arXiv 2608.27443, 27 Aug 2026, rev. 30 Oct 2026): 113 non-professional participants supervised an 18-action simulated day with 7 overreach actions across HITL / AUTO / POLICY conditions; "many actions outside users' original requests went through after users approved them"; in POLICY, 133 of 148 executed overreach actions followed human approval; POLICY blocked less overreach than HITL (−20.1 pp, 95% CI [−32.1, −8.1]) — [arXiv 2608.27443](https://arxiv.org/abs/2608.27443)
- Pochampally, An & Chen, "Assistant or Actor?" (arXiv 2607.18257, Oct 2026): 20 university students, five daily tasks with the OpenClaw agent; "delegation regret appeared consistently when the agent executed actions without preview" even when output was successful; participants "demanded confirmation for irreversible, externally visible actions"; trust loss driven by "irreversibility combined with external visibility, rather than stakes alone" — [alphaXiv 2607.18257](https://www.alphaxiv.org/abs/2607.18257)
- "Continue? Y/N" browser game (Alex Wauters, 2026): 40,000+ plays, 409,000+ decisions; mean accuracy 66.3% (missed ~1 in 3 threats); 'npm run analyze' approved 64.7% of the time; caveat: ~34% of shown commands were threats and players knew they were tested — [ThreatCluster](https://threatcluster.io/cluster/humans-in-the-loop-miss-a-third-of-dangerous-ai-coding-agent-095b959e); [LetsDataScience](https://letsdatascience.com/news/game-data-shows-humans-missed-one-in-three-dangerous-agent-c-a25c079f). A blog attributing this to a "Berkeley study" is unsupported — [Neura Market](https://www.neura.market/blog/humans-missed-1-in-3-threats-when-approving-ai-agent-commands)
- Systematic review of 35 studies (AI & Society, 2026): automation bias stems from AI literacy, expertise and cognitive profile, not only over-trust; "Explainable AI and transparency mechanisms frequently fail to reduce automation bias" — [LongtermWiki summary of review](https://www.longtermwiki.com/resources/a96cbf6f98644f2f)
- Widely repeated "78% of users rely on AI outputs without scrutiny (MDPI 2024)" could not be verified against the original — flagged as unreliable — [LongtermWiki automation bias](https://www.longtermwiki.com/knowledge-base/risks/accident/automation-bias/)
- Agent-side analogue: ShopSimulator found "nearly 80%" of BuyNow errors were "purchases without confirming details" (Jan 2026) — [ShopSimulator HTML](https://arxiv.org/html/2601.18225)
- Microsoft Magentic Marketplace recommends "supervised autonomy, where agents assist with tasks but humans maintain final control and review each recommendation" (Nov 2025) — [Microsoft Research](https://www.microsoft.com/en-us/research/publication/magentic-marketplace-an-open-source-environment-for-studying-agentic-markets/)

### Inferences
- (Author inference) The confirmation gate's protective value falls with prompt volume: at ~17% -> 5% catch rates, a shopping agent that asks for confirmation on every order will have its gate rubber-stamped by frequent users, so human-behaviour errors (approving a wrong cart) are structural and may grow as agent use scales, unless confirmations are rare and high-salience.
- (Author inference) All cited studies are coding or general-task agents; none measures how carefully shoppers review an agent-assembled cart. Purchases differ in that the user sees a concrete price and item, which may raise scrutiny — this is untested.
- (Author inference) Users' stated preference for confirmation on irreversible, externally visible actions aligns with every platform's current purchase gate; "delegation regret" is a measurable form of buyer's-remorse-by-proxy that persists even when the agent is correct.

### Gaps
- No study measures rubber-stamping of AI-proposed purchases or carts specifically; no consent-fatigue study in the commerce context.
- Anthropic's 1,053-tester study methodology is only described in the company blog; no independent replication found.
- No data on how often users change their mind between agent confirmation and delivery (buyer's remorse rates specific to agent orders).

---

## Cross-cutting summary table (author synthesis of the findings above)

| Bucket | Best measured share of agent failures | Trend | Who can fix |
|---|---|---|---|
| (1) Model capability (attribute mismatch, unverified attributes, buy-without-confirming, poor search, first-proposal bias) | ~55–90% of benchmark failures (tau-bench 36/40; ShopSimulator >80% after removing simulator noise; ShoppingBench "largest proportion") | Shrinking across generations (GPT-4o 36.6% -> GPT-4.1 48.2% ShoppingBench; attribute hallucination 0.25 -> 0.055 across open-weight generations) | Model labs |
| (2) Intent ambiguity | 10% (tau-bench authors) to 33% (AgentRx) of failures; ShopSimulator multi-turn stuck ~35% success | Not clearly shrinking; clarification "rarely restores baseline" | Product UX (clarification prompts), user; partly model |
| (3) Merchant data (price/stock/variant/substitution) | Not measured in any agent benchmark; 7% of feed items disapproved (DataFeedWatch 2022); Instacart fill rate +5pp YoY 2025 | Slowly improving on merchant side; platforms mitigate via price-ceiling rules and disapproving fast-changing SKUs | Merchants, feed platforms, Google/Instacart inventory prediction |
| (4) Human behaviour (rubber-stamping, remorse) | 97% prompt approval; 13.6% catch rate; ~19% online return rate baseline | Likely worsening with volume (catch rate 17% -> 5% within a session) | UX design (rare, salient confirmations), not models |
| (5) Adversarial (injection, fake listings) | Injection: 23.6% -> 0–3.8% (Anthropic self-eval); 0/140 independent; listing manipulation and position bias unquantified but demonstrated | Direct injection shrinking fast; persuasive-listing manipulation not addressed | Labs (injection); platforms/marketplaces (listing integrity) |
