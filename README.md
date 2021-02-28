# `%sql` Extension vs `sqlite3` Library - Which one is faster in executing SQL query?
This Notebook will measure runtime to execute SQL queries using two (2) different methods:

<b>`%sql` Extension</b> [Read the documentation here](https://pypi.org/project/ipython-sql/)
<br>versus<br>
<b>`sqlite3` Library</b> [Read the documentation here](https://docs.python.org/3/library/sqlite3.html)

The runtime analysis is executed using the `%%timeit` IPython Magic function. [Read the documentation here](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit)

This Notebook is made possible thanks to the Kaggle.com Notebook entry <b>Getting Started</b> by Anna Wolak. [Check out the Notebook here!](https://www.kaggle.com/annawolak/getting-started-1)

## Source for the Data
The data is provided by the US Dept. of Agriculture under the Pesticide Data Program (2015).
- Read the official documentations [here](https://www.ams.usda.gov/datasets/pdp/pdpdata).
- The data, in compressed .ZIP format is openly availabe in Kaggle.com Datasets [here](https://www.kaggle.com/usdeptofag/pesticide-data-program-2015).

<b>REMINDER:</b> To run this Notebook in your local machine,
1. Download this Notebook, and move it into a new folder
2. Download the `database.sqlite.zip` file either from this repository or from the Kaggle.com Datasets [link](https://www.kaggle.com/usdeptofag/pesticide-data-program-2015)
3. Extract the `database.sqlite` file, and move it into the same directory/folder as this Notebook in your local machine


## Summary on the Notebook
- A total of 8 runtime tests including one (1) extra runtime test are conducted using the `%%timeit` IPython Magic function
- Recorded runtimes are displayed via Seaborn `barplot` to show the difference in runtime performances

### Test #1 - Querying the entire Table `sampledata15`
```{python}
'SELECT * FROM sampledata15'
```
![Test_1_results](test_1.png?raw=true)

### Test #2 - Querying the first 10 rows of the Table `sampledata15`
```{python}
'SELECT * FROM sampledata15 LIMIT 10'
```
![Test_2_results](test_2.png?raw=true)

### Test #3 - Querying the first column of the Table `sampledata15`
```{python}
'SELECT sample_pk FROM sampledata15'
```
![Test_3_results](test_3.png?raw=true)

### Test #4 - Querying the common values in a specific column from the Table `sampledata15`
```{python}
'SELECT DISTINCT state FROM sampledata15'
```
![Test_4_results](test_4.png?raw=true)

### Test #5 - Querying the Table `resultsdata15` where `concen` column does not have any empty values
```{python}
'SELECT * FROM resultsdata15 WHERE concen IS NOT NULL'
```
![Test_5_results](test_5.png?raw=true)

### Test #6 - Querying the Table `resultsdata15` where `concen` column is sorted in descending order
```{python}
'SELECT * FROM resultsdata15 WHERE concen IS NOT NULL ORDER BY concen DESC'
```
![Test_6_results](test_6.png?raw=true)

### Test #7 - Querying the combination of Tables `resultsdata15` and `sampledata15` by INNER JOIN
```{python}
'''SELECT r.sample_pk, r.commod, r.commtype, r.lod, s.state, s.claim
FROM resultsdata15 AS r INNER JOIN sampledata15 AS s
ON r.sample_pk = s.sample_pk AND r.commod = s.commod AND r.commtype = s.commtype'''
```
![Test_7_results](test_7.png?raw=true)
<br>

- From all these 7 runtime tests, `%sql` Extension performs faster than `sqlite3` Library in executing SQL queries.

<br>


### Extra - Storing the Test #7 query before in a Pandas DataFrame
![Test_8_results](test_8.png?raw=true)

- When involving the query results to be stored in a DataFrame, `%sql` Extension performs a bit slower than `sqlite3` Library.
