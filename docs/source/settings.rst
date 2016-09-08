.. _settings-label:

Settings
========

Settings contain your environment variables and are open to being defined and extended with additional fields being added to the settings struct.

Below is an example of configuring settings:

.. code-block:: matlab

    function [p, settings] = trendfollowing(DATE, OPEN, HIGH, LOW, CLOSE, VOL, exposure, equity, settings)
    settings.markets     = {'CASH', 'F_AD', 'F_BO', 'F_BP', 'F_C', 'F_CC', 'F_CD', 'F_CL', 'F_CT', 'F_DX', 'F_EC', 'F_ED', 'F_ES', 'F_FC', 'F_FV', 'F_GC', 'F_HG', 'F_HO', 'F_JY', 'F_KC', 'F_LB', 'F_LC', 'F_LN', 'F_MD', 'F_MP', 'F_NG', 'F_NQ', 'F_NR', 'F_O', 'F_OJ', 'F_PA', 'F_PL', 'F_RB', 'F_RU', 'F_S', 'F_SB', 'F_SF', 'F_SI', 'F_SM', 'F_TU', 'F_TY', 'F_US', 'F_W', 'F_XX', 'F_YM'};
    settings.samplebegin = 20120506;
    settings.sampleend   = 20150506;
    settings.lookback    = 504;
    settings.slippage    = 0.05;
    settings.budget      = 1000000;


Default Settings
----------------

Your algorithm will always be called with the following settings by default unless you define them otherwise.

.. code-block:: matlab

    settings.lookback      = 504;
    settings.budget        = 1000000;
    settings.slippage      = 0.05;


Note that you aren't required to define any settings field, but for most trading systems **lookback** will likely need to be changed from 1 if your trading system needs any historical market data. Leaving **lookback** at the default value while attempting to access market prices further back than the previous day will result in an error.

Markets
-------

Currently Quantiacs trades in both Stock and Futures markets. A Future is a contract to deliver a specified amount of the underlying product at a certain date. You can learn more about quant trading with Futures from the video below.

.. raw:: html

    <iframe width="712" height="400" src="https://www.youtube.com/embed/5zKq4GZfnjs" frameborder="1" allowfullscreen></iframe>


Trading in both Stocks and Futures allows for a great degree of diversification and helps you to generate a trading system that is successful in many market environments. You can find the full list of available markets on Quantiacs' `Markets`_ page.

.. _Markets: https://quantiacs.com/For-Quants/GetStarted/Markets.aspx

In general, you can find the contract unit of any futures by looking at the `Futures Contracts Specifications`_. And finding the contract unit allows you to relate a futures contract to their commonly found prices such as on Yahoo Finance.

.. _Futures Contracts Specifications: http://www.barchart.com/futures/specifications.php

Another unique aspect of futures contracts that doesn't apply to stocks, is an expiration date on each contract. The only real difference in your trading data is that the Quantiacs toolbox automatically implements a roll over (see :ref:`rollover-label` section below) whenever a futures contract expires, to make the pricing data the latest and most accurate.

.. _marketdata-label:

Market Data
-----------

Note that Quantiacs market data is not updated in realtime, this means you won't be able to do live trading using the toolbox. If a market is not defined for some time period, its values are set to not a number (NaN). If data is missing within the time series (for example due to a holiday), the missing data is replaced by the last known value with zero volume.

If you want to backtest across every available market, you can get a text file with a string for all of the markets `here`_. This makes it easy to copy paste all of the available markets into your settings struct.

.. _here: https://quantiacs.com/Data/markets.txt

Here is an example of what the data looks like for a futures contract:

+----------+-------------------+-------+-------+------+------+---+---+-------+
| F_AD     |             Australian Dollar                                   |
+----------+---------+---------+-------+-------+------+------+---+---+-------+
| DATE     | OPEN    | HIGH    | LOW   | CLOSE | VOL  | OI   | P | R | RINFO |
+==========+=========+=========+=======+=======+======+======+===+===+=======+
| 19900102 | 77300   | 77400   | 77020 | 77020 | 125  | 2559 | 0 | 0 | 0     |
+----------+---------+---------+-------+-------+------+------+---+---+-------+
| 19900103 | 76890   | 77030   | 76700 | 76740 | 1495 | 3215 | 0 | 0 | 0     |
+----------+---------+---------+-------+-------+------+------+---+---+-------+
| 19900104 | 77080   | 77610   | 77000 | 77490 | 932  | 3122 | 0 | 0 | 0     |
+----------+---------+---------+-------+-------+------+------+---+---+-------+
| 19900105 | 77050   | 77280   | 76980 | 76980 | 272  | 2542 | 0 | 0 | 0     |
+----------+---------+---------+-------+-------+------+------+---+---+-------+
| 19900108 | 77280   | 77300   | 77090 | 77200 | 177  | 2439 | 0 | 0 | 0     |
+----------+---------+---------+-------+-------+------+------+---+---+-------+
| 19900109 | 77289   | 77370   | 77210 | 77290 | 106  | 2379 | 0 | 0 | 0     |
+----------+---------+---------+-------+-------+------+------+---+---+-------+

And here is what the data fields look like for a stock:

+------------+--------------------------------------------------------------+
| AAPL       | (Apple)                                                      |
+------------+----------+----------+----------+----------+------------+-----+
| DATE       | OPEN     | HIGH     | LOW      | CLOSE    | VOL        | P   |
+============+==========+==========+==========+==========+============+=====+
|            |          |          |          |          |            |     |
|   20010102 |   1.067  |   1.0893 |   1.0446 |   1.0625 |   1.12E+08 |   0 |
|            |          |          |          |          |            |     |
+------------+----------+----------+----------+----------+------------+-----+
|            |          |          |          |          |            |     |
|   20010103 |   1.0357 |   1.192  |   1.0313 |   1.1696 |   2.02E+08 |   0 |
|            |          |          |          |          |            |     |
+------------+----------+----------+----------+----------+------------+-----+
|            |          |          |          |          |            |     |
|   20010104 |   1.2946 |   1.3125 |   1.2054 |   1.2188 |   1.84E+08 |   0 |
|            |          |          |          |          |            |     |
+------------+----------+----------+----------+----------+------------+-----+
|            |          |          |          |          |            |     |
|   20010105 |   1.2098 |   1.2411 |   1.1473 |   1.1696 |   1.02E+08 |   0 |
|            |          |          |          |          |            |     |
+------------+----------+----------+----------+----------+------------+-----+
|            |          |          |          |          |            |     |
|   20010108 |   1.2098 |   1.2098 |   1.1384 |   1.183  |   92568000 |   0 |
|            |          |          |          |          |            |     |
+------------+----------+----------+----------+----------+------------+-----+
|            |          |          |          |          |            |     |
|   20010109 |   1.2009 |   1.2589 |   1.183  |   1.2277 |   1.44E+08 |   0 |
|            |          |          |          |          |            |     |
+------------+----------+----------+----------+----------+------------+-----+

The P column is for backwards compatibility to support the Quantiacs 1.X Toolbox versions. OI represents open interest for futures contracts, and R and RINFO both provide information about futures contracts roll overs (see :ref:`rollover-label` section below).

Loading Market Data
-------------------

Whenever you run ``runts``, it will automatically download the necessary market data. When backtesting across new markets, or a new sample size, the toolbox will automatically download the corresponding market data if it hasn't been downloaded before.

To manually initiate this process, you can use the command ``loadData``. You can find a full breakdown of ``loadData`` under :ref:`reference-label` section. The main argument loaddata needs is the settings struct.

.. _rollover-label:

Roll Overs (R & RINFO)
----------------------

Futures, as opposed to Stocks, come in single contracts with an expiration (delivery) date. This requires that we treat futures contracts slightly different than stocks in the backtester. Since there is an expiration to the contract, we have to sell the contract before the expiry and buy a different contract (of the same underlying) that expires further in the future (this is called ‘rolling' a contract).  There are extra costs and uncertainties associated with this.

Rolling explains why the plot of the prices of the time series (as shown on the website) is not necessarily what you get when you buy and hold that commodity. The differences between the price plot and the trading result are higher for commodities and lower for financial futures, since the cost of carry for a Stock Index Future or a Government Bond is usually very low.

In the market data files (found in the *data* folder of the toolbox), R and RINFO columns address roll overs. The data column R contains the roll announcement - the contract maturity of the new contract (i.e. the contract we're rolling into) in the format yyyymm. RINFO is the roll difference in the time series data. At a roll we back-adjust the data in the lookback window by RINFO to keep the time series data steady. We also adjust the performance by the roll amount since the price difference between the two contracts at the same time is not a win or a loss that can be traded. So our raw data are not continuous contracts, but single contracts.

Here is an example of rollover data from F_AD.txt:

+----------+------------+------------+--------+-----------+
| DATE     | OPEN       | CLOSE      | R      | RINFO     |
+==========+============+============+========+===========+
| 20150902 | 70120.0000 | 70250.0000 | 0      | 0.0000    |
+----------+------------+------------+--------+-----------+
| 20150903 | 70360.0000 | 70100.0000 | 0      | 0.0000    |
+----------+------------+------------+--------+-----------+
| 20150904 | 70080.0000 | 69230.0000 | 201512 | 0.0000    |
+----------+------------+------------+--------+-----------+
| 20150908 | 68820.0000 | 69930.0000 | 0      | -290.0000 |
+----------+------------+------------+--------+-----------+
| 20150909 | 69840.0000 | 69840.0000 | 0      | 0.0000    |
+----------+------------+------------+--------+-----------+
| 20150910 | 69500.0000 | 70480.0000 | 0      | 0.0000    |
+----------+------------+------------+--------+-----------+

Roll overs are all done automatically in ``runts``, and because of this on-the-fly rolling method you always get:

1.  The true Dollar value of the commodity at that point in time - at least for the last data point, i.e. the last row of the CLOSE matrix.
2.  A steady course with no disruptions/gaps because of rolls.

Why Only Daily Data
-------------------

Quantiacs only supports daily historical market data for several reasons. The first is that our investors want scalable strategies that can manage hundreds of millions rather than just hundreds of thousands. As limit orders can only be filled during those times of the session, in which the market trades below the limit, we'd only have a fraction of the session to execute these orders. Naturally this leads to a much lower capacity of the trading strategy. Additionally, if we'd allow limit orders we would have to account for partial fills in the backtest, which could make the backtest results no longer representative in extreme cases.

Secondly, we are a Commodity Trading Advisor registered with the NFA, and we have to comply with the rules of our regulators. We have to protect our institutional clients from front-running, arbitrage and other potentially criminal activities. It's impossible to protect investors trading third party strategies on 1 minute bars. On end of day data we can ensure their protection from criminal activities.

We have to separate the strategic part of the trading system (its logic of when to buy what) strictly from the actual order execution and risk management, that's handled by us (and might actually involve the use of leverage, limit orders, stop loss orders etc.).

Sample Size
-----------

By default, the system will load market data for all dates available, so the backtest will run across the entire 25+ years of historical market data. Alternatively, you have the ability to define the specific start and end dates for your backtests through `beginInSample` and `endInSample` respectively. Both fields follow the format of YYYYMMDD.

Budget
------

Although you can change your budget to any size, it is good to test it at $1 million because that would provide it with the proper scale to effectively trade futures in the real world. Moreover, good trading strategies will show similar results whether they're traded at $1 million or $10 million.

Our backtester, no matter the budget allocated, assumes the ability to purchase non-discrete or fractional amounts of contracts. In reality this is not possible, however, it allows the trading strategy to be evaluated without significant deviation caused by budgets. Since futures generally have a very large contract size, there would be a big difference between real and intended allocations at lower capital sizes.

For example, if you attempted to manage your algorithm with 500k and had the following target allocation:

+--------+------------+-------------------+---------------------+
| Market | Allocation | Cash in market    | Price of 1 contract |
+========+============+===================+=====================+
| F_ES   | 0.5        | 0.5 * 500k = 250k | 104k                |
+--------+------------+-------------------+---------------------+
| F_SI   | 0.2        | 0.2 * 500k = 250k | 79k                 |
+--------+------------+-------------------+---------------------+
| F_GC   | 0.1        | 0.1 * 500k = 50k  | 118k                |
+--------+------------+-------------------+---------------------+
| F_TY   | 0.1        | 0.1 * 500k = 50k  | 126k                |
+--------+------------+-------------------+---------------------+
| F_FV   | 0.1        | 0.1 * 500k = 50k  | 119k                |
+--------+------------+-------------------+---------------------+

Again because of the large contract sizes of futures (and the fact that it is impossible to buy half contracts) a naïve discrete representation would give you 2 contracts F_ES, 1 contract F_SI, and ignore the rest. Thus the real exposure would be:

+--------+-----------------------+---------------------+
| Market | Allocation            | Price of 1 contract |
+========+=======================+=====================+
| F_ES   | 2 * 104 / 500 = 0.416 | 104k                |
+--------+-----------------------+---------------------+
| F_SI   | 1 * 79 / 500 = 0.158  | 79k                 |
+--------+-----------------------+---------------------+
| F_GC   | 0                     | 118k                |
+--------+-----------------------+---------------------+
| F_TY   | 0                     | 126k                |
+--------+-----------------------+---------------------+
| F_FV   | 0                     | 119k                |
+--------+-----------------------+---------------------+
| CASH   | 0.426                 |                     |
+--------+-----------------------+---------------------+

Realistically, any institution would put down at least $1 million to trade futures with. So our non-discrete trading positions turn out to be a better representation of real life trading situations.

Trading Costs
-------------

When writing your trading system, all trading costs are based off **slippage** (see :ref:`slippage-label` section below), for example setting it to 0 will test your system without any trading costs. Trading costs can have a significant effect on the performance of a trading algorithm. The two main contributors to trading costs are commissions and slippage. Commissions are fees charged by the exchange and the broker. You cannot avoid them. In most cases they are quite low compared to amount of your trade. Slippage is the price at which you expected or placed your order and the price at which your order was actually filled. Factors like the liquidity and the volatility contribute to the slippage as well as the volume you want to trade. A good estimate for slippage is the daily range, therefore slippage can be estimated ex post.

In our backtesting toolbox we use a very simple yet conservative approach to estimate slippage and commissions: We take 5% of the daily range as the trading costs. This computes as (HIGH - LOW) * 0.05. This covers the assumption, that you'll have more slippage on days with larger market moves, than on days with smaller. This approximation might overestimate the real trading costs. In this case, it is better to overestimate than underestimate.

.. _slippage-label:

Slippage
^^^^^^^^

Slippage is the difference between the price at which you expected or placed your order and the price at which your order was actually filled. The following factors contribute to the slippage. The liquidity of the market: Higher liquidity results in lower slippage. In very liquid markets your positions are filled almost immediately. In an illiquid market, the order execution could cost significant time, in this time the price might move against you. You will notice that the impact of slippage on your trading system depends on how frequently you trade and how much return each trade generates. If you trade often and have trades with smaller returns per trade, slippage will be an issue. If you don't change the size of your exposure often, slippage will be almost irrelevant for your results. These factors contribute to slippage:

* Your trading volume: The more shares you want to buy, the longer the order execution takes. The longer the order execution takes, the further the fill price might be.
* The bid-ask spread: This is the difference in the price quoted for an immediate buy (ask) and the price quoted for an immediate sale (bid). To get an order filled, you usually have to cross the spread. This is typically on the far side for you. If you want to sell 100 shares of Stock X you need to find a buyer for them. If there is one in the orderbook you will find him at the other side of the spread. Large bid/ask spreads lead to a high slippage.
* The volatility of the market: For the sake of simplicity, let's define volatility as the average change of price per unit of time. Thus, if the volatility is high, it's evident that slippage will be higher in volatile markets since prices tend to move more while your order is executed.

Extensibility and Custom Fields
-------------------------------

The best part of settings is the ability to add custom fields to the settings struct.

.. code-block:: matlab

    settings.anotherField =  some_value

The only way to retain custom data from your trading system across multiple instances is the settings struct. Let's say you build a custom indicator, and you want to save the values it generates and make them available to your trading system. Remember that your trading system is just one big function that is called again for each new day of market data. In other words, nothing within your trading system (except the market positions) is saved across multiple dates. Settings are an exception to this rule, and they remain intact during the entire backtest. This allows you to record custom values and datatypes by adding your own fields to settings.
