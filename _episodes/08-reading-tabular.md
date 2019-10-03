---
title: "Reading Tabular Data into DataFrames"
teaching: 10
exercises: 10
questions:
- "How can I read tabular data?"
objectives:
- "Import the Pandas library."
- "Use Pandas to load a simple CSV data set."
- "Get some basic information about a Pandas DataFrame."
keypoints:
- "Use the Pandas library to get basic statistics out of tabular data."
- "Use `index_col` to specify that a column's values should be used as row headings."
- "Use `DataFrame.info` to find out more about a dataframe."
- "The `DataFrame.columns` variable stores information about the dataframe's columns."
- "Use `DataFrame.T` to transpose a dataframe."
- "Use `DataFrame.describe` to get summary statistics about data."
---
## Use the Pandas library to do statistics on tabular data

*   Pandas is a widely-used Python library for statistics, particularly on tabular data.
*   Borrows many features from R's dataframes.
    *   A 2-dimensional table whose columns have names
        and potentially have different data types.
*   Load it with `import pandas as pd`. The alias pd is commonly used for Pandas.
*   Read a Comma Separate Values (CSV) data file with `pd.read_csv`.
    *   Argument is the name of the file to be read.
    *   Assign result to a variable to store the data that was read.

~~~
import pandas as pd

data = pd.read_csv('data/DisProt_data_Caenorhabditis-elegans.csv')
print(data)
~~~
{: .language-python}
~~~
   DisProt entry ID UniProt ACC  Start   End      Reference  Length
0           DP00613      Q22472      1   378  pmid:19899809     378
1           DP00868      Q9NHC3    102   117  pmid:21616056     279
2           DP01090      G4SLH0   3546  3795  pmid:20346955   18562
3           DP01113      D0PV95      1   168  pmid:26015579     708
4           DP01313      Q9N4U5    163   223  pmid:27150041     547
5           DP01407      O61667     93   106  pmid:15383288     106
6           DP01436      P54936    423   487  pmid:12110687     961
7           DP01437      P90976    115   180  pmid:12110687     297
8           DP01558      Q9XTY3     17   211  pmid:22947085     211
9           DP01973      Q09514      1    18  pmid:26621324     266
10          DP02025      P30429      1   105  pmid:16208361     571
11          DP02216      Q9U3S5      1   100  pmid:30036386    1050
12          DP02313      Q8IG33     18    88  pmid:21397184     186
~~~
{: .output}

*   The columns in a dataframe are the observed variables, and the rows are the observations.
*   Pandas uses backslash `\` to show wrapped lines when output is too wide to fit the screen.

> ## File Not Found
>
> Our lessons store their data files in a `data` sub-directory,
> which is why the path to the file is `data/DisProt_data_Caenorhabditis-elegans.csv'`.
> If you forget to include `data/`,
> or if you include it but your copy of the file is somewhere else,
> you will get a [runtime error]({{ page.root }}/04-built-in/#runtime-error)
> that ends with a line like this:
>
> ~~~
> OSError: File b'gapminder_gdp_oceania.csv' does not exist
> ~~~
> {: .error}
{: .callout}

## Use `index_col` to specify that a column's values should be used as row headings
*   Row headings are numbers (0 and 1 in this case).
*   Really want to index by country.
*   Pass the name of the column to `read_csv` as its `index_col` parameter to do this.

~~~
data = pd.read_csv('data/DisProt_data_Caenorhabditis-elegans.csv', index_col='UniProt ACC')
print(data)
~~~
{: .language-python}
~~~
DisProt entry ID  Start   End      Reference  Length
UniProt ACC                                                     
Q22472               DP00613      1   378  pmid:19899809     378
Q9NHC3               DP00868    102   117  pmid:21616056     279
G4SLH0               DP01090   3546  3795  pmid:20346955   18562
D0PV95               DP01113      1   168  pmid:26015579     708
Q9N4U5               DP01313    163   223  pmid:27150041     547
O61667               DP01407     93   106  pmid:15383288     106
P54936               DP01436    423   487  pmid:12110687     961
P90976               DP01437    115   180  pmid:12110687     297
Q9XTY3               DP01558     17   211  pmid:22947085     211
Q09514               DP01973      1    18  pmid:26621324     266
P30429               DP02025      1   105  pmid:16208361     571
Q9U3S5               DP02216      1   100  pmid:30036386    1050
Q8IG33               DP02313     18    88  pmid:21397184     186
~~~
{: .output}

## Use `DataFrame.info` to find out more about a dataframe

~~~
data.info()
~~~
{: .language-python}
~~~
<class 'pandas.core.frame.DataFrame'>
Index: 13 entries, Q22472 to Q8IG33
Data columns (total 5 columns):
DisProt entry ID    13 non-null object
Start               13 non-null int64
End                 13 non-null int64
Reference           13 non-null object
Length              13 non-null int64
dtypes: int64(3), object(2)
memory usage: 624.0+ bytes
~~~
{: .output}

*   This is a `DataFrame`
*   Thirteen rows named `"Q22472"`, `"Q9NHC3"`...
*   Five columns, some of which have 64-bit floating point and the others string values.
*   Uses 624.0+ bytes of memory.
We will talk later about null values, which are used to represent missing observations.

## The `DataFrame.columns` variable stores information about the dataframe's columns

*   Note that this is data, *not* a method.
*   Like `math.pi`.
*   So do not use `()` to try to call it.
*   Called a *member variable*, or just *member*.

~~~
print(data.columns)
~~~
{: .language-python}
~~~
Index(['DisProt entry ID', 'Start', 'End', 'Reference', 'Length'], dtype='object')
~~~
{: .output}

## Use `DataFrame.T` to transpose a dataframe

*   Sometimes want to treat columns as rows and vice versa.
*   Transpose (written `.T`) doesn't copy the data, just changes the program's view of it.
*   Like `columns`, it is a member variable.

~~~
print(data.T)
~~~
{: .language-python}
~~~
UniProt ACC              Q22472         Q9NHC3         G4SLH0         D0PV95  \
DisProt entry ID        DP00613        DP00868        DP01090        DP01113   
Start                         1            102           3546              1   
End                         378            117           3795            168   
Reference         pmid:19899809  pmid:21616056  pmid:20346955  pmid:26015579   
Length                      378            279          18562            708   

UniProt ACC              Q9N4U5         O61667         P54936         P90976  \
DisProt entry ID        DP01313        DP01407        DP01436        DP01437   
Start                       163             93            423            115   
End                         223            106            487            180   
Reference         pmid:27150041  pmid:15383288  pmid:12110687  pmid:12110687   
Length                      547            106            961            297   

UniProt ACC              Q9XTY3         Q09514         P30429         Q9U3S5  \
DisProt entry ID        DP01558        DP01973        DP02025        DP02216   
Start                        17              1              1              1   
End                         211             18            105            100   
Reference         pmid:22947085  pmid:26621324  pmid:16208361  pmid:30036386   
Length                      211            266            571           1050   

UniProt ACC              Q8IG33  
DisProt entry ID        DP02313  
Start                        18  
End                          88  
Reference         pmid:21397184  
Length                      186  
​
~~~
{: .output}

## Use `DataFrame.describe` to get summary statistics about data

DataFrame.describe() gets the summary statistics of only the columns that have numerical data. 
All other columns are ignored, unless you use the argument `include='all'`.
~~~
print(data.describe())
~~~
{: .language-python}
~~~
             Start          End        Length
count    13.000000    13.000000     13.000000
mean    344.769231   459.692308   1855.538462
std     968.988059  1010.109267   5028.345679
min       1.000000    18.000000    106.000000
25%       1.000000   105.000000    266.000000
50%      18.000000   168.000000    378.000000
75%     115.000000   223.000000    708.000000
max    3546.000000  3795.000000  18562.000000
~~~
{: .output}

> ## Reading Other Data
>
> Read the data in `DisProt_data_Escherichia_coli.csv`
> (which should be in the same directory as `DisProt_data_Caenorhabditis-elegans.csv`)
> into a variable called `coli`
> and display its summary statistics.
>
> > ## Solution
> > To read in a CSV, we use `pd.read_csv` and pass the filename 'data/gapminder_gdp_americas.csv' to it. 
> > We also once again pass the column name 'UniProt ACC' to the parameter `index_col` in order to index by UniProt ACC:
> > ~~~
> > coli = pd.read_csv('data/DisProt_data_Escherichia_coli.csv', index_col='UniProt ACC')
> > ~~~
> >{: .language-python}
> {: .solution}
{: .challenge}



> ## Inspecting Data
>
> After reading the data for Escherichia coli,
> use `help(coli.head)` and `help(coli.tail)`
> to find out what `DataFrame.head` and `DataFrame.tail` do.
>
> 1.  What method call will display the first three rows of this data?
> 2.  What method call will display the last three columns of this data?
>     (Hint: you may need to change your view of the data.)
>
> > ## Solution
> > 1. We can check out the first five rows of `coli` by executing `coli.head()` (allowing us to view the head
> > of the DataFrame). We can specify the number of rows we wish to see by specifying the parameter `n` in our call
> > to `coli.head()`. To view the first three rows, execute:
> >
> > ~~~
> > coli.head(n=3)
> > ~~~
> >{: .language-python}
> > 
> > The output is then
> > ~~~
> >	DisProt entry ID	Start	End	Reference	Length
> > UniProt ACC					
> > P09883	DP00342	1	83	pmid:12054823	582
> > P09983	DP00389	962	1023	pmid:7703231	1023
> > P08083	DP00461	67	90	pmid:9687368	387
> > ~~~ 
> >{: .output}
> > 2. To check out the last three rows of `coli`, we would use the command, `coli.tail(n=3)`,
> > analogous to `head()` used above. However, here we want to look at the last three columns so we need
> > to change our view and then use `tail()`. To do so, we create a new DataFrame in which rows and 
> > columns are switched
> > 
> > ~~~
> > coli_flipped = coli.T
> > ~~~
> >{: .language-python}
> >
> > We can then view the last three columns of `coli` by viewing the last three rows of `coli_flipped`:
> > ~~~
> > coli_flipped.tail(n=3)
> > ~~~
> >{: .language-python}
> > The output is then
> > ~~~
> > UniProt ACC	P09883	P09983	P08083	P77072	P07674	P22995	Q47184
> > End	83	1023	90	20	64	83	30
> > Reference	pmid:12054823	pmid:7703231	pmid:9687368	pmid:15222745	pmid:20200158	pmid:11743881	pmid:15943811
> > Length	582	1023	387	212	358	83	192
> > ~~~ 
> >{: .output}
> > Note: we could have done the above in a single line of code by 'chaining' the commands:
> > ~~~
> > coli.T.tail(n=3)
> > ~~~
> >{: .language-python}
> {: .solution}
{: .challenge}

> ## Reading Files in Other Directories
>
> The data for your current project is stored in a file called `microbes.csv`,
> which is located in a folder called `field_data`.
> You are doing analysis in a notebook called `analysis.ipynb`
> in a sibling folder called `thesis`:
>
> ~~~
> your_home_directory
> +-- field_data/
> |   +-- microbes.csv
> +-- thesis/
>     +-- analysis.ipynb
> ~~~
> {: .output}
>
> What value(s) should you pass to `read_csv` to read `microbes.csv` in `analysis.ipynb`?
> 
> > ## Solution
> > We need to specify the path to the file of interest in the call to `pd.read_csv`. We first need to 'jump' out of
> > the folder `thesis` using '../' and then into the folder `field_data` using 'field_data/'. Then we can specify the filename `microbes.csv.
> > The result is as follows:
> > ~~~
> > data_microbes = pd.read_csv('../field_data/microbes.csv')
> > ~~~
> >{: .language-python}
> {: .solution}
{: .challenge}

> ## Writing Data
> 
> As well as the `read_csv` function for reading data from a file,
> Pandas provides a `to_csv` function to write dataframes to files.
> Applying what you've learned about reading from files,
> write one of your dataframes to a file called `processed.csv`.
> You can use `help` to get information on how to use `to_csv`.
> > ## Solution
> > In order to write the DataFrame `coli` to a file called `processed.csv`, execute the following command:
> > ~~~
> > coli.to_csv('processed.csv')
> > ~~~
> >{: .language-python}
> > For help on `to_csv`, you could execute, for example,
> > ~~~
> > help(coli.to_csv)
> > ~~~
> >{: .language-python}
> > Note that `help(to_csv)` throws an error! This is a subtlety and is due to the fact that `to_csv` is NOT a function in 
> > and of itself and the actual call is `coli.to_csv`. 
> > 
> {: .solution}
{: .challenge}
