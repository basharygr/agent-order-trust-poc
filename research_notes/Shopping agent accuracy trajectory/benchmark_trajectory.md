# Measured accuracy trajectory of AI agents on shopping, purchase and checkout benchmarks (2022 – 3 Nov 2026)

As-of date: 3 November 2026. All figures are pass@1 / single-run unless marked pass^k. "Lab-reported" = vendor self-report in a launch post or system card, not independently reproduced. Figures from third-party aggregators (llm-stats, BenchLM, Steel, Vellum, benchmarklist) are flagged as such. Where a figure could not be found it is listed under Gaps, not estimated.

Important scope caveat up front: most "retail" numbers the labs publish (tau-bench / tau2-bench Retail) are **customer-service tasks on existing orders** (cancel, modify, exchange, return, address change) run against a simulated user — they are not add-to-cart-and-checkout tasks. The benchmarks that actually score a cart/purchase outcome (WebShop, ShoppingBench, ShopSimulator, EComAgentBench, Amazon-Bench, DeepShop, the Sept-2026 open-weight e-commerce paper) sit at **30–60% success for the best model tested**, and none of them has published a result for a model newer than Claude Opus 4.6 / GPT-5.4.

---

## Key Question 1: Score progressions per benchmark — best score per year and the model that set it

### Takeaway
Customer-service-style retail tasks (tau2-bench Retail) went from 63% (GPT-4o, 2024) to 92% (Claude Opus 4.6, Feb 2026) and have no lab-reported frontier update since February 2026; genuine purchase/cart benchmarks have moved far less — the best end-to-end shopping success rates in 2025–2026 papers are 32–60%, and WebShop's best agent only reached average-human parity (50.5% vs 50.0%) in Aug 2024, still under the 59.6% expert bar.

### Cited Findings

**WebShop (Yao et al., 2022; 12,087 instructions, 1.18M Amazon products; success = purchased product matches all attributes/options/price)**
- Test-set results in the original paper: rule baseline 9.6% success (score 45.6); IL 29.1% (59.9); IL+RL 28.7% (62.4); human average 50.0% success (score 75.5, 8 workers, Appendix A.6); human expert 59.6% success (score 82.1). — [WebShop paper (ar5iv)](https://ar5iv.labs.arxiv.org/html/2207.01206)
- The paper attributes the largest agent–human gap to product *options* (option sub-score 45.2 IL vs 73.9 human expert, "a 28% gap"); other sub-scores: attribute 69.3–74.0 (agent) vs 81.8 (human), type 86.4–89.7 vs 94.4, price 84.0–88.7 vs 97.7. — [WebShop paper (ar5iv)](https://ar5iv.labs.arxiv.org/html/2207.01206)
- Agent Q (MultiOn, Aug 2024): Llama-3-70B base 28.6% → 48.4% with test-time MCTS search → 50.5% for the trained Agent Q + MCTS, "slightly above" the 50.0% average-human figure but below the 59.6% expert figure. — [Agent Q, arXiv 2408.07199](https://arxiv.org/abs/2408.07199v1)
- Aggregator (benchmarklist) restates the headline as "human experts 59%, best trained agent 29%, rule heuristic 9.6%". — [benchmarklist WebShop](https://benchmarklist.com/benchmarks/webshop/)
- Aggregator listings (sota2/wizwand) show many WebShop variants with top reported numbers of 80.5%, 82.9%, 84.02% "success rate" and up to 91.5–100 "score", but the page does not expose method, model, or date per entry and the task variants/splits differ, so these cannot be attributed. — [sota2 WebShop dataset page](https://sota2.com/research/dataset/webshop)

**tau-bench Retail (Sierra, June 2024; pass^1 unless stated; FC = function calling)**
- Original paper Table 2 (pass^1, FC): gpt-4o 61.2% retail / 35.2% airline; gpt-4-turbo 57.7 / 32.4; claude-3-opus 44.2 / 34.7; claude-3-sonnet 26.3 / 27.6; gemini-1.5-pro 21.7 / 14.0; mistral-large 30.7 / 22.4; llama-3-70B (ReAct) 14.8 / 14.4; gpt-3.5-turbo 20.0 / 10.8. Paper submitted 17 Jun 2024. — [tau-bench, arXiv 2406.12045](https://arxiv.org/html/2406.12045v1)
- Claude 3.5 Sonnet (Oct 2024): 69.2% retail pass^1 (third-party listing citing Anthropic). — [llm-stats tau-bench retail](https://llm-stats.com/benchmarks/tau-bench-retail)
- Claude 3.7 Sonnet (extended thinking, Feb 2025): 81.2% retail; Claude Opus 4 (May 2025): 81.4%; Claude Sonnet 4: 80.5%; Claude Opus 4.1: 83.2% — all secondary listings citing Anthropic announcements. — [HF benchmark file citing Anthropic posts](https://huggingface.co/spaces/Presidentlin/llm-pricing-calculator/blob/main/src/lib/benchmarks/anthropic.ts); [llm-stats](https://llm-stats.com/benchmarks/tau-bench-retail)
- Sierra's GitHub flags the original tau-bench retail/airline task sets as superseded; the maintained lineage is tau2-bench → tau3-bench (v1.0.1, 2026-07-15 changelog) → tau2 2.0 (50 replacement tasks each in retail and telecom; "2.0 scores are not comparable with the 1.x leaderboard"). — [sierra-research/tau2-bench](https://github.com/sierra-research/tau2-bench)

**tau2-bench Retail (Sierra, June 2025; lab-reported pass^1 timeline, compiled by Frontier Log with source links)**
- GPT-4o 63.4% (model May 2024, reported later by OpenAI); GPT-4.1 74.0% (Apr 2025); o4-mini 70.5%, o3 80.2% (Apr 2025); Kimi K2 70.6% (Jul 2025); GPT-5 81.1% with thinking / 72.8% without (7 Aug 2025); Claude Sonnet 4.5 86.2% (29 Sep 2025); Claude Haiku 4.5 83.2% (15 Oct 2025); GPT-5.1 77.9% (12 Nov 2025); Claude Opus 4.5 88.9% (24 Nov 2025, avg of 5 trials, Opus 4.1 as user simulator); GPT-5.2 82.0% (11 Dec 2025, xhigh); LongCat-Flash-Thinking-2601 88.6% (20 Jan 2026); **Claude Opus 4.6 91.9%** (5 Feb 2026, adaptive thinking max effort, avg of 5 trials); Claude Sonnet 4.6 91.7% (17 Feb 2026, avg of 10 trials); Gemini 3.1 Pro 90.8% (19 Feb 2026, Thinking High); Gemma 4 31B 86.4% (2 Apr 2026); Nemotron 3 Ultra 73.68% (Jun 2026); Nemotron 3.5 Super 69.24% (15 Oct 2026). The page notes "Checked" means a human confirmed the number matches the cited lab source, not that anyone reproduced it. — [Frontier Log tau2-bench retail](https://frontierlog.ai/benchmark/tau2-bench/retail)
- tau2-bench paper (9 Jun 2025) text: gpt-4.1 74% retail / 56% airline / 34% telecom pass^1; claude-3.7-sonnet 49% telecom. — [tau2-bench, arXiv 2506.07982](https://arxiv.org/html/2506.07982)
- Third-party leaderboard (llm-stats, updated 3 Nov 2026, self-reported, 28 models): Opus 4.6 0.919; Sonnet 4.6 0.917; Opus 4.5 0.889; GPT-5.2 0.820; GPT-5 0.811; o3 0.802; Nova 2 Omni 0.783; GPT-5.1 0.779; Qwen3-235B-Thinking-2507 0.719; Kimi K2 0.706; GPT-4o 0.634. — [llm-stats tau2-retail](https://llm-stats.com/benchmarks/tau2-retail)
- Vellum (5 Feb 2026) cross-model table: Opus 4.6 91.9%, Opus 4.5 88.9%, Sonnet 4.5 86.2%, Gemini 3 Pro 85.3%, GPT-5.2 82.0% (retail). — [Vellum Opus 4.6 benchmarks](https://www.vellum.ai/blog/claude-opus-4-6-benchmarks)
- taubench.com (official site) headline shows only a combined retail+airline+telecom pass^1 top-3: Qwen3.5-397B-A17B 87.9%, Gemini 3.0 Pro 85.4%, Claude Opus 4.5 85.3%. The official leaderboard page I could fetch only rendered the τ³-Banking tab (Qwen 3.8 Max 55.2%, Claude Opus 5 48.7%, GPT-5.6-sol 46.9%, GPT-5.5 44.6%, Claude Opus 4.7 40.2%, GPT-5.4 39.4%, Opus 4.6 27.3%; all with gpt-5.2 user simulator) — banking is not retail and is a much harder, newer domain. — [taubench.com](https://taubench.com); [taubench.com leaderboard](http://taubench.com/leaderboard/?benchmark=core)
- Steel's "tau-bench" aggregate (updated 4 Sep 2026) mixes harnesses; it lists Claude Opus 5 at 78.0% and Claude Fable 5.1 at 79.3% (OpenRouter provider evals, not official tau-bench runs), GLM-4.7 87.4%, Step-3.5-Flash 88.2%; the page itself says rows "use different user simulators, harnesses, and trial counts, so scores aren't directly comparable." — [Steel tau-bench leaderboard](https://leaderboard.steel.dev/leaderboards/tau-bench.md)

**ShoppingBench (Alibaba International, AAAI 2026; arXiv Aug 2025, v4 Jun 2026; 3,310 instructions, 2.5M-product sandbox; success = all instruction conditions met in terminal state)**
- Absolute success rate (ASR), domain-weighted average: GPT-4.1 48.2% (Products Finder 59.6 / Knowledge 62.0 / Multi-product seller 46.4 / Coupon & Budget 30.4); o3-mini 39.2; Claude-4-Sonnet 39.0; DeepSeek-R1 39.2; GPT-4o 36.6; Qwen2.5-max 35.9; Gemini-2.5-Flash 35.4; DeepSeek-V3 35.4; Qwen3-235B-A22B 32.3; Qwen3-4B 18.0; authors' SFT+RL-Qwen3-4B 48.7% (Coupon & Budget 33.2). — [ShoppingBench, arXiv 2508.04266v4](https://arxiv.org/html/2508.04266)
- Coupon & Budget (constraint-satisfaction) is the hardest intent for every model: 30.4% GPT-4.1, 33.2% best. — [ShoppingBench](https://arxiv.org/html/2508.04266)
- Human study: 3 participants on 200 sampled tasks; numbers are only in Figure 6 (not text-extractable); authors say "humans also have room to improve on the harder intents, and a noticeable gap remains between humans and the best LLMs." — [ShoppingBench](https://arxiv.org/html/2508.04266)
- A June 2026 follow-up (Bittensor subnet distillation) reports Qwen3-4B base 18.0 → distilled 42.7% ASR; SFT-only 43.6%; SFT+GRPO reference 48.7%; pass@1 34.8% vs pass@8 53.3% for the supervised stack — no frontier-model results. — [arXiv 2606.10064](https://arxiv.org/abs/2606.10064)

**ShopSimulator (Alibaba, 26 Jan 2026, ACL 2026; 2.8K eval tasks on 1.3M Taobao products; multi-turn with simulated shopper; "Succ" = full success)**
- Full-success overall: GPT-5 32.65% (single-turn 40.78, single-turn+personalized 36.41, multi-turn 29.29, multi-turn+personalized 24.13); DeepSeek-V3.1 31.81; Claude-4-Sonnet 31.52; Gemini 2.5 Pro 29.55; DeepSeek-R1 28.51; o3 28.48; GPT-4.1 25.99; Qwen3-235B 25.66; Kimi-K2 24.31; GPT-OSS-120B 16.99; Qwen3-8B 12.87. — [ShopSimulator, arXiv 2601.18225](https://arxiv.org/html/2601.18225v1)
- Shopper-simulator errors account for under 8% of failures, so agent-side errors dominate. — [ShopSimulator](https://arxiv.org/html/2601.18225v1)

**EComAgentBench (23 Jun 2026; 662 validated tasks on real Amazon products/reviews; accuracy = exact product match or full rubric satisfaction; single recommended product, no multi-item carts)**
- Accuracy / rubric satisfaction / finish rate: Claude Opus 4.6 57.1% / 76.6% / 84.6%; GPT-5.4 47.0 / 81.0 / 99.4; Kimi K2.6 46.4 / 73.4 / 88.7; GPT-5 39.6 / 76.1 / 97.7; MiniMax M2.7 36.6; GPT-5 mini 31.6; Qwen3-30B-A3B 19.5. Judge = Gemini 3.1 Pro. No human baseline. — [EComAgentBench, arXiv 2606.17698v2](https://arxiv.org/html/2606.17698v2)

**Amazon-Bench (Aug 2025, v2 Apr 2026; 400 live-Amazon queries, 7 categories)**
- Overall success / harmful-failure rate: GPT-4.1 59.75% / 4.75%; Claude-3.7 56.50 / 7.50; o4-mini 51.00 / 8.50; GPT-4o 49.75 / 9.00; Nova-Act 46.30 / 4.00; WebVoyager (Claude-3.7) 44.00 / 6.50; DeepSeek-R1 42.25 / 7.75. GPT-4.1 by category: Product Search 70.45, Deal Search 67.65, Product Interaction 63.64, Store Interaction 32.14. — [Amazon-Bench, arXiv 2508.15832v2](https://arxiv.org/html/2508.15832v2)

**DeepShop (3 Jun 2025; 150-query balanced eval on Amazon, easy/medium/hard; holistic task success)**
- Browser Use 32.00%; OpenAI Deep Research 30.00%; Gemini Deep Research 30.00%; WebVoyager 16.00%; SeeAct 10.67%; Simple RAG 7.33%; Agent-E 6.67%. Web agents average 28.5% easy, 17% medium, ~7 pp lower on hard; OpenAI Deep Research ~20% on hard. — [DeepShop, arXiv 2506.02839](https://arxiv.org/html/2506.02839v1)

**Open-weight e-commerce agents with environment-grounded verification (14 Sep 2026; 8 open-weight 20–35B models; 160 trials each; 10,000-product catalog; Claude Sonnet 4.5 as user simulator and judge)**
- Exact-cart success ranged 0.30–0.57 across the eight agents; per-tier examples: Gemma4-26B 0.59 direct purchase / 0.27 light exploration / 0.53 complex; GPT-OSS-20B 0.59 / 0.42 / 0.67 (multi-item). Qwen3.5-35B had highest goal achievement (0.66) and cart recall (0.85) but cart precision 0.89 (over-purchase). — [arXiv 2609.16093](https://arxiv.org/html/2609.16093)

**Shopping Companion Bench (Mar 2026, v3 May 2026; 1.2M items; cross-session preference memory)**
- Abstract only: "state-of-the-art models such as GPT-5 achieve success rates below 70%"; failures from "cascading errors caused by preference hallucination" and "insufficient verification of product attributes." No per-model table in the abstract. — [arXiv 2603.14864](https://arxiv.org/abs/2603.14864)

**ShopGym (15 May 2026)**: 224 tasks across 6 sandbox shops; abstract reports only that synthetic-shop performance correlates with live-storefront performance; no per-model numbers in the abstract. — [arXiv 2605.16116](https://arxiv.org/abs/2605.16116)

**E-Commerce Bench (31 Aug 2026)**: a 365-day store-operation simulation (seller side, not buyer side); GPT-5.6 Sol grew $100,000 to $1,431,425 but ranked 16/18 on fraud avoidance; Qwen3.8-Max-Preview $416,252. Not a purchase benchmark. — [arXiv 2608.30730](https://arxiv.org/abs/2608.30730)

**Shopping MMLU (Amazon, NeurIPS 2024)**: 57 tasks / 4 skill areas, >20 LLMs; QA-style, not transactional; proprietary > open-source; few-shot sometimes hurts. — [Shopping MMLU, arXiv 2410.20745](https://arxiv.org/abs/2410.20745v2)

**WebArena (Jul 2023; 812 tasks; shopping is 187 tasks)**
- GPT-4 baseline 14.41% vs human 78.24% (2023). — [WebArena paper](https://arxiv.org/pdf/2307.13854v2)
- AgentOccam (Oct 2024, GPT-4-Turbo): 43.1% overall, 40.6% on the Shopping subset (187 tasks). — [AgentOccam, arXiv 2410.13825](https://arxiv.org/pdf/2410.13825)
- OpAgent 71.6% (Jan 2026) and WebTactix on DeepSeek v3.2 74.3% (mid-2026), both via Steel's secondary aggregation; BrowserGym slice tops out at 44.6% (GenericAgent-Claude-3.7-Sonnet). One 2026 review summarizes WebArena rising "from 8.87% in March 2023 to 71.6% in January 2026." — [capitalandcompute WebArena](https://capitalandcompute.net/ai-benchmarks/webarena/); [benchmarklist WebArena](https://benchmarklist.com/benchmarks/webarena/); [arXiv 2506.23978](https://arxiv.org/pdf/2506.23978)
- No 2026 per-domain Shopping-subset score for a frontier model was found. — [search result summary](https://benchlm.ai/md/benchmarks/webarena.md)

**VisualWebArena (ACL 2024)**: human 88.7%; best listed entry GPT-4o 19.78%, GPT-4V 16.37% — the benchmarklist page (sampled Jul 2026) still only lists 2024-era results; **outdated / no frontier entries**. — [benchmarklist VisualWebArena](https://benchmarklist.com/benchmarks/visualwebarena/)

**WebVoyager (Jan 2024)**: original GPT-4V agent 59.1%; OpenAI claimed 87% for Operator but an independent re-evaluation found 68.6%; Browser Use's 89.1% could not be reproduced by Notte; 2026 aggregator entries (Surfer 2 97.1%, Alumnium 98.5%, Claude Sonnet 4-based 93.9%) are self-reported. — [WebVoyager, arXiv 2401.13919](https://arxiv.org/abs/2401.13919v4); [Emergence WebVoyager re-eval, ICLR 2026](https://iclr.cc/virtual/2026/10016354); [open-operator-evals](https://dev.to/andreanotte/opensource-web-agent-outclasses-browser-use-ne1); [Steel WebVoyager](https://leaderboard.steel.dev/leaderboards/webvoyager.md)

**Online-Mind2Web (Apr 2025, v4 Oct 2025; 300 tasks on 136 live sites; human-evaluated)**
- OpenAI Operator 61.3%; Claude Computer Use 3.7 56.3%; SeeAct 30.7%; Browser Use 30.0%; Claude CU 3.5 29.0%; Agent-E 28.0%. Easy-task rates: Claude CU 3.7 90.4%, Operator 83.1%; success falls 31.6 pp easy→medium and 15.4 pp medium→hard. — [An Illusion of Progress, arXiv 2504.01382](https://arxiv.org/html/2504.01382)
- Steel lists UI-TARS-2 at 88.2% (self-reported technical report) above Operator 61.3%. No human-evaluated 2026 frontier (Opus 4.x/5, GPT-5.x, Gemini 3.x) entry was found; the HF leaderboard is JS-rendered and could not be scraped. — [Steel Online-Mind2Web](https://leaderboard.steel.dev/leaderboards/online-mind2web); [OSU-NLP Online-Mind2Web GitHub](https://github.com/OSU-NLP-Group/Online-Mind2Web)

**Mind2Web 2 (Jun 2025; 130 long-horizon agentic-search tasks; Agent-as-a-Judge)**
- Partial completion / success rate / pass@3: OpenAI Deep Research 0.54 / 0.28 / 0.40; Grok DeeperSearch 0.52 / 0.27 / 0.40; Gemini Deep Research 0.45 / 0.18 / 0.30; Perplexity DR 0.42 / 0.15 / 0.26; Claude Research 0.32 / 0.10 / 0.19; OpenAI Operator 0.26 / 0.10 / 0.17; **Human 0.79 / 0.54 / 0.83** (18.4 min/task). Top system "50–70% of human performance while spending half the time." — [Mind2Web 2, arXiv 2506.21506](https://arxiv.org/html/2506.21506v2)

**OSWorld-Verified (computer use, 361–369 tasks; human 72.36%)**
- Operator (CUA) 32.6% at 50 steps (Jan 2025); Agent S2 + Claude 3.7 34.5% (2025); Claude Sonnet 4.5 61.4% (Sep 2025); Claude Opus 4.5 66.3% (Nov 2025); Claude Opus 4.6 72.7% (Feb 2026); GPT-5.4 75.0% (Mar 2026); Claude Opus 4.7 78.0% (Apr 2026); Claude Sonnet 4.6 78.5%; Claude Opus 4.8 83.4% (May 2026); Claude Sonnet 5 81.2%; Claude Fable 5 / Mythos 5 85.0%, Mythos Preview 85.4% (Claude 5 system card, 100 steps, 5-run avg); Qwen3.8-Max 86.1%. All self-reported; Steel notes Anthropic's revised harness scores are "not directly comparable to older entries." Page updated 11 Oct 2026. — [Steel OSWorld leaderboard](https://leaderboard.steel.dev/leaderboards/osworld.md); [Vellum Opus 4.7](https://www.vellum.ai/blog/claude-opus-4-7-benchmarks-explained)
- The official OSWorld site only exposes the 72.36% human figure and the original 12.24% best-model result (2024); its leaderboard loads dynamically and could not be read. — [OSWorld site](http://osworld-v1.xlang.ai/)

**BrowseComp (agentic search, not transactional)**: Claude Sonnet 4.5 43.9%, Gemini 3 Pro 59.2%, Claude Opus 4.5 67.8% (Nov 2025); GPT-5.2 Pro 77.9%; Claude Opus 4.6 84.0% (Feb 2026); Gemini 3.1 Pro 85.9%; Claude Mythos Preview 86.9%; GPT-5.4 Pro 89.3% (Mar–Apr 2026); BenchLM snapshot 26 Oct 2026: Atria Dawn Preview 92.5%, GPT-5.6 Sol 92.2%, Kimi K3.1 Pro 92.1%. — [Vellum Opus 4.6](https://www.vellum.ai/blog/claude-opus-4-6-benchmarks); [Vellum Opus 4.7](https://www.vellum.ai/blog/claude-opus-4-7-benchmarks-explained); [BenchLM BrowseComp](https://benchlm.ai/benchmarks/browse-comp)

**Best-score-per-year summary (best attributable figure found)**
- 2022: WebShop IL 29.1% success (Yao et al.). — [ar5iv](https://ar5iv.labs.arxiv.org/html/2207.01206)
- 2023: WebArena GPT-4 14.41% (human 78.24%). — [WebArena](https://arxiv.org/pdf/2307.13854v2)
- 2024: WebShop Agent Q 50.5% (Aug); tau-bench retail gpt-4o 61.2% (Jun) → Claude 3.5 Sonnet 69.2% (Oct); WebArena AgentOccam 43.1% / shopping 40.6% (Oct); WebVoyager 59.1% (Jan). — sources above
- 2025: tau2-bench retail Claude Opus 4.5 88.9% (Nov); tau-bench retail Opus 4.1 83.2% (secondary); ShoppingBench GPT-4.1 48.2% (Aug); Amazon-Bench GPT-4.1 59.75%; DeepShop Browser Use 32%; Online-Mind2Web Operator 61.3%; OSWorld-Verified Opus 4.5 66.3%; Mind2Web 2 OpenAI DR SR 0.28 (human 0.54). — sources above
- 2026 (to 3 Nov): tau2-bench retail Claude Opus 4.6 91.9% (Feb; no later lab-reported retail figure found); ShopSimulator GPT-5 32.65% (Jan); EComAgentBench Opus 4.6 57.1% (Jun); open-weight exact-cart 0.30–0.57 (Sep); WebArena WebTactix 74.3% (aggregator); OSWorld-Verified 85–86% (Claude 5 family / Qwen3.8-Max). — sources above

### Inferences
- On the one retail benchmark with a dense dated series (tau2-bench Retail), lab-reported pass^1 rose from 74.0% (GPT-4.1, Apr 2025) to 91.9% (Opus 4.6, Feb 2026): +17.9 pp in ~10 months, i.e. the failure rate fell from 26% to 8.1% (~3.2x). Since February 2026 no lab has published a new Retail number for its flagship (Opus 4.7/4.8/5, GPT-5.4/5.5/5.6, Gemini 3.5), which is consistent with the benchmark being treated as saturated and the labs moving to τ³-Banking / τ 2.0, where frontier pass^1 is only 40–55%.
- Benchmarks that score an actual cart outcome show a very different picture: the best model success stayed in a 30–60% band from 2025 through Sep 2026 (ShoppingBench 48%, DeepShop 32%, ShopSimulator 33%, Amazon-Bench 60%, EComAgentBench 57%, open-weight exact-cart ≤57%). Part of this is that each new benchmark is deliberately built harder than the last, so cross-benchmark comparison understates within-benchmark progress; but within-benchmark, GPT-4.1→GPT-5→GPT-5.4 on EComAgentBench-style tasks moved 39.6→47.0 and Opus 4.6 reached 57.1, i.e. roughly +10–17 pp per model generation, well short of a human-parity trajectory.
- WebShop illustrates the "agent ≈ average human, < expert" pattern that recurs on Mind2Web 2 (SR 0.28 vs 0.54 human) and WebArena (74% vs 78%): the easiest half of tasks saturates first and the residual gap is concentrated in option/variant selection and multi-constraint tasks.

### Gaps
- No lab-reported tau2-bench Retail (or tau-bench Retail) score for Claude Opus 4.7, Opus 4.8, Opus 5 / Fable 5 / Mythos, GPT-5.4, GPT-5.5, GPT-5.6, or Gemini 3.5 could be found in launch posts or system cards; Frontier Log's last frontier entry is Gemini 3.1 Pro (19 Feb 2026). The Steel figures for Opus 5 (78.0%) and Fable 5.1 (79.3%) are OpenRouter provider evals of unclear domain/harness and should not be treated as official.
- ShoppingBench human-performance numbers (Figure 6) and error-category percentages (Figure 4) are only in figures; not extractable. No ShoppingBench results for any 2026 frontier model.
- MMShopBench (arXiv 2607.29002) and Shopping Reasoning Bench (2606.12608) PDFs could not be text-extracted (no PDF tool available); per-model numbers unknown.
- No per-domain WebArena Shopping-subset number for any 2025–2026 frontier agent; no VisualWebArena frontier entries at all.
- Official OSWorld and Online-Mind2Web leaderboards are JS-rendered; all OSWorld numbers above come via Steel/Vellum restating vendor cards.
- WebShop: no dated, attributed post-2024 SOTA series could be assembled; aggregator figures of 80–97% exist but without method/date attribution.

---

## Key Question 2: Frontier models' exact-cart-correct rates and the failure-mode split (a) wrong item/variant/quantity, (b) constraint violation, (c) hasty purchase, (d) stopping without buying, (e) hallucination/stale data

### Takeaway
No published benchmark reports an exact-cart-correct rate for any model newer than Claude Opus 4.6 / GPT-5.4; the closest figures are EComAgentBench (Opus 4.6 57.1%, GPT-5.4 47.0% exact-product-or-full-rubric, Jun 2026) and the Sep-2026 open-weight study (exact cart 0.30–0.57). Across benchmarks, wrong-attribute/variant selection is the dominant failure (81.5% of failed rubrics on EComAgentBench; "largest share" on ShoppingBench; 28-pt option gap on WebShop), constraint/filter violations are second (57.7% of Operator errors on Online-Mind2Web; Coupon & Budget is the worst ShoppingBench intent at ~30%), and hasty purchase is a measurable minority (15.3% of cart additions before confirmation for GPT-OSS-20B; ~80% of Claude-4-Sonnet's buy-now errors were purchases without confirmation or after rejection).

### Cited Findings

**Exact-cart / exact-product rates on the newest models with data**
- EComAgentBench (Jun 2026): accuracy ("exact product match or full rubric satisfaction") Claude Opus 4.6 57.1%, GPT-5.4 47.0%, Kimi K2.6 46.4%, GPT-5 39.6%; finish rate (ended with a recommendation rather than exhausting budget) Opus 4.6 84.6%, GPT-5.4 99.4%, GPT-5 97.7%. — [EComAgentBench](https://arxiv.org/html/2606.17698v2)
- Open-weight study (Sep 2026, 20–35B models only): exact-cart success 0.30–0.57; cart precision 0.89 for the best-recall agent (Qwen3.5-35B), i.e. ~11% of cart items were not in the target cart. — [arXiv 2609.16093](https://arxiv.org/html/2609.16093)
- ShopSimulator (Jan 2026): GPT-5 full-success 32.65%, Claude-4-Sonnet 31.52%; multi-turn + personalized drops GPT-5 to 24.13%. — [ShopSimulator](https://arxiv.org/html/2601.18225v1)
- tau2-bench Retail (order-service, not checkout): Opus 4.6 91.9% pass^1 (Feb 2026). — [Frontier Log](https://frontierlog.ai/benchmark/tau2-bench/retail)

**(a) Wrong item / variant / quantity**
- EComAgentBench: of 12,467 failed rubrics, 81.5% are `attribute_match`; `review_opinion` rubrics have the lowest satisfaction (33.7–67.4%); accuracy falls from 51.8% to 27.2% as the number of implicit requirements goes from 0–1 to 3+; per-requirement failure rates rise with how hidden the requirement is: query 12–31%, persona 27–57%, clarification 29–55%. — [EComAgentBench](https://arxiv.org/html/2606.17698v2)
- ShoppingBench: 60 failed GPT-4.1 trajectories were categorised as attribute mismatch, metric issue, product missing, constraint not satisfied, knowledge error; "the largest share comes from missing or mismatched product attributes" (exact split only in Figure 4). — [ShoppingBench](https://arxiv.org/html/2508.04266)
- WebShop (2022): option sub-score 45.2 (IL) vs 73.9 (human expert), "agents have trouble selecting the correct product options." — [ar5iv](https://ar5iv.labs.arxiv.org/html/2207.01206)
- tau-bench (gpt-4o, retail, 36 agent-caused failures of 115 tasks): ~55% "wrong argument or information", 25% "incorrect decision-making", 19% "partial resolution of compound requests" (manual labeling, 1 trial per task). — [tau-bench](https://arxiv.org/html/2406.12045v1)
- Shopping Companion (2026): failures from "insufficient verification of product attributes against user requirements." — [arXiv 2603.14864](https://arxiv.org/abs/2603.14864)

**(b) Constraint violation (budget, filters, sort)**
- Online-Mind2Web: 57.7% of Operator's errors are filter-and-sorting mistakes, 19.6% navigation; it "often misses numeric and temporal constraints" (e.g. applying 2001–2012 when 2004–2012 was requested). — [Illusion of Progress](https://arxiv.org/html/2504.01382)
- ShoppingBench: Coupon & Budget is the lowest intent for every model (GPT-4.1 30.4%, Claude-4-Sonnet 24.0%, Gemini-2.5-Flash 22.8%, best 33.2%). — [ShoppingBench](https://arxiv.org/html/2508.04266)
- DeepShop: fine-grained filter success 34.33% (Browser Use) – 46.15% (OpenAI DR); sort success 30.68% – 58.82%; agents "cannot operate sliders, dropdowns, or nested menus, so price-range filters fail." — [DeepShop](https://arxiv.org/html/2506.02839v1)
- Open-weight study: over-purchase (adding items outside the target cart) identified as a distinct failure profile (Qwen3.5-35B). — [arXiv 2609.16093](https://arxiv.org/html/2609.16093)

**(c) Hasty purchase / acting without confirmation**
- ShopSimulator: for Claude-4-Sonnet, "nearly 80% of [buy-now] errors came from purchasing without confirming details or after the shopper rejected the request"; 9.40% of ask-shopper errors involved not asking when information was missing. — [ShopSimulator](https://arxiv.org/html/2601.18225v1)
- Open-weight study: GPT-OSS-20B made 15.3% of cart additions before customer confirmation; hard-refusal rates on refusal probes ranged only 0.00–0.20 while catalog search was attempted on 0.80–1.00 of probes (agents proceed when they should refuse). — [arXiv 2609.16093](https://arxiv.org/html/2609.16093)
- Amazon-Bench: "harmful failure" rates (irreversible/undesired actions) 4.00% (Nova-Act) – 9.00% (GPT-4o); GPT-4.1 4.75%, Claude-3.7 7.50%; most common in product interaction and account management. — [Amazon-Bench](https://arxiv.org/html/2508.15832v2)

**(d) Stopping without completing / under-action**
- Open-weight study: Qwen3-30B exhausted the turn budget in 63% of trials (under-action); Gemma4-26B exhausted budget on 22 of 33 light-exploration trials; Qwen3.5-35B had 4 of 17 complex trials end in abandonment. — [arXiv 2609.16093](https://arxiv.org/html/2609.16093)
- EComAgentBench: Opus 4.6 finish rate 84.6% (15.4% of runs exhausted the 100-step budget without a recommendation) vs GPT-5.4 99.4%. — [EComAgentBench](https://arxiv.org/html/2606.17698v2)
- Online-Mind2Web: non-Operator agents "repeat actions or stop early when faced with pop-ups or uncertainty"; failed tasks take ~2x the steps of successful ones for Browser Use, Claude 3.7 and Operator; Operator uses ~2.6x human reference steps. — [Illusion of Progress](https://arxiv.org/html/2504.01382)
- Mind2Web 2: "Incompleteness" (info not found / partial missing) is the main error family for non-deep-research systems; Operator and HF Open Deep Research "often fail outright." — [Mind2Web 2](https://arxiv.org/html/2506.21506v2)

**(e) Hallucinated product / attribute / stale data**
- Open-weight study: attribute-hallucination rate 0.050–0.063 for six agents but 0.236 (GPT-OSS-20B) and 0.252 (Qwen3-30B), ~4x higher; phantom actions (claiming a cart action with no matching tool call) 0.050–0.158; numeric hallucination 0.011–0.045. — [arXiv 2609.16093](https://arxiv.org/html/2609.16093)
- DeepShop: deep-research systems "claim products meet constraints they do not, and sometimes return non-Amazon or generic links." — [DeepShop](https://arxiv.org/html/2506.02839v1)
- Online-Mind2Web: other agents "tend to neglect task requirements and hallucinate unmet constraints"; WebVoyager's LLM judge disagreed with humans partly "because of hallucinated final responses." — [Illusion of Progress](https://arxiv.org/html/2504.01382)
- ShopSimulator personalization errors: 55.82% partial omission of preference info, 35.71% over-interpretation of preferences, 8.79% confusing immediate needs with long-term preferences. — [ShopSimulator](https://arxiv.org/html/2601.18225v1)
- Mind2Web 2: "Invalid Attribution" (expired or fabricated URLs) — HF Open Deep Research fabricated links; Operator reported slightly incorrect URLs. — [Mind2Web 2](https://arxiv.org/html/2506.21506v2)

**Simulator/judge noise that caps measurable accuracy**
- tau2-bench user-simulator audit: retail 12% of conversations had critical simulator errors (28% benign); airline 13% critical; telecom 6%. — [tau2-bench](https://arxiv.org/html/2506.07982)
- Open-weight study cross-judge (Claude Sonnet 4.5 vs GPT-5.4, 300 pairs): tool-call correctness r=0.924, MAE 0.030; clarification MAE 0.111, r=0.490; GPT-5.4 scored hallucination 0.052 lower; neither judge calibrated against humans. — [arXiv 2609.16093](https://arxiv.org/html/2609.16093)
- Online-Mind2Web WebJudge (o4-mini) agrees with humans 85.7% with a 3.8 pp success-rate gap; WebJudge-7B 87.0%. — [OSU-NLP GitHub](https://github.com/OSU-NLP-Group/Online-Mind2Web)
- DeepShop GPT-4o judge vs human agreement: 86% overall task success. — [DeepShop](https://arxiv.org/html/2506.02839v1)

### Inferences
- A product team should assume that even the best current purchase agent on an unmodified catalog gets the exact cart right roughly 50–60% of the time on multi-constraint tasks (EComAgentBench Opus 4.6 57%) and ~30–35% on multi-turn personalized tasks (ShopSimulator), and that the remaining failures split roughly: attribute/variant mismatch (majority, >50% of failures everywhere it is measured), constraint/filter violations (second, ~20–60% depending on benchmark design), premature/unconfirmed actions (5–15% of actions on weaker models; up to ~80% of a specific model's buy-now errors), non-completion (0–15% of runs on frontier, up to 63% on weak open models), and outright hallucinated attributes (~5% of trials for most models, ~25% for outliers).
- The tau2-bench Retail 92% figure should not be read as "92% of purchases are correct": the tasks are order-service operations with full tool access and a cooperative simulated user, and the 8% residual is within range of the 12% critical-user-simulator-error rate the authors measured, so the benchmark is near its measurement ceiling.

### Gaps
- No benchmark reports exact-cart-correct rates for Claude Opus 4.7/4.8/5, GPT-5.5/5.6, or Gemini 3.x on any shopping benchmark; EComAgentBench (Opus 4.6, GPT-5.4) is the newest. ShoppingBench, ShopSimulator, DeepShop and Amazon-Bench have not been re-run on 2026 flagships in any paper found.
- No source provides a single consistent (a)–(e) percentage split for one frontier model; the splits above are assembled from different benchmarks with different taxonomies.
- ShoppingBench's error-category percentages (Figure 4) could not be extracted.

---

## Key Question 3: pass^k / consistency rates — tau-bench pass^8 by model over time

### Takeaway
Published pass^k data are thin: the only exact pass^k series are gpt-4o retail pass^1 61.2% → pass^8 "<25%" (Jun 2024) and Claude 3.5 Sonnet retail pass^1 0.692 → pass^4 0.462 (Oct 2024); no lab has published pass^8 (or pass^4) for any 2025–2026 flagship on tau2 Retail, and Princeton HAL's independent reliability study concludes that "overall reliability shows only small improvements over time while accuracy steadily climbs."

### Cited Findings
- tau-bench paper (Jun 2024): "pass^8 <25% in retail" for gpt-4o function-calling (pass^1 61.2%); pass^2–pass^4 values are in Figure 4 only. — [tau-bench abstract](https://arxiv.org/abs/2406.12045); [tau-bench HTML](https://arxiv.org/html/2406.12045v1)
- Secondary (Beancount research log, Jun 2026, restating 2024 vendor tables): GPT-4o retail pass@1 0.604 → pass@4 0.383; Claude 3.5 Sonnet (Oct 2024) retail pass@1 0.692 → pass@4 0.462, airline pass@4 0.225 (source writes "pass@k"; likely means pass^k). — [Beancount tau-bench log](https://beancount.io/nl/bean-labs/research-logs/2026/06/12/tau-bench-tool-agent-user-interaction-real-world-domains)
- Frontier Log's compilation of 29 lab-reported tau2 Retail scores: "No row reports a pass^k value"; labs report averages over 4–10 trials (Opus 4.6: avg of 5; Sonnet 4.6: avg of 10; Nemotron: avg of 8). — [Frontier Log](https://frontierlog.ai/benchmark/tau2-bench/retail)
- Mind2Web 2 reports pass@3 (any of 3 succeeds — the opposite of pass^k): OpenAI Deep Research 0.40 vs SR 0.28; human 0.83 vs 0.54. — [Mind2Web 2](https://arxiv.org/html/2506.21506v2)
- ShoppingBench distillation follow-up: pass@1 34.8% vs pass@8 53.3% (any-of-8), again showing high per-run variance. — [arXiv 2606.10064](https://arxiv.org/abs/2606.10064)
- Princeton HAL reliability leaderboard (models through Jan 2026 plus GPT-5.5, Gemini 3.1 Pro, Gemini 3.5 Flash, Opus 4.7 added in the final version; tau-bench airline is the retail-adjacent domain shown): accuracy / reliability / consistency — Claude Opus 4.7 79.0% / 0.86 / 0.84; Claude Opus 4.5 74.6 / 0.87 / 0.82; Gemini 3.5 Flash 80.0 / 0.85 / 0.77; Gemini 3.1 Pro 79.1 / 0.84 / 0.78; GPT-5.5 71.1 / 0.84 / 0.72; GPT-5.2 46.7 / 0.76 / 0.70; GPT-4 Turbo 44.2 / 0.74 / 0.76; Claude 3.5 Haiku 27.6 / 0.76 / 0.76. Headline: "overall reliability shows only small improvements over time while accuracy steadily climbs"; consistency "often runs inverse to capability"; on GAIA, GPT-5.5 and Opus 4.7 "are no more reliable than their predecessors." — [HAL reliability](https://hal.cs.princeton.edu/reliability)
- Gradually.ai (secondary) reports tau-bench Airline (clean) consistency 0.84 for both Opus 4.5 and GPT-5.5. — [gradually.ai](https://www.gradually.ai/llm-vergleich/claude-opus-4.5-vs-gpt-5.5/)
- channel.tel (Apr 2026, secondary, "approximate") claims Opus 4.7 ~52%, GPT-5.4 ~49%, Gemini 3.1 Pro ~43% retail pass^1 and pass^8 "under 25%" for all three, citing the Sierra repo; these conflict with the 90%+ lab-reported tau2 Retail numbers and appear to use a different harness/version; treat as unreliable. — [channel.tel](https://www.channel.tel/blog/gpt5-opus47-gemini3-cx-agent-shootout)
- METR: some tasks "require 98%+ success probabilities to be worth automating"; 99%-reliability horizons cannot be fit with current benchmarks (~300 tasks per time bucket with ~1% label noise would be needed). — [METR limitations note](https://metr.org/notes/2026-01-22-time-horizon-limitations/)

### Inferences
- If per-trial failures were independent, a 91.9% pass^1 would imply pass^8 ≈ 0.919^8 ≈ 51%; tau-bench's 2024 data showed real pass^k decays faster than independence (61% → <25% at k=8 vs 0.61^8 ≈ 2% under independence — i.e. failures are partly task-correlated, which actually makes pass^8 *higher* than independence predicts for a fixed pass^1). Without published pass^8 for Opus 4.6-class models the consistency rate is unknown; the HAL finding that consistency barely moved (0.76 → 0.84 from GPT-4 Turbo to Opus 4.7) suggests pass^k gains lag pass^1 gains substantially.

### Gaps
- No pass^8 (or pass^4) figure for any tau2-bench Retail run of Claude 4.x/5, GPT-5.x or Gemini 3.x was found in system cards, launch posts, Frontier Log, llm-stats, Steel, or taubench.com. Sierra's leaderboard has Pass^2–4 columns but they rendered empty on the fetched page.
- The original tau-bench Figure 4 pass^k curves and tau2-bench Figure 3 are image-only; exact values not extracted.

---

## Key Question 4: Distance to human expert performance, and whether authors attribute the gap to model capability or task ambiguity

### Takeaway
Where a human baseline exists, agents are at or above *average* human on the easiest benchmarks (WebShop 50.5% vs 50.0%; WebArena 74% vs 78%; OSWorld 85% vs 72%) but well below *expert* or careful-human performance on shopping-specific ones (WebShop expert 59.6%; Mind2Web 2 human SR 0.54 vs 0.28; ShoppingBench "noticeable gap remains"); benchmark authors consistently attribute the residual gap to agent capability (option selection, filter/constraint handling, verification, early stopping) and measure simulator/judge noise at well under the agent error rate.

### Cited Findings
- WebShop: Agent Q 50.5% vs human average 50.0% vs expert 59.6% (Aug 2024). — [Agent Q](https://arxiv.org/abs/2408.07199v1); [WebShop](https://ar5iv.labs.arxiv.org/html/2207.01206)
- WebArena: human 78.24%; best aggregator-listed 74.3% (WebTactix, 2026) and 71.6% (OpAgent, Jan 2026). — [capitalandcompute](https://capitalandcompute.net/ai-benchmarks/webarena/)
- VisualWebArena: human 88.7%; no frontier entry listed (best listed GPT-4o 19.78%, 2024). — [benchmarklist VWA](https://benchmarklist.com/benchmarks/visualwebarena/)
- OSWorld-Verified: human 72.36%; GPT-5.4 75.0% (Mar 2026) was the first claim above human; Claude 5 family 85.0–85.4% and Qwen3.8-Max 86.1% by Oct 2026. — [Steel OSWorld](https://leaderboard.steel.dev/leaderboards/osworld.md)
- Mind2Web 2: human SR 0.54 / PC 0.79 / pass@3 0.83 vs best system (OpenAI Deep Research) SR 0.28 / PC 0.54 / pass@3 0.40; authors: top system reaches "50–70% of human performance while spending half the time"; Deep Research systems have *surpassed* humans on the "criteria violation" error category, with the remaining gap being incompleteness and attribution. — [Mind2Web 2](https://arxiv.org/html/2506.21506v2)
- Online-Mind2Web: authors argue prior ~90% WebVoyager claims were an "illusion of progress" caused by narrow task diversity, shortcut-solvable tasks (naive search agent solved 51% of WebVoyager but 22% of Online-Mind2Web) and unreliable LLM judges; real frontier success 30–61%. — [Illusion of Progress](https://arxiv.org/html/2504.01382)
- ShoppingBench: 3 humans on 200 tasks; authors say humans "also have room to improve on the harder intents" and "a noticeable gap remains between humans and the best LLMs." — [ShoppingBench](https://arxiv.org/html/2508.04266)
- ShopSimulator: shopper-simulator errors <8% of failures → gap is agent-side ("deep search and product selection in long trajectories, fail to balance personalization cues, fail to engage with users"). — [ShopSimulator](https://arxiv.org/html/2601.18225v1)
- tau2-bench: 12% of retail conversations contain a critical user-simulator error, bounding how much of the ~8% residual at the top of the leaderboard is attributable to the model. — [tau2-bench](https://arxiv.org/html/2506.07982)
- tau-bench: of 40 gpt-4o retail failures, 4 (10%) were from user-instruction typos/ambiguity (fixed), 36 (90%) were agent errors. — [tau-bench](https://arxiv.org/html/2406.12045v1)
- DeepShop authors attribute failures to grounding, lack of replanning, limited action space (sliders/dropdowns), and no learning from execution — all agent-side. — [DeepShop](https://arxiv.org/html/2506.02839v1)
- The open-weight study authors argue single task-success scores hide "under-action, over-purchasing, unsupported product attributes, and poor search," i.e. capability failures invisible to terminal-state scoring. — [arXiv 2609.16093](https://arxiv.org/abs/2609.16093)
- METR: visual computer-use time horizons are 40–100x lower than software, attributed to poor perception. — [METR limitations](https://metr.org/notes/2026-01-22-time-horizon-limitations/)

### Inferences
- Benchmark authors overwhelmingly place the gap on the model side (verification, constraint handling, early stopping) and quantify simulator/judge noise at 6–13% of conversations — enough to matter only once a model is above ~85–90%, which is why tau2 Retail looks saturated while cart benchmarks at 30–60% have ample headroom that is genuinely model-limited.
- "Human parity" claims on OSWorld and WebArena are against non-expert crowd baselines (72% and 78%); the shopping-specific expert bars (WebShop 59.6%, Mind2Web 2 pass@3 0.83) have not been reached by any agent reported.

### Gaps
- No human baseline exists for ShopSimulator, EComAgentBench, Amazon-Bench, DeepShop or the open-weight study; ShoppingBench's human numbers are figure-only.
- No author of a 2026 shopping benchmark published an explicit "task ambiguity vs model" decomposition beyond the simulator-error audits cited.

---

## Key Question 5: Published rate-of-improvement analyses and human-parity projections for web/commerce tasks

### Takeaway
No METR-style time-horizon or doubling-time analysis specific to web shopping or checkout tasks exists; METR's general doubling time is ~6–7 months (TH1.1 fits ~129–188 days) on software tasks, with computer-use horizons 40–100x shorter, and the only commerce-adjacent trend statement found is a 2026 review noting WebArena rose from 8.87% (Mar 2023) to 71.6% (Jan 2026) — about 20 pp/year — with no author projecting a parity date for purchase tasks.

### Cited Findings
- METR: "maximum task length that models can successfully complete roughly doubles every 7 months" (2025); TH1.1 fitted doubling ~188 days all-time, ~129 days since 2023 (community fits 89–131 days post-2024); time horizons rose ~6x in ~9 months after the paper; Claude Opus 4.5 50% horizon ≈ 4h49m (95% CI 1h49m–20h25m). Domains covered: software, research, math, robotics, scientific reasoning, computer use, self-driving — "web browsing and e-commerce are not mentioned." Visual computer use is 40–100x lower than software. Linear extrapolation reaches a 1-month (167 working-hour) horizon "around 2030." — [METR limitations note](https://metr.org/notes/2026-01-22-time-horizon-limitations/); [METR Time Horizons now 10x/year (community)](https://www.greaterwrong.com/posts/EYb2K9acKfyG2bome/metr-time-horizons-now-10x-year)
- WebArena: 8.87% (Mar 2023) → 71.6% (Jan 2026) vs 78.24% human, per a 2026 review; a 2026 AI Index summary lists 74.3%. — [arXiv 2506.23978](https://arxiv.org/pdf/2506.23978); [adaline.ai](https://www.adaline.ai/blog/evaluating-ai-agents-in-2026)
- A 2026 arXiv paper fits SWE-bench Verified solve-odds growth at ~5.8x per year (R²=0.78, n=14) — coding, not commerce; no equivalent fit for web benchmarks was found. — [arXiv 2608.13675](https://arxiv.org/pdf/2608.13675)
- Online-Mind2Web authors' framing: reported progress on web agents was partly an artefact of benchmark design (WebVoyager ~90% claims vs 30–61% real). — [Illusion of Progress](https://arxiv.org/html/2504.01382)
- HAL: "overall reliability shows only small improvements over time while accuracy steadily climbs." — [HAL reliability](https://hal.cs.princeton.edu/reliability)
- Sierra moved the frontier to τ³-Banking (Jul 2026 models: Qwen 3.8 Max 55.2%, Claude Opus 5 48.7%, GPT-5.6-sol 46.9%, GPT-5.5 44.6%, Opus 4.7 40.2%) after tau2 Retail saturated above 90%. — [taubench.com leaderboard](http://taubench.com/leaderboard/?benchmark=core)

### Inferences (my arithmetic from the cited series, not a source's projection)
- tau2-bench Retail: +17.9 pp in 10 months (74.0 → 91.9, Apr 2025–Feb 2026), error rate shrinking ~3.2x; then no published frontier movement for 8+ months (likely saturation/retirement rather than stall).
- Cart-outcome benchmarks: GPT-4.1 (Apr 2025) → GPT-5 (Aug 2025) → GPT-5.4 (Mar 2026) on EComAgentBench 25.99*/39.6/47.0 (*ShopSimulator value for GPT-4.1, different benchmark — not comparable) and Opus 4.6 57.1 suggest ~10–20 pp per year on exact-product tasks; at that pace a 90% exact-cart rate on multi-constraint tasks would be 2–3 years out, and pass^k reliability (per HAL, nearly flat) would lag further. This is an extrapolation across non-identical benchmarks and should be presented as such.
- OSWorld-Verified moved 32.6% (Jan 2025) → 85% (mid-2026), ~35 pp/year, crossing the 72% crowd-human bar in Feb–Mar 2026; WebArena needed ~2.8 years (Mar 2023 → Jan 2026) to go from 9% to 72%. These are the fastest-moving relevant series; purchase-specific benchmarks are moving slower partly because each successive one is built harder.

### Gaps
- No METR-style horizon measurement, doubling-time fit, or human-parity projection specific to web shopping, checkout, or e-commerce tasks was found from METR, Epoch AI, Scale SEAL, Artificial Analysis, or benchmark authors.
- No source gives a dated SOTA series for WebShop after 2024 or for ShoppingBench/ShopSimulator beyond their launch papers, so no within-benchmark 2025→2026 slope can be computed for a purchase task.
- Epoch AI and Artificial Analysis pages were not fetched (not surfaced by searches run); their agent-benchmark hubs may hold additional dated tau2/OSWorld series.
