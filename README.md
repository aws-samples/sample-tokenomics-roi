# AI Token ROI Workbench

A single-file, offline HTML app (`tokenomics_roi.html`) that walks a customer through the return on investment (ROI) of LLM tokens used by **coding agents in engineering** and **AI agents in customer support**. It works backwards from business outcomes to the token spend that drives them.

## Who it's for

- **Presenters** (solutions architects, account teams) running a live ROI conversation with a customer.
- **Customers** who want to see how their own numbers turn into ROI.
- **Engineering and FinOps leads** looking for ways to cut token cost (tab 8, Tokenomics).

The app is vendor-neutral in wording. Token prices are Amazon Bedrock list prices.

## How to use it

Open `tokenomics_roi.html` in any browser. It needs no install, and works offline (only **Get latest token prices** and source links use the network). Work through the tabs in order:

| Tab | What happens |
|---|---|
| 1. Outcomes | Pick the outcome that matters: lower churn, more growth, or lower cost. |
| 2. Customer inputs | Today's numbers and the customer's expectation **with AI at full adoption**, side by side. |
| 3. Tokens & models | Token prices, model mix (up to 3 models with % shares), and tokens per feature and per support conversation. |
| 4. AI adoption & spend | Adoption ramp per year, how extra features become customers, and non-token AI spend. |
| 5. ROI results | One card per outcome: ROI, net benefit, payback, NPV, year-by-year detail. |
| 6. How it was calculated | Every formula with the customer's numbers filled in. |
| 7. What-if | Factors influencing token costs (pick factors, see cost and ROI across 10 values each), sliders, scenario vs. inputs, a model-mix explorer and a "which assumption matters most" chart. Each section collapses. |
| 8. Tokenomics | 16 practical ways to cut token cost and avoid throttling, each under 100 words with a diagram and a source, most with a what-if from your inputs. |
| 9. Measure it | Metrics to baseline before rollout, and tools that measure AI impact. |

Inputs are saved in the browser (localStorage), on your machine only; they don't travel with the file. The current tab is in the address (for example `#whatif`), and a refresh keeps your place and the sections you opened. **Reset to defaults** clears everything.

To share the app, send the HTML file. Nothing else is needed.

## What-if (tab 7)

- **Factors influencing token costs.** Pick any of: prompt cache reads %, cache writes %, long-context requests %, features per engineer, model calls per session and fixed context per call. Each picked factor gets a table of values with input and output tokens per feature, token cost per feature, the change vs today and the yearly engineering token cost; today's value is highlighted. Cache reads and writes never pass 100% together. Prompt cache reads is picked by default.
- **What-if sliders** move several inputs at once; **Scenario vs. your inputs** compares the two side by side.
- **Engineering model mix explorer** tries three models and their shares without changing tab 3, and lists every combination in 25%, 20% or 10% steps, sortable by token cost or ROI.
- **Which assumption matters most?** ranks inputs by how far a fixed change moves ROI.
- Every section collapses, and an **i** button explains each table.

## Tokenomics (tab 8)

16 strategies, each in at most 4 short paragraphs (under 100 words) with a diagram and a source link (AWS documentation, Anthropic or OpenAI docs):

1. Use tiered model selection
2. Use explicit prompt caching, with all 4 cache checkpoints
3. Pick the right cache time to live (TTL)
4. Meet each model's minimum cache length
5. Search inside files instead of reading them whole
6. Ask for structured output
7. Let the model write code that calls the tools
8. Delegate to sub agents
9. Use dynamic tool calling in sub agents
10. Load tools on demand, not all up front
11. Set max_tokens to what you need
12. Know the output-token burndown rate
13. Control thinking tokens
14. Ask for short answers
15. Clear old tool results
16. Fix your biggest token costs first

**What-ifs.** Eleven strategies end with a what-if. Each sits in its own sub-section, closed by default, so you can read the strategy first:

| Section | What-if |
|---|---|
| 1. Tiered models | Single model vs tiered (simple, moderate, complex tiers on the tab 3 models, cheapest first, shares from the tab 3 mix) plus a Haiku 5.5 router: price, cost per feature on each tier, weighted cost, savings per feature and per year. Router tasks per feature are worked out from the tab 3 stages (164 by default; type your own). A heat map shows the tiered cost for every pair of Claude and GPT models on the simple and moderate tiers, ordered by unit cost, green to red, for a complex-tier model you pick; the default combination is outlined. |
| 2. Prompt caching | Cost per feature on each tier model as cache reads go from 0 to 95%, and the extra cost of not moving checkpoint 4 to the end of each turn. |
| 3. Cache TTL | 5-minute vs 1-hour cache cost per session by gap between prompts (2 to 90 minutes). |
| 4. Minimum cache length | Cost when 0 to 100% of prompts are below the model's minimum cache length. |
| 5. Search vs read | Configurable scenario (sessions, turns, share of turns reading files, KB read vs searched, context, model). |
| 6. Structured output | Cost as output tokens fall by 10 to 80%. |
| 7. Programmatic tool calling | Configurable scenario: separate tool calls vs one script for a share of sessions. |
| 10. Tools on demand | All tool definitions up front vs a search tool plus the tools it finds, by number of tools. |
| 14. Short answers | Cost as output falls by 0 to 50%. |
| 15. Clear old tool results | Cost as 0 to 80% of earlier tool results are cleared. |
| 16. Top spenders | Your engineering cost per feature by lifecycle stage and by token type. |

What-ifs use your tab 2 and 3 inputs, so they follow any change there. Inputs you change in tab 8 turn yellow, like on the other tabs; clear one to go back to its default. **Open all** and **Close all** open or close every strategy.

## How ROI is calculated

**ROI = (Return + people-cost savings − AI investment) ÷ AI investment**, summed over the horizon (1–5 years, default 3).

- **AI investment** = token cost + tool licenses + AI assistant platform + measurement tools + one-time enablement.
- **Return** depends on the outcome:

| Outcome | Teams in scope | Return |
|---|---|---|
| 1. Reduce churn | Engineering + support | Customers × (churn without AI − churn with AI) × ARR |
| 2. Grow revenue | Engineering only | Extra customers won from extra features shipped × ARR |
| 3. Reduce or avoid cost | Engineering + support | People no longer needed (or not hired) × fully loaded cost |

**The three outcomes are independent views. Never add them up.** Each carries the full AI spend for its teams. Outcomes 1 and 3 also show each team's own people savings minus AI spend ("By team"), so support can be judged on its own.

**Freed time.** On each result card the user chooses where the time AI saves goes:
- **Ship more:** keep everyone. Outcome 1 gets better quality and service; outcome 2 gets more features. No people savings.
- **Spend less:** keep output the same with fewer people. People savings count, and the churn or growth return falls in proportion.
- **Split:** a share goes to each.

Defaults: outcomes 1 and 2 ship more; outcome 3 spends less.

## Key assumptions

**Engineering**
- The gain is entered either as features each engineer ships per year (without and with AI) or as % time saved. The two are equivalent: time saved = 1 − features per engineer today ÷ with AI. The team ships features per engineer × engineers. Time saved is capped at 90%.
- "With AI" means the same team, every engineer using coding agents, and all freed time spent shipping more.
- Token cost = features built with coding agents × tokens per feature × blended price. Tokens per feature are summed over lifecycle stages (13 standard stages from research to sales POCs, plus 8 extra stages that start unticked); each stage's tokens are worked out from researched factors (sessions, model calls, context per call, artifact size) that the user can open and change, or type over. It includes 30% extra for non-feature work (bugs, maintenance). So token cost follows features shipped, not headcount.
- In outcome 1, freed time goes to quality and service, so tokens are charged on today's features only.
- With 0 engineers, engineering is left out: no features, coding-agent tokens or engineering gain (its enablement and measurement spend on tab 4 still count).

**Customer support**
- The gain comes from handle time: minutes per ticket without AI, minutes per ticket a human still handles with AI help, and the share of tickets the AI resolves without a human.
- Agents with AI = agents today × (1 − AI share) × minutes with AI ÷ minutes today.
- If tickets left for humans take longer than today (minutes with AI above today's), the extra agents are always counted, whatever share of freed time the result card sends to shipping more.
- The share of the day on tickets sets ticket volume and token cost, not the people savings: breaks, training and meetings scale with headcount, so agents needed fall in proportion to ticket work.
- Tickets per year are worked out, not entered: agents × 8 hours × 60 × share of the day on tickets × 230 working days ÷ minutes per ticket.
- AI conversations burn tokens whether or not they resolve the ticket. Tickets humans still handle also burn agent-assist tokens.

**Business**
- Churn and growth gains ramp with adoption. Outcome 1's churn gain needs freed time: if neither team frees any time (no engineering time saved, no support workload removed), its return is 0 and the card says why.
- A customer kept or won keeps paying ARR in later years, less churn with AI, until the end of the horizon (not beyond).
- Customer growth = customers × "% won per 10% more features" × (extra features ÷ 10%).
- The customer count is otherwise flat. Engineering and support grow at their own hiring rates.

**Money and time**
- Adoption ramp (default 60 / 85 / 95 / 100 / 100%) scales both benefits and spend.
- Enablement is paid at month 0, both in payback and in NPV.
- NPV discounts each year's net benefit at the finance discount rate (default 10%).
- Break-even shows how many times token spend could grow before net benefit reaches zero.
- The app assumes every model needs the same tokens for the same work. So a model mix costs the share-weighted average of the models, and the cheapest mix is always 100% on the cheapest model.

**Not modeled**
- Batch, Flex and Priority tiers; 1-hour cache writes in the ROI (tab 8 shows the trade-off); private pricing (edit prices on tab 3 to model a discount).
- Salary inflation, and token price changes over time (use What-if).
- Quality differences between models.
- Minimum staffing (24x7 cover, languages); an AI-only support team with no agents.

## Token prices

- **Source:** AWS Price List API via boto3 (`pricing.get_products`), service codes `AmazonBedrockFoundationModels` and `AmazonBedrock`, region us-east-1, On-Demand, **Standard tier**. Rates are normalized to USD per 1M tokens; per-1K rows are × 1000.
- **Models:** Claude Opus / Sonnet / Haiku 5.5, GPT-6.1 Sol / Terra, GPT-6 Sol / Astra / Luna, Kimi K3, GLM 5.3, plus a Custom model row. Default mix for engineering and support: Opus 5.5 60%, GPT-6.1 Sol 30%, Sonnet 5.5 10%.
- **Two inference options:** US Geo cross-Region (default, 10% above Global) and Global cross-Region.
- **Long-context rates** (Haiku 5.5 above 100K tokens; GPT-6.x above 272K) apply to the share of tokens marked long-context.
- **Prompt cache:** input tokens are split into cache reads, cache writes and uncached input (engineering default 85 / 5 / 10%).
- **GLM 5.3** isn't in the Price List in any region or Bedrock service code (checked 2026-11-11). Its rates come from the Bedrock pricing page.
- **Built-in snapshot:** published 2026-11-11. Every rate was re-derived and cross-checked against the model cards.
- **Get latest prices** downloads the same public Price List files (about 2 MB) in the browser, with no credentials, and applies the same rules (the "ultrafast" tier is excluded; Standard only).

## Default customer (illustrative)

| Area | Defaults |
|---|---|
| Business | 2,000 customers, $50K ARR, churn 5% → 3% with AI, 1.2% more customers per 10% more features |
| Engineering | 100 engineers at $220K, 12 → 16 features per engineer per year (1,200 → 1,600 for the team, 25% time saved), 5% hiring growth |
| Support | 100 agents at $90K, 15 → 10 minutes per ticket, AI resolves 60%, 75% of the day on tickets (552,000 tickets a year), 10% growth |
| AI spend | Coding-agent licenses $20 per engineer per month (support $0), running the AI assistant 10% of its token cost, enablement $50K + $50K, measurement $40K + $20K/year |

These are demo values. Some are optimistic, for example 60% AI resolution. Replace them with the customer's numbers and stress-test them on the What-if tab.

## Design choices

- **One self-contained HTML file.** No server, no external assets, works offline, easy to email. Its only network call is the optional price download.
- **Work backwards from outcomes.** The app starts from what the customer values (churn, growth, cost), then levers, then the token work, then the price.
- **Independent outcome cards** instead of one total, to avoid double counting the same people.
- **Paired "without AI / with AI" inputs at full adoption.** The adoption ramp turns them into yearly values, so adoption is never counted twice.
- **One source of truth per lever.** Where two entry forms exist (features or time saved), one is typed and the other is worked out. Switching modes keeps the ROI unchanged.
- **Fail visibly.** Impossible inputs show a field error and hide ROI ("Fix inputs"). A model mix that doesn't add to 100% hides ROI ("Check model mix").
- **Show what changed.** Any input that differs from its default turns yellow, with the default in its tooltip. Yellow is used only for that and for warnings; input cards alternate blue, green and lavender.
- **Show the math.** Tab 6 prints every formula with live numbers, and help ("?" and "i") buttons explain each input and table.
- **Secure by default.** A Content-Security-Policy lets only the app's own script run and allows only the AWS Price List host for the price download; every saved or downloaded value is escaped before display.
- **Plain, factual wording.** It avoids hype and doesn't endorse any vendor; the Measure tab lists tools in no order of preference.
