---
layout: post
title: Evan Miller's Rating Sorting in Python
---

A common problem with a website is how to sort things by their rating.
Evan Miller describes it well in his 2009 post,
[How Not To Sort By Average Rating](http://www.evanmiller.org/how-not-to-sort-by-average-rating.html).

Here is a Python snippet that implements his method:

```python
import math
from scipy import stats

def ci_lower_bound(positive_ratings, total_ratings, confidence):
    """
    Following http://www.evanmiller.org/how-not-to-sort-by-average-rating.html#changes
    :return: the lower bound of the confidence interval
    """
    n =  total_ratings
    z = stats.norm.ppf(1 - (1 - confidence) / 2)
    p_hat = 1.0 * positive_ratings / n
    result = (
        (
             p_hat + z * z / (2 * n)
             - z * math.sqrt(
                 (p_hat * (1 - p_hat) + z * z / (4 * n)) / n
             )
        ) / (1 + z * z / n)
    )
    return result
```


Note that this assumes you have the python package `scipy` installed. You can find it
[here](https://www.scipy.org/install.html).

As Evan Miller notes, your z value will be constant so you can also hardcode it, for example to `1.96` if you want a 95% confidence interval.

You can interpret the output of this function as a corrected rating, which 'punishes' items with a low number of votes.

