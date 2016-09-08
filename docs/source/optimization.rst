Optimization
============

To compute several different parameterizations of the same system we can use the ``optimize`` function. To use this feature, place a comment next to the parameter you wish to scan. Within the comment place the parameter domain and step-size between two pound signs (similar to the Matlab vector notation). For our example system, we will scan over ``periodLong`` and ``periodRecent``:

.. code-block:: matlab

	periodLong    = 200; %#[150:10:300]#
	periodRecent  =  40; %#[20:5:60]#

With these settings we choose to vary periodLong from 150 to 300 in steps of 10, and periodRecent from 20 to 60 in steps of 5. To evaluate the entire parameter space, input the following into the Matlab prompt.

``>> res = optimize('trendfollowing')``

Press return to compute all instances of that parameter space. The result of the optimization is stored in the struct res. It contains the statistics of each instance--Sharpe Ratio, average yearly performance, average volatility, etc.

To find the parameters of the system with the best Sharpe Ratio, enter into the prompt:

``>> [m mIx] = max(res.sharpe)``

Next we just only need look up the parameter values for the optimal instance. To find the parameter values, we find the parameter value at the optimal index, mIx. To do this type the following at the Matlab prompt:

``>> res.periodLong(mIx)``

``>> res.periodRecent(mIx)``
