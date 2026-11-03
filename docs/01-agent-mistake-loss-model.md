# What Agent Mistakes Cost Merchants

Losses when an AI agent places an order the human later rejects: outcome paths, cost by vertical, and agent error rates (HP vs NHP). Compiled 28 October 2026.

## Key findings

- **Chargeback figures overstate the typical loss.** A chargeback costs merchants about $4–5 per $1 disputed, but most agent mistakes end in a return, a refund or a silent keep. In a $100-order example the expected loss is about **$26 per mistake (~26% of order value)**.
- **Chargebacks are the expensive tail:** about 8% of mistakes, but about 46% of the expected loss.
- **Loss as % of agent-order revenue at a 5% mistake rate ranges from ~0.6% (travel) to ~2.7% (food delivery).** Low-value verticals suffer most because the ~$75 fixed cost of a dispute dwarfs a $30–45 order.
- **Relative to today's losses**, apparel and home rise only ~16% (they already price in returns); food delivery roughly doubles and digital rises ~84%.
- **The extra chargeback rate (+0.4–0.75% at 5% mistakes) is the most dangerous number**: it can push a normal merchant past card-network monitoring thresholds (~0.9–1.0%).
- **There is no published real-world error or dispute rate for agent orders, HP or NHP.** Benchmarks suggest ~8% task failure for HP-like flows and 43–70% for NHP-like flows; working estimates of rejected orders are ~1–3% (HP) and ~5–15% (NHP).

## 1. The cost of a disputed sale (chargeback benchmarks)

| Metric | Value | Source |
|---|---|---|
| All-in loss per $1 disputed | >$4 (Chargebacks911); $5.13 US (LexisNexis 2026) | Chargebacks911 2026 Field Report; LexisNexis True Cost of Fraud |
| Internal cost per chargeback | $82 average; $151.88 at merchants with $1B+ revenue | Mastercard / Javelin 2026 |
| Third-party fees per chargeback | $46 average | Javelin (via Mastercard) |
| Processor dispute fee | $15–45 per dispute, non-refundable | Stripe / Adyen schedules |
| Representment win rate | 44.6% of disputes fought; net recovery 10.7% | Chargebacks911 2026 Field Report |
| Card-network monitoring threshold | ~0.9–1.0% dispute ratio | Visa / Mastercard |
| Typical online chargeback rate | 0.6–1.0%, average ~0.65% | Eightx 2026 benchmarks |

**Why agent orders are worse to defend:** device fingerprints, IP addresses and browsing sessions describe the agent, not the shopper; no network rule yet assigns liability among consumer, AI provider and merchant, so the merchant pays by default; and monitoring programs don't distinguish agent-caused disputes.

## 2. All outcomes of an agent mistake ($100 order example)

Assumes cost of goods $50 and outbound shipping $8. Loss = cash lost versus the order never happening. The share of mistakes per outcome is an illustrative assumption based on ordinary e-commerce behaviour; no agent-specific split is published.

| Outcome | % of mistakes (assumed) | Loss per case | Basis | Weighted loss |
|---|---|---|---|---|
| 0. Cancelled before shipping | 10% | ~$3 | Non-refunded payment fees ~2–3% of order | $0.30 |
| 1. Customer keeps it, says nothing | 35% | $0 direct | Hidden churn cost (see below) | $0 |
| 2. Keeps it for a partial refund | 10% | ~$20 | Assumed 15–30% discount | $2.00 |
| 3. Exchange | 5% | ~$25 | Return handling; revenue kept | $1.25 |
| 4. Return and refund | 25% | ~$25 ($20–65) | Return costs 20–65% of item value; Pitney Bowes average 21% | $6.25 |
| 5. Refund, customer keeps item | 7% | ~$58 | Goods + shipping; used for items under ~$20–25 | $4.06 |
| 6. Chargeback | 7% | ~$140 ($115–165) | Goods + shipping + $15–45 fee + staff time | $9.80 |
| 7. Refund *and* chargeback | 1% | ~$200 | Pays out twice plus fees | $2.00 |
| 8. Court / arbitration | ~0% | Varies | Rare for consumer orders | — |
| **Expected loss per agent mistake** | | | | **≈ $26 (≈26% of order)** |

**Weighted loss** = (% of mistakes ending this way) × (loss per case). Summing the rows gives the average loss per mistake: 100 mistakes ≈ $2,600.

**Hidden cost of path 1:** the classic (old, widely repeated) estimate is that 96% of unhappy customers don't complain and 91% never buy again. Excluded from the total because it depends on customer lifetime value.

**Sensitivity:** only 25% of cardholders contact the merchant before disputing. If 20% of mistakes became chargebacks instead of 8%, expected loss rises to about $40 per mistake.

## 3. Loss by vertical: today vs. extra from agent mistakes

CB = chargeback. All figures as % of revenue. Agent columns apply to revenue from agent orders. Loss % of revenue = mistake rate × loss per mistake. Loss today = return rate × return cost + chargeback rate × chargeback cost.

| Vertical | Typical order | Loss today | Today: returns | Today: CBs | Loss per mistake | Agents @2% | Agents @5% | Agents @10% | Rise @5% | CB share of agent loss | Extra CB rate @5% |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Apparel & footwear | $100 | 7.8% | 7.0% | 0.8% | 25% | +0.5% | +1.3% | +2.5% | +16% | 44% | +0.40% |
| Consumer electronics | $300 | 4.0% | 3.5% | 0.5% | 28% | +0.6% | +1.4% | +2.8% | +35% | 39% | +0.45% |
| Beauty & personal care | $45 | 4.3% | 3.0% | 1.2% | 34% | +0.7% | +1.7% | +3.4% | +40% | 63% | +0.50% |
| Home & furniture | $250 | 9.9% | 9.4% | 0.6% | 31% | +0.6% | +1.6% | +3.1% | +16% | 31% | +0.45% |
| Grocery & household | $80 | 4.8% | 4.3% | 0.5% | 42% | +0.9% | +2.1% | +4.2% | +44% | 44% | +0.50% |
| Food delivery | $35 | 2.7% | 2.6% | 0.2% | 54% | +1.1% | +2.7% | +5.4% | +100% | 57% | +0.50% |
| Travel & lodging | $500 | 1.5% | 1.0% | 0.5% | 12% | +0.2% | +0.6% | +1.2% | +42% | 45% | +0.50% |
| Digital & subscriptions | $30 | 2.6% | 0.2% | 2.4% | 43% | +0.9% | +2.2% | +4.3% | +84% | 94% | +0.75% |

### What stands out

- **Cheap orders lose the most.** A chargeback on a $35 food order costs ~3× the order because fixed dispute costs (~$75) don't scale down.
- **Travel loses the least**: most mistakes are caught in free-cancellation windows. Exception: a wrong booking found after the date is a 100% loss.
- **Digital goods have a chargeback problem, not a returns problem** (94% of loss from chargebacks; little delivery evidence).
- **Verticals that already price in returns absorb agent orders cheaply**; those that don't (food, digital, grocery) see the biggest relative jump.
- **Share of all orders by outcome** = mistake rate × outcome share. At 5% mistakes: 95% fine, ~1.25% returns, ~0.4% chargebacks.

### Assumptions behind the vertical model

| Vertical | Gross margin | Return / refund rate | Cost per return (% of order) | Chargeback rate | Outcome mix of agent mistakes |
|---|---|---|---|---|---|
| Apparel & footwear | ~60% | 25% | 28% | 0.65% | Standard physical-goods split |
| Consumer electronics | ~20% | 10% | 35% | 0.45% | Standard; more returns |
| Beauty & personal care | ~70% | 8% | 38% (mostly returnless) | 0.6% | Leans to keep-it / returnless refund |
| Home & furniture | ~45% | 18% | 52% (bulky shipping) | 0.6% | Standard |
| Grocery & household | ~25% | 5% | 85% (refunded, not returned) | 0.3% | Leans to keep-it / returnless refund |
| Food delivery | ~30% | 3% refunds (est.) | 85% | 0.05% | No returns; refunds and keep-it |
| Travel & lodging | n/a | 10% refunds (est.) | 10% | 1.0% | 40% caught by early cancellation |
| Digital & subscriptions | ~90% | 4% refunds (est.) | 5% | 0.9% | Refunds and chargebacks; no returns |

Chargeback cost per case = lost goods + shipping + $25 fee + ~$50 staff time. Not counted: lost profit on the sale, network penalties, stolen-card fraud, fraud-tool costs, customer churn.

## 4. Agent error rates: HP vs NHP

**Definitions (AP2 protocol):** **HP (human present)** — the customer approves the exact final checkout cart (items, quantities, price, merchant, shipping); the merchant signs the checkout and the user signs a Cart Mandate tied to it. **NHP (human not present)** — the user signs an Intent Mandate up front (e.g. "buy X if under $Y") and the agent builds and pays for the cart unseen.

### HP caveats

- It approves the result, not the process: it catches cart-visible errors (size, quantity, price) but not a worse seller, a lookalike brand or a wrong variant under a vague name.
- People skim and approve: rubber-stamping moves HP error rates toward NHP, though the signed approval still makes disputes easier to win.
- The cart can change after approval: substitutions, split shipments and price changes drive many grocery and food disputes.

### Benchmark evidence (lab, not production)

| Mode | Benchmark | Best result | Failure rate | Caveat |
|---|---|---|---|---|
| HP-like | tau2-bench Retail (simulated customer, real tools, store policy) | Claude Opus 4.6: 91.9% (Sept 2026) | ~8% per task | Customer-service tasks, not purchases; confirm-before-acting, not a signed cart |
| HP-like | tau-bench consistency (all of 8 tries) | Earlier models <25% | Errors spread across tasks | Older models |
| NHP-like | WebShop (1.18M real products) | Best agent 29%; human experts 59% | ~70% (humans 41%) | 2022; any mismatched attribute = failure |
| NHP-like | ShoppingBench (budgets, coupons) | GPT-4.1 <50% | >50% | |
| NHP-like | Open-weight e-commerce agents (Sept 2026) | Exact cart right 30–57% | 43–70% | Best agent also over-purchased |

Benchmark failures overstate wrong orders: many are the agent stopping without buying, or buying something slightly off the customer would accept. A simulation study found the main failures were violating explicit user constraints and hasty purchases without user interaction — exactly what an HP check catches.

### Working estimates (not sourced)

| Mode | Rejected-order rate | Column to use in Section 3 | Share of mistakes reaching a chargeback |
|---|---|---|---|
| HP | ~1–3% | 2% | Lower than modelled; signed cart approval makes chargebacks easier to win |
| NHP | ~5–15% | 5–10% | Higher than modelled; customer may not recognize the charge |

Net: NHP likely costs merchants 3–5× more per order than HP — mistakes are both more frequent and harder to defend.

## 5. Real-world data: what exists

- **No published production error or dispute rate for agent orders**, HP or NHP. Chargeflow: no public source reports one. Visa's July 2026 agentic payments report: dispute processes for agent purchases aren't established.
- **ChatGPT Instant Checkout** (an HP flow) was retired 4 March 2026 after ~5 months; no return or dispute data released. Walmart's in-chat checkout converted at one-third the rate of its own site (conversion, not errors).
- **Amazon "Buy for Me"**: anecdotal reports of orders for out-of-stock or discontinued items, with sellers handling returns even when the agent erred. No figures.
- **Card-network pilots** published volumes only (Visa: hundreds of agent-initiated transactions in December 2025).
- **Consumer attitudes:** 60% of UK shoppers would stop using an agent after one mistake; 25% would stop if a purchase was hard to dispute.
- **Why no numbers:** volume is tiny (agent-completed orders ~0.68% of US online orders, Q3 2026); merchants rarely tag agent orders, almost never HP vs NHP; platforms hold the data.
- **Likely first sources:** payment providers (Stripe, Adyen, Checkout.com) or dispute firms (Chargeflow, Chargebacks911), or merchants who tag agent orders by mode and track outcomes (kept, returned, refunded, disputed).

## Sources

- [Chargebacks911 – Chargebacks are quietly raising prices (2026 Field Report)](https://chargebacks911.com/consumer-pricing/)
- [Mastercard – What's the true cost of a chargeback](https://www.mastercard.com/cz/cs/news-and-trends/Insights/2025/what-s-the-true-cost-of-a-chargeback-in-2025.html)
- [Mastercard / Javelin – 2026 Chargebacks white paper](https://www.mastercard.com/content/dam/mccom/shared/news-and-trends/insights/2026/2026-javelin-chargebacks-white/2026%20Chargebacks%20Javelin%20White%20Paper.pdf)
- [DashDevs – LexisNexis $5.13 figure](https://dashdevs.com/blog/best-chargeback-management-software/)
- [Redo – Chargeback statistics 2026](https://redo.com/resources/articles/chargebacks/chargeback-statistics)
- [VoxBooster – Chargeback statistics 2026](https://voxbooster.com/blog/chargeback-statistics-2026)
- [Chargeflow – AI agent chargeback liability](https://www.chargeflow.io/blog/ai-agent-chargeback-liability)
- [Chargeflow – Agentic commerce, fraud and chargebacks](https://www.chargeflow.io/blog/agentic-commerce-and-its-impact-on-fraud-and-chargebacks)
- [Startups.co.uk – Agentic AI chargebacks](https://startups.co.uk/news/agentic-ai-chargebacks/)
- [Chargebacks911 – Could agentic AI risk more chargebacks](https://chargebacks911.com/could-agentic-ai-risk-more-chargebacks-for-ecommerce-businesses/)
- [Merchant Cost Consulting – Agentic commerce chargeback liability](https://merchantcostconsulting.com/?p=9509)
- [BigCommerce – Ecommerce returns costs](https://www.bigcommerce.com/ecommerce-answers/what-are-self-service-returns)
- [Charle – Ecommerce returns process (Pitney Bowes 21%)](https://www.charle.co.uk/articles/ecommerce-returns-process/)
- [Eightx – Return rate & returnless refunds](https://eightx.co/return-rate)
- [Eightx – UK return benchmark](https://eightx.co/blog/uk-ecommerce-return-rate-benchmark)
- [Eightx – Average ecommerce return rate 2026](https://eightx.co/blog/average-ecommerce-return-rate)
- [Eightx – Chargeback rate by vertical 2026](https://eightx.co/blog/average-chargeback-rate-by-vertical)
- [Richpanel – Return rates by category](https://www.richpanel.com/learn/ecommerce-return-rates.md)
- [Chargeback.io – Chargeback statistics 2026](https://www.chargeback.io/nl/blog/chargeback-statistics)
- [Signifyd – Chargeback statistics](https://www.signifyd.com/?p=53082)
- [Business News – Customer complaints](https://www.businessnews.com.au/node/943)
- [CDP.com – Agentic commerce (AP2 HP/NHP modes)](https://cdp.com/glossary/agentic-commerce/)
- [tau2-bench Retail leaderboard](https://anotherwrapper.com/tools/llm-pricing/evals/tau2-retail)
- [WhySoGeek – Tau-bench and agent reliability](https://whysogeek.com/tau-bench-agent-reliability-2026/)
- [WebShop paper (Yao et al.)](https://alphaxiv.org/abs/2207.01206v4)
- [ShoppingBench](https://arxiv.org/html/2508.04266v2)
- [Evaluating open-weight e-commerce agents (Sept 2026)](https://arxiv.org/pdf/2609.16093)
- [ShopSimulator](https://arxiv.org/pdf/2601.18225)
- [Digital Applied – Agent checkout & card networks](https://www.digitalapplied.com/blog/agent-checkout-authentication-card-networks-2026)
- [GrowthCentr – ChatGPT Instant Checkout fees & retirement](https://www.growthcentr.com/chatgpt-instant-checkout-fees-and-requirements/)
- [Forkast – Visa agent commerce ceiling](https://forkast.news/visa-hundreds-vs-millions-gap-commercial-launch-data-shows-the-agent-commerce-ceiling/)
- [My Amazon Guy – Buy for Me backlash](https://myamazonguy.com/?p=150660)
- [Retail Gazette – Amazon Buy for Me](https://www.retailgazette.co.uk/blog/2026/01/amazon-independent-shops/)
- [Stellagent – Visa/Mastercard agentic strategy](https://stellagent.ai/ja/insights/visa-mastercard-card-network-agentic-strategy)
- [Paz.ai – AI shopping trust gap](https://www.paz.ai/blog/ai-shopping-trust-gap-one-mistake)
- [Checkout.com – Agentic Commerce 2026](https://www.checkout.com/guides-and-reports/agentic-commerce-2026)
- [First Page Sage – Agentic commerce statistics (Oct 2026)](https://firstpagesage.com/seo-blog/agentic-commerce-statistics)
