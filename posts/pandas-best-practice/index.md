# Pandas Best Practices


{{< admonition type=info title="What is this article about?" open=true >}}
This article provides a collection of the daily best practices and use cases of the Pandas library. It is meant to be a rolling release note by which I will mark down all of the better practice I came across along my journey towards data science. 
{{< /admonition >}}


<!--more-->

## What is Pandas?
{{< image src="/posts/pandas-best-practice/not_pandas.jpg" caption="This is not a Panda or the Pandas I'm talking about." title="credit: www.unsplash.com">}}

**[Pandas](https://pandas.pydata.org/docs/getting_started/index.html)** is one of the most-used standard Python library when dealing with matrix-like transformable datasets which range from a simple tabular to a more complex structure dataset. It is an open-source library providing faster, flexible, and expressive mean for data manipulation and one of the essential to learn library in case you are getting in the data science field.<br> 
Mainly, it provides two well-suited data structures to multiple data type include **Series** and **DataFrame**. This article will get into how to use both of them and the case on which one may be helpful over the other in the following.


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
As mentioned earlier and to handle different types of datasets, pandas provides 2 data structures mainly wrapped over Numpy. `Series` is nothing but one dimension representation similar to the Numpy array but structured in the way it is explicitly associate an index with the values. Whereas `DataFrame` is a multi-dimensional array that can be thought of as a collection of series and its rows are also indexed by default unless it is explicitly done. They are pretty similar in terms of operations as one can apply almost the same manipulation to both of them. 

{{< figure src="/posts/pandas-best-practice/pandas_main_comps.png" title="Pandas main components" alt="credit: www.learndatasci.com" height="500" width="600">}}


## Dataset
The dataset being used in the following is either being created manually or loaded from Seaborn repository or the **[Traffic and Pedestrian Stops](https://www.kaggle.com/faressayah/stanford-open-policing-project/notebooks)** by the Police in Rhode Island to make advanced operations. The choice lies in the fact that this dataset has multiple data types from which multiple scenarios can be derived.

## Best practices  

### 1. Importing necessary libraries 

```Python 
import pandas as pd 
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt 

# whether to explicitly show the whole interactive output cells in 
# jupyter notebook rather than showing only the last line.
# use `last_expr` to revert back the default behavior
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
# whether to display all DataFrame rows and cols 
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)  
```
{{< admonition type=info open=true >}}
- For the full data analysis and visualization tasks, pandas is often incomplete and you need to have hands-on skills to other Python libraries like Numpy, Matplotlib or Seaborn but they are not mandatory. They are only used here to help us generating dataset or ploting important findings.
- In this article, additional information about the code is explained using Python comments.

{{< /admonition >}}



### 2. Get to know the dataset 
This is the first essential tasks that you would perform before getting started your analysis. Understanding the dataset may extend from knowing what's inside, its content, its shape, the variable names, the variable datatypes, the ratio of null values, or going deeper to know the number of unique values of categorical variables, or even gather the descriptive statistics or the variable distribution. The following code snippet shows a few essential functions along with the must to know arguments to answer these questions.   

```Python 
df = pd.read_csv('datasets/police_project.csv', 
                 sep=',', 
                 parse_dates=['stop_date'],
                 index_col=None,
                 usecols=['stop_date', 'driver_gender','driver_age_raw', 'violation'])

df.head(3)
df.info(memory_usage='deep')
df.dropna().describe(include=[np.number, np.datetime64, np.object])
```

{{< admonition type=question title="Output" open=true >}}

**`df.read_csv()`** is one of the most common Pandas method designed specifically, but not limited to, to read a CSV data source. By default, CSV is separated by comma `,` as the name stands for but sometimes the file comes with different column separation such as `;`, `|`,`\tab`. In such case, you can explicitly specify the separation character using the argument `sep`. Here, the argument `parse_dates` is used to format the date column to be of type `datetime` and `index_col` is the way to set some variable as the DataFrame index. You can pass a list of columns to this argument. Using `usecols`, you can define a set of columns in which you are about to perform the analysis.

<br>

**`df.head(3)`** shows the top 3 rows. You can specify as many rows as you want. Without specification, it will give the top 5 rows. 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>stop_date</th>
      <th>driver_gender</th>
      <th>driver_age</th>
      <th>violation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2005-01-02</td>
      <td>M</td>
      <td>20.0</td>
      <td>Speeding</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2005-01-18</td>
      <td>M</td>
      <td>40.0</td>
      <td>Speeding</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2005-01-23</td>
      <td>M</td>
      <td>33.0</td>
      <td>Speeding</td>
    </tr>
  </tbody>
</table>

<br>

**`df.info()`** will trough us some important properties of the entire dataset. By specifying `memory_usage='deep'` it will introspect and return the exact amount of memory consumption. 
```shell 
  RangeIndex: 91741 entries, 0 to 91740
  Data columns (total 4 columns):
    #   Column         Non-Null Count  Dtype         
  ---  ------         --------------  -----         
    0   stop_date      91741 non-null  datetime64[ns]
    1   driver_gender  86406 non-null  object        
    2   driver_age     86120 non-null  float64       
    3   violation      86408 non-null  object        
  dtypes: datetime64[ns](1), float64(1), object(2)
  memory usage: 12.0 MB
```

<br> 

**`df.dropna().describe()`** is a way to get a quick summary of statistics of each column. `include=[np.number, np.datetime64, np.object]` filters out columns by data types and by default it includes all the columns. Therefore, it doesn't have any effect in this case as all the dataset dtypes are including in the list. We can notice that some of the outputs are `NaN` and that is so because properties are dtypes dependent. Some properties that can apply to object data cannot apply to other data types and vice-versa.
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>stop_date</th>
      <th>driver_gender</th>
      <th>driver_age</th>
      <th>violation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>86113</td>
      <td>86113</td>
      <td>86113.000000</td>
      <td>86113</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>3767</td>
      <td>2</td>
      <td>NaN</td>
      <td>6</td>
    </tr>
    <tr>
      <th>top</th>
      <td>2012-02-28 00:00:00</td>
      <td>M</td>
      <td>NaN</td>
      <td>Speeding</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>64</td>
      <td>62649</td>
      <td>NaN</td>
      <td>48359</td>
    </tr>
    <tr>
      <th>first</th>
      <td>2005-01-02 00:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>last</th>
      <td>2015-12-31 00:00:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>34.011868</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>std</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.738786</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>min</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>15.000000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.000000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>31.000000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>43.000000</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>max</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>99.000000</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

<br>

{{< /admonition >}} 










