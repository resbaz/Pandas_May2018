
# Introduction to Pandas

This workshop will be focused on the exploration, cleaning and basic visualisation of a prepared set of sample data - the Canberra Climate Sensor Data. If you have not already downloaded this data from the [Github](https://github.com/resbaz/Pandas_May2018/blob/master/pedestrian_counts.zip), please do so now, and place this data file in the same folder as this jupyter notebook. And remember, please unzip your folder before trying to use it!

## Learning objectives

Throughout this session we're going to be teaching you a range of tools and skills related to cleaning, manipulation and visualising large datasets. Using the pandas package, we can read in and manipulate large spreadsheets of data, and matplotlib lets you visualise these datasets in a useable, customisable format.

The three over-arching themes I'll be taking you through today are:

- Data examination
- Dataframe manipulation
- Plotting your data with pyplot


## Setting Up

First, we need to import Python's *pandas*, *matplotlib* and *numpy* packages, and then use inline plotting "magic" command so that all plots generated will appear within this notebook instead of in a new browser tab.

While numpy isn't directly related to this course, it's handy for generating random values, which will be useful when learning how to create your own dataframe


```python
import pandas as pd
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
mpl.style.use('ggplot')
```


```python
%pylab inline
```

## A Basic Pandas Introduction

- creating your own dataframe
    - the "series" object
- subsetting columns
- subsetting rows
    - `head()` and `tail()`
    - slicing
    - `loc` vs `iloc`


### Creating a Dataframe

A Pandas dataframe can be thought of as a collection of lists (of equal length), where each list makes up a column inside your dateframe. 

The key difference between a list and a Pandas column though, is that every single item inside your column _must be of the same data type_. If you have a column of integers, and a single string value, like this, `[1,2,3,4,'seven']`, then every single value inside your column is going to be a string-type.

Instead of using a list - which can take any type of values -, Pandas performs this type-coercion by using a data type called a Series.


```python
pd.Series([1,2,3,4,5])
```

Each "Series" object can be thought of as it's own miniature dataframe. So our previous example would be a dataframe with one column, and 5 rows.

Therefore, when creating a dataframe, you actually have to create it as a collection of these "Series" objects


```python
df = pd.DataFrame(
         {'A':['a', 'b', 'c','d','e','f'], 
             'B':[54, 67, 89, 100, None, 64],
             'C': np.random.randn(6),
             'D': [2.6,None, 8.0, 9.4, 3.3, None]
                },
          index=[49, 48, 47, 1, 2, 3] # Lets you set your row names
            )
```


```python
df
```

#### Subsetting Columns

To select a particular column in your dataframe, you can use one of two options:

1) Calling the column as an "attribute"
    - `df.columnName`

2) Subsetting the dataframe
    - `df['columnName']`

The first option is useful for some functions, but the second form is essential if you want to call more than column at once. You do this by inserting a list, [], of column names, instead a single column.

For example: `df[["Column1", "Column2", ... , etc]]`

We're first going to try this on our toy dataset from earlier, for ease of use.


```python
df
```


```python
# As a subset
df['A']
```


```python
#As an attribute
df.D
```


```python
# Try subsetting the first two columns
df[["A","B"]]
```


```python
# What happens if you change the order around?
df[['D','A']]

```


```python
# What about a column that doesn't exist?
df.d
```

#### Slicing

Sometimes you need to examine specific rows and columns in the middle of your data though, which aren't covered by `head()` or `tail()`. Instead, you can use index slicing.

Slicing works similarly to how you might slice a string, or a list. You simply call the indexes of the rows you want from the dataframe: `df[rowNumbers]`


```python
pdsn.Location[5:10] # Gives rows 5, 6, 7, 8, 9
```


```python
pdsn[["Location","Counts"]][:8] #gives rows 0 through 7
```

#### `loc` vs `iloc`
You can also use `loc` and `iloc` to slice rows and columns.

`iloc` is positional based, so only takes integer values that correlate with the row and column numbers you want to subset

`loc` is label based, and takes the row and column **labels** as inputs


```python
#Using iloc to get rows 0, 1 and 2
df.iloc[:3]
```


```python
# Using loc to get row labels 1, 2 and 3
df.loc[1:3]
```

If you only enter one set of values into `loc` and `iloc`, they will return the values for every column in your dataset.

By using a second integer though, you can choose which rows and columns you specifically want to subset. The first value corresponds to the row, and the second to the columns, or rows x columns. You can also think of this with the moniker *"Roman Catholic"*


```python
# Using loc to get row labels 1, 2 and 3 for Columns A and B
df.loc[1:3,["A","B"]]
```


```python
# Using iloc to get rows 0, 1 and 2 for columns 0 and 1.
df.iloc[:3,:2]
```

The differences between `loc` and `iloc` can seem minor, but they're very important, and which one you should use depends on what your needs are at the time.

`iloc` is based on dataframe position, so calling `iloc[:3]` would give you rows 0 through 3. 

`loc` however is based on the index label, so if you were to call `df.loc[:3]`, it would give you all rows UP TO the row labelled as index 3.

While the indexes are in order and all present, this isn't an issue. Consider what happens when the indexes are out of order though, with our dataframe 's'


```python
# Can see that this only takes the first 2 rows
df.iloc[:2]
```


```python
# Whereas this takes all rows UP TO index label 2
df.loc[:2]
```

Since `loc` is based on labels, if you try to subset a row or column label that doesn't exist, even if it corresponds to a positional row, python will throw you an error

Due to this, it's important that you carefully consider which tool is appropriate for your needs

#### Challenge 1

Consider the following Python dictionary data and Python list labels:

```Python 
data = {'animal': ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
        'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
        'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'priority': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}

labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
```

**Part 1**

Create a DataFrame `vet_info` from this dictionary data which has the index (i.e. row names) `labels`


```python

```

**Part 2**

Return the first 3 rows of the DataFrame `vet_info`.


```python

```

**Part 3** 

Select just the 'animal' and 'age' columns from `vet_info`.


```python

```

**Part 4** 

Select the data in rows [3, 4, 8] and in columns ['animal', 'age'].


```python

```

# Working with Data

## Reading in Data

The first step to any data exploration and manipulation is to open your data within your program. The **pandas** package can directly read in spread-sheet style data and convert them into *dataframes*.

These dataframes work with rows and columns, like a spreadsheet, except that all data within a single column has to be the same data type. 

For example, imagine you had a spreadsheet containing two columns - "labels" and "numbers", and that the rows in the "labels" column contains either a text or number sequence. Because you cannot turn text into a number, every single row in that "labels" column would need to be a string (text) type. Similarly, if some (but not all) of the rows in the "numbers" column contained decimals, **all** of the rows within this column would need to be of a decimal (float) data type.

To read in a comma-separated file, or \*.csv, you can use the pandas function `read_csv()`


```python
# Reading in a *.csv file
pdsn = pd.read_csv("pedestriancounts_melbourne.csv")
```

You can also open a variety of other file types using the "reader" functions found in this [IO tools documentation](http://pandas.pydata.org/pandas-docs/version/0.20/io.html "Pandas IO tools"). 

This includes file types such as excel (\*.xlsx) files, and text (\*.txt) files. You can use the parameters within these functions to specify file or data attributes such as column separators, whether there's column/row names, and even specifying your own column names.


```python
pdsn.head()
```

Oops! It appears that our data doesn't actually have any headers, so our column have been read in with the incorrect names. 

Fortunately, pandas.read_csv() has a range of keyword arguments we can use while reading in our data.

- `sep`: the type of separator between our columns
-  `header`: the row number to take as the start of the data. Useful if you have metadata attached at the beginning of your file.
- `names`: You can also specify your column names. Takes a list of values. If your file contains no header, also use `header = none`. Otherwise, use `header = 0`.
* `parse_dates`: Treat one or more columns like dates.
* `dayfirst`: Use DD.MM.YYYY format, not month first.
* `infer_datetime_format`: Tell pandas to guess the date format.
- `na_values`: Specify values to be treated as empty.


```python
pdsn = pd.read_csv("pedestriancounts_melbourne.csv",
                  header = None, names = ["Date", "Day","Hour","SensorID","Location","Counts"])
```


```python
pdsn.head()
```

## Examining your Data

- `head()`, `tail()`
- `shape`; `shape[0]` vs. `shape[1]`
- `columns`
- `dtypes`
- `describe()`
- Subsetting with conditionals


One of the first steps in exploring your data is to see what it looks like, what data types are present, and how many rows/columns there are.

For example, `df.head()` and `df.tail()` show you the first/last 5 rows of your dataframe selection


```python
pdsn.tail()
```

You can also specify how many rows you want `head` and `tail` to return


```python
pdsn.head(20)
```

The `shape` function gives you the dimensions of your data, in the form `(#rows, #columns)`.


```python
# This returns a tuple (or linked pairs) of the number of rows and columns in your dataframe.
pdsn.shape
```

So we can see here that we have 658,823 rows, and 6 columns in our dataframe


```python
# Calling the first or second element of the tuple can give you either the rows or the columns
#Gives you the rows (remember, 0 indexing!)
print(pdsn.shape[0])

#Gives you the columns
print(pdsn.shape[1])
```

You can also use `df.columns` to examine the column names.


```python
# What are the column names in your dataframe?
pdsn.columns
```

You can also examine the data types inside your columns using `dtypes`

Knowing what type of data is in your dataframe is extremely important, as it limits what functions you can and can't do on that column. It's useless to try and do string manipulations on an integer, or try to find the sum of a column of names.

Within pandas, str = "object", int = "int64", and float = "float64"


```python
pdsn.dtypes
```

Knowing this, you can now examine a quick summary of your data to see what you're working with using `describe()`


```python
pdsn.describe()
```

What you might notice here is that only the *numeric* columns have been returned. To view both your numeric and string/date/other columns, you will need to use the parameter `include`


```python
pdsn.describe(include = "all")
```

As all of this data is output in the same table, the rows which aren't applicable to that data type are filled that NaN's

### Summary Statistics

We can also find specific statistics for each of these columns by using `max()`, `min()`, `count()`, `std()`, and `sum()`. 

Just remember though that many of these functions rely on numeric data types, and will cause errors if used on a str type. Calling `sum()` on a string however will concatentate those strings.


```python
#Try finding the maximum of the Location (a string) column
pdsn.Location.max()
```


```python
# what about the Standard Deviation (std) of Counts (numeric)?
pdsn.Counts.std()
```

Other useful functions include `mean()` and `unique()`.

Where `mean()` takes the average of numeric data, `unique()` works on both numeric and string data, and gives you a list of all of the unique values within a particular column.



```python
# Unique works on numeric data
pdsn.Hour.unique()
```


```python
# As well as on str data
pdsn.Day.unique()
```

As this returns a numpy array though, rather than a pandas dataframe or series object, you can use either `len()` or `array.size` to find the number of unique values for that column


```python
# Using the numpy "size" function
# pdsn.Day.unique().size

# Using the len() function
# len(pdsn.Day.unique())
```

### Subsetting with conditionals
You can also subset using conditional statements, such as:  
* `==` or `!=`
* `>` or `<`
etc.

For example, if I want to find all of the rows in our toy dataframe, `df` where `B > 60`, I would type:


```python
df[df['B']>60]
```

Just as with lists and for loops, etc, you can also combine these conditionals using & {and} , or | {or}


```python
#Find the rows where C < 0.5 AND B != 100
df[(df.C < 0.5) & (df.B != 100)]
```


```python
#You can also get a list of the row indexes for your subset
list(df[df.B > 60].index)
```

Similarly, you can subset using a list of boolean values


```python
# a boolean list. The 1st, 3rd and 5th values are True.
na = [True,False,True,False,True,False]

df[na] #subsets the 1st, 3rd and 5th rows
```

#### Challenge 2

**Part 1**

Find how many rows belong to the sensor at Lygon St (West) between the hours of 12am and 2am?

*Hint 1: an entry of "3" in the Hour column means that the pedestrian counts have been monitored from 3am to 4am*

*Hint 2: remember that you find the length of a list, or you can use `array.size`*


```python
(pdsn[(pdsn['Location'] == "Lygon St (West)") & (0 <= pdsn.Hour) & (pdsn.Hour < 2)].index).size
```

**Part 2**

Calculate the sum of all  (the total number of visits)


```python

```


```python

```

**Part 3**

Calculate the mean pedestrian count seen between (each of):
* 12am and 1am
* 1pm and 2pm
* 10pm and 11pm

At the Melbourne Central crossing


```python

```

##### Optional Extra

Suppose you have DataFrame with 10 columns of real numbers, for example

`df2 = pd.DataFrame(np.random.random(size=(5, 10)), columns=list('abcdefghij'))`

Which column of numbers has the smallest sum? Find that column's label.

*Hint: https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.idxmin.html*


```python

```


```python

```

# Manipulating Your Dataframe

- Changing data: `set_value()`
- Finding NaNs: 
    - `count()` 
    - Using booleans: `isnull()`
    - Chaining: `isnull().any()`, `isnull().sum()`
    - Subsetting: `df[df['Column'].isnull()]`
- Data type conversion
- Using the "timestamp" data type
- Adding and deleting columns/rows
- `groupby()`

## Changing data inside your dataframe

Occasionally, you might notice a particular value inside your data frame that you need to change - it could be a singular error, a spelling mistake, etc.

This can be done using the `set_value(index,"column", value)` function. For the index parameter, you can pass it either a single index, or a list, [], of indexes to be replaced. 

`set_value()` works with column/row labels, similar to `loc`, rather than the indexes.


```python
df
```


```python
# Changing index 48 in column D with a 0
df.set_value(48,"D",0)

df
```

Say that you noticed a repeated spelling error in some of the labels, and wanted to fix them, or change something for more clarity. 

In this instance, let's replace all numbers in column B that are less than 70 with 0's instead.


```python
df.set_value(list(df[df.B < 70].index), "B", 0)
```

Alternatively, if you want to apply a function to an entire column, you can just reassign it directly (as you could when replacing the value in a dictionary)


```python
df['A'] = df["A"].str.capitalize()

df
```

## Using `apply`, `applymap` or `map` to change your data

- **apply** iterates over columns and computes the aggregated result value of the function applied to all vals in a column. Creates an aggregate value for the column/s, and returns a Series object.
- **applymap** iterates over the columns of a dataframe and computes a result for each value of the column. Applies your function element by element over your dataframe. Returns a Dataframe.
- **map** similar to applymap, but operates on Series objects only (rather than whole dataframes). Perfect for single column operations. Returns a Series object.





```python
# Apply

# Gives back a series object
df_sum = df[['B','C','D']].apply(lambda x: sum(x))

#total sum
df_sum
```


```python
type(df_sum)
```


```python
# Applymap
df = pd.DataFrame(np.random.randn(3, 3))
df
```


```python
df = df.applymap(lambda x: '%.2f' % x)
df
```

Now say we wanted to replace all of the extended day names (e.g. Saturday) with the shortened versions instead (e.g. Sat).
We could do this with map


```python
# First create a dictionary of the values we want
days = {'Monday': 'Mon',
        "Tuesday": "Tues",
       "Wednesday":"Wed",
       "Thursday":"Thurs",
       "Friday":"Fri",
       "Saturday":"Sat",
       "Sunday":"Sun"}

```


```python
# Replace our Day column with our new Day column values
# `get()` accesses values safely from dictionary
pdsn['Day'] = pdsn["Day"].map(days.get)
```


```python
pdsn.Day.unique()
```

## Finding Null Data

Earlier, we explored the use of `shape` to determine the dimensions of our dataframe. Another way to do this is with `count`.


```python
pdsn.count()
```

But you can see that not all of the columns have the same length. This is because count sums the number of rows within that column that contain values - so missing data, or NaNs, will cause the count to be smaller.

You can check which columns inside our test dataframe, df, contains null values by using `isnull()`.



```python
df
```


```python
df.isnull()
```

As you can see, this returns a `True` or `False` boolean value at each point in our dataframe. This is alright for something like this, but what about when we have 12 columns? 1000 rows? 

To get a more informative information, we can also pair `isnull()` with the `any()` function. `any()` will check whether there are any `True` values in the columns of your Dataframe object


```python
#Check for nulls in each column
pdsn.isnull().any()
```

We can also combine `isnull()` and `sum()` to find an exact count of how many NaNs are in each column as well, like `df.isnull().sum()`


```python
pdsn.isnull().sum()
```

## Resolving Null Data

There are 2 ways to resolve Null data - replace it with new data, or delete them.

For the purposes of this training, I'm only going to show you how to delete them (though another section below this will teach you to replace it, for the purposes of your own data investigations).

### Deleting Null Data

Firstly, let's view the columns where SensorID, Location and Counts have null values.

We can do this using the `isnull()` function. As mentioned before, we can subset with a boolean list, which is what `isnull()` returns. Where the test evaluates to 'True', the row is output from the dataframe.


```python
#Show the rows where SensorID has null values
pdsn[pdsn['SensorID'].isnull()]
```


```python
#Show the rows where Counts has null values
pdsn[pdsn.Counts.isnull()]
```

You can also show *every* row with null values at once using `isnull().any()`. `any()` contains the optional parameter "axis", which allows you to choose whether it operates on the rows or the columns. By default `axis = 0`, which gives you columns, but setting `axis = 1` checks over the rows instead


```python
pdsn.isnull().any(axis = 0)
```


```python
pdsn.isnull().any(axis = 1)
```


```python
# And remember, we can subset using boolean lists
pdsn[pdsn.isnull().any(axis = 1)]
```

From here, there are a few ways that we can remove our null data.

The first option is to simply subset for every row except the null values. This can be easily done by replacing `isnull()` with the `notnull()` command


```python
df[df.B.notnull()]
```


```python
pdsn.isnull().any()
```


```python
# Unfortunately notnull() will only work on individual columns at a time, not the whole dataframe
no_nans = pdsn[pdsn.SensorID.notnull() & pdsn.Counts.notnull()]

#Compare to make sure they've worked
print(pdsn.shape)
print(no_nans.shape)
```

Another way is to take the `isnull().any()` boolean list that we've created, and then subset for all values *except* those.

A handy python trick is that you can quickly invert the values in a True/False list with a `~` symbol


```python
pdsn[~pdsn.isnull().any(axis = 1)].shape
```

Conversely, there's also a dedicated `dropna()` function

`dropna()` can work on either the rows or the columns, and allows you to specify whether you want to remove rows/column where either 'any' or 'all' of the data are nulls. Alternatively, you can set a threshold value, where rows/columns are discarded if they don't contain at least a certain number of non-null values


```python
no_nans = pdsn.dropna(axis = 0, # 0 or‘index’, 1 or ‘columns’, or tuple/list to drop on multiple axes
                      how = 'any'#{‘any’, ‘all’},
                      #thresh= 5, #require at least this many non-NA values
                      #inplace = {False (default), True} whether operation returns dataframe or edits source directly.
                     )

no_nans.shape
```

If we examine the `tail()` of the altered dataframe though you can see that the row indexes are still the same from before the deletions though. We can fix this using `reset_index()`.

Remember that while the indexes aren't that important if you're using `iloc`, but if you're using `loc` it's important that these index labels are correct.



```python
no_nans.tail()
```


```python
no_nans = no_nans.reset_index()
```


```python
no_nans.tail()
```

We can see however, that this method has added in another column, Index, with the original index values. This can be pretty easily resolved by deleting the column (if you don't want to keep that data). But we'll get to that later.

#### Challenge 3

Take this dataframe of malformed flight data.
```Python
flightData = pd.DataFrame({'From': ['LoNDon', 'MAdrid', 'londON','Budapest', 'Brussels'],
                   'To':['paris','miLAN',"Stockholm","PaRiS","LondoN"],
              'FlightNumber': [10045, np.nan, 10065, np.nan, 10085],
              'RecentDelays': [[23, 47], [], [24, 43, 87], [13], [67, 32]],
                   'Airline': ['KLM(!)', '<Air France>', '(British Airways. )', 
                               '<Air France>', '"Swiss Air"']})
```



**Part 1**

Some values in the the FlightNumber column are missing. These numbers are meant to increase by 10 with each row so 10055 and 10075 need to be put in place. Fill in these missing numbers.


```python

```

**Part 2**

Notice how the capitalisation of the city names is all mixed up in this temporary DataFrame. Standardise the strings so that only the first letter is uppercase (e.g. "londON" should become "London".)


```python

```

**Part 3**

In the Airline column, you can see some extra puctuation and symbols have appeared around the airline names. Pull out just the airline name. E.g. '(British Airways. )' should become 'British Airways'

*Hint: strings have a `strip()` function*


```python

```


```python

```


```python

```

That ends the "deleting nulls" section of data manipulation. To continue with the exercises, now go to "Converting Data Types, Dataframe Manipulation". 

This next text section will show instead how you might replace your null values with useable data.

### Replacing Nulls - Personal reading section

#### Replacing all null values with a single value
In the Counts column for example, you can see that there are 14 missing values. You might choose to just replace all of these with 0. You can do this using the `fillna()` function, and specifying the value you want to replace it with.



```python
df = pd.DataFrame({"A": ['a','b','c','d',np.nan],
                 "B": [1,5,np.nan,7,9],
                 "C": np.random.randn(5)})
df
```


```python
df['B'] = df['B'].fillna(value=0)

df
```

`fillna` also allows you to forward (`ffill`) or backfill (`bfill`) your dataframe with the "methods" argument.

Find out more about the options available here:  
http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.fillna.html


```python
df["A"] = df["A"].fillna(method = 'ffill')
df
```

##### Interpolating (Predicting) Missing Data

One option is to use the function `Series.interpolate` to predict and fill in the missing values based on the index

If we examine the function more closely, we can see that it can take multiple arguments, including the prediction method (default = linear), and the direction in which it can replace your data.

Be very careful about using this method though, as it is literally predicting what data *might* have been there, rather than what actually *was* there. This naturally carries some degree of statistical uncertainty, which can be introduced into your data and calculations. If in doubt about which prediction method is appropriate, or the accuracy of your predictions, maintain an unaltered copy of your data (always recommended regardless) and consult a statistician.


```python
help(df.interpolate)
```

## Converting Data Types, Dataframe Manipulation

While we currently have a "Date" column, if you go into dtypes you can see that pandas has read this in as string, rather than as a datetime type. If we left it as a string, to get the Month, Day or Year we would need to perform a series of string manipulations every time we wanted to access these values - a costly, redundant and time consuming exercise if trying to use the whole dataframe. 

Instead, we can convert this column to a datetime data type, using the `to_datetime()` function.

Tip: this will take a while, so if you're working on the server try to stagger the run times


```python
no_nans['Date'] = pd.to_datetime(no_nans['Date'])
```


```python
no_nans.Date.head()
```


```python
no_nans.columns
```

### Adding and Deleting Columns

Adding a column is a pretty simple exercise - just as we would with a dictionary, you create a new key (column) and assign it some data.

Here, let's split our date column so that we can examine statistics for the different months, years, etc, later on.


```python
# Adding a Column with the Day
no_nans['Date2'] = pd.DatetimeIndex(no_nans['Date']).day

# Adding a Column with the Month (from 1-12)
no_nans["Month"] = pd.DatetimeIndex(no_nans["Date"]).month

# Adding a Column with the Year
no_nans["Year"] = pd.DatetimeIndex(no_nans["Date"]).year
```


```python
no_nans.head()
```

Because of the way our columns are named, we don't currently have anything sensible to call our second date column. We can change these columns names pretty easily to something more sensible


```python
no_nans.columns = ['index', 'Timestamp', 'Day', 'Hour', 'SensorID', 'Location', 'Counts',"Date","Month","Year"]
```


```python
no_nans.head()
```

To delete a column, we can simply use the `del` command


```python
del no_nans['index']
```


```python
no_nans.head()
```

Lets also add another column, Seasons, that tells us which season the traffic is occurring


```python
help(df.isin)
```


```python
season_dict = {1: "Summer", 2:"Summer", 3:"Autumn",4:"Autumn",5:"Autumn",
               6:"Winter",7:"Winter",8:"Winter",9:"Spring",10:"Spring",
              11:"Spring",12:"Summer",}


no_nans["Season"] = no_nans["Month"].apply(season_dict.get)

# Check it's been added in
no_nans.Season.unique()

#If I wanted to, could also use a for/if combination with set_value
```


```python
no_nans[no_nans['Month'].isin([1,2,12])].tail()
```

### Changing Your Data Types

You can also convert other columns by using the `df[column].astype(<datatype>)` function. This includes 'str', 'int64' and 'float64' data types, amongst others.

You must be careful **not to use `astype()` on a column with null values**. `astype()` does not preserve the null values, and makes resolving them later more difficult.

Let's create a dataframe, df, in an example of converting types


```python
df = pd.DataFrame({'A':['a','b','c',np.nan,'d','e'],
             'B': np.random.randn(6),
             'C': [2.6,np.nan, 8.0, 9.4, 3.3, np.nan]})
df
```


```python
df["A"] = df["A"].astype(str)
```

#### Exploration Challenge

Convert column A and C in `df` to `str` and `float`, respectively, and then shown what the resulting values at the previously null indexes are

*Hint: you can get the index of the current null values before the conversion*


```python

```

#### Challenge 4

Create a new column "D" for our toy dataframe, df. "D" will contain the values from column B + 100. 

_Hint 1: You can test your output by using the "apply" function without assigning it to a new column first_

_Hint 2: you can create your own functions using "lambda". e.g. _`apply(lambda x: ` _`insert stuff with x`_`)`



```python

```


```python

```

##### Extension Challenge

Harken back to our `flightData` dataframe from Challenge 3.

In the RecentDelays column, the values have been entered into the DataFrame as a list. We would like each first value in its own column, each second value in its own column, and so on. If there isn't an Nth value, the value should be NaN.

Expand the Series of lists into a DataFrame named delays, rename the columns delay_1, delay_2, etc. and replace the unwanted RecentDelays column in df with delays.

*Hint: You can create a new dataframe from the list data in the column by combining `apply` and pd.Series.*  
*Hint 2: `help(df.join)`*


```python
flightData
```


```python

```


```python

```


```python

```

## Grouping Data with `groupby`

It's often useful to be able to group the data within a column according to the data in another column. 

For example, you might wish to take the mean pedestrian counts for each month, or each year. We could do this is a complicated and time consuming way where you subset the data for each month, and then take the mean of the Counts column...or we could just use the `groupby` function. `groupby` outputs a reformatted version of your data where all values associated with your "grouping factor" are taken together. You can then perform mathematical and statistical tests on this grouped output (and plotting!), and the tests will be performed within each of the "groups" you defined.

For example, say that we wanted to find the mean pedestrian counts seen in each month:


```python
no_nans.groupby(by=["Month"]).mean()
```

Unfortunately these tests are non-discriminatory, and will take the `mean()` of each numeric data column. 

To get the columns you want, such as Counts, you need to specify this in the chained command:


```python
# the mean pedestrian counts within each month
no_nans.groupby(by=["Month"]).mean().Counts
```


```python
# Can also specificy multiple columns to be output
    # the mean "Hour" and pedestrian counts within each month
no_nans.groupby(by=["Month"]).mean()[["Counts","Hour"]]
```

We can also choose to only apply to certain columns, or even group by multiple factors


```python
# Just remember to keep the months you're grouping by in your column selection!
no_nans[["Counts"]].groupby(by=["Month","Year"]).mean()
```


```python
no_nans[["Counts","Month","Year"]].groupby(by=["Month","Year"]).mean()
```


```python
# The order of the groupby matters
no_nans[["Counts","Month","Year"]].groupby(by=["Year", "Month"]).mean()
```

By chaining more commands together you can also find the maximum, minimum, etc, values for your groups.


```python
# Minimum
no_nans.groupby(by=["Month"]).Counts.sum().max()
```

But which month is this associated with?


```python
# Finding the Month associated with the maximum value
countMax = no_nans.groupby(by=["Month"]).Counts.sum().max()

no_nans.groupby(by=["Month"]).Counts.sum()[no_nans.groupby(by=["Month"]).Counts.sum() == countMax]
```

You might also it useful to chain groupby and the `aggregate` function together for more functionality. We are limited with being able to apply this in interesting ways in our current dataset, but feel free to do some reading in your own time.


```python
help(df.aggregate)
```

#### Challenge 5
**Part 1** Which season has the greatest average Pedestrian traffic? 


```python

```

**Part 2** Which year has the greatest standard deviation in pedestrians?


```python

```

**Part 3** Which hour, of which day, sees the greatest average pedestrian traffic overall?

Hint: `help(df.sort_values)` might be useful


```python
no_nans[["Counts","Hour","Day"]].groupby(by = ['Day',"Hour"]).mean().apply(lambda t: t[t==t.max()])
```


```python

```

# Plotting Your Data

Now that we've examined and cleaned our data, it's time to look at how we might explore that visually.

Pandas has some in-built plotting functionality, that builds off of the traditional matplotlib library, which are reasonably easy to use. However, as Pandas plotting is built-upon matplotlib, customising your plots will require you to understand matplotlib commands. 

This section will teach you the basics of plotting within pandas. This is hardly comprehensive, and please feel free to delve into some of user guides and tutorials and questions available on Google and Stack Overflow to learn more about these tools and how to make them work for you.

A reasonably good matplotlib tutorial can be found at https://matplotlib.org/users/pyplot_tutorial.html

The [matplotlib API](https://matplotlib.org/devdocs/api/_as_gen/matplotlib.pyplot.html) contains all of the functions you can call within matplotlib, along with detailed information about how you can use them and their parameters

A handy "quick guide" to the different kinds of plotting functions within pyplot can also be found at the [pyplot API](https://matplotlib.org/api/pyplot_api.html)

Let's first try using `df.plot()` on our no_nans dataframe


```python
no_nans.plot()
plt.show()
```


![png](output_203_0.png)


But that's not particularly informative, and much of the graph is completely unreadable.

Matplotlib has great customisability, so let's play around with trying to plot different types of graphs with our data

## Line Plot

Line plots are great for plotting series data, usually a time series of some description.

Line plots are also the default plot drawn when calling `.plot()`.

Within the plot function you can also specify the figure title and the figure size


```python
no_nans.groupby(by = ['Year', 'Month']).mean().Counts.plot( # Drawing a pot from the grouped Year, Month data
                                                figsize = (8,6), # The figure size
                                                title = 'Mean Pedestrian Counts per Month, per Year') #Figure Title

# #Let's customise it a bit further
# #specifying the x-axis label
# plt.xlabel('Pedestrian Counts per Year, Month')

# # the y-axis label
# plt.ylabel('Counts (mean)')

# #Rotating the x-axis labels
# plt.xticks(rotation=45)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x29800036c18>




![png](output_206_1.png)


## Bar Chart

Bar charts are great for categorical data, like examining "Seasons", for example.

Bar charts aren't generally recommended for time series data, such as plotting counts/hour, or mean counts per month, as these are more suited for line plots.


```python
# You can still use the plot function, but specify which columns and plot type you would like to use
no_nans.groupby(by = ['Season']).mean().Counts.plot(kind = 'bar',figsize = (8, 6)) 

# #Figure title
# title("Mean Pedestrian Counts per Season")

# # x-axis label
# plt.xlabel('Mean Counts')

# #y-axis label
# plt.ylabel('Season')

```




    <matplotlib.axes._subplots.AxesSubplot at 0x29800171278>




![png](output_208_1.png)


## Boxplot
Box plots are good for viewing the spread of data within certain categories. These can be useful to measure whether two conditions could be said to be approximately equal, for example.


```python

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Season</th>
      <th>Counts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>64995</th>
      <td>Autumn</td>
      <td>1616.0</td>
    </tr>
    <tr>
      <th>64996</th>
      <td>Autumn</td>
      <td>1223.0</td>
    </tr>
    <tr>
      <th>64997</th>
      <td>Autumn</td>
      <td>741.0</td>
    </tr>
    <tr>
      <th>64998</th>
      <td>Autumn</td>
      <td>1069.0</td>
    </tr>
    <tr>
      <th>64999</th>
      <td>Autumn</td>
      <td>941.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure(figsize = (6,8))
# ax = fig.gca() #defining axis

# The data and plot to use                                      
no_nans[['Season','Counts']][0:65000].boxplot(by = 'Season')

# # x-axis label
# plt.xlabel('Season')

# #y-axis label
# plt.ylabel('Mean Counts')

# #figure title
# title("Pedestrian Counts per Season")

# #If you want to SAVE your plot, make sure you do it BEFORE you show it in the notebook
# #plt.savefig('SeasonsBoxplot.png', dpi=300)

#Generating an image
plt.show()
```


    <matplotlib.figure.Figure at 0x2980044dcc0>



![png](output_211_1.png)


## Histograms

Histograms let you observe the distribution of your (continuous) data, which means that it isn't suitable for all data types.

One thing we could use to observe, however, are how the average number of pedestrians per month is distributed


```python
#plt.hist takes the data you want to plot
plt.hist(no_nans.groupby(by = "Month").mean().Counts, bins = 12, normed = False)

#specifying the x-axis label
plt.xlabel('Mean Pedestrian Counts')

# the y-axis label
plt.ylabel('Frequency')

#Generating an image
plt.show()

```


![png](output_213_0.png)


## ScatterPlots

Scatter plots also allow you observe the relationship between two conditions within your data. This works best when you have two continuous variables. With the use of colours and shapes, you can also observe how certain conditions cluster throughout this relationship.


```python
no_nans.plot(kind='scatter', x = "Counts", y = "Month")

plt.show()
```


![png](output_215_0.png)


Unfortunately we don't actually have two continuous variables within this dataset, so our scatter plot sorts according to their discrete categories.

Instead, let's try reading in the metadata for the sensor location, in the "pedestrian_sensor_locations.csv" file.

Tihs file contains latitude and longitude data, and would show a more ideal scatterplot projection.


```python
sensorLt = pd.read_csv("pedestrian_sensor_locations.csv")
sensorLt.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Sensor ID</th>
      <th>Sensor Name</th>
      <th>Sensor Description</th>
      <th>Status</th>
      <th>Upload Date</th>
      <th>Year Installed</th>
      <th>Location Type</th>
      <th>Geometry</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22</td>
      <td>Eli274_T</td>
      <td>Flinders St-Elizabeth St (East)</td>
      <td>Installed</td>
      <td>19/08/2016 12:39:50 PM +0000</td>
      <td>2013</td>
      <td>Outdoor</td>
      <td>(-37.8178644478193, 144.965068228214)</td>
      <td>-37.817864</td>
      <td>144.965068</td>
    </tr>
    <tr>
      <th>1</th>
      <td>34</td>
      <td>Fli32_T</td>
      <td>Flinders St-Spark La</td>
      <td>Installed</td>
      <td>19/08/2016 12:39:50 PM +0000</td>
      <td>2014</td>
      <td>Outdoor</td>
      <td>(-37.8153798501116, 144.974150495806)</td>
      <td>-37.815380</td>
      <td>144.974150</td>
    </tr>
    <tr>
      <th>2</th>
      <td>11</td>
      <td>WatCit_T</td>
      <td>Waterfront City</td>
      <td>Installed</td>
      <td>19/08/2016 12:39:50 PM +0000</td>
      <td>2009</td>
      <td>Outdoor</td>
      <td>(-37.8154565643471, 144.939579299407)</td>
      <td>-37.815457</td>
      <td>144.939579</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
      <td>WebBN_T</td>
      <td>Webb Bridge</td>
      <td>Installed</td>
      <td>19/08/2016 12:39:50 PM +0000</td>
      <td>2009</td>
      <td>Outdoor</td>
      <td>(-37.8229354263742, 144.947175106953)</td>
      <td>-37.822935</td>
      <td>144.947175</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7</td>
      <td>Fed_T</td>
      <td>Birrarung Marr</td>
      <td>Installed</td>
      <td>19/08/2016 12:39:50 PM +0000</td>
      <td>2009</td>
      <td>Outdoor</td>
      <td>(-37.8183237506589, 144.971414820121)</td>
      <td>-37.818324</td>
      <td>144.971415</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Setting up a list of "area" values, scaled according to the mean pedestrian counts for each location

# This lets us create a size scale for our scatter plot points based on the proportion of pedestrians at each location
area = (pdsn.groupby("Location").mean().Counts)/(pdsn.groupby("Location").mean().Counts.max()) * 100

sensorLt.plot(kind='scatter', x = "Latitude", y = "Longitude", s = area)

plt.show()
```


![png](output_218_0.png)



```python

```


```python

```

# Summary

I hope this has been an informative, although limited, introduction into the plotting and functional capabilites of the Pandas library. There are still many things you can learn, and many other ways to use it. If you're stuck, don't be afraid to throw as many key words into Google Overlord as possible, and hope that the people at StackOverflow have been kind enough (and kind) to answer a similar question.

To keep you going on your journey, here are a few tutorials or Pandas-based problem sets you might like to use to practice:  
* [Github Repo 1](https://github.com/guipsamora/pandas_exercises)
* [Github Repo 2](https://github.com/ajcr/100-pandas-puzzles) (some of these might seem familiar)
* [10 minutes to Pandas](http://pandas.pydata.org/pandas-docs/version/0.17.0/10min.html)
* [Essential Basic Functionality](http://pandas.pydata.org/pandas-docs/version/0.17.0/basics.html)
* [Pandas Dev recommended tutorials](http://pandas.pydata.org/pandas-docs/stable/tutorials.html)

Follow us on Twitter and Eventbrite to keep up-to-date on new and up-coming trainings
- [@ResPlat](https://twitter.com/resplat)
- [All currently planned Eventbrite trainings](https://www.eventbrite.com.au/o/research-platforms-services-10600096884)


```python

```
