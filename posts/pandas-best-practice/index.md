# Pandas Best Practices



{{&lt; admonition type=info title=&#34;What is this article about?&#34; open=true &gt;}}

This article provides a collection of the daily best practices and use cases of the Pandas library. It is meant to be a rolling release note by which I will mark down all of the better practice I came across along my journey towards data science. Accordingly, the list is going to be (very) long thus feel free to use the table of content to jump over to the topic you may interest.

{{&lt; /admonition &gt;}}


&lt;!--more--&gt;

## What is Pandas?
{{&lt; image src=&#34;/posts/pandas-best-practice/not_pandas.jpg&#34; caption=&#34;This is not a Panda or the Pandas I&#39;m talking about.&#34; title=&#34;credit: www.unsplash.com&#34;&gt;}}

**[Pandas](https://pandas.pydata.org/docs/getting_started/index.html)** is one of the most-used standard Python library when dealing with matrix-like transformable datasets which range from a simple tabular to a more complex structure dataset. It is an open-source library providing faster, flexible, and expressive mean for data manipulation and one of the essential to learn library in case you are getting in the data science field.&lt;br&gt; 
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

{{&lt; figure src=&#34;/posts/pandas-best-practice/pandas_main_comps.png&#34; title=&#34;Pandas main components&#34; alt=&#34;credit: www.learndatasci.com&#34; height=&#34;500&#34; width=&#34;600&#34;&gt;}}


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
InteractiveShell.ast_node_interactivity = &#34;all&#34;
# whether to display all DataFrame rows and cols 
pd.set_option(&#39;display.max_rows&#39;, None)
pd.set_option(&#39;display.max_columns&#39;, None)  
```
{{&lt; admonition type=info open=true &gt;}}
- For the full data analysis and visualization tasks, pandas is often incomplete and you need to have hands-on skills to other Python libraries like Numpy, Matplotlib or Seaborn but they are not mandatory. They are only used here to help us generating dataset or ploting important findings.
- In this article, additional information about the code is explained using Python comments.

{{&lt; /admonition &gt;}}



### 2. Get to know the dataset 
This is the first essential tasks that you would perform before getting started your analysis. Understanding the dataset may extend from knowing what&#39;s inside, its content, its shape, the variable names, the variable datatypes, the ratio of null values, or going deeper to know the number of unique values of categorical variables, or even gather the descriptive statistics or the variable distribution. The following code snippet shows a few essential functions along with the must to know arguments to answer these questions.   

```Python 
police = pd.read_csv(&#39;datasets/police_project.csv&#39;, 
                 sep=&#39;,&#39;, 
                 parse_dates=[&#39;stop_date&#39;],
                 index_col=None,
                 usecols=[&#39;stop_date&#39;, &#39;driver_gender&#39;,&#39;driver_age_raw&#39;, &#39;violation&#39;])

police.head(3)
police.info(memory_usage=&#39;deep&#39;)
police.dropna().describe(include=[np.number, np.datetime64, np.object])
police.select_dtypes(include=[&#39;number&#39;, &#39;object&#39;]).describe()
```

{{&lt; admonition type=question title=&#34;Output&#34; open=false &gt;}}

**`df.read_csv()`** is one of the most common Pandas method designed specifically, but not limited to, to read a CSV data source. By default, CSV columns are separated by comma `,` as the name stands for but sometimes the data source comes with different column character separation such as `;`, `|`,`\tab`. In such case, you can explicitly specify the separation character using the argument `sep`. Here, the argument `parse_dates` is used to format the date column to be of type `datetime` and `index_col` is the way to set some variable as the DataFrame index. You can pass a list of columns to this argument. Using `usecols`, you can define a set of columns in which you are about to perform the analysis.

&lt;br&gt;

**`df.head(3)`** shows the top 3 rows. You can specify as many rows as you want. Without specification, it will give the top 5 rows. 

&lt;table border=&#34;1&#34; class=&#34;dataframe&#34;&gt;
  &lt;thead&gt;
    &lt;tr style=&#34;text-align: right;&#34;&gt;
      &lt;th&gt;&lt;/th&gt;
      &lt;th&gt;stop_date&lt;/th&gt;
      &lt;th&gt;driver_gender&lt;/th&gt;
      &lt;th&gt;driver_age&lt;/th&gt;
      &lt;th&gt;violation&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;th&gt;0&lt;/th&gt;
      &lt;td&gt;2005-01-02&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;20.0&lt;/td&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;1&lt;/th&gt;
      &lt;td&gt;2005-01-18&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;40.0&lt;/td&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;2&lt;/th&gt;
      &lt;td&gt;2005-01-23&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;33.0&lt;/td&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
    &lt;/tr&gt;
  &lt;/tbody&gt;
&lt;/table&gt;


&lt;br&gt;

**`df.info()`** will trough us some important properties of the entire dataset. By specifying `memory_usage=&#39;deep&#39;` it will introspect and return the exact amount of memory consumption. 
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

&lt;br&gt; 

**`dropna().describe()`** and **`dropna().select_dtypes().describe()`** are two way to get a quick summary of statistics by filtering columns by data types. `include=[np.number, np.datetime64, np.object]` and `select_dtypes(include=[&#39;number&#39;])` filter out columns by data types and by default the first one will include all the columns whereas the second will through an error as `select_dtypes()` required at least one parameter. The first doesn&#39;t have any effect in this case as all the dataset dtypes are including in the list whereas the second only outputs column number of type `number` which is here `driver_age_raw`. We can notice from the first output that some of the outputs are `NaN` and that is so because properties are dtypes dependent. Some properties that can apply to object data cannot apply to other data types and vice-versa.

&lt;table border=&#34;1&#34; class=&#34;dataframe&#34;&gt;
  &lt;thead&gt;
    &lt;tr style=&#34;text-align: right;&#34;&gt;
      &lt;th&gt;&lt;/th&gt;
      &lt;th&gt;stop_date&lt;/th&gt;
      &lt;th&gt;driver_gender&lt;/th&gt;
      &lt;th&gt;driver_age_raw&lt;/th&gt;
      &lt;th&gt;violation&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;th&gt;count&lt;/th&gt;
      &lt;td&gt;86405&lt;/td&gt;
      &lt;td&gt;86405&lt;/td&gt;
      &lt;td&gt;86405.000000&lt;/td&gt;
      &lt;td&gt;86405&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;unique&lt;/th&gt;
      &lt;td&gt;3767&lt;/td&gt;
      &lt;td&gt;2&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;6&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;top&lt;/th&gt;
      &lt;td&gt;2012-02-28 00:00:00&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;freq&lt;/th&gt;
      &lt;td&gt;64&lt;/td&gt;
      &lt;td&gt;62895&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;48461&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;first&lt;/th&gt;
      &lt;td&gt;2005-01-02 00:00:00&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;last&lt;/th&gt;
      &lt;td&gt;2015-12-31 00:00:00&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;mean&lt;/th&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;1970.536080&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;std&lt;/th&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;110.514739&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;min&lt;/th&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;0.000000&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;25%&lt;/th&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;1967.000000&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;50%&lt;/th&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;1980.000000&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;75%&lt;/th&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;1987.000000&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;max&lt;/th&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
      &lt;td&gt;8801.000000&lt;/td&gt;
      &lt;td&gt;NaN&lt;/td&gt;
    &lt;/tr&gt;
  &lt;/tbody&gt;
&lt;/table&gt;

&lt;table border=&#34;1&#34; class=&#34;dataframe&#34;&gt;
  &lt;thead&gt;
    &lt;tr style=&#34;text-align: right;&#34;&gt;
      &lt;th&gt;&lt;/th&gt;
      &lt;th&gt;driver_age_raw&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;th&gt;count&lt;/th&gt;
      &lt;td&gt;86414.000000&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;mean&lt;/th&gt;
      &lt;td&gt;1970.491228&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;std&lt;/th&gt;
      &lt;td&gt;110.914909&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;min&lt;/th&gt;
      &lt;td&gt;0.000000&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;25%&lt;/th&gt;
      &lt;td&gt;1967.000000&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;50%&lt;/th&gt;
      &lt;td&gt;1980.000000&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;75%&lt;/th&gt;
      &lt;td&gt;1987.000000&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;max&lt;/th&gt;
      &lt;td&gt;8801.000000&lt;/td&gt;
    &lt;/tr&gt;
  &lt;/tbody&gt;
&lt;/table&gt;
&lt;br&gt;

{{&lt; /admonition &gt;}} 

### 3. Generate data sample and rename columns
Generating synthetic sample may help us in some situation like conducting an experiment, for instance, testing algorithms, making visualization to better understand changes over your experiment, and so on. There are several ways of generating samples for any purpose but the following made use of Pandas and Numpy to make it simple.    

```Python
# Create a dataframe and rename columns 
# Eg. 1
foo = pd.DataFrame({&#39;Col One&#39;: [100, 200], &#39;Col Two&#39;: [300, 400]})
foo.rename({&#39;Col One&#39;: &#39;col_one&#39;, &#39;Col Two&#39;: &#39;col_two&#39;}, axis=1, inplace=True)
# Eg. 2
bar = pd.DataFrame(np.random.rand(4, 5), columns=list(&#39;abcde&#39;))
bar.add_prefix(&#39;X_&#39;)
# 
```

{{&lt; admonition type=question title=&#34;Output&#34; open=false &gt;}}
Here we have 2 different ways to create DataFrame and later renaming columns. The first will create a 2 columns and 2 rows DataFrame then renames the column names using the function `rename()`. 

&lt;table border=&#34;1&#34; class=&#34;dataframe&#34;&gt;
  &lt;thead&gt;
    &lt;tr style=&#34;text-align: right;&#34;&gt;
      &lt;th&gt;&lt;/th&gt;
      &lt;th&gt;col_one&lt;/th&gt;
      &lt;th&gt;col_two&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;th&gt;0&lt;/th&gt;
      &lt;td&gt;100&lt;/td&gt;
      &lt;td&gt;300&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;1&lt;/th&gt;
      &lt;td&gt;200&lt;/td&gt;
      &lt;td&gt;400&lt;/td&gt;
    &lt;/tr&gt;
  &lt;/tbody&gt;
&lt;/table&gt;

&lt;br&gt;

The second consists of constructing random samples and names columns after the list passed over the `columns` attribute then prefixes these names using the method `add_prefix()`

&lt;table border=&#34;1&#34; class=&#34;dataframe&#34;&gt;
  &lt;thead&gt;
    &lt;tr style=&#34;text-align: right;&#34;&gt;
      &lt;th&gt;&lt;/th&gt;
      &lt;th&gt;X_a&lt;/th&gt;
      &lt;th&gt;X_b&lt;/th&gt;
      &lt;th&gt;X_c&lt;/th&gt;
      &lt;th&gt;X_d&lt;/th&gt;
      &lt;th&gt;X_e&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;th&gt;0&lt;/th&gt;
      &lt;td&gt;0.226161&lt;/td&gt;
      &lt;td&gt;0.588816&lt;/td&gt;
      &lt;td&gt;0.335782&lt;/td&gt;
      &lt;td&gt;0.866124&lt;/td&gt;
      &lt;td&gt;0.274059&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;1&lt;/th&gt;
      &lt;td&gt;0.364500&lt;/td&gt;
      &lt;td&gt;0.021647&lt;/td&gt;
      &lt;td&gt;0.243754&lt;/td&gt;
      &lt;td&gt;0.628392&lt;/td&gt;
      &lt;td&gt;0.124118&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;2&lt;/th&gt;
      &lt;td&gt;0.507057&lt;/td&gt;
      &lt;td&gt;0.769180&lt;/td&gt;
      &lt;td&gt;0.364644&lt;/td&gt;
      &lt;td&gt;0.246468&lt;/td&gt;
      &lt;td&gt;0.716658&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;3&lt;/th&gt;
      &lt;td&gt;0.865142&lt;/td&gt;
      &lt;td&gt;0.201212&lt;/td&gt;
      &lt;td&gt;0.165379&lt;/td&gt;
      &lt;td&gt;0.397729&lt;/td&gt;
      &lt;td&gt;0.195590&lt;/td&gt;
    &lt;/tr&gt;
  &lt;/tbody&gt;
&lt;/table&gt;

{{&lt; /admonition &gt;}} 


### 4. Reverse row and column order
These are basic operations which consist of reversing the original order of the DataFrame row-wise or column-wise. Here, `reset_index()` is used to drop the old index and create a new one starting from 0. We can notice the difference between the first and the second output table. 

```Python 
# Reverse rows
police.loc[::-1, :].head(3)
# Reverse rows and drop the old index 
police.loc[::-1, :].reset_index(drop=True).head(3)
# Reverse columns
police.loc[:, ::-1].head(3)
```

{{&lt; admonition type=question title=&#34;Output&#34; open=false &gt;}}

`police.loc[::-1, :].head(3)`
&lt;table border=&#34;1&#34; class=&#34;dataframe&#34;&gt;
  &lt;thead&gt;
    &lt;tr style=&#34;text-align: right;&#34;&gt;
      &lt;th&gt;&lt;/th&gt;
      &lt;th&gt;stop_date&lt;/th&gt;
      &lt;th&gt;driver_gender&lt;/th&gt;
      &lt;th&gt;driver_age_raw&lt;/th&gt;
      &lt;th&gt;violation&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;th&gt;91740&lt;/th&gt;
      &lt;td&gt;2015-12-31&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;1959.0&lt;/td&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;91739&lt;/th&gt;
      &lt;td&gt;2015-12-31&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;1993.0&lt;/td&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;91738&lt;/th&gt;
      &lt;td&gt;2015-12-31&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;1992.0&lt;/td&gt;
      &lt;td&gt;Moving violation&lt;/td&gt;
    &lt;/tr&gt;
  &lt;/tbody&gt;
&lt;/table&gt;

&lt;br&gt;


`police.loc[::-1, :].reset_index(drop=True).head(3)`
&lt;table border=&#34;1&#34; class=&#34;dataframe&#34;&gt;
  &lt;thead&gt;
    &lt;tr style=&#34;text-align: right;&#34;&gt;
      &lt;th&gt;&lt;/th&gt;
      &lt;th&gt;stop_date&lt;/th&gt;
      &lt;th&gt;driver_gender&lt;/th&gt;
      &lt;th&gt;driver_age_raw&lt;/th&gt;
      &lt;th&gt;violation&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;th&gt;0&lt;/th&gt;
      &lt;td&gt;2015-12-31&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;1959.0&lt;/td&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;1&lt;/th&gt;
      &lt;td&gt;2015-12-31&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;1993.0&lt;/td&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;2&lt;/th&gt;
      &lt;td&gt;2015-12-31&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;1992.0&lt;/td&gt;
      &lt;td&gt;Moving violation&lt;/td&gt;
    &lt;/tr&gt;
  &lt;/tbody&gt;
&lt;/table&gt;

&lt;br&gt;

`police.loc[:, ::-1].head(3)`
&lt;table border=&#34;1&#34; class=&#34;dataframe&#34;&gt;
  &lt;thead&gt;
    &lt;tr style=&#34;text-align: right;&#34;&gt;
      &lt;th&gt;&lt;/th&gt;
      &lt;th&gt;violation&lt;/th&gt;
      &lt;th&gt;driver_age_raw&lt;/th&gt;
      &lt;th&gt;driver_gender&lt;/th&gt;
      &lt;th&gt;stop_date&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;
  &lt;tbody&gt;
    &lt;tr&gt;
      &lt;th&gt;0&lt;/th&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
      &lt;td&gt;1985.0&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;2005-01-02&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;1&lt;/th&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
      &lt;td&gt;1965.0&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;2005-01-18&lt;/td&gt;
    &lt;/tr&gt;
    &lt;tr&gt;
      &lt;th&gt;2&lt;/th&gt;
      &lt;td&gt;Speeding&lt;/td&gt;
      &lt;td&gt;1972.0&lt;/td&gt;
      &lt;td&gt;M&lt;/td&gt;
      &lt;td&gt;2005-01-23&lt;/td&gt;
    &lt;/tr&gt;
  &lt;/tbody&gt;
&lt;/table&gt;
&lt;br&gt;

{{&lt; /admonition &gt;}} 
























&lt;!--Sources --&gt;
&lt;!-- 1. https://jakevdp.github.io/PythonDataScienceHandbook/03.08-aggregation-and-grouping.html --&gt;
&lt;!-- 2. Python Data Science Handbook, ESSENTIAL TOOLS FOR WORKING WITH DATA, Jake VanderPlas --&gt;
&lt;!-- 3. Pandas Cookbook, Recipes for Scientific Computing, Time Series Analysis and Data Visualization using Python, Theodore Petrou --&gt;
















