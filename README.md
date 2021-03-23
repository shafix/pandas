Lambda function examples:
```
add_two = lambda my_input: my_input + 2
is_substring = lambda my_string: my_string in "This is the master string"
check_if_A_grade = lambda grade: 'Got an A!' if grade >= 90 else 'Did not get an A...'
```

Import pandas:
```
import pandas as pd
```

Data frames:
Inspecting:
```
df.head()
df.info()
```

Read from csv:
```
pd.read_csv('my-csv-file.csv')
```

Crete df from dictionary: (random column order)
```
df1 = pd.DataFrame({
    'name': ['John Smith', 'Jane Doe', 'Joe Schmo'],
    'address': ['123 Main St.', '456 Maple Ave.', '789 Broadway'],
    'age': [34, 28, 51]
})
```

Create df from list of lists: (specified column order)
```
df2 = pd.DataFrame(
[
	['John Smith', '123 Main St.', 34],
	['Jane Doe', '456 Maple Ave.', 28],
	['Joe Schmo', '789 Broadway', 51] 
],
columns=['name', 'address', 'age'])
```

Selecting 1 column:
```
clinic_north = df["clinic_north"]
```

Select multiple columns:
```
clinic_north_south = df[ ['clinic_north','clinic_south'] ]
```

Selecting rows:
```
march = df.iloc[2]
march_may = df.iloc[2:5]
```

Selecting rows with logic:
```
df[df.age == 30]
df[(df.age < 30) | (df.name == 'Martha Jones')]
df[ df.name.isin( ['Martha Jones', 'Rose Tyler', 'Amy Pond'] ) ]
```

Reset indexes: 
```
df.reset_index(drop=True, inplace=True)
```

Add column to the dataframe filled with values (needs to be same length as other columns):
```
df['Quantity'] = [100, 150, 50, 35]
```

Add column to the data frame filled with the same value for all rows:
```
df['In Stock?'] = True
```

Add a column filled based on values in other columns:
```
df['Sales Tax'] = df.Price * 0.075
```

Update column by applying a function to the value for every row:
```
df['Name'] = df.Name.apply(my_1arg_function)
```

Update column based on more than one other column value
```
df['Price with Tax'] = df.apply(lambda row:
     row['Price'] * 1.075
     if row['Is taxed?'] == 'Yes'
     else row['Price'],
     axis=1
)
```

Update column names:
```
df.columns = ['First Name', 'Age']
df.rename(columns={
    'name': 'First Name',
    'age': 'Age'},
    inplace=True)
```

....
