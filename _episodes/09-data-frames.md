---
title: "Pandas DataFrames"
teaching: 10
exercises: 10
questions:
- "How can I do statistical analysis of tabular data?"
objectives:
- "Select individual values from a Pandas dataframe."
- "Select entire rows or entire columns from a dataframe."
- "Select a subset of both rows and columns from a dataframe in a single operation."
- "Select a subset of a dataframe by a single Boolean criterion."
keypoints:
- "Use `DataFrame.iloc[..., ...]` to select values by integer location."
- "Use `:` on its own to mean all columns or all rows."
- "Select multiple columns or rows using `DataFrame.loc` and a named slice."
- "Result of slicing can be used in further operations."
- "Use comparisons to select data based on value."
- "Select values or NaN using a Boolean mask."
---

## Note about Pandas DataFrames/Series

A [DataFrame][pandas-dataframe] is a collection of [Series][pandas-series];
The DataFrame is the way Pandas represents a table, and Series is the data-structure
Pandas use to represent a column.

Pandas is built on top of the [Numpy][numpy] library, which in practice means that
most of the methods defined for Numpy Arrays apply to Pandas Series/DataFrames.

What makes Pandas so attractive is the powerful interface to access individual records
of the table, proper handling of missing values, and relational-databases operations
between DataFrames.

## Selecting values

To access a value at the position `[i,j]` of a DataFrame, we have two options, depending on
what is the meaning of `i` in use.
Remember that a DataFrame provides a *index* as a way to identify the rows of the table;
a row, then, has a *position* inside the table as well as a *label*, which
uniquely identifies its *entry* in the DataFrame.

## Use `DataFrame.iloc[..., ...]` to select values by their (entry) position

*   Can specify location by numerical index analogously to 2D version of character selection in strings.

~~~
import pandas as pd
data = pd.read_csv('data/DisProt_data_Caenorhabditis-elegans.csv', index_col='DisProt entry ID')
print(data.iloc[0, 0])
~~~
{: .language-python}
~~~
DP00613
~~~
{: .output}

## Use `DataFrame.loc[..., ...]` to select values by their (entry) label.

*   Can specify location by row name analogously to 2D version of dictionary keys.

~~~
data = pd.read_csv('data/DisProt_data_Caenorhabditis-elegans.csv', index_col='DisProt entry ID')
print(data.loc["DP00613", "Start"])
~~~
{: .language-python}
~~~
1
~~~
{: .output}
## Use `:` on its own to mean all columns or all rows

*   Just like Python's usual slicing notation.

~~~
print(data.loc["DP00613", :])
~~~
{: .language-python}
~~~
UniProt ACC           Q22472
Start                      1
End                      378
Reference      pmid:19899809
Length                   378
Name: DP00613, dtype: object
~~~
{: .output}

*   Would get the same result printing `data.loc["DP00613"]` (without a second index).

~~~
print(data.loc[:, "Length"])
~~~
{: .language-python}
~~~
DisProt entry ID
DP00613      378
DP00868      279
DP01090    18562
DP01113      708
DP01313      547
DP01407      106
DP01436      961
DP01437      297
DP01558      211
DP01973      266
DP02025      571
DP02216     1050
DP02313      186
Name: Length, dtype: int64
~~~
{: .output}

*   Would get the same result printing `data["Length"]`
*   Also get the same result printing `data.Length` (since it's a column name)

## Select multiple columns or rows using `DataFrame.loc` and a named slice.

~~~
print(data.loc['DP01113':'DP01407', 'Start':'End'])
~~~
{: .language-python}
~~~
                  Start  End
DisProt entry ID            
DP01113               1  168
DP01313             163  223
DP01407              93  106
~~~
{: .output}

In the above code, we discover that **slicing using `loc` is inclusive at both
ends**, which differs from **slicing using `iloc`**, where slicing indicates
everything up to but not including the final index. 


## Result of slicing can be used in further operations.

*   Usually don't just print a slice.
*   All the statistical operators that work on entire dataframes
    work the same way on slices.
*   E.g., calculate max of a slice.

~~~
print(data.loc['DP01113':'DP01407', 'Length'].max())
~~~
{: .language-python}
~~~
708
~~~
{: .output}

* The function `data.min()` would print the minimum value.

## Use comparisons to select data based on value

*   Comparison is applied element by element.
*   Returns a similarly-shaped dataframe of `True` and `False`.

~~~
# Use a subset of data to keep output readable.
subset = data.loc['DP01113':'DP01407', 'Length']
print('Subset of data:\n', subset)

# Which values were greater than 100 ?
print('\nWhere are values large?\n', subset > 150)
~~~
{: .language-python}
~~~
Subset of data:
 DisProt entry ID
DP01113    708
DP01313    547
DP01407    106
Name: Length, dtype: int64

Where are values large?
 DisProt entry ID
DP01113     True
DP01313     True
DP01407    False
Name: Length, dtype: bool
~~~
{: .output}

## Select values or NaN using a Boolean mask

*   A frame full of Booleans is sometimes called a *mask* because of how it can be used.

~~~
mask = subset > 150
print(subset[mask])
~~~
{: .language-python}
~~~
DisProt entry ID
DP01113    708
DP01313    547
Name: Length, dtype: int64
~~~
{: .output}

*   Get the value where the mask is true, and NaN (Not a Number) where it is false.
*   Useful because NaNs are ignored by operations like max, min, average, etc.

~~~
print(subset[subset > 150].describe())
~~~
{: .language-python}
~~~
count      2.000000
mean     627.500000
std      113.844192
min      547.000000
25%      587.250000
50%      627.500000
75%      667.750000
max      708.000000
Name: Length, dtype: float64
~~~
{: .output}

## Interacting with the data

Pandas vectorizing methods and grouping operations are features that provide users 
much flexibility to analyse their data.

For instance, let's say we want to have a clearer view on how Disprot proteins  
split themselves according to their length.

1.  We may split the entries in two groups, the first including the proteins with a length *higher* 
    than the average and the second with those with a *lower* length.
2.  From this analysis we may identify outliers and remove them with the `drop` function.

~~~
mask_higher = data.loc[:, "Length"] > data.loc[:, "Length"].mean()
~~~
{: .language-python}
~~~
DisProt entry ID
DP00613    False
DP00868    False
DP01090     True
DP01113    False
DP01313    False
DP01407    False
DP01436    False
DP01437    False
DP01558    False
DP01973    False
DP02025    False
DP02216    False
DP02313    False
Name: Length, dtype: bool
~~~
{: .output}

Just one of the entries is above the mean, meaning that is is significantly longer than the others. 
We may remove it and recalculate the mean. 

~~~
new_data = data.drop("DP01090")
mask_higher = new_data.loc[:, "Length"] > new_data.loc[:, "Length"].mean()
print(mask_higher)
~~~
{: .language-python}
~~~
DisProt entry ID
DP00613    False
DP00868    False
DP01113     True
DP01313     True
DP01407    False
DP01436     True
DP01437    False
DP01558    False
DP01973    False
DP02025     True
DP02216     True
DP02313    False
Name: Length, dtype: bool
~~~
{: .output}

> ## Selection of Individual Values
>
> Assume Pandas has been imported into your notebook
> and the DisProt data for E.coli has been loaded:
>
> ~~~
> import pandas as pd
> coli = pd.read_csv('data/DisProt_data_Escherichia_coli.csv', index_col='UniProt ACC')
> ~~~
> {: .language-python}
>
> Write an expression to find the length of P09883.
{: .challenge}
>
> > ## Solution
> > The selection can be done by using the labels for both the row ("Serbia") and the column ("gdpPercap_2007"):
> > ~~~
> > print(df.loc['P09883', 'Length'])
> > ~~~
> > {: .language-python}
> > The output is
> > ~~~
> > 582
> > ~~~
> >{: .output}
> {: .solution}
{: .challenge}

> ## Extent of Slicing
>
> 1.  Do the two statements below produce the same output?
> 2.  Based on this,
>     what rule governs what is included (or not) in numerical slices and named slices in Pandas?
> 
> ~~~
> print(coli.iloc[0:2, 0:2])
> print(coli.loc['P09883':'P08083', 'DisProt entry ID':'End'])
> ~~~
> {: .language-python}
> 
{: .challenge}
> 
> > ## Solution
> > No, they do not produce the same output! The output of the first statement is:
> > ~~~
> >             DisProt entry ID  Start
> > UniProt ACC                        
> > P09883               DP00342      1
> > P09983               DP00389    962
> > ~~~
> >{: .output}
> > The second statement gives:
> > ~~~
> >            DisProt entry ID  Start   End
> > UniProt ACC                              
> > P09883               DP00342      1    83
> > P09983               DP00389    962  1023
> > P08083               DP00461     67    90
> > ~~~
> >{: .output}
> > Clearly, the second statement produces an additional column and an additional row compared to the first statement.  
> > What conclusion can we draw? We see that a numerical slice, 0:2, *omits* the final index (i.e. index 2)
> > in the range provided,
> > while a named slice, ''DisProt entry ID':'End', *includes* the final element.
> {: .solution}
{: .challenge}

> ## Selecting Indices
>
> Explain in simple terms what `idxmin` and `idxmax` do in the short program below.
> When would you use these methods?
>
> ~~~
> data = pd.read_csv('data/DisProt_data_Caenorhabditis-elegans.csv', index_col='DisProt entry ID')
> print(data.loc[:, "Length"].idxmin())
> print(data.loc[:, "Length"].idxmax())
> ~~~
> {: .language-python}
{: .challenge}
>
> > ## Solution
> > For each column in `data`, `idxmin` will return the index value corresponding to each column's minimum;
> > `idxmax` will do accordingly the same for each column's maximum value.
> >
> > You can use these functions whenever you want to get the row index of the minimum/maximum value and not the actual minimum/maximum value.
> {: .solution}
{: .challenge}

> ## Practice with Selection
>
> Assume Pandas has been imported and the Caenorhabditis elegans data has been loaded.
> Write an expression to select each of the following:
>
> 1.  Position of start of all entries.
> 2.  All data for DP00613 only.
> 3.  If entries are longer than 150.
{: .challenge}
>
> > ## Solution
> > 1:
> > ~~~
> > data.loc["":, "Start"]
> > ~~~
> > {: .language-python}
> >
> > 2:
> > ~~~
> > data.loc["DP00613"]
> > ~~~
> > {: .language-python}
> >
> > 3:
> > ~~~
> > data["Length"] > 150
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

> ## Using the dir function to see available methods
>
> Python includes a `dir` function that can be used to display all of the available methods (functions) that are built into a data object.  As an example, the  functions available for a [list data type](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists) are:
> ~~~
> potatoes = ["Russet", "Norkota", "Yukon Gold", "Pontiac"]
> dir(potatoes)
> ~~~
> {: .language-python}
>
> This command returns:
> ~~~
> ['__add__',
> ...
> '__subclasshook__',
>  'append',
>  'clear',
>  'copy',
>  'count',
> 'extend',
> 'index',
> 'insert',
> 'pop',
> 'remove',
> 'reverse',
> 'sort']
> ~~~
> {: .language-python}
>
> The double underscore functions can be ignored for now; functions that are not surrounded by double underscores are the *public interface* of the [list type](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists). So, if you want to sort the list of potatoes, according to `dir` you should try,
> ~~~
> potatoes.sort()
> ~~~
> {: .language-python}
{: .callout}

> ## Using new functions
> 
> Assume Pandas has been imported and the Caenorhabditis elegans data has been loaded as `data`.  
> Then, use `dir` to find the function that prints out the median of Length across all entries.  
> > ## Solution
> > Among many choices, dir lists the `median()` function as a possibility.  Thus,
> > ~~~
> > data.median()
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}


[pandas-dataframe]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html
[pandas-series]: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html
[numpy]: http://www.numpy.org/
