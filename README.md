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

Get single column statistics:
```
df.column_name.command()
```

Command	Descriptions
```
mean	Average of all values in column
std		Standard deviation
median	Median
max		Maximum value in column
min		Minimum value in column
count	Number of values in column
nunique	Number of unique values in column
unique	List of unique values in column
```

Grouping for statistics (generates series):
```
df.groupby('column_name').column_name.command()
```

Grouping for statistics (generates series):
```
df.groupby('group_column').target_column.command().reset_index()
df.groupby(['group_column1','group_column2'])['target_column'].command().reset_index()
```

Rename the newly generated column in case of df:
```
df.rename(columns={ 'current_column_name': 'new_column_name', }, inplace=True)
```

Use lambda function to perform percentile calculations:
```
high_earners = df.groupby('category')
	.wage
    .apply(lambda x: np.percentile(x, 75))
    .reset_index()
```

Pivot table:
```
df.pivot(columns='ColumnToPivot',
         index='ColumnToBeRows',
         values='ColumnToBeValues').reset_index()
```

Fill new column with true/false depending on whether another column is filled (~ means NOT):
```
ad_clicks['ad_click_timestamp'] = ~ad_clicks.ad_click_timestamp.isnull() 
```

Inner join two tables via implicit same column name (pd function):
```
sales_vs_targets = pd.merge(sales, targets)
```

Inner join two (or more) tables via implicit same column name (df in-built function):
```
big_df = orders.merge(customers).merge(products)
```

Inner join two tables via implicit same column name (pd function) by using renaming:
```
new_df = pd.merge( orders, customers.rename(columns={'id': 'customer_id'})) -- Use renaming to continue using implicit merge column
```

Inner join two tables via explicit column name (pd function):
```
new_df = pd.merge(
    orders,
    customers,
    left_on='customer_id',
    right_on='id',
    suffixes=['_order', '_customer']
)
```

Outter joins of two tables with implicit colum name (pd function):
```
pd.merge(company_a, company_b, how='outer')
pd.merge(company_a, company_b, how='left')
pd.merge(company_a, company_b, how='right')
```

Concatenate (UNION) several frames with the same columns:
```
pd.concat([df1, df2])
```

Do something with all columns using map:
```
restaurants.columns = map(str.lower, restaurants.columns) -- puts all column names to LOWER
```

Count the number of unique values in each column:
```
restaurants.nunique() 
```

Count the number of missing values in each column:
```
restaurants.isna().sum()
```

Replace some values using a where clause:
```
# here our .where() function replaces latitude values greater than 40 with NaN values
restaurants['latitude'] = restaurants['latitude'].where(restaurants['latitude'] > 40, np.nan) 
```

Characterizing missingness with crosstab:
```
pd.crosstab(
 
        # tabulates the boroughs as the index
        restaurants['boro'],  
 
        # tabulates the number of missing values in the url column as columns
        restaurants['url'].isna(), 
 
        # names the rows
        rownames = ['boro'],
 
        # names the columns 
        colnames = ['url is na'])
```

Removing some text prefixes (for example url) using lstrip (left - strip):
```
# .str.lstrip('https://') removes the “https://” from the left side of the string
restaurants['url'] = restaurants['url'].str.lstrip('https://') 
 
# .str.lstrip('www.') removes the “www.” from the left side of the string
restaurants['url'] = restaurants['url'].str.lstrip('www.') 
 
# the .head(10) function will show us the first 10 rows in our dataset
print(restaurants.head(10))
```

Import multiplefiles with the same structure and merge them all into one data frame:
```
import pandas as pd
import glob

student_files = glob.glob("exams*.csv")
 
df_list = []
for filename in student_files:
  data = pd.read_csv(filename)
  df_list.append(data)
 
df = pd.concat(df_list)
```

Using "melt" to transform columns into rows:
```
Account		Checking	Savings
“12456543”	8500		8900
“12283942”	6410		8020
 
pd.melt(frame=df, 
		id_vars=["name"], -- put the columns you want to keep untouched here
		value_vars=["Checking","Savings"], 
		value_name="Amount", 
		var_name="Account Type")
		
df.columns(["Account", "Account Type", "Amount"]) -- explicitly naming the columns afterwards if needed
		
Account 	Type		Amount
“12456543”	“Checking”	8500
“12456543”	“Savings”	8900
```

Dealing with duplicate rows:
```
df.duplicated() -- shows true if another row was encountered along the way to this row that has the exact same values in all columns
df.drop_duplicates() -- drops pure duplicates (keeps first occurance)
df.drop_duplicates(subset=['item']) -- drops duplicates based on a specific column only (keeps first occurance)

duplicate_students = students.duplicated() -- get true/false series
print(duplicate_students.value_counts()) -- check how many are duplicated
students = students[~duplicate_students] -- drop the duplicates manually
```

Splitting column value into several columns (primitive):
```
# Example : "M14" (gender_age)
students["gender"] = students.gender_age.str[0:1]
students["age"] = students.gender_age.str[1:]
```

Splitting column value into several columns (with delimiter):
```
# Example : "admin_US" and "user_Kenya" (type)

df['str_split'] = df.type.str.split('_') # Create the 'str_split' column
df['usertype'] = df.str_split.str.get(0) # Create the 'usertype' column
df['country'] = df.str_split.str.get(1)  # Create the 'country' column

# or..

str_split = df["type"].str.split('_') 	 # Create the 'str_split' series
df['usertype'] = str_split.str.get(0) 	 # Create the 'usertype' column
df['country'] = str_split.str.get(1)  	 # Create the 'country' column
```

Get series object of data types of each column:
```
print(df.dtypes)
```

Replace a part of a string using regexp and convert data type to numeric:
```
fruit.price = fruit.price.replace('[\$]', '', regex=True)
fruit.price = pd.to_numeric(fruit.price)
# or
fruit.price = pd.to_numeric( fruit.price.replace('[\$]', '', regex=True) )
```

Split string on a certain part using regex (results in a data frame):
```
Example: “lunges - 30 reps” becomes "lunges - " | "30" | "reps"
split_df = df['exerciseDescription'].str.split('(\d+)', expand=True) -- splits on a number

df.reps = pd.to_numeric(split_df[1]) -- add column "reps" to the original df
df.exercise = split_df[2].replace('[\- ]', '', regex=True) -- add column "excercise" to the original df after cleaning it up a bit with regexp replace
```

Dealing with NaN values in a data frame:
```
df = df["column_name"].isnull() -- creates a series of true/false depending on whether a certain column value is NaN
print(df.value_counts()) -- check how many are true(NaN) vs false
df = df.dropna() -- drops all rows that have NaN in any column
df = df.dropna(subset=["column_name"]) -- drops all rows that have NaN in a certain column
df = df.fillna( value={ "column_name":df.column_name.mean() } ) -- fills the column with the mean of that column
df = df.fillna( value={ "column_name": 0 } ) -- fills the column with 0
df = df.fillna( 0 ) -- fills NaN with 0 in all columns
```


