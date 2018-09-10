# Data-Wrangling-With-Pandas

### Install numpy and pandas
```python
pip install numpy
pip install pandas
```

---

### Import numpy and pandas
```python
import numpy as np
import pandas as pd
```

---

### Read data from a CSV file (https://raw.githubusercontent.com/jackiekazil/data-wrangling/master/data/chp3/data-text.csv)

##### 1. Get the Metadata from the above file
```python
print(df.info())
```

##### 2. Get the row names from the above file
```python
df.index.values
```

##### 3. Change any column name
```python
df.rename(columns={'Indicator': 'Indicator_id'}).head() # use argument inplace=TRUE to keep the changes
```

##### 5. Change the name of multiple columns
```python
df.rename(columns={'PUBLISH STATES': 'Publication Status', 'WHO region': 'WHO Region'}, inplace=True) 
```

##### 6. Arrange values of a particular column in ascending order
```python
df.sort_values(by=['Year'], ascending=True).head(5)
```

##### 7. Arrange multiple column values in ascending order
```python
df.sort_values(by=['WHO Region', 'Country', 'Indicator_id', 'Publication Status']).head(3)
```

##### 8. Make country as the first column of the data frame
```python
df[pd.unique(['Country'] + df.columns.values.tolist()).tolist()]
```

##### 9. Get the column array using a variable
```python
array_region = df["WHO Region"].values
```

##### 10. Get the subset rows 11, 24, 37
```python
df.loc[[11, 24, 37], :]
```

##### 11. Get the subset rows excluding 5, 12, 23, 36
```python
bad_df = df.index.isin([5, 12, 23, 56])

df[~bad_df]
```
---

### Read data from other CSV files
```python
users = pd.read_csv('https://raw.githubusercontent.com/ben519/DataWrangling/master/Data/users.csv')
sessions = pd.read_csv('https://raw.githubusercontent.com/ben519/DataWrangling/master/Data/sessions.csv')
products = pd.read_csv('https://raw.githubusercontent.com/ben519/DataWrangling/master/Data/products.csv')
transactions = pd.read_csv('https://raw.githubusercontent.com/ben519/DataWrangling/master/Data/transactions.csv')

```

---

##### 12. Join users to transactions, keeping all rows from transactions and only matching rows from users (left join)
```python
transactions.merge(users, how='left', on='UserID')
```

##### 13. Which transactions have a UserID not in users? ###
```python
transactions[~transactions['UserID'].isin(users['UserID'])]
```

##### 14. Join users to transactions, keeping only rows from transactions and users that match via UserID (inner join) 
```python
transactions.merge(users, how="inner", on='UserID')
```

##### 15. Join users to transactions, displaying all matching rows AND all non-matching rows (full outer join)
```python
transactions.merge(users, how="outer", on='UserID')
```

##### 16. Determine which sessions occurred on the same day each user registered
```python
users.merge(sessions, left_on=['UserID', 'Registered'], right_on=['UserID', 'SessionDate'])
```

##### 17. Build a dataset with every possible (UserID, ProductID) pair (cross join) 
```python
users_1 = users
users_1['key'] = 0

products_1 = products
products_1['key'] = 0

pd.merge(users_1, products_1, on='key', how="outer")[['UserID', 'ProductID']]
```

##### 18. Determine how much quantity of each product was purchased by each user 
```python
users.merge(products, how='outer').merge(transactions, on=['UserID','ProductID'], how="outer").loc[:, ["UserID", "ProductID", "Quantity"]].fillna(0)
```

##### 19. For each user, get each possible pair of pair transactions (TransactionID1,TransacationID2)
```python
pd.merge(transactions, transactions, on='UserID')
```

##### 20. Join each user to his/her first occuring transaction in the transactions table
```python
first_transactions = transactions[transactions['UserID'].isin(users['UserID'])].groupby('UserID').first().reset_index()

data = users.merge(first_transactions, on='UserID', how="outer")
```
---
