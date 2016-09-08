Performance
===========

What makes your system a good system? Universally defining the quality of a system is impossible, but a good first approximation is the Sharpe Ratio. The Sharpe Ratio compares the realized performance against the risk (or volatility) taken to achieve that performance. Every investor would prefer systems with high performance and low risk. This is why a higher Sharpe Ratio is usually better.
A good way to assess your trading strategy is to separately backtest it against an in-sample and out-of-sample set of historical data. For example, you can first backtest your strategy against 2/3 of the available historical data (the in-sample), optimize/test various parameters, then backtest it against the remaining 1/3 (out-of-sample) historical data. This allows you to get a sense of your trading system's true performance on the out-of-sample data because it won't have any of the optimization bias from your in-sample backtest.


Overfitting
-----------

Overfitting is the natural enemy of trading algorithms. With enough parameters, it’s easy to fit a model perfectly to the past. Mathematically: You can always fit n data points perfectly with a polynomial of degree n-1. In other words, you can build extremely complicated algos that perfectly retell the past but have no clue what happens next. So, here are a few rules of thumb how to avoid overfitting:

* Don’t use too many parameters. You’ll have a hard time to find the best solution in the parameter space. If you manage to isolate an effect with only a few parameters, it might be more stable.
* Test your hypothesis. Don’t use all the data for the development of your algorithm. Save some for a test once you’re done. You’ll have a much higher chance of success on live market data, if your system doesn’t fail your out-of-sample test.
* Avoid putting in knowledge of future market events. This means not stopping your strategy from trading during the 2008 recession, or using Apple as the stock for a long-only strategy since we already know it’s historically a great choice for buying and holding.
* Use walk forward analysis. This is a bit more complex but involves testing your strategy over a small period of time, optimizing it, then testing it on another equal period of time that’s out of sample. The intent is to mimic real world trading of the strategy.
