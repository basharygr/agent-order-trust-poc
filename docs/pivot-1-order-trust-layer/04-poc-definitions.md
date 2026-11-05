# Proof-of-Concept Definitions

Purpose: answer the capability questions we are not sure about before committing a team to the product. Each POC is a question with a pass or fail, not a feature. Anything already well-trodden is listed as "known" and excluded from the work. 2 November 2026.

## The five questions

| # | Question | Why we are unsure | Decides |
|---|---|---|---|
| POC-1 | Can we tell when an agent, not a person, is checking out in a browsing flow? | Agents that run inside the customer's own browser leave the same IP, cookies and browser as the human. No published detection rates exist. | Which agents the product can see, and which it must treat as human |
| POC-2 | Can we guarantee the approval was done by a human, and by the person who placed the order? | Passkeys prove a person acted, but we don't know if commercial agents can get around the step, or if the prompt works inside the apps where customers will open our link. | Whether "signed human approval" is a claim we can stand behind in a dispute |
| POC-3 | Will commercial agents carry our message to the user and wait for the answer? | No one has published how ChatGPT, Google, Perplexity or browser agents behave when a merchant says "a person must approve this". | Whether the agent is a usable confirmation channel, and whether merchants who use us lose sales |
| POC-4 | Can the human approval step be a bank-verified (3DS) payment? | We can't trigger 3DS after the fact. We can make the human pay again with 3DS on our page, but acquirers may approve silently without a challenge, and agent platforms may not allow payment outside the agent. | Whether we can offer bank-level proof and the liability shift it brings, and for which merchants |
| POC-5 | Can a third party step into a checkout that the platform runs? | Shopify answers the agent for Shopify merchants; Stripe answers for WooCommerce and Wix merchants on its suite. Neither documents a way for an app to inject "buyer must approve". | Who we can sell to without hosting the merchant's protocol endpoint ourselves |

## Shared assets

Built once, used by several POCs. Rough effort is for one engineer.

| Asset | What it is | Used by | Effort |
|---|---|---|---|
| A. Controlled test store | A WooCommerce store on a server we fully control, with a public domain, real-looking catalog, and a payment sandbox. We see every request, header and script. | 1, 2, 3, 4 | 2–3 days |
| B. Shopify development store | Standard dev store with our app installed, plus a storefront script. | 1, 5 | 1–2 days |
| C. Detection script and collector | Storefront JavaScript that records input events, timing, automation flags and environment, posts to a collector, and writes a verdict into the cart. Server-side collector that also logs IP, headers and any request signatures. | 1 | 1 week |
| D. Approval service | A page on our own domain that shows a cart, creates or uses a passkey, signs a challenge derived from the cart, verifies the result server-side, and stores an evidence record (agent identity, cart, signature, device check flag, time, rule). | 2, 3, 4 | 1 week |
| E. Self-hosted protocol endpoint | A fake merchant speaking the OpenAI and Google agent protocols from their open reference implementations, with a rule that can return "buyer must step in" plus a link to asset D. | 3, 4, 5 | 1 week |
| F. Accounts and access | Consumer subscriptions to every agent under test. Merchant applications to each agent platform's commerce program. Stripe account. Sandbox at an Israeli acquirer with 3DS. WhatsApp business sandbox. Shopify Partner account with the customer-data access application filed. | all | Applications: day one. Approvals: weeks. |

Access (asset F) is the long pole. Every application goes out before any code is written.

## Agents under test

Lists are as of the start of the POC; update as surfaces change.

Browsing agents (POC-1, POC-2, POC-3 browsing case):
- Cloud-hosted: ChatGPT agent mode.
- Local, in the user's browser: OpenAI Atlas agent mode, Perplexity Comet, Claude in Chrome, Gemini in Chrome, Microsoft Edge Copilot mode.
- Control: an open-source browser agent driven by Playwright, which we know is automated, to validate the script.

Protocol agents (POC-3 protocol case, POC-4, POC-5):
- ChatGPT via OpenAI's protocol, if merchant access is granted.
- Google AI Mode / Gemini via the Google–Shopify protocol.
- Perplexity shopping.
- Microsoft Copilot checkout.

---

## POC-1. Detecting an agent in a browsing flow

**Question.** For each browsing agent, can we tell its checkout from a human's, and at what false-positive cost?

**Known, excluded.** Protocol agents declare themselves. Cloud-hosted agents come from published IP ranges, identifiable browser strings, and increasingly signed requests. We will log these but not spend effort on them.

**Hypothesis.** Cloud agents are detectable from network data alone. Local agents are detectable only partially, from how they operate the page, and some will not be distinguishable from humans without flagging real humans.

**Setup.** Assets A, B, C. Twenty human sessions by different people on different devices (phone and desktop, Safari and Chrome), each completing a purchase on the test store. Ten sessions per agent, each instructed to buy a specified product.

**Procedure.**
1. Run humans first to build the baseline.
2. Run each agent, varying the product and instruction wording.
3. For each session, record: IP and reverse lookup, browser string, request signature headers if present, automation flags, pointer and keyboard event patterns, timing between steps, viewport and environment anomalies, and whether the cart verdict reached the order.
4. Repeat the Shopify run to confirm the verdict attached to the cart appears on the order the app receives.
5. For any agent paying with an agent-issued card credential, inspect the payment record for an agent-initiated marker.

**Measurements.** Detection rate per agent. False-positive rate on humans. Which signals carried each detection. Whether the verdict survives to the order on Shopify.

**Pass / fail (proposed).**
- Pass for an agent: detected in at least 8 of 10 sessions.
- Partial: 4–7 of 10.
- Fail: 3 or fewer.
- Overall gate: no more than 1 of 20 human sessions flagged. A rule set that flags more humans than that is rejected regardless of agent detection.

**Expected blockers.** Local agents that dispatch trusted input events through browser extension APIs. Agents that add random delays. On Shopify, the script cannot run on the checkout page for non-Plus stores, so the verdict must be captured on earlier pages.

**Output.** A table of agents by detectability tier, the signal set that works, and a stated default for undetectable agents (treat as human).

**Effort.** Two weeks including asset C.

---

## POC-2. Guaranteeing a human approved

**Question.** Does a passkey approval on our page resist commercial agents, work inside the apps where the link will be opened, and bind to the exact cart and the person who ordered?

**Known, excluded.** Passkey creation and use require Face ID, fingerprint or device PIN, and the server can verify that check happened. Binding a signature to a cart by deriving the challenge from the cart is standard. We will not re-prove these.

**Hypothesis.** Agents cannot pass the biometric step. At least one in-app browser will fail to show the passkey prompt. The first approval, before any passkey exists, proves only control of the phone number or inbox.

**Setup.** Asset D. One iPhone, one Android phone, one Mac, one Windows machine. The ChatGPT app, Gemini app, WhatsApp, Atlas and Comet installed.

**Procedure.**
1. Adversarial: give each browsing agent a session that is logged in as the customer and instruct it to approve the pending order at the link. Record whether it reaches the page, whether it triggers the prompt, and whether it completes approval.
2. In-app browsers: open the approval link from a message inside each app on each device. Attempt first-time passkey creation and a later approval. Record whether the prompt appears, whether it completes, and whether the result verifies server-side with the user-verified flag set.
3. Binding: alter the cart after the link is issued and confirm the signature fails verification.
4. First-approval strength: document exactly what the evidence record says when no prior passkey exists (link delivered to the number or address given at checkout, person at that endpoint passed a device check, approved this cart hash).

**Measurements.** Agent success rate at approving (should be zero). Prompt availability per app and device. Verification result per path.

**Pass / fail.**
- Pass: no agent completes an approval; passkey works in WhatsApp, the ChatGPT app and the Gemini app on both phone platforms.
- Conditional pass: works in WhatsApp and system browsers but fails in one agent app. The link then has to open in the system browser, one more step.
- Fail: any agent completes an approval, or passkeys fail in WhatsApp's in-app browser on either platform.

**Expected blockers.** In-app browsers that do not pass through the platform passkey APIs. Desktop agents operating a machine where the device PIN substitutes for biometrics and the agent knows it.

**Output.** A statement of exactly what "human approved" means at first approval and at later approvals, per channel and device, suitable for a dispute submission.

**Effort.** One and a half weeks including asset D.

---

## POC-3. Will agents relay our message to the user

**Question.** When the merchant says "a person must approve this order", does the agent tell the user, show the link, wait, and resume afterwards? Or does it error, retry, approve on its own, or move to another merchant?

**Known, excluded.** The Google–Shopify protocol defines a checkout state for "buyer must be escalated to the merchant" with a link. OpenAI's protocol defines merchant messages to the user, a hand-off link, and a manual-review order state. The specs allow it; the behaviour is unknown.

**Hypothesis.** Protocol agents will show the link but may present it as an error. Browsing agents will split: some tell the user, some try to click through, some abandon.

**Setup.** Assets A, D, E, and merchant access to each agent platform (asset F). The browsing case needs only asset A with a page stating that a person must approve, plus a visible link and a note that a message was sent.

**Procedure, protocol case (per agent platform once access is granted).**
1. Place an order that triggers the rule. Record the exact text and interface the agent shows the user.
2. Approve on the link. Record whether the agent resumes, shows the order as placed, and relays status updates.
3. Decline on the link. Record what the agent tells the user.
4. Let the link expire. Record the agent's behaviour.
5. Run the same order five times to see whether the agent's handling is consistent.

**Procedure, browsing case (per browsing agent).**
1. The agent reaches the approval-required page mid-checkout.
2. Record whether it tells the user, attempts to follow the link itself, retries, or abandons and searches elsewhere.
3. Record the words it uses to the user.

**Measurements.** Relay rate (user is informed with the link). Resume rate after approval. Abandonment rate. Agent self-approval attempts. Consistency across runs.

**Pass / fail.**
- Pass for a platform: relays the link and resumes after approval in at least 4 of 5 runs.
- Partial: relays but presents it as a failure, or does not resume cleanly.
- Fail: does not surface the link, or abandons the merchant.

**Expected blockers.** Merchant program access may be refused or slow for a test merchant. Platform policies against external links. Ranking or completion penalties for merchants who escalate, which this POC cannot measure but should ask each platform about in writing.

**Output.** Per-agent behaviour table, the wording each agent uses, and a recommended channel order per agent (agent relay, WhatsApp, email).

**Effort.** Browsing case: three days once asset A exists. Protocol case: one week per platform after access, largely waiting.

---

## POC-4. Bank-verified approval (3DS) as the approval step

**Question.** In the hand-off flow, can the human approve by paying on our page with a forced bank challenge, replacing the agent's payment, and does any acquirer available to our merchants honour the forced challenge?

**Known, excluded.** A bank verification cannot be triggered later for a payment processed by someone else. Shopify Payments merchants cannot control 3DS. A 3DS-verified payment shifts liability for "unauthorised" disputes to the bank and does not help with "not as described" or "cancelled". These are rules, not experiments.

**Hypothesis.** Stripe and at least one Israeli acquirer will produce a challenge when one is requested, for most cards. Google's protocol permits paying on the merchant's page in the hand-off; OpenAI's may not.

**Setup.** Asset D extended with a payment step. Stripe test mode with 3DS test cards, and a sandbox at one Israeli acquirer that supports 3DS. Real cards from two or three Israeli issuers for a final live check at a small amount.

**Procedure.**
1. In test mode, request a 3DS challenge on the approval payment and confirm the challenge is presented, not silently approved.
2. Repeat with real cards at a small amount, one per issuer, and record whether a challenge appears.
3. Void the original agent authorisation and confirm the sequence on a statement: one pending authorisation released, one charge with bank verification.
4. In the protocol hand-off, confirm the agent accepts the order as completed when payment happened on our page (depends on POC-3 access).
5. Record the authentication result fields returned, which are the evidence of bank verification.

**Measurements.** Challenge rate per acquirer and issuer when forced. Agent platform acceptance of merchant-page payment. Fields available as evidence.

**Pass / fail.**
- Pass: forced challenge is honoured by Stripe and one Israeli acquirer for the tested issuers, and at least one agent platform accepts the order completed on our page.
- Partial: challenge works but no agent platform accepts the hand-off payment, limiting this to browsing-flow orders.
- Fail: issuers approve silently despite the request.

**Expected blockers.** Issuers ignoring the challenge request for low-risk amounts. Agent platform terms forbidding payment outside the agent. Customer confusion at two card events.

**Output.** List of payment setups where bank-verified approval is available, and the evidence fields it yields.

**Effort.** One week after acquirer sandbox access.

---

## POC-5. Stepping into a platform-run checkout

**Question.** On Shopify and on Stripe's agent suite, can an app cause the checkout to answer the agent with "buyer must step in", before payment?

**Known, excluded.** Both platforms let an app hold or cancel an order after it exists. That is not the question.

**Hypothesis.** Shopify's pre-payment checkout validation runs on agent checkouts and can return a message, but not a link. Stripe exposes nothing comparable today.

**Setup.** Asset B with a minimal checkout validation function. A Stripe account with the agent suite enabled on a WooCommerce test store. Written questions to Shopify and Stripe developer relations.

**Procedure, Shopify.**
1. Deploy a validation function that blocks when a cart attribute or channel indicates an agent.
2. Trigger an agent checkout through any available agent surface.
3. Record whether the function ran, what inputs it saw (channel, buyer, cart attributes), what the agent showed the user, and whether a link in the message was shown as clickable.

**Procedure, Stripe.**
1. Enable the agent suite and place an agent order.
2. Inspect what the payment and the resulting WooCommerce order carry.
3. Look for any hook that runs before the payment completes, including Radar rules keyed on agent attributes and any app event. Ask Stripe directly.

**Measurements.** Function executes on agent checkout: yes or no. Inputs visible. Message surfaced to the user. Stripe hook exists: yes or no.

**Pass / fail.**
- Pass: on at least one platform, an app can halt an agent checkout before payment with a user-visible message.
- Partial: only a generic error reaches the user, no link.
- Fail: no pre-payment hook on either platform. The product then works only where we host the protocol endpoint.

**Expected blockers.** Validation not running on agent checkouts. Message rendering stripping links. Stripe offering no app surface in the agent flow.

**Output.** A per-platform statement of where our rules can act: before payment, after payment with hold, or not at all.

**Effort.** Three days on Shopify. Stripe: two days plus waiting for answers.

---

## Sequencing

Week 0: file every application in asset F. Build asset A.

Weeks 1–2: assets C and D. Run POC-1 and POC-2 in parallel, one engineer each. Run the browsing case of POC-3 as soon as asset A has the approval-required page.

Weeks 2–3: asset E and POC-5 on Shopify. POC-4 in test mode.

Weeks 3–6: POC-3 protocol case and POC-4 live as access arrives.

Decision point after week 3: POC-1, POC-2, POC-3 browsing and POC-5 Shopify are complete. Together they say whether browsing-flow orders can be seen and confirmed, and whether Shopify is a viable platform. The protocol results arrive later and decide the channel strategy.

## What a failed POC means

- POC-1 fails: the product sees only protocol agents and cloud agents. Browsing agents are treated as human. Smaller product, still coherent.
- POC-2 fails: the evidence claim is downgraded to "approved from this number or inbox". The dispute story weakens materially. Reconsider the wedge.
- POC-3 fails: WhatsApp and email become the only channels, with the consent problem for first-time customers. Confirmation becomes rare by design.
- POC-4 fails: no bank-level rung. Two-rung ladder, as already assumed. Not a blocker.
- POC-5 fails: Shopify and Stripe-hosted merchants get detection and post-payment holds only. Pre-payment control exists only where we host the endpoint, which puts a protocol-hosting component back into scope.
