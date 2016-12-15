We fit data points we made up with lines using statsmodels.api.OLS ordinary least sq fit.

All went well with OLS.

We added "noise" to the data by adding random numbers in a gaussian distribution with mean 0 and std 7

All went well with OLS.

We ran into an issue when trying to use the statsmodels.formula.api.WLS function, which does a weighted square fit. 
We tried to pass it 1/errors where the errors were the offset generated to add noise to the data 
(the inverse of the offset is what you want to weight your fit by: more weight is given to datapoints with smaller errors)
We got NaN results everywhere when running WLS.

**The first thing I should have done when we got the NaN result in class for WLS was looking at the input data.**

The solution was obvious when I printed the errors array: the errors we were passing were 1.0/(our offsets from the y = ax+b values), 
but while the offsets can be positive or negative the weights you pass must be positive! 
It does not make sense to have a negative weight. So you want to use the magnitude (i.e. absolute value) of the offset 
to weight the importance of your datapoints in the fit.

**The correct thing to use for the weights in the function is**

**1.0 / np.abs(errors)**
