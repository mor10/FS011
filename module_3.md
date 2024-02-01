# Module 3: Tidy Data and Joining Dataframes

## Learning outcomes
- Explain what tidy data is.
- Use `.melt()` and `.pivot()` to reshape dataframes, specifically to make tidy data.
- Learn how to reset the index of a dataframe.
Combine dataframes using `.merge()` and `.concat()` and know when to use these different methods.
Understand the different joining methods.

## Tidy data
Tidy data means 
- each row is a single _observation_
- each _variable_ is a single column
- each _value_ is a single cell

Tidy data is structured and consistent so it can be processed easily by multiple tools without having to reformat or filter the data.

Think one row for each thing, one column for each type of data about the thing, and one data point for each cell.

An example of untidy / non-tidy data is when two or more variables are mixed in a single column (eg both 'protein' and 'calories'). To make this data tidy, you have to first filter based on each variable.

## Statistical Questions and Tidy Data
The concept of Tidy data depends on whether the data is easy to process. 

You can modify (filter/transform) existing dataframes with more data or a different structure to make it tidy for your specific task: 

- Add a new column containing one data point for each observation. This produces a _wide_ dataframe (because more columns = more width).
- Reorganize a dataframe by pulling variables (columns) from the original and adding a new isolated observation based on the value of each column (effectively turning the data on its side). This produces a _long_ dataframe (because more instances of observations means more rows).

Example of long dataframe:

|continent|country|2012|2013|2014|2015|
|Africa   |Algeria|26.1|25.8|25.8|25.6|

Turns into

|continent|country|year|mort|
|Africa   |Algeria|2012|26.1|
|Africa   |Algeria|2013|25.8|
|Africa   |Algeria|2014|25.8|
|Africa   |Algeria|2015|25.6|

## Reshaping with `.pivot()`
`.pivot()` can be used to transform long dataframes into wide dataframes. For example, if you have a long df with a 'nutrition' column containing two variables - 'calories' and 'protein' - you can transform it into a wide df with a column for 'calories' and another for 'protein'.

So for this:

          name mfr nutrition  value
0    Special K   K  calories    110
1    Special K   K   protein      6
2  Apple Jacks   K  calories    110
3  Apple Jacks   K   protein      2
4  Raisin Bran   K  calories    120
5  Raisin Bran   K   protein      3
6     Cheerios   G  calories    110
7     Cheerios   G   protein      6
8     Wheaties   G  calories    100
9     Wheaties   G   protein      3

We can reshape to wide df using this:

`cereal_wide = cereal_long.pivot(index='name', columns='nutrition', values='value')`

- `index` - the new df index
- `columns` - the column to be split into new cols based on the containing values
- `values` - the values to be transferred into the new columns.

When using `.pivot()` the index is lost. 
To reassert the index, use `cereal_wide.reset_index()`

Doing so may pull a header to the index column. 
Clean that up using `cereal_wide.rename_axis('', axis='columns')` where the first argument is the new name and the `axis` argument is the target column.

## Reshaping with `.pivot_table()`
`.pivot()` discards any column not being directly affected by the pivot (specified in the `index` argument).

Using `.pivot_table()` we can preserve the unaffected columns by adding them to the `index` argument:

`cereal_long.pivot_table(index=['name', 'mfr'], columns='nutrition', values='value')`

Remember to `.reset_index()` and `.rename_axis()`.

### Problem
If the `index` column targeted in a `.pivot()` has duplicate entries, we get a conflict. df can't be reshaped because of conflicting variable data in a single observation.

Using `.pivot()` on the same data causes the two conflicting values to be averaged in the new column. __This may or may not be what you want.__

### Solution
Use `.duplicated()` to check for duplicates in the original df before running `.pivot()` or `.pivot_table()`:

`cereal_problem.duplicated(subset=['name', 'nutrition'], keep=False)`

- `subset` - array of relevant columns (in this use case `index` and `column` arguments from the `.pivot()` method.
- `keep` - `true` shows only duplicate rows, `false` also shows original row.

Filter duplicate rows from original dataframe:

```python
duplicate_info =cereal_problem.duplicated(subset=['name', 'nutrition'], keep=False)
cereal_problem[duplicate_info]
```

To remove a duplicate, use `.drop()`:

`cereal_no_problem = cereal_problem.drop(axis=0, index=1)`

- `axis` - which axis to check, can be 0, 1, `index`, or `column`
- `index` - index number of row to be dropped.

### Expanded example

```python
# Convert the untidy data into tidy data using pivot_table 
# Assign set_num as your index and name the new dataframe tidied_lego

tidied_lego = (lego.pivot_table(
					index=['set_num','name','year'],
					columns='lego_info',
					values='value')
                   .reset_index()
              )

# Find the mean number of parts for each production year and save it in an object name year_parts_mean

year_parts_mean = tidied_lego.groupby(by='year').mean()[['num_parts']].round()

year_parts_mean
```

## Reshaping with `.melt()`
`.melt()` is the reverse of `.pivot()`. It reshapes _wide_ dfs to _long_ dfs.

```python
melted_cereal  = (cereal.melt(id_vars=['name', 'mfr'] , 
                              value_vars=['calories', 'protein'], 
                              var_name='nutrition', 
                              value_name='value')
                  )
```

- `id_vars` - main (identifying) column and other columns to keep
- `value_vars` - columns to be melted together
- `var_name` - new melted column name
- `value_name` - the value column corresponding to the new column

## Concatenation
Two different methods for joining dataframes:
- `pd.concat()` - sticking two dfs together
- `.merge()` - merging two dfs under common cols etc.

### Horizontal concatenation
`candy_nutrition = pd.concat([candy, candy2], axis=1)`

- `[df1, df2]` - the dfs to combine
- `axis` - `0` combine along row axis, `1` combine along col axis.

This can cause duplicate columns. To fix it:

`candy_nutrition.loc[:,~candy_nutrition.columns.duplicated()]`

- The `~` operator inverts the boolean values.
Translated: "Give me all columns except the duplicated ones."

### Vertical concatenation
`large_candybars = pd.concat([candy, candy_more], axis=0)`

This causes index chaos as new entries are added.

Reset index using `.reset_index(drop=True)`

- `drop=True` - discards original index

### Caveat
Don't concat dfs horizontally when the rows are out of order. This causes chaos.