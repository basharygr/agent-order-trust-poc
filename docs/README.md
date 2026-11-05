# docs — one folder per pivot

Each folder holds the documents written while a single product thesis was in force. Read a folder as one position, not as a sequence of decisions. Later pivots do not delete earlier ones; the earlier folder stays as the record of what was believed and why it changed.

Evidence shared across pivots lives outside this tree: `reports/` (research reports) and `research_notes/` (the notes behind them). Terminology is defined once in [GLOSSARY.md](GLOSSARY.md). The e-commerce flow and what agents change at each step is in [ECOMMERCE-FLOW.md](ECOMMERCE-FLOW.md).

| Pivot | Thesis | Docs | Status |
|---|---|---|---|
| [pivot-1-order-trust-layer](pivot-1-order-trust-layer/) | Agents will place wrong orders; merchants need detection, confirmation, signed evidence and a guarantee. Israel travel on Woo, Wix, custom and non-US Shopify. | 01 loss model, 02 directions, 03 brief and competitors, 04 POC definitions | Superseded, Nov 2026. Accuracy research showed always-on confirmation and standalone detection erode; rules and evidence persist. Production data showed agent orders are too few for a loss pitch. |
| [pivot-2-agent-serving-layer](pivot-2-agent-serving-layer/) | Merchants are not at the standards table and their platforms do half the job. Build an end-to-end serving layer for agents: Truth, Serve, Decide, Prove, Guarantee. Enablement is the sale. | 07 story (read first), 05 research conclusions, 06 traffic and coverage map | Current. Doc 06 tested 05 against traffic and platform data and proposed go-to-market changes (Truth first, long tail, per-store pricing, growth pitch). The wedge is not settled; wedge discussions stay in this folder. |

## Conventions

- Doc numbers continue across pivots (the next doc is 08) so a number always identifies one file.
- A new pivot gets a new folder only when the customer, the problem or the product category changes. A change of wedge, sequencing, pricing or pitch stays inside the current pivot's folder.
- Each pivot folder's first doc should state the thesis and what in the previous pivot it replaces.
