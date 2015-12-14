---
layout: post
title:  "Vanilla Real estate"
categories: advicelogic news
---

Let's start with a simple real estate financial projection.

|             Year |    1 |   2 |    3 |   4 |    5 |
|------------------|-----:|----:|-----:|----:|-----:|
| NOI              |  1.1 | 1.3 |  1.6 | 1.8 |  2.1 |
| Capital expenses | 0.25 |     | 0.35 |     |    1 |
| Terminal value   |      |     |      |     | 78.9 |
<sup>(numbers are in $ M)</sup>

With a cost of capital of say 10%, we have a net present value, at 0, of $53.6M.

What about the *rental income risk*?
Well. Lets define a process for the NOI with:

* an initial value, at 0, of 1
* an annual drift of 13.75%
* an annual volatility of 10%

And now, build an [input file] (https://raw.githubusercontent.com/advicelogic/vanilla-cash/master/exapmples/vanilla_real_estate_rental_income_risk.xlsx)
with the above assumptions.

Running the model on [airr] (http://app.airr.io) provides the following statistics:

| Statistics                |       |
|---------------------------|------:|
| average net present value |  53.6 |
| 1 year 1% Value at Risk   | -0.22 |
<sup>(numbers are in $ M)</sup>

Taking into account risk produces an expected net present value of $53.6M.
But, having histograms and distributions, we have risk metrics. And, they tell
us that 99% of the time, I would not lose more that $220k during the first year.
Detailed output for this run is can be downloaded [here] (https://raw.githubusercontent.com/advicelogic/vanilla-cash/master/exapmples/vanilla_real_estate_rental_income_risk_output.xlsx)

What about *yield risk*?
Lets define a different process. For capturing yield risk:

* an initial value, at 0, of 2.66% (which produces the original $78.9M value)
* an annual drift of -2%
* an annual volatility of 5%

And now, lets build another [input file] (https://raw.githubusercontent.com/advicelogic/vanilla-cash/master/exapmples/vanilla_real_estate_rental_yield_risk.xlsx) with the above assumptions.

This time, running the model provides the following statistics:

| Statistics                |       |
|---------------------------|-------|
| average net present value |  59.5 |
| 1 year 1% Value at Risk   |  -2.6 |
<sup>(numbers are in $ M)</sup>

Taking into account the yield risk produces an expected net present value of
$59.5M. This is not a surprise given the favorable outlook we gave to the
yield evolutions (negative drift).
However, this time, the investment is exposed to higher losses through that factor.
Indeed, simulation paths, that can be looked at [here] (https://raw.githubusercontent.com/advicelogic/vanilla-cash/master/exapmples/vanilla_real_estate_rental_income_risk_output.xlsx) tell now that the net present value is now at risk for $2.6M.

*What about combining these two risks?*
That will be for a next post.:-)

You can find these examples input and output files in the [Vanilla cash repo]
