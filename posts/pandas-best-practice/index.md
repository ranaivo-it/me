# Pandas Best Practices


{{< admonition type=info title="What is this article about?" open=true >}}
This article provides a collection of the daily best practices and use cases of the Pandas library. It is meant to be a rolling release note by which I will mark down all of the better practice I came across along my journey towards data science. 
{{< /admonition >}}


<!--more-->

## What is Pandas?
**[Pandas](https://pandas.pydata.org/docs/getting_started/index.html)** is one of the most-used standard Python library when dealing with matrix-like transformable datasets which range from a simple tabular to a more complex structure dataset. It is an open-source library providing faster, flexible, and expressive mean for data manipulation and one of the essential to learn library in case you are getting in the data science field.<br> 
Mainly, it provides two well-suited data structures to multiple data type include **Series** and **DataFrame**. This article will get into both of them on which case one may be helpful over the other in the following.


## Installation
Pandas can be installed via pip from **[PyPI](https://pypi.org/project/pandas/)**
```shell 
pip install pandas
```
or using the [Anaconda](https://docs.anaconda.com/anaconda/install/) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html) environment,
```shell 
conda install pandas
```

## Pandas components
As mentioned above and for handling different types of datasets pandas provides 2 data structures mainly wrapped over Numpy. `Series` is nothing but one dimension representation similar to the Python list but stuctured in the different way and `DataFrame` is a multi-dimensional array which can be thought as a collection of series. They are pretty similar in terms of operation as one can apply almost the same manipulation to both of them. 

![Image credit: www.learndatasci.com](/posts/pandas-best-practice/pandas_main_comps.png "Pandas main components")


## Dataset
The dataset being used in the following is either being created manually or the **[Traffic and Pedestrian Stops](https://www.kaggle.com/faressayah/stanford-open-policing-project/notebooks)** by the Police in Rhode Island available on Kaggle. The choice lies on the fact that this dataset has multiple data types from which multiple scenarios can be derived. 

## Best practices  

### 1. Importing necessary libraries 

```Python 
import pandas as pd 
import numpy as np
import matplotlib.pyplot as plt 
%matplotlib inline 

# whether to explicitly show the whole interactive output of jupyter notebook 
# rather than showing only the last line.
# use `last_expr` to revert back the default behavior
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```
{{< admonition info >}}
- For the full data analysis and visualization tasks, pandas is often incomplete and you need to have hands-on skills to other Python libraries like Numpy, Matplotlib or Seaborn but they are not mandatory. They are only used here to help us generating dataset or ploting important findings.  
- In this article, additional information about the code is explained using Python comments.  
{{< /admonition >}}


### 2. Get to know the data the dataset 
One 









