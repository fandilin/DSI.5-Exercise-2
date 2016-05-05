

```python
import pandas as pd
import numpy as np
from scipy import stats
```


```python
data = pd.io.stata.read_stata('us_job_market_discrimination.dta')[['race','call']]
[sum(data[data.race=='b'].call),sum(data[data.race=='w'].call)]
```




    [157.0, 235.0]



To determine whether race has a significant effect on callback rate, we can compare the rates of callback for black-sounding names and white-sounding names. A binomial test can determine whether the rate of callback is significantly lower for black-sounding names. The binomial distribution can be approximated by the normal distribution as a consequence of the CLT. The null hypothesis is that the callback rate for black-sounding names is equal to that of white-sounding names. The alternative hypothesis is that the callback rate for black-sounding names is less than that of white-sounding names.


```python
brate = sum(data[data.race=='b'].call)/len(data[data.race=='b'])
wrate = sum(data[data.race=='w'].call)/len(data[data.race=='w'])
bsize = len(data[data.race=='b'])
wsize = len(data[data.race=='w'])
brate,wrate,bsize,wsize
```




    (0.064476386036960986, 0.096509240246406572, 2435, 2435)




```python
z = (brate-wrate)/np.sqrt(brate*(1-brate)/bsize+wrate*(1-wrate)/wsize)
p = stats.norm.cdf(z)
z,p
```




    (-4.1155504357300003, 1.9312826037613112e-05)



The chances of observing a difference in callback rates this large by random sampling are very small.


```python
diff = brate-wrate
se = np.sqrt(brate*(1-brate)/bsize+wrate*(1-wrate)/wsize)
diff,se
```




    (-0.032032854209445585, 0.0077833705866767544)



The standard error of the difference in callback rates is 0.008.


```python
zradius_95 = stats.norm.ppf(.975)
zradius_95
```




    1.959963984540054




```python
ci_95 = [diff-zradius_95*se,diff+zradius_95*se]
ci_95
```




    [-0.047287980237660412, -0.016777728181230755]



This is the 95% confidence interval for the difference between callback rates. The analysis shows that there is a significant difference between the callback rates for white-sounding names and black-sounding names. The entire 95% confidence interval lies to the left of the origin.


```python

```
