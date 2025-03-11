Learn how to summarize the columns available in an R data frame. 
  You will also learn how to chain operations together with the
  pipe operator, and how to compute grouped summaries using.

## Welcome!

Hey there! Ready for the first lesson?

The dfply package makes it possible to do R's dplyr-style data manipulation with pipes in python on pandas DataFrames.

[dfply website here](https://github.com/kieferk/dfply)

[![](https://www.rforecology.com/pipes_image0.png "https://github.com/kieferk/dfply"){width="600"}](https://github.com/kieferk/dfply)


```python
import pandas as pd
import seaborn as sns
cars=sns.load_dataset('mpg')
from dfply import*
cars>>head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
    </tr>
  </tbody>
</table>
</div>



## The \>\> and \>\>=

dfply works directly on pandas DataFrames, chaining operations on the data with the >> operator, or alternatively starting with >>= for inplace operations.

*The X DataFrame symbol*

The DataFrame as it is passed through the piping operations is represented by the symbol X. It records the actions you want to take (represented by the Intention class), but does not evaluate them until the appropriate time. Operations on the DataFrame are deferred. Selecting two of the columns, for example, can be done using the symbolic X DataFrame during the piping operations.

### Exercise 1.

Select the columns 'mpg' and 'horsepower' from the cars DataFrame.


```python
cars>>select(X.mpg,X.horsepower)>>head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>horsepower</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>130.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>165.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>150.0</td>
    </tr>
  </tbody>
</table>
</div>



## Selecting and dropping

There are two functions for selection, inverse of each other: select and drop. The select and drop functions accept string labels, integer positions, and/or symbolically represented column names (X.column). They also accept symbolic "selection filter" functions, which will be covered shortly.

### Exercise 2.

Select the columns 'mpg' and 'horsepower' from the cars DataFrame using the drop function.


```python
cars>>drop(X.cylinders, X.displacement, X.weight, X.acceleration, X.model_year, X.origin, X.name)>>head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>horsepower</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>130.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>165.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>150.0</td>
    </tr>
  </tbody>
</table>
</div>



## Selection using \~

One particularly nice thing about dplyr's selection functions is that you can drop columns inside of a select statement by putting a subtraction sign in front, like so: ... %>% select(-col). The same can be done in dfply, but instead of the subtraction operator you use the tilde ~.

### Exercise 3.

Select all columns except 'model_year', and 'name' from the cars DataFrame.


```python
cars>>select(~X.model_year, ~X.name)>>head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>origin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>usa</td>
    </tr>
  </tbody>
</table>
</div>



## Filtering columns

The vanilla select and drop functions are useful, but there are a variety of selection functions inspired by dplyr available to make selecting and dropping columns a breeze. These functions are intended to be put inside of the select and drop functions, and can be paired with the ~ inverter.

First, a quick rundown of the available functions:

-   starts_with(prefix): find columns that start with a string prefix.
-   ends_with(suffix): find columns that end with a string suffix.
-   contains(substr): find columns that contain a substring in their name.
-   everything(): all columns.
-   columns_between(start_col, end_col, inclusive=True): find columns between a specified start and end column. The inclusive boolean keyword argument indicates whether the end column should be included or not.
-   columns_to(end_col, inclusive=True): get columns up to a specified end column. The inclusive argument indicates whether the ending column should be included or not.
-   columns_from(start_col): get the columns starting at a specified column.

### Exercise 4.

The selection filter functions are best explained by example. Let's say I wanted to select only the columns that started with a "c":


```python
cars >> select(starts_with('c')) >> head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cylinders</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



### Exercise 5.

Select the columns that contain the substring "e" from the cars DataFrame.


```python
cars >> select(contains('e')) >> head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>plymouth satellite</td>
    </tr>
  </tbody>
</table>
</div>



### Exercise 6.

Select the columns that are between 'mpg' and 'origin' from the cars DataFrame.


```python
cars >> select(columns_between('mpg', 'origin')) >> head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
    </tr>
  </tbody>
</table>
</div>



## Subsetting and filtering

### row_slice()

Slices of rows can be selected with the row_slice() function. You can pass single integer indices or a list of indices to select rows as with. This is going to be the same as using pandas' .iloc.

#### Exercise 7.

Select the first three rows from the cars DataFrame.


```python
cars >> head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
    </tr>
  </tbody>
</table>
</div>



### distinct()

Selection of unique rows is done with distinct(), which similarly passes arguments and keyword arguments through to the DataFrame's .drop_duplicates() method.

#### Exercise 8.

Select the unique rows from the 'origin' column in the cars DataFrame.


```python
cars >> distinct(X.origin)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>14</th>
      <td>24.0</td>
      <td>4</td>
      <td>113.0</td>
      <td>95.0</td>
      <td>2372</td>
      <td>15.0</td>
      <td>70</td>
      <td>japan</td>
      <td>toyota corona mark ii</td>
    </tr>
    <tr>
      <th>19</th>
      <td>26.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>46.0</td>
      <td>1835</td>
      <td>20.5</td>
      <td>70</td>
      <td>europe</td>
      <td>volkswagen 1131 deluxe sedan</td>
    </tr>
  </tbody>
</table>
</div>



## mask()

Filtering rows with logical criteria is done with mask(), which accepts boolean arrays "masking out" False labeled rows and keeping True labeled rows. These are best created with logical statements on symbolic Series objects as shown below. Multiple criteria can be supplied as arguments and their intersection will be used as the mask.

### Exercise 9.

Filter the cars DataFrame to only include rows where the 'mpg' is greater than 20, origin Japan, and display the first three rows:


```python
cars>>mask(X.mpg>20, X.origin=="japan" )>>head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>24.0</td>
      <td>4</td>
      <td>113.0</td>
      <td>95.0</td>
      <td>2372</td>
      <td>15.0</td>
      <td>70</td>
      <td>japan</td>
      <td>toyota corona mark ii</td>
    </tr>
    <tr>
      <th>18</th>
      <td>27.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>88.0</td>
      <td>2130</td>
      <td>14.5</td>
      <td>70</td>
      <td>japan</td>
      <td>datsun pl510</td>
    </tr>
    <tr>
      <th>29</th>
      <td>27.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>88.0</td>
      <td>2130</td>
      <td>14.5</td>
      <td>71</td>
      <td>japan</td>
      <td>datsun pl510</td>
    </tr>
  </tbody>
</table>
</div>



## pull()

The pull() function is used to extract a single column from a DataFrame as a pandas Series. This is useful for passing a single column to a function or for further manipulation.

### Exercise 10.

Extract the 'mpg' column from the cars DataFrame, japanese origin, model year 70s, and display the first three rows.


```python
cars>>filter_by(X.origin=='japan', X.model_year==70)>>select(X.mpg)>>head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>24.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>27.0</td>
    </tr>
  </tbody>
</table>
</div>



## DataFrame transformation

*mutate()*

The mutate() function is used to create new columns or modify existing columns. It accepts keyword arguments of the form new_column_name = new_column_value, where new_column_value is a symbolic Series object.

### Exercise 11.

Create a new column 'mpg_per_cylinder' in the cars DataFrame that is the result of dividing the 'mpg' column by the 'cylinders' column.


```python
cars>>mutate(mpg_per_cylinder=X.mpg/X.cylinders)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
      <th>mpg_per_cylinder</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
      <td>2.250</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
      <td>1.875</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
      <td>2.250</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>amc rebel sst</td>
      <td>2.000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>70</td>
      <td>usa</td>
      <td>ford torino</td>
      <td>2.125</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>27.0</td>
      <td>4</td>
      <td>140.0</td>
      <td>86.0</td>
      <td>2790</td>
      <td>15.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford mustang gl</td>
      <td>6.750</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>82</td>
      <td>europe</td>
      <td>vw pickup</td>
      <td>11.000</td>
    </tr>
    <tr>
      <th>395</th>
      <td>32.0</td>
      <td>4</td>
      <td>135.0</td>
      <td>84.0</td>
      <td>2295</td>
      <td>11.6</td>
      <td>82</td>
      <td>usa</td>
      <td>dodge rampage</td>
      <td>8.000</td>
    </tr>
    <tr>
      <th>396</th>
      <td>28.0</td>
      <td>4</td>
      <td>120.0</td>
      <td>79.0</td>
      <td>2625</td>
      <td>18.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford ranger</td>
      <td>7.000</td>
    </tr>
    <tr>
      <th>397</th>
      <td>31.0</td>
      <td>4</td>
      <td>119.0</td>
      <td>82.0</td>
      <td>2720</td>
      <td>19.4</td>
      <td>82</td>
      <td>usa</td>
      <td>chevy s-10</td>
      <td>7.750</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 10 columns</p>
</div>




*transmute()*

The transmute() function is a combination of a mutate and a selection of the created variables.

### Exercise 12.

Create a new column 'mpg_per_cylinder' in the cars DataFrame that is the result of dividing the 'mpg' column by the 'cylinders' column, and display only the new column.


```python
cars>>transmute(mpg_per_cylinder=X.mpg/X.cylinders)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg_per_cylinder</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.250</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.875</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.250</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.125</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>6.750</td>
    </tr>
    <tr>
      <th>394</th>
      <td>11.000</td>
    </tr>
    <tr>
      <th>395</th>
      <td>8.000</td>
    </tr>
    <tr>
      <th>396</th>
      <td>7.000</td>
    </tr>
    <tr>
      <th>397</th>
      <td>7.750</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 1 columns</p>
</div>



## Grouping

*group_by() and ungroup()*

The group_by() function is used to group the DataFrame by one or more columns. This is useful for creating groups of rows that can be summarized or transformed together. The ungroup() function is used to remove the grouping.

### Exercise 13.

Group the cars DataFrame by the 'origin' column and calculate the lead of the 'mpg' column.


```python
cars>>group_by(X.origin)>>mutate(mpg_lead=lead(X.mpg))
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
      <th>mpg_lead</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19</th>
      <td>26.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>46.0</td>
      <td>1835</td>
      <td>20.5</td>
      <td>70</td>
      <td>europe</td>
      <td>volkswagen 1131 deluxe sedan</td>
      <td>25.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>25.0</td>
      <td>4</td>
      <td>110.0</td>
      <td>87.0</td>
      <td>2672</td>
      <td>17.5</td>
      <td>70</td>
      <td>europe</td>
      <td>peugeot 504</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>24.0</td>
      <td>4</td>
      <td>107.0</td>
      <td>90.0</td>
      <td>2430</td>
      <td>14.5</td>
      <td>70</td>
      <td>europe</td>
      <td>audi 100 ls</td>
      <td>25.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>25.0</td>
      <td>4</td>
      <td>104.0</td>
      <td>95.0</td>
      <td>2375</td>
      <td>17.5</td>
      <td>70</td>
      <td>europe</td>
      <td>saab 99e</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>26.0</td>
      <td>4</td>
      <td>121.0</td>
      <td>113.0</td>
      <td>2234</td>
      <td>12.5</td>
      <td>70</td>
      <td>europe</td>
      <td>bmw 2002</td>
      <td>28.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>392</th>
      <td>27.0</td>
      <td>4</td>
      <td>151.0</td>
      <td>90.0</td>
      <td>2950</td>
      <td>17.3</td>
      <td>82</td>
      <td>usa</td>
      <td>chevrolet camaro</td>
      <td>27.0</td>
    </tr>
    <tr>
      <th>393</th>
      <td>27.0</td>
      <td>4</td>
      <td>140.0</td>
      <td>86.0</td>
      <td>2790</td>
      <td>15.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford mustang gl</td>
      <td>32.0</td>
    </tr>
    <tr>
      <th>395</th>
      <td>32.0</td>
      <td>4</td>
      <td>135.0</td>
      <td>84.0</td>
      <td>2295</td>
      <td>11.6</td>
      <td>82</td>
      <td>usa</td>
      <td>dodge rampage</td>
      <td>28.0</td>
    </tr>
    <tr>
      <th>396</th>
      <td>28.0</td>
      <td>4</td>
      <td>120.0</td>
      <td>79.0</td>
      <td>2625</td>
      <td>18.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford ranger</td>
      <td>31.0</td>
    </tr>
    <tr>
      <th>397</th>
      <td>31.0</td>
      <td>4</td>
      <td>119.0</td>
      <td>82.0</td>
      <td>2720</td>
      <td>19.4</td>
      <td>82</td>
      <td>usa</td>
      <td>chevy s-10</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 10 columns</p>
</div>



## Reshaping

*arrange()*

The arrange() function is used to sort the DataFrame by one or more columns. This is useful for reordering the rows of the DataFrame.

### Exercise 14.

Sort the cars DataFrame by the 'mpg' column in descending order.


```python
cars>>arrange(desc(X.mpg))
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>322</th>
      <td>46.6</td>
      <td>4</td>
      <td>86.0</td>
      <td>65.0</td>
      <td>2110</td>
      <td>17.9</td>
      <td>80</td>
      <td>japan</td>
      <td>mazda glc</td>
    </tr>
    <tr>
      <th>329</th>
      <td>44.6</td>
      <td>4</td>
      <td>91.0</td>
      <td>67.0</td>
      <td>1850</td>
      <td>13.8</td>
      <td>80</td>
      <td>japan</td>
      <td>honda civic 1500 gl</td>
    </tr>
    <tr>
      <th>325</th>
      <td>44.3</td>
      <td>4</td>
      <td>90.0</td>
      <td>48.0</td>
      <td>2085</td>
      <td>21.7</td>
      <td>80</td>
      <td>europe</td>
      <td>vw rabbit c (diesel)</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>82</td>
      <td>europe</td>
      <td>vw pickup</td>
    </tr>
    <tr>
      <th>326</th>
      <td>43.4</td>
      <td>4</td>
      <td>90.0</td>
      <td>48.0</td>
      <td>2335</td>
      <td>23.7</td>
      <td>80</td>
      <td>europe</td>
      <td>vw dasher (diesel)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>67</th>
      <td>11.0</td>
      <td>8</td>
      <td>429.0</td>
      <td>208.0</td>
      <td>4633</td>
      <td>11.0</td>
      <td>72</td>
      <td>usa</td>
      <td>mercury marquis</td>
    </tr>
    <tr>
      <th>103</th>
      <td>11.0</td>
      <td>8</td>
      <td>400.0</td>
      <td>150.0</td>
      <td>4997</td>
      <td>14.0</td>
      <td>73</td>
      <td>usa</td>
      <td>chevrolet impala</td>
    </tr>
    <tr>
      <th>25</th>
      <td>10.0</td>
      <td>8</td>
      <td>360.0</td>
      <td>215.0</td>
      <td>4615</td>
      <td>14.0</td>
      <td>70</td>
      <td>usa</td>
      <td>ford f250</td>
    </tr>
    <tr>
      <th>26</th>
      <td>10.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>200.0</td>
      <td>4376</td>
      <td>15.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevy c20</td>
    </tr>
    <tr>
      <th>28</th>
      <td>9.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>193.0</td>
      <td>4732</td>
      <td>18.5</td>
      <td>70</td>
      <td>usa</td>
      <td>hi 1200d</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 9 columns</p>
</div>




*rename()*

The rename() function is used to rename columns in the DataFrame. It accepts keyword arguments of the form new_column_name = old_column_name.

### Exercise 15.

Rename the 'mpg' column to 'miles_per_gallon' in the cars DataFrame.


```python
cars>>rename(miles_per_gallon= 'mpg')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>miles_per_gallon</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>amc rebel sst</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>70</td>
      <td>usa</td>
      <td>ford torino</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>27.0</td>
      <td>4</td>
      <td>140.0</td>
      <td>86.0</td>
      <td>2790</td>
      <td>15.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford mustang gl</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>82</td>
      <td>europe</td>
      <td>vw pickup</td>
    </tr>
    <tr>
      <th>395</th>
      <td>32.0</td>
      <td>4</td>
      <td>135.0</td>
      <td>84.0</td>
      <td>2295</td>
      <td>11.6</td>
      <td>82</td>
      <td>usa</td>
      <td>dodge rampage</td>
    </tr>
    <tr>
      <th>396</th>
      <td>28.0</td>
      <td>4</td>
      <td>120.0</td>
      <td>79.0</td>
      <td>2625</td>
      <td>18.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford ranger</td>
    </tr>
    <tr>
      <th>397</th>
      <td>31.0</td>
      <td>4</td>
      <td>119.0</td>
      <td>82.0</td>
      <td>2720</td>
      <td>19.4</td>
      <td>82</td>
      <td>usa</td>
      <td>chevy s-10</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 9 columns</p>
</div>




*gather()*

The gather() function is used to reshape the DataFrame from wide to long format. It accepts keyword arguments of the form new_column_name = new_column_value, where new_column_value is a symbolic Series object.

### Exercise 16.

Reshape the cars DataFrame from wide to long format by gathering the columns 'mpg', 'horsepower', 'weight', 'acceleration', and 'displacement' into a new column 'variable' and their values into a new column 'value'.


```python
cars>>gather("variable", "value", X.mpg, X.horsepower, X.weight, X.acceleration, X.displacement)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cylinders</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
      <th>variable</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
      <td>mpg</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
      <td>mpg</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
      <td>mpg</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
      <td>70</td>
      <td>usa</td>
      <td>amc rebel sst</td>
      <td>mpg</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>70</td>
      <td>usa</td>
      <td>ford torino</td>
      <td>mpg</td>
      <td>17.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1985</th>
      <td>4</td>
      <td>82</td>
      <td>usa</td>
      <td>ford mustang gl</td>
      <td>displacement</td>
      <td>140.0</td>
    </tr>
    <tr>
      <th>1986</th>
      <td>4</td>
      <td>82</td>
      <td>europe</td>
      <td>vw pickup</td>
      <td>displacement</td>
      <td>97.0</td>
    </tr>
    <tr>
      <th>1987</th>
      <td>4</td>
      <td>82</td>
      <td>usa</td>
      <td>dodge rampage</td>
      <td>displacement</td>
      <td>135.0</td>
    </tr>
    <tr>
      <th>1988</th>
      <td>4</td>
      <td>82</td>
      <td>usa</td>
      <td>ford ranger</td>
      <td>displacement</td>
      <td>120.0</td>
    </tr>
    <tr>
      <th>1989</th>
      <td>4</td>
      <td>82</td>
      <td>usa</td>
      <td>chevy s-10</td>
      <td>displacement</td>
      <td>119.0</td>
    </tr>
  </tbody>
</table>
<p>1990 rows × 6 columns</p>
</div>




*spread()*

Likewise, you can transform a "long" DataFrame into a "wide" format with the spread(key, values) function. Converting the previously created elongated DataFrame for example would be done like so.

### Exercise 17.

Reshape the cars DataFrame from long to wide format by spreading the 'variable' column into columns and their values into the 'value' column.


```python

cars>>spread(X.variable, X.value)
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    ~\AppData\Local\Temp\ipykernel_6672\1789188645.py in ?()
    ----> 1 cars>>spread(X.variable, X.value)
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(self, other)
        138         with warnings.catch_warnings():
        139             warnings.simplefilter("ignore")
        140             other_copy._grouped_by = getattr(other, '_grouped_by', None)
        141 
    --> 142         result = self.function(other_copy)
        143 
        144         for p in self.chained_pipes:
        145             result = p.__rrshift__(result)
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(x)
    --> 149         return pipe(lambda x: self.function(x, *args, **kwargs))
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(self, *args, **kwargs)
        276     def __call__(self, *args, **kwargs):
        277         df = args[0]
        278 
    --> 279         args = self._recursive_arg_eval(df, args[1:])
        280         kwargs = self._recursive_kwarg_eval(df, kwargs)
        281 
        282         return self.function(df, *args, **kwargs)
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(self, df, args)
        237         eval_symbols = self._find_eval_args(self.eval_symbols, args)
        238         eval_as_label = self._find_eval_args(self.eval_as_label, args)
        239         eval_as_selector = self._find_eval_args(self.eval_as_selector, args)
        240 
    --> 241         return [
        242             self._symbolic_to_label(df, a) if i in eval_as_label
        243             else self._symbolic_to_selector(df, a) if i in eval_as_selector
        244             else self._symbolic_eval(df, a) if i in eval_symbols
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(self, df, arg)
        230     def _symbolic_to_label(self, df, arg):
    --> 231         return self._evaluator_loop(df, arg, self._evaluate_label)
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(self, df, arg, eval_func)
        221     def _evaluator_loop(self, df, arg, eval_func):
        222         if isinstance(arg, (list, tuple)):
        223             return [self._evaluator_loop(df, a_, eval_func) for a_ in arg]
        224         else:
    --> 225             return eval_func(df, arg)
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(self, df, arg)
        180     def _evaluate_label(self, df, arg):
    --> 181         arg = self._evaluate(df, arg)
        182 
        183         cols = list(df.columns)
        184         if isinstance(arg, pd.Series):
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(self, df, arg)
        172     def _evaluate(self, df, arg):
        173         if isinstance(arg, Intention):
        174             negate = arg.inverted
    --> 175             arg = arg.evaluate(df)
        176             if negate:
        177                 arg = ~arg
        178         return arg
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(self, context)
         70     def evaluate(self, context):
    ---> 71         return self.function(context)
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\dfply\base.py in ?(x)
    ---> 74         return Intention(lambda x: getattr(self.function(x), attribute),
         75                          invert=self.inverted)
    

    ~\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\LocalCache\local-packages\Python312\site-packages\pandas\core\generic.py in ?(self, name)
       6295             and name not in self._accessors
       6296             and self._info_axis._can_hold_identifiers_and_holds_name(name)
       6297         ):
       6298             return self[name]
    -> 6299         return object.__getattribute__(self, name)
    

    AttributeError: 'DataFrame' object has no attribute 'variable'



## Summarization

*summarize()*

The summarize() function is used to calculate summary statistics for groups of rows. It accepts keyword arguments of the form new_column_name = new_column_value, where new_column_value is a symbolic Series object.

### Exercise 18.

Calculate the mean 'mpg' for each group of 'origin' in the cars DataFrame.


```python
cars>>group_by(X.origin)>>summarise(mean_mpg=mean(X.mpg))
```


*summarize_each()*

The summarize_each() function is used to calculate summary statistics for groups of rows. It accepts keyword arguments of the form new_column_name = new_column_value, where new_column_value is a symbolic Series object.

### Exercise 19.

Calculate the mean 'mpg' and 'horsepower' for each group of 'origin' in the cars DataFrame.


```python
cars>> group_by(X.origin) >> summarize(mean_mpg=X.mpg.mean(), mean_horsepower=X.horsepower.mean()) 
```


*summarize() can of course be used with groupings as well.*

### Exercise 20.

Calculate the mean 'mpg' for each group of 'origin' and 'model_year' in the cars DataFrame.


```python
cars>> group_by(X.origin, X.model_year) >> summarise(mean_mpg=mean(X.mpg))
```
