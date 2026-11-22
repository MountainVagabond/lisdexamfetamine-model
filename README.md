# lisdexamfetamine-model

A simple model of how lisdexamfetamine and its active metabolite, dexamphetamine (d-amphetamine), may vary over time.

**Live page:** https://mountainvagabond.github.io/lisdexamfetamine-model/

> This is an illustrative model for learning and discussion. It is not medical advice and should not be used to choose or change a dose. Talk to a prescriber or pharmacist about dosing.

## What the chart shows

The chart plots the modelled concentration of d-amphetamine in blood plasma (ng/mL) over the 24 hours after a dose.

Lisdexamfetamine itself is an inactive prodrug. After it's swallowed, enzymes in red blood cells split off the active d-amphetamine. The prodrug clears within a couple of hours, so the curve that matters for effect is the d-amphetamine curve, which is what's shown.

Two dosing patterns are modelled:

- **One-off dose:** a single dose taken with no drug already in the body. The curve starts at zero, peaks about 3.5–4 hours after the dose, then declines.
- **Daily use:** the same dose taken at the same time every 24 hours, after levels have settled into a repeating pattern (steady state, reached after about five days). Some drug is left over from the previous day, so the curve starts above zero and peaks higher than a one-off dose.

## Controls

- **Dose:** 20–70 mg in 10 mg steps (the range of available capsule strengths). Concentration scales in direct proportion to dose.
- **Weight:** 40–120 kg. The model assumes the drug spreads through a volume proportional to body weight, so a heavier person gets a lower concentration from the same dose. Weight does not change the half-life in this model.
- **Spread (One-off / Daily use):** chooses which dosing pattern gets the percentile lines. The other pattern is shown as a single median line, to keep the chart readable.
- **SD menu:** chooses how widely half-life varies between people (see below).

The page opens at neutral starting values of 30 mg (a usual starting dose) and 70 kg (a standard reference adult weight).

## Half-life variation and the percentile lines

The half-life of d-amphetamine (the time for the level to fall by half) differs from person to person. The model treats it as log-normally distributed, which keeps it positive and allows a longer tail on the slow side.

| Setting | Mean | Median | sigma (SD of ln half-life) | mu (mean of ln half-life) |
|---|---|---|---|---|
| SD 2 h | 11.0 h | 10.8 h | 0.180 | 2.382 |
| SD 2.5 h (default) | 11.0 h | 10.7 h | 0.224 | 2.373 |
| SD 3 h | 11.0 h | 10.6 h | 0.268 | 2.362 |

In each setting, mu is adjusted so the arithmetic mean stays at 11 hours.

### Why there are three SD settings

The true spread is uncertain, so the three settings act as a sensitivity test. If a conclusion holds across all three, the uncertainty in the SD doesn't matter much.

- **SD 2 h:** about what controlled studies of healthy volunteers show, roughly 20% variation around the mean. This is the best-supported choice for adults with normal kidney function who aren't taking medications that change urine pH.
- **SD 2.5 h:** a judgement call between the volunteer data and a broader population. It is not a published figure.
- **SD 3 h:** roughly what might be expected once people taking medications that affect urine pH are included.

### Reading the lines

Each line uses the half-life at a given percentile of the distribution. A longer half-life gives a higher, flatter curve.

| Line style | Percentiles | Meaning |
|---|---|---|
| Solid or long dash, thick | 50th (median) | The typical person |
| Short dash | 33.33rd and 66.67th | The middle third of people fall between these |
| Dash-dot | 16.67th and 83.33rd | The middle two-thirds of people fall between these |
| Dotted | 2.5th and 97.5th | 95% of people fall between these |

At the default SD of 2.5 h, these correspond to half-lives of about 6.9, 8.6, 9.7, 10.7, 11.8, 13.3 and 16.7 hours.

### Between people, not day to day

The lines describe variation **between people**. For any one person, they show the range that person's typical half-life is likely to fall in. One person's half-life also varies from day to day (mostly with diet and hydration), but by less, perhaps a third to half of the between-person spread.

The spread matters more for daily use than for a one-off dose, because someone who clears the drug slowly carries more over from each previous day.

## Model details

- **Structure:** one-compartment model with first-order absorption and first-order elimination.
- **Absorption rate (ka):** 0.7 per hour, combining prodrug absorption and conversion into one step. This puts the d-amphetamine peak about 3.5–4 hours after a dose.
- **Volume of distribution:** 3.2 L/kg.
- **Conversion:** 29.48% of the lisdexamfetamine dimesylate dose by weight becomes d-amphetamine (the molecular weight ratio), assuming complete conversion.
- **Daily use:** standard steady-state formula for a repeated dose every 24 hours.

Concentration at time *t* after a one-off dose:

```
C(t) = (Dose × 0.2948 × 1000) / (3.2 × weight) × ka / (ka − ke) × (e^(−ke·t) − e^(−ka·t))
```

where ke = ln 2 / half-life. For daily use, each exponential term is divided by (1 − e^(−k·24)) using its own rate constant.

## Limitations

- Only half-life varies between the lines. Absorption speed and distribution volume are fixed, so real-world spread is wider than shown.
- **Urine pH** has a large effect that the main settings don't capture: acidic urine can shorten the half-life to around 7 hours, and alkaline urine can extend it well beyond 16 hours.
- Kidney function, other medications, food and individual metabolism can all shift the curve.
- There is no established blood level for when effects or side effects (such as trouble sleeping) begin, so the curve cannot tell you how a level will feel.
- Concentrations are population-average estimates, not measured values for any person.

## Files

- `index.html`: the interactive page. It is self-contained apart from loading Chart.js from cdnjs.
