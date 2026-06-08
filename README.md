# Matta ROI Calculator

A lightweight, single-file ROI calculator that quantifies the value of deploying Matta's industrial-AI agents — **Sentry, Gauge, Tally, Trace** — on a discrete-manufacturing line. Built to be used live in a sales or customer workshop and exported to a clean two-page PDF for the client to circulate internally.

**▶️ Live demo:** https://doumaire.github.io/matta-roi-calculator/

> Illustrative demo. All figures are examples, not official Matta pricing or benchmarks.

---

## Why I built this

I'm applying to Matta as **Chief of Staff**, and rather than just describe how I think, I wanted to ship something a Matta operator could use on Monday morning.

For an industrial-AI platform, the hardest part of the sale isn't the technology — it's getting a plant or quality leader to **put a number on the problem** Matta solves. A tool that turns a 20-minute workshop into a credible, CFO-ready ROI case is exactly the kind of small lever that compounds: it shortens the sales cycle, gives every account the same disciplined value story, and removes a recurring bottleneck from the go-to-market motion.

That's the lens I'd bring to the role — finding where a bit of structure or tooling unlocks the team, then **building it end-to-end** instead of waiting for someone else to. To make this I taught myself Matta's product and value levers, made the judgment calls about what to model and what to deliberately leave out, and shipped a working, shareable artifact. It's a sample of how I operate, not a finished product.

## The four value levers

Matta frames its agents as "the first step towards factory sentience." The model maps those four agents to the four ways they move money on a discrete line:

| Lever | Matta agents | What it captures |
|---|---|---|
| **Scrap & rework** | Sentry + Gauge | **Sentry** detects known *and* unseen defects inline (down to 8-pixel micro-defects, no manual labelling); **Gauge** adds AI metrology at 100% inspection — fewer bad units escape, so less scrap and rework |
| **Inspection labour** | Sentry + Gauge | Automated vision replaces manual visual inspection and CMM/metrology checks, freeing inspection FTEs |
| **Throughput / OEE** | Tally | **Tally**'s automated counting and per-unit cycle timing give real-time throughput visibility, surfacing micro-stoppages and flow losses that recover good output |
| **Warranty / returns** | Trace | **Trace** digitises route cards and links QC events to each serial/unique ID — stronger containment and traceability shrink quality escapes and claims |

## Judgment calls (the interesting part)

The hard part wasn't the maths — it was deciding what to leave out. A tool only gets used if a busy plant leader can fill it in live, so every simplification below is a deliberate trade of precision for adoption.

- **≤ 13 inputs, all shop-floor-knowable.** A plant manager can answer every field from memory. I deliberately cut anything that needs a finance lookup.
- **Scrap cost via a single "cost to make one unit."** Instead of asking for separate per-scrapped and per-reworked unit costs (numbers that live in finance and vary by part), the client gives *one* make-cost. A scrapped unit loses its full make-cost; a reworked unit is modelled at 30% of it. One easy number replaces two hard ones.
- **Throughput value derived, not asked.** Rather than asking for "contribution margin" (jargon), the client enters a **selling price**; the tool computes profit per unit as `price − make-cost`. It's intentionally conservative (make-cost includes overhead), so the throughput benefit is understated rather than oversold — better for credibility.
- **Matta impact rates are fixed "Given" benchmarks, not sliders.** The percentage improvements (e.g. 30% scrap reduction) are presented as fixed assumptions, so the conversation stays on the *client's* numbers and the output can't be gamed in the room. They live in one place in the code for easy governance.
- **Conservative by default + guardrails.** If savings don't cover the subscription, the tool says "No payback" instead of printing a misleading number.
- **Built for the export.** The whole thing prints to a clean two-page PDF (inputs + assumptions on page 1, results on page 2) with the client name and lines-in-scope in the header — the artifact the client forwards internally.

## What it outputs

- **Total annual savings**, **annual net benefit**, **payback period (months)**, **simple 3-year ROI**
- A **savings-by-lever breakdown** with proportion bars
- An **auto-written client summary** sentence naming the top one or two levers
- A **two-page PDF** for internal sharing

## How the team would use it

1. Open the link on a customer call; fill in the client's volume, costs, scrap/rework, headcount, and warranty spend together.
2. Set the Matta commercial terms (initial investment + subscription) and number of lines.
3. The ROI, payback, and summary update live as you talk.
4. Export the PDF and send it — the client walks away with a quantified case to take to their CFO.

One link, no training, consistent value story across every rep — the kind of small operational asset a Chief of Staff ships to make a go-to-market motion repeatable.

## Tech

A single, dependency-free `index.html` — HTML, CSS, and vanilla JavaScript. No build step, no server, works fully offline. Opens in any browser; deploys anywhere static. Deliberately lightweight: the point was to ship something real and useful fast, not to over-engineer.

## The model, briefly

```
savings_scrap_rework = volume × scrap% × make_cost × 30%
                     + volume × rework% × (make_cost × 30%) × 30%
savings_inspection   = inspection_FTEs × cost_per_FTE × %time_on_inspection × 40%
throughput_savings   = good_units × 3% × (price − make_cost)
savings_warranty     = annual_warranty_cost × 25%

total_annual_savings = sum of the four levers  (× number_of_lines)
annual_net_benefit   = total_annual_savings − annual_subscription
payback_months       = initial_investment ÷ annual_net_benefit × 12
3yr_ROI              = (3 × net_benefit − initial) ÷ (initial + 3 × annual_subscription)
```
