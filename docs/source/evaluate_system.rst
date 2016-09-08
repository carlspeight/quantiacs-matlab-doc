Evaluating Your System
======================

Backtesting
-----------

Start testing your system by running MATLAB command:

``>> runts('tsFilename')``

``runts`` loads the market data and calls your tradingsystem for each day of the backtest with the most recent market data. It simulates the equity curve for your output values ``p``. ``runts`` returns a struct with the exposures and the equity curves of the trading system. It computes the performance numbers of your system and plots its Factsheet.

Trading Results
---------------

The answer struct contains the following:

+------------------------------+-----------+--------------------------------------------------+
| Field                        | Data Type | Description                                      |
+==============================+===========+==================================================+
| ans.tsName                   | STRING    | a string that contains the name of your trading  |
|                              |           | system                                           |
+------------------------------+-----------+--------------------------------------------------+
| ans.fundDate                 | INTEGERS  | a list of the dates available to your trading    |
|                              |           | system                                           |
+------------------------------+-----------+--------------------------------------------------+
| ans.fundEquity               | DOUBLE    | a list defining the equity of your portfolio     |
+------------------------------+-----------+--------------------------------------------------+
| ans.marketEquity             | DOUBLE    | a list defining the equity earned in each market |
+------------------------------+-----------+--------------------------------------------------+
| ans.marketExposure           | DOUBLE    | a list containing the equity made from each      |
|                              |           | market in settings.markets                       |
+------------------------------+-----------+--------------------------------------------------+
| ans.errorLog                 | CELL ARRAY| a list describing any errors made in evaluation  |
+------------------------------+-----------+--------------------------------------------------+
| ans.runtime                  | DOUBLE    | a float containing the time required to evaluate |
|                              |           | your system                                      |
+------------------------------+-----------+--------------------------------------------------+
| ans.evalDate                 | INTEGER   | an int that describes the last date of           |
|                              |           | evaluation for your system (in YYYYMMDD format)  |
+------------------------------+-----------+--------------------------------------------------+
| ans.stats                    | STRUCT    | a struct containing the performance statistics of|
|                              |           | your system                                      |
+------------------------------+-----------+--------------------------------------------------+
| ans.settings                 | STRUCT    | a struct containting the settings defined in     |
|                              |           | mySettings                                       |
+------------------------------+-----------+--------------------------------------------------+
| ans.returns                  | STRUCT    | a list defining the market returns of your       |
|                              |           | trading system                                   |
+------------------------------+-----------+--------------------------------------------------+
