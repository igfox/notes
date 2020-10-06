# Approximate Normality
Many statistical techniques assume that a distribution is approximately normal. How is this confirmed? Can data be made more normal?

## Tests of Normality
https://www.statisticshowto.com/assumption-of-normality-test/
http://webspace.ship.edu/pgmarr/Geo441/Lectures/Lec%205%20-%20Normality%20Testing.pdf (following this)
https://en.wikipedia.org/wiki/Normality_test
Normal distribution has some important properties:
    symmetric (mean == median)
    standard deviation (distance to inflection point) encompasses 68% of data (95% for 2)
can visually compare histogram vs ideal normal for given mean/std
Graphical and statistical techniques
### Graphical
Visual inspection, can be misleading for small sample sizes
#### QQ Plot
Plot quantiles of plots against eachother
    Does this mean take empirical quantile vs quantile under ideal normal?
#### Cumulative Frequency (PP) Plot
#TODO
### Statistical
Uses different approaches to calculate probability that observed sample drawn from a normal distribution, null hypothesis is underlying distribution normal

Each test emphasizes different aspect of normality (ie: range, tail weight, bimodality)
#### W/S Test
Very simple, requires sample std and data range
get 'studentized' range q, which is 
$$
q = w/s
$$
where $s$ is std
accepts if q within critical range, depends on n and level of significance $\alpha$
#### Jarque-Bera Test
#### Shapiro-Wilks Test
#### Kolmogorov-Smirnov Test
#### D'Agostino Test
Very powerful, based on D statistic
$$
D = \frac{T}{\sqrt{n^3 SS}}\\
T = \sum(i-\frac{n+1}{2})x_i
$$
$SS$ is sum of squares of the data (diff from mean), $n$ sample size, $i$ is the order/rank of obs $x$ 
Essentially, weights obs size by rank distance from middle, $T$ indicates which tail has more weight

## Power Transform
https://en.wikipedia.org/wiki/Power_transform
Create monotonic transform of data using power function
Box-Cox
https://www.youtube.com/watch?v=vGOpEpjz2Ks
often trying to regress function like
$$
y = X \beta + \epsilon
$$
but underying relationship between $X$ and $y$ may be nonlinear and errors $\epsilon$ may be non-normal
assume actual relationship has form
$y^\lambda = X \beta + \epsilon$
for unknown $\lambda$ 
do MLE on $\lambda$ 
Has R demo
Power Transform
https://en.wikipedia.org/wiki/Power_transform
generally a function that transforms response variables using exponent and geometric mean:
$$
y_i^{(\lambda)} = 
\begin{cases}
    \frac{y_i^\lambda-1}{\lambda (GM(y))^{\lambda-1}} & \text{if } \lambda \neq 0 \\
    GM(y)\ln(y_i) & \text{else}
\end{cases}
$$
where $GM$ is geometric mean $\sqrt[^n]{y_1 \dots y_n}$

Box-Cox transformation is simpler:
$$
y_i^{(\lambda)} = 
\begin{cases}
    \frac{y_i^\lambda-1}{\lambda} & \text{if } \lambda \neq 0 \\
    \ln(y_i) & \text{else}
\end{cases}
$$





