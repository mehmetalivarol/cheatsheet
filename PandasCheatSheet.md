# Cheat Sheet of Useful Pandas Idioms

## Processing Data

Keep only the first two columns in a dataframe

```python
df= df.ix[:,:2]
```

Rename columns with regular expressions
```python
t.columns = t.columns.str.replace(r'[^\x00-\x7F]+','')
```

Keep several groups of columns in a dataframe

```python
df1 = df[list(df.columns[0:1]) + list(df.columns[10:131])]
```

Flatten heirarchical index into one column (assuming previous index was 2 columns)
```python
df4 = pd.DataFrame(df3.to_records())
```

Append multiple files in the same directory into a single dictionary where keys are file names and values are dataframes
(if statement is optional, I use as safeguard to get only specific files from a directory)

```python
d = {f: pd.read_csv(f) for f in os.listdir(os.getcwd()) if f.endswith(".csv")}

##accessing the dictionary:

for k,v in d.items():
    print k #filenames
    print v #dataframes

##accessing specific dataframe: 

d['filename.csv']

```

## Aggregating Data

### Group by columns

```python
df.groupby( [ 'column1', 'column2'] ).size().reset_index(name='count')
```

### Get distinct counts of all columns in dataframe:

```python
df.nunique()
```
### Send column names to a list

```python
columns = df.columns.tolist()
```


## Selecting Data

Selecting specific index value (instead of by position)

```python
df1.loc[df1.index == '11/000,028']
```

or 

```python
df3.loc['11/000,028']
```

Group by and order by, SQL-like; two column dataframe 
```python 
df2 = pd.DataFrame(df.groupby('column').size().reset_index())
df2.columns = ['column', 'total']
df2.sort_values(['column'])
```

Select column from table where column_2 = x

```python
df[df['column_2'] == '1700']['column'].head(5)
```

## Statistical Methods

Pairwise correlation of discrete or continuous variables [(methods pearson, kendall, spearman available)](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.corr.html)

```python
df.corr(method='pearson')["column"]
```

## Charting Data

### Histogram of non-numeric data

```python
histo = df.groupby('column').size()
plt.figure();
histo.plot.bar(); plt.axhline(0, color='k')
```

### Histogram of all columns in DF, regardless of numeric values

```python
for colname in df: 
    histo = df.groupby(colname).size()
    plt.figure();
    histo.plot.bar(); plt.axhline(0, color='k')
```
