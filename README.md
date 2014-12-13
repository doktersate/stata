stata
=====
gen date = tq(1961q1) + _n-1 *** generates new variable that makes date as 1961q1
tsset date *** tells stata that it date is time series
format %tq date *** assigns the tq to the actual date

list date CPI Ur in 1/15 *** lists the first 15 of date CPI and Ur

gen logcpi = log(CPI) *** generates new variable

gen infrate = (logcpi - logcpi[_n-1])*400 

tsline Ur *** generates time series plot
tsline infrate *** generates time series plot

list infrate in 1/15 
list l.infrate *** lists the lag of the infrate
list l.infrate l2.infrate l3.infrate l4.infrate in 1/15 *** lists the first, second, third and fourth lags of infrate
list l.Ur in 1/15 *** lists the first lag of unemployment rate

gen accinf = infrate - infrate[_n-1] *** generates new variable

reg accinf Ur *** run regression of accinf on Ur

predict ehat, resid *** retrieve residuals from regression and names it ehat

reg accinf Ur if tin(1961q1, 2003q4) *** run regression but restrict sample period to 1961q1 to 2003q4

*****
tsline g u

label var g "gdp growth"

label var u "% unemployed"

tsline u g, Lpattern (solid dash)

tsline u g if tin (1990q2, 2005q3), Lpattern (solid dash)

list u l.u in 1/10

list u in l1.u l2.u l3.u in 1/10

gen du = u - l.u

list du in 1/10

reg du g if tin(1986q1, 2009q3)

list g lu.g in 1/10

***
