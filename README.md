# Matta ROI Calculator

A lightweight, single-file ROI calculator that quantifies the value of deploying Matta's industrial-AI agents — **Sentry, Gauge, Tally, Trace** — on a discrete-manufacturing line. Built to be used live in a sales or customer workshop and exported to a clean two-page PDF for the client to circulate internally.

**▶️ Live demo:** `https://<your-username>.github.io/matta-roi-calculator/`
*(replace with your URL once GitHub Pages is enabled)*

> Illustrative demo. All figures are examples, not official Matta pricing or benchmarks.

---

## Why I built this

For an industrial-AI platform like Matta, the hardest part of the sale isn't the technology — it's getting a plant or quality leader to **put a number on the problem** Matta solves. A calculator that turns a 20-minute workshop conversation into a credible, one-page ROI case is exactly the kind of tool that shortens a B2B sales cycle.

I built this to show how I'd approach that: tie the product's capabilities to the financial levers a factory actually cares about, keep the inputs to things a leader can answer from memory, and produce something they can forward to their CFO without me in the room.

## The four value levers

The model maps Matta's agents to the four ways they move money on a discrete line:

| Lever | Matta agents | What it captures |
|---|---|---|
| **Scrap & rework** | Sentry + Gauge | Inline defect/measurement catches reduce scrapped and reworked units |
| **Inspection labour** | Sentry + Gauge | Vision replaces manual visual inspection / metrology effort |
| **Throughput / OEE** | Tally | Exposing micro-stoppages and mis-kits recovers good output |
| **Warranty / returns** | Trace | Traceability + digital route cards shrink quality escapes and claims |

## Design decisions (the interesting part)

Every simplification here was deliberate — the goal was a tool a salesperson can actually drive in a live conversation, not a finance-grade model.

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

## How an SE would use it

1. Open the link on a call; fill in the client's volume, costs, scrap/rework, headcount, and warranty spend together.
2. Set the Matta commercial terms (initial investment + subscription) and number of lines.
3. The ROI, payback, and summary update live as you talk.
4. Export the PDF and send it — the client now has a quantified case to take to their CFO.

## Tech

A single, dependency-free `index.html` — HTML, CSS, and vanilla JavaScript. No build step, no server, works fully offline. Opens in any browser; deploys anywhere static.

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
