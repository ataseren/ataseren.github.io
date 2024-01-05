---
title: "LeetCode: Introduction to Pandas Study Plan"
tag: software-development

---

This [study plan](https://leetcode.com/studyplan/introduction-to-pandas/) in LeetCode aims to teach basics of Pandas with 15 simple questions.

I’m writing this note to remember the functions of Pandas and their abilities. In this note, I will simply name the questions, type my answers to the problems and explain the methods I used in the solutions. 

I’m not going to give the details of the problems since they are available in a better format on LeetCode.

### **Create a DataFrame from List**
```python
df = pd.DataFrame(student_data)
df.columns = ["student_id", "age"]
return df
```

Creates a dataframe with specific column names. `df.columns` is used to name the columns of a dataframe.<br><br>


### **Get the Size of a DataFrame**
    
```python
return [players.shape[0], players.shape[1]]
```

`df.shape` returns a tuple of rows and columns of df: `(row_count, column_count)`<br><br>
    
### **Display the First Three Rows**
    
```python
return employees.head(3)
```

`df.head(n)` returns first n rows of df.<br><br>
    
### **Select Data**
    
```python
return students.loc[students['student_id'] == 101, ['name', 'age']]
```

In `df.loc`, first parameter is the condition used for the search, second parameters is a list with desired columns.

Here is an example with multiple conditions:

```python
students.loc[(students['student_id'] == 101) & (students['name'] == "Ulysses"), ['name', 'age']]
```
<br>
    
### **Create a New Column**
    
```python
bonus = []
    for s in employees["salary"]:
        bonus.append(s*2)

    result = employees.assign(bonus=bonus)
    return result
```

In `df.assign`, there is a column name and a list of values to be used in the column:

```python
df.assign(column_name=[element1, element2, element3])
```
<br>

### **Drop Duplicate Rows**

```python
df = customers.drop_duplicates(subset=['email'])
    return df
```

`df.drop_duplicates` simply drop duplicates according to the values given in a column or columns.

```python
dedup_df = df.drop_duplicates(subset=['A', 'B'])
```
<br>

### **Drop Missing Data**
    
```python
return students.dropna(subset=['name'])
```

`df.dropna` drops the rows with missing values. In this question, a column name is given to drop the rows with missing values if they are only in the given column. `df.dropna` can get various parameters to handle missing values in different ways.<br><br>
    
### **Modify Columns**

```python
employees.salary = employees.salary*2
    return employees
```

In this question, it is asked to double the values of a column and I directly accessed the column with `df.row_name` and doubled its values.

Here is an additional example:

```python
import numpyas np

# Step 1: Select the column
age_column= df['age']

# Step 2: Apply a function to each value
def sqrt(x):
    return np.sqrt(x)

new_age_column= age_column.apply(sqrt)

# Step 3: Assign the new values back to the column
df['age']= new_age_column
```

`df.apply` is used to apply a function to each value in the column.<br><br>

### **Rename Columns**

```python
return students.rename(columns = {'id':'student_id', 
                                        'first':'first_name', 
                                        'last':'last_name',
                                        'age':'age_in_years'})
```

`df.rename` can be used to change names of index or columns like this case. With `inplace=True` parameter, df can be modified instead of creating a new one.<br><br>

### **Change Data Type**

```python
return students.astype({'grade': int})
```

`df.astype` is used to change the data type of an object in the dataframe. It can be used for specific or all columns. To solve this question, different approaches can be used such as `df.apply` to all elements in a column or `df.to_numeric` to convert non-numeric objects into numeric ones if possible.<br><br>

### **Fill Missing Data**

```python
products['quantity'] = products['quantity'].fillna(0)
return products
```

In this case, it is asked to fill missing data in a single column. That’s why I operated on “quantity” column. `df.fillna(x)` can be used to replace all missing values with given parameter `x`. 

To achieve the same result, `df.replace` could be used too:

```python
df['DataFrame Column'] = df['DataFrame Column'].replace(np.nan, 0)
```
<br>

### **Reshape Data: Concatenate**

```python
return pd.concat([df1,df2], axis = 0)
```

`pd.concat` can be used to concatenate 2 dataframes horizontally (same rows, new columns) or vertically (same columns, new rows). `axis = 0` is for vertical and `axis = 1` is for horizontal concatenation. Other than this, `pd.merge`, `df.append` and `df.join` can be used for concatenation.

```python
#Concatenation with pd.merge
result = pd.merge(df, df1, on='Courses', how='outer', suffixes=('_df1', '_df2')).fillna(0)

result['Fee'] = result['Fee_df1'] + result['Fee_df2']
result = result[['Courses', 'Fee']]

#Concatenation with df.join
result = df.join(df1)

#Concatenation with df.append (only vertical concatenation)
result = df.append(df1, ignore_index=True
```
<br>

### **Reshape Data: Pivot**

```python
return weather.pivot(index = 'month', columns = 'city', values = 'temperature')
```

`df.pivot` is used to pivot a dataframe with 3 columns. This function is used to reshape to a simpler, smaller dataframe that the same meaning can be deduced from it. With this function, the index and columns of a dataframe can be set and the new dataframe can be filled with desired values.<br><br>

### **Reshape Data: Melt**

```python
return pd.melt(report, id_vars=['product'], 
                value_vars=['quarter_1', 'quarter_2', 'quarter_3', 'quarter_4'],
                var_name='quarter', value_name='sales')
```

`pd.melt` reshapes a dataframe to be more computer friendly. In this problem, pd.melt is used to merge values of multiple columns to a single column. The names of the columns are also used as variable names for the values.<br><br>

### **Method Chaining**

```python
return animals[animals['weight'] > 100].sort_values(['weight'], ascending = False,)[['name']]
```

**Method chaining** is a newer approach to data manipulation by allowing for the execution of multiple operations in a single line of code. With method chaining, each operation is chained together using the dot notation.