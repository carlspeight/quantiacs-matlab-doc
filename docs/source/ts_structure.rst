Trading System Structure
========================

In MATLAB, your trading system will be a function file that returns market positions and settings. MATLAB's documentation has a great explanation of `how to write functions`_. Every trading system needs the following MATLAB function:

.. _how to write functions: http://www.mathworks.com/help/matlab/ref/function.html

.. code-block:: matlab

    function [p, settings] = ts(DATE, OPEN, HIGH, LOW, CLOSE, VOL, exposure, equity, settings)

Reading from the right of the equals sign we have a trading system called “ts” that takes in DATE, OPEN, HIGH etc. as a parameter. On the left hand side of the equals sign we see parameters we are returning: ``p`` and ``settings``. Your function will be called each day of the backtesting period with the most recent data as arguments. Your function has to define ``p``, your trading system’s market positions for the next day. ``p`` is just an array of numbers [0 1 -1 …] that resemble your market positions (no position, long, short).

Arguments/Parameters
--------------------
Data can be requested for your trading system through the arguments of the function definition. The ``myTradingSystem`` function isn’t required to call any arguments, the example above is just a representation of various values that can be called.

Here is a breakdown of what parameters can be loaded into the trading system:

+--------------+--------------------------------------------------+------------------+
| Parameter    | Description                                      | Dimensions       |
|              |                                                  | (rows x columns) |
+==============+==================================================+==================+
| **DATE**     | a date integer in the                            | Lookback x 1     |
|              | format YYYYMMDD                                  |                  |
+--------------+--------------------------------------------------+------------------+
| **OPEN**     | the first price of the session                   | Lookback x       |
|              |                                                  | # of Markets     |
+--------------+--------------------------------------------------+------------------+
| **HIGH**     | the highest price of the session                 | Lookback x       |
|              |                                                  | # of Markets     |
+--------------+--------------------------------------------------+------------------+
| **LOW**      | the lowest price of the session                  | Lookback x       |
|              |                                                  | # of Markets     |
+--------------+--------------------------------------------------+------------------+
| **CLOSE**    | the last price of the session                    | Lookback x       |
|              |                                                  | # of Markets     |
+--------------+--------------------------------------------------+------------------+
| **VOL**      | number of stocks/contracts                       | Lookback x       |
|              | traded per session                               | # of Markets     |
+--------------+--------------------------------------------------+------------------+
| **exposure** | the realized quantities of your trading system,  | Lookback x       |
|              | or all the trading positions you take            | # of Markets     |
+--------------+--------------------------------------------------+------------------+
| **equity**   | cumulative trading performance in each market,   | Lookback x       |
|              | reflects gains and losses                        | # of Markets     |
+--------------+--------------------------------------------------+------------------+
| **OI, R,     | also available to be called as parameters,       |                  |
| RINFO**      | described in more detail under                   |                  |
|              | :ref:`marketdata-label` section                  |                  |
+--------------+--------------------------------------------------+------------------+

The data is structured with the most recent information in the last index or row. Where **lookback** is simply how many historical data points you want to load in each iteration of your trading system. Given the data is daily, lookback represents the maximum number of days of market data you want to have loaded. Now the two parameters that aren’t explicitly related to market data are **exposure** and **equity**. These two relate to trading in your simulated broker account. Whenever you make a trading position, that’s recorded in **exposure**. And whatever the market result of your trading position is, is then recorded in **equity**. Or simply put: **equity** is what you made with each market over your **lookback** period.
