# Spanish Day-Ahead Battery Arbitrage

Estimates the profit a small battery storage system could earn by charging on
cheap electricity and discharging when prices are high, using real Spanish
day-ahead market data.

## Data
Hourly PVPC prices for August 2026, from REE's public REData API
(apidatos.ree.es) — no authentication required.

## Method
Linear programming model (PuLP + CBC solver). For every hour, the model
decides how much to charge/discharge a 1 MW / 2 MWh battery (90% round-trip
efficiency) to maximize profit, subject to power and capacity limits.

A degradation cost (5 €/MWh cycled) is included in the objective, following
the common practice in the literature of penalizing cycling to avoid
unrealistically aggressive battery use (see e.g. Sandia's Electricity Storage
Handbook, ~2 $/MWh as a conservative O&M reference point).

## Results (August 2026, 696 hours)
- Profit, perfect-foresight, no degradation cost: €15,183.61
- Profit with degradation cost: €14,223.25 (-6.3%)
- Charging hours: 132/696 · Discharging hours: 90/696
  (vs. ~58/58 theoretical minimum for one clean cycle/day)
- Profit with degradation cost: €14,223.25
- Charging hours: 132/696 · Discharging hours: 90/696
  (vs. ~58/58 theoretical minimum for one clean cycle/day)

## Limitations
- Perfect foresight: the model knows all future prices; real operation would
  rely on forecasts, so actual profit would be lower
- No market/grid fees; degradation modeled as a flat cost per MWh cycled,
  not a full electrochemical model
- Single market (Spain), single month sample

## How to run
Open the notebook in Google Colab, run `!pip install pulp pandas requests`,
then execute the cells top to bottom.
