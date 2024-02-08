## Movie Correlations

I checked which features of the movies affected on gross earning.


### Installation

- pip install pandas
- pip install numpy
- pip install matplotlib
- pip install seaborn


### Data Sources

[Data](https://www.kaggle.com/datasets/danielgrijalvas/movies)

### Description

I want to know what factors affect gross earnings.
This loop iterates over each column in the DataFrame `df` and calculates the percentage of missing values in each column. It then prints the column name and the corresponding percentage of missing values.

``` python
for col in df.columns:
    pct_missing = np.mean(df[col].isnull())
    print('{} - {}%'.format(col, pct_missing))
```

These lines of code fill any missing values in the 'budget' and 'gross' columns of the DataFrame `df` with 0 and then convert the values in the column to the integer data type. 

``` python
df['budget'] = df['budget'].fillna(value=0)
df['budget'] = df['budget'].astype(int)

df['gross'] = df['gross'].fillna(value=0)
df['gross'] = df['gross'].astype(int)
```


This function sorts the DataFrame `df` in descending order based on the values in the 'gross' column. The `by` parameter specifies the column to sort by, `inplace=False` returns a new sorted DataFrame without modifying the original, and `ascending=False` ensures that the sorting is done in descending order.

``` python
df.sort_values(by=['gross'], inplace=False, ascending=False)
```

This function sets the display option for pandas to show all rows of a DataFrame without any truncation. In this case, `None` is used to indicate that there is no limit to the number of rows displayed

```python
pd.set_option('display.max_rows', None)
```

This line of code returns the unique values in the 'company' column of the DataFrame `df` while removing any duplicates. The values are then sorted in descending order.

```python
df['company'].drop_duplicates().sort_values(ascending=False)
```

These lines of code create a scatter plot using the 'budget' column as the x-axis and the 'gross' column as the y-axis.

``` python
plt.scatter(x=df['budget'], y=df['gross'])
plt.title('Budget vs Gross Earnings')
plt.xlabel('Gross Earnings')
plt.ylabel('Budget for Film')
plt.show()
```
This function creates a scatter plot with a regression line using Seaborn. It specifies the 'budget' column as the x-axis, the 'gross' column as the y-axis, and the DataFrame `df` as the data source. The `scatter_kws` parameter sets the color of the scatter points to red, and the `line_kws` parameter sets the color of the regression line to blue.

``` python
sns.regplot(x='budget', y='gross', data=df, scatter_kws={"color": "red"}, line_kws={"color":"blue"})
```

This function calculates the pairwise correlation between all numeric columns in the DataFrame `df`. It returns a correlation matrix with correlation coefficients ranging from -1 to 1, where values close to 1 indicate a strong positive correlation, values close to -1 indicate a strong negative correlation, and values close to 0 indicate no correlation.

``` python
df.corr(numeric_only = True)
```

These lines of code create a heatmap using Seaborn to visualize the correlation matrix. It takes the `correlation_matrix` as input and displays the correlation coefficients as annotations in the heatmap.

``` python
correlation_matrix = df.corr(method='pearson', numeric_only = True)
sns.heatmap(correlation_matrix, annot=True)
plt.title('correlation Matrix for Numeric Features')
plt.xlabel('Movie Features')
plt.ylabel('Movie Features')
plt.show()
```

These lines of code create a new DataFrame `df_numerized` that is a copy of the original DataFrame `df`. It then converts columns with the data type 'object' (typically strings) into the 'category' data type and assigns numerical codes to the categories. Finally, it displays the first few rows of the modified DataFrame.

``` python
df_numerized = df

for col_name in df_numerized.columns:
    if(df_numerized[col_name].dtype == 'object'):
        df_numerized[col_name] = df_numerized[col_name].astype('category')
        df_numerized[col_name] = df_numerized[col_name].cat.codes
        
df_numerized.head()
```

This function calculates the correlation matrix for the DataFrame `df_numerized` using the Pearson correlation coefficient. It only considers numeric columns (`numeric_only=True`) and returns a correlation matrix.

``` python
correlation_matrix = df_numerized.corr(method='pearson', numeric_only = True)
sns.heatmap(correlation_matrix, annot=True)
plt.title('Correlation Matrix for Numeric Features')
plt.xlabel('Movie Features')
plt.ylabel('Movie Features')
plt.show()
```

This function calculates the correlation matrix for the DataFrame `df_numerized` using the default method (Pearson correlation coefficient).

``` python
df_numerized.corr()
```

These lines of code calculate the correlation matrix for the DataFrame `df_numerized` and then transform it into a Series object called `corr_pairs`.

``` python
correlation_mat = df_numerized.corr()
corr_pairs = correlation_mat.unstack()
corr_pairs
```
This line of code sorts the `corr_pairs` Series in ascending order based on the correlation coefficients.

``` python
sorted_pairs = corr_pairs.sort_values()
sorted_pairs
```

This line of code selects the pairs of columns from the `sorted_pairs` Series where the correlation coefficient is greater than 0.5. These pairs represent high correlation relationships between the columns in the DataFrame.

``` python
high_corr = sorted_pairs[(sorted_pairs) > 0.5]
high_corr
```

votes and budget have the highest correlation to gross earnings and Company has low correlation.
