---
layout: post
title:  "Using EVCA Fund growth method for real estate risk"
categories: airr news
---
Few days ago, we announced the release of our first risk model: Brown-one.

It was still hot out of dev, but we wanted to put it out there. With no more
explanations. Just wanted it out. :-)

Today, we'll show you how one could use this model to produce Value at Risk
metrics applied to illiquid assets.

Lets see how we can use it to quantify risks from a real estate investment.

First things first: an initial cash flow projection.   
We'll consider a very simple projection made of three *important* real estate
cash flows:

* NOI: the net operating income
* Capital expenses
* Terminal Value

Consider the following numbers:

|                  |   y1 |  y2 |   y3 |  y4 |   y5 |
|------------------|-----:|----:|-----:|----:|-----:|
| NOI              |  1.1 | 1.3 |  1.6 | 1.8 |  2.1 |
| Capital expenses | -0.25 |     | -0.35 |     |    -1 |
| Terminal value   |      |     |      |     | 78.9 |
<sup>(numbers are in $ M)</sup>

With a [cost of capital](https://en.wikipedia.org/wiki/Cost_of_capital) of say 10%,
we have a [net present value](https://en.wikipedia.org/wiki/Net_present_value),
at 0, of $53.6M.

What about the *rental income risk*?

After downloading the [template](), we can give it a more specific name. What about *real_estate_rental_risk.xlsx*.

There are 3 tabs.

### Risk Factor

Well. Lets include uncertainty by defining a [stochastic process](https://en.wikipedia.org/wiki/Stochastic_process)
for the NOI. Currently, EVCA Fund growth method only supports the [Geometric Brownian motion](https://en.wikipedia.org/wiki/Geometric_Brownian_motion).
We could give it a try with the following parameters:

|        | initial value |  drift | sigma |
|-------:|--------------:|-------:|------:|
| noi_bm |             1 | 0.1375 |  0.06 |
<sup>(drift and sigma are the ones frm the [classic GBM](https://github.com/airr-templates/brown-one/wiki/Model-config-variables) )</sup>

In the engines output you'll find a tab with a sample of this *noi_bm* process.

### Cash Flows

The Cash flows are the financial objects the risk engine will simulate. We have to slightly adapt our financial projection.
We have to tell the engine that the NOI component should take, at the first 5 annual steps, the corresponding value of the risk factor.

|                  |   y1 |  y2 |   y3 |  y4 |   y5 |
|------------------|-----:|----:|-----:|----:|-----:|
| NOI              |  noi_bm |  noi_bm |  noi_bm |  noi_bm |   noi_bm |
| Capital expenses | -0.25 |     | -0.35 |     |    -1 |
| Terminal value   |      |     |      |     | 78.9 |

Actualy, the risk engine understands more complicated cash flow expressions.
Like *noi_bm-0.25*, or *5/noi_bm*, but this will be for another post.

### Discount Rate

We'll use our cost of capital of 10%

| short rate |
|-----------:|
|       0.10 |

The resulting config file should look like [this]
(https://github.com/airr-templates/evca-fund-growth-method/blob/master/examples/real_estate_rental_income_risk.xlsx?raw=true)
with the above assumptions.



Running the model on [airr] (http://app.airr.io) provides the following statistics:

| average net present value | 1 year 1% Value at Risk |
|--------------------------:|------------------------:|
|                      53.6 |                   -0.25 |

Taking into account risk produces an average net present value of $53.6M.
But, having histograms and distributions, we have risk metrics. And, they tell
us that 99% of the time, the investment would not lose more that $250k after 1 year.

Detailed output for this run can be downloaded [here] (https://github.com/airr-templates/evca-fund-growth-method/blob/master/examples/real_estate_rental_income_risk_output.xlsx?raw=true).
