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
# .str.lstrip('https://') removes the ???https://??? from the left side of the string
restaurants['url'] = restaurants['url'].str.lstrip('https://') 
 
# .str.lstrip('www.') removes the ???www.??? from the left side of the string
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
???12456543???	8500		8900
???12283942???	6410		8020
 
pd.melt(frame=df, 
		id_vars=["name"], -- put the columns you want to keep untouched here
		value_vars=["Checking","Savings"], 
		value_name="Amount", 
		var_name="Account Type")
		
df.columns(["Account", "Account Type", "Amount"]) -- explicitly naming the columns afterwards if needed
		
Account 	Type		Amount
???12456543???	???Checking???	8500
???12456543???	???Savings???	8900
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
Example: ???lunges - 30 reps??? becomes "lunges - " | "30" | "reps"
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

Single number statistics:
```
import numpy as np
from scipy import stats

my_arr = [3, 5, -2, 49, 10]

# Central tendency summary statistics
my_arr_median = np.median(my_arr)
my_arr_mode = stats.mode(my_arr)
my_arr_mean = np.mean(my_arr)

print(f'Mean {str(my_arr_mean)}')
print(f'Median {str(my_arr_median)}')
print(f'Mode {str(my_arr_mode)}')

# Spread summary statistics
variance = np.var(my_arr)
standard_deviation = np.std(my_arr)
print(f'Variance {str(variance)}')
print(f'Standard Deviation {str(standard_deviation)}')
```

Replacing missing values with NaN and assign type float
```
df['some_column'] = df['some_column'].replace(['missing'], np.nan)
df['some_column'] = df['some_column'].astype('float')
```

Transforming variables to categorical and add categorical label encoding:
```
movies['rating'] = pd.Categorical(movies['rating'], ['G', 'PG', 'PG-13', 'R','UNRATED', 'NOT RATED'], ordered=True)
print(movies['rating'].unique())
movies['rating_codes'] = movies['rating'].cat.codes
```

One-Hot Encoding / label encoding without order (doesn't need categorical var):
```
titanic = pd.get_dummies(data=titanic, columns=['Embarked'])
```

Calculating data range:
```
min_age = np.amin(exercise_ages) # Answer is 22
max_age = np.amax(exercise_ages) # Answer is 62
age_range = max_age - min_age
```

Histograms, bins:
```
ages = np.array([22, 27, 45, 62, 34, 52, 42, 22, 34, 26, 24, 65, 34, 25, 45, 23, 45, 33, 52, 55])

import numpy as np
np.histogram(ages, range = (20, 70), bins = 5)

from matplotlib import pyplot as plt
plt.hist(exercise_ages, range = (20, 70), bins = 5, edgecolor='black')
plt.title("Decade Frequency")
plt.xlabel("Ages")
plt.ylabel("Count")
plt.show()
```

Quartiles:
```
dataset = [50, 10, 4, -3, 4, -20, 2]

import numpy as np
third_quartile = np.quantile(dataset, 0.75)
quartiles = np.quantile(dataset, [0.25, 0.5, 0.75])
five_equal_parts = np.quantile(dataset, [0.2, 0.4, 0.6, 0.8])

import matplotlib.pyplot as plt
plt.hist(dataset)
for quartile in quartiles:
  plt.axvline(x=quartile, c = 'r')
plt.show()
```

IQR - inter quartile range:
```
iqr = q3 - q1

or 

From scipy.stats import iqr
interquartile_range = iqr(dataset)
```

Boxplots:
```
import matplotlib.pyplot as plt
 
dataset_one = [1, 2, 3, 4, 5]
dataset_two = [3, 4, 5, 6, 7]
plt.boxplot([dataset_one, dataset_two],labels = ["dataset_one", "dataset_two"])
plt.show()
```

Getting median of ordinal categorical data using categorical cat codes:
```
# Check unique statuses
tree_health_statuses = nyc_trees["health"].unique()
print(tree_health_statuses)

# Create order worst -> best
health_categories = ['Poor','Fair','Good']

# Transform to categorical
nyc_trees['health'] = pd.Categorical(nyc_trees['health'], health_categories, ordered=True)

# Get median
nyc_trees_health_median_index = np.median(nyc_trees['health'].cat.codes)
print(nyc_trees_health_median_index) # Output: 2
 
# Find the median category
nyc_trees_health_median_category = health_categories[int(nyc_trees_health_median_index)]
print(nyc_trees_health_median_category) # Output: Good
```

Categorical variable proportions:
```
df['education'].value_counts(dropna = False, normalize = True)
```


Binary variable mean / proportion:
```
np.mean(df['income_>50K']) #output: 0.24 or 24% have income >50k
```

Non binary variable made to binary to calculate frequency and proportion:
```
(df.workclass == 'Local-gov').sum()  #output: 2093
(df.workclass == 'Local-gov').mean() #output: 0.064 or 6.4% work in the local government
```

Get full summary statistics for all columns in a data frame:
```
df.describe(include='all')
```

Barcharts with seaborn:
```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
sns.countplot(flu_results["Results"])
plt.show()

# Giving some ordering
sns.countplot( df["victory_status"], order=df["victory_status"].value_counts(ascending=True).index )
sns.countplot( df["Grade Level"], 	 order=["First Year", "Second Year", "Third Year", "Fourth Year"] )
```

Visibility improvments for barcharts:
```
# rotates the value labels slightly so they don???t overlap, also slightly increases font size
plt.xticks(rotation=30, fontsize=10)
# increases the variable label font size slightly to increase readability
plt.xlabel(column, fontsize=12)
```


Piecharts with matplotlib:
```
import pandas as pd
import matplotlib.pyplot as plt
wedge_sizes = student_grades["Proportions"]
grade_labels = student_grades["Grades"]
plt.pie(wedge_sizes, labels = grade_labels)
plt.axis('equal') # make the pie flat
plt.tight_layout() # adjust spacing
plt.title(???Student Grade Distribution???)
plt.show()
```

Plot multiple columns as barcharts:
```
columns = df.columns.tolist()

for column in columns:
  sns.countplot( df[column] )
  sns.countplot( df[column], order=df[column].value_counts().index)
  plt.xticks(rotation=30, fontsize=10)
  plt.xlabel(column, fontsize=12)
  plt.title(column + ' Value Counts')
  plt.show()
  plt.clf()
```

Data centering:
```
#Data centering involves subtracting the mean of a data set from each data point so that the new mean is 0.
#Centered data is useful because it tells us how far above or below the mean each data point is.
```

Normalizing data (fit data between 0 and 1) and Standartizing data:
```
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
normalized_data = scaler.fit_transform(data)

scaler = StandardScaler()
standardized_data = scaler.fit_transform(data)
```

Binning Numerical Data
```
student_ages = dance_class['Age']
print(student_ages.min()) # 23
print(student_ages.max()) # 65
bins = [20, 30, 40, 50, 60, 70]
dance_class['binned_age'] = pd.cut(dance_class['Age'], bins)

  binned_age  Age
0   (20, 30]   23
1   (20, 30]   28
2   (40, 50]   45
3   (60, 70]   63
4   (30, 40]   35

# -----------------------------------------------------------
# Plot the bar graph of binned ages
dance_class['binned_age'].value_counts().plot(kind='bar')
 
# Label the bar graph 
plt.title('Dance Class Age Distribution')
plt.xlabel('Ages')
plt.ylabel('Count') 
 
# Show the bar graph 
plt.show()
# -----------------------------------------------------------
# If we want to label the bins...
# Store the labels for our bins
age_labels = ['Young Adult', 'Adult', 'Middle Aged', 'Middle-Older Age', 'Senior']
 
# Bin the values of the 'Age' column and specify the labels 
dance_class['binned_age'] = pd.cut(dance_class['Age'], bins, labels = age_labels)
# -----------------------------------------------------------
```

Combining Categories
```
votes = election_data['Vote'].value_counts()
print(votes)

Liliana    1067
John        998
William     494
Emilie      196
Pattie        6
Neil          3
Bob           2
Demi          1
David         1
Hester        1

mask = election_data.isin( votes[votes < 200].index )
election_data[mask] = 'Other'
print(election_data['Vote'].value_counts())

Liliana    1067
John        998
William     494
Other       210
```

Log transformation:
```
data = [1, 1, 1, 1, 2, 2, 2, 3, 3, 4, 4]

import numpy as np
log_data = np.log(data)
print(log_data)
# 0, 0, 0, 0, 0.693, 0.693, 0.693, 1.098, 1.098, 1.386, 1.386

or..

from sklearn.preprocessing import PowerTransformer
log_transform = PowerTransformer()
log_transform.fit_transform(data)
```

Sort dataframe by certain column:
sorted_data = dataframe_name.sort_values(by=['Column Name'])


Relationship between categorical and quantitative variables:
```
scores_GP = students["score"][students.school == 'GP'] # Scores of school 1
scores_MS = students["score"][students.school == 'MS'] # Scores of school 2

# Check mean difference
mean_GP = np.mean(scores_GP)
mean_MS = np.mean(scores_MS)
print(mean_GP) #output: 10.49
print(mean_MS) #output: 9.85
print(mean_GP - mean_MS) #Output: 0.64

# Check median difference
median_GP = np.median(scores_GP)
median_MS = np.median(scores_MS)
print(median_GP) #Output: 11.0
print(median_MS) #Output: 10.0
print(median_GP-median_MS) #Output: 1.0

# Comparing boxplots
sns.boxplot(data = students, x = 'school', y = 'score')
plt.show()
plt.clf()

# Compare histograms
plt.hist(scores_GP , color="blue", label="GP", normed=True, alpha=0.5) # might be "density" instead of "normed"
plt.hist(scores_MS , color="red", label="MS", normed=True, alpha=0.5) # might be "density" instead of "normed"
plt.legend()
plt.show()
plt.clf()
```


Relationship between two quantitative variables:
```
# Scatterplot:
plt.scatter(x = housing.price, y = housing.sqfeet)
plt.xlabel('Rental Price (USD)')
plt.ylabel('Area (Square Feet)')
plt.show()

# Covariance (linear)
cov_mat_price_sqfeet = np.cov(housing.price, housing.sqfeet)
print(cov_mat_price_sqfeet) #output: [ [184332.9  57336.2] [ 57336.2 122045.2] ]

# Pearson Correlation (linear)
from scipy.stats import pearsonr
corr_price_sqfeet, p = pearsonr(housing.price, housing.sqfeet)
print(corr_price_sqfeet) #output: 0.507

```

Relationship between two categorical variables:
```
# Frequenceies
inluence_leader_freq = pd.crosstab(npi.influence, npi.leader) #do you influence people yes/no + are you a good leader yes/no
print(inluence_leader_freq)

# Proportions
influence_leader_prop = influence_leader_freq/len(npi)
print(influence_leader_prop)

# Marginal proportions
leader_marginals = influence_leader_prop.sum(axis=0)
print(leader_marginals)
influence_marginals =  influence_leader_prop.sum(axis=1)
print(influence_marginals)

# Expected Contingency Table
from scipy.stats import chi2_contingency
chi2, pval, dof, expected = chi2_contingency(influence_leader_freq)
print(np.round(expected))

# The Chi-Square Statistic (how strongly related are they?)
from scipy.stats import chi2_contingency
chi2, pval, dof, expected = chi2_contingency(influence_leader_freq)
print(chi2) # output: 1307.88 ( a Chi-Square statistic larger than around 4 would strongly suggest an association between the variables )

```

