---
title: "LeetCode: 30 Days of Pandas Study Plan"
tag: software-development

---
This [study plan](https://leetcode.com/studyplan/30-days-of-pandas/) on LeetCode covers the essential topics that are often asked in Pandas interviews. It consists of 32 questions. Therefore, you can schedule the questions for every day of a month. I used Pandas few times, mostly for machine learning projects. Now, I want to delve into more features of Pandas and improve my knowledge on another computer science topic.

I only solved 28 of these problems because other 4 questions were available to only LeetCode Premium subscribers. If I subscribe to it one day, I’ll add those questions, too.


## Big Countries

A country is **big** if:

- it has an area of at least three million (i.e., 3000000 km<sup>2</sup>), or
- it has a population of at least twenty-five million (i.e., 25000000).

Write a solution to find the name, population, and area of the **big countries**.

Return the result table in **any order**.

```python
def big_countries(world: pd.DataFrame) -> pd.DataFrame:
    return world.loc[(world['area'] >= 3000000) | (world['population'] >= 25000000), ['name', 'population', 'area']]
```

In this question, df.loc function is used to locate the desired entries. The most important part is the conditional part used in the function. `df.loc()` function is very useful to apply conditions to a search process among the entries.

## Recyclable and Low Fat Products

Write a solution to find the IDs of products that are both low fat and recyclable.

Return the result table in **any order**.

```python
def find_products(products: pd.DataFrame) -> pd.DataFrame:
    return products.loc[(products['low_fats'] == 'Y') & (products['recyclable'] == 'Y'), ['product_id']]
```

Like the previous question, I used conditions in `df.loc()` function.

## **Customers Who Never Order**

Write a solution to find all customers who never order anything.

Return the result table in **any order**.

```python
def find_customers(customers: pd.DataFrame, orders: pd.DataFrame) -> pd.DataFrame:
    result = customers[~customers['id'].isin(orders['customerId'])]
    return result[['name']].rename(columns={'name': 'Customers'})
```

This time, we have a new function caller `df.isin()` and a character tilde “~”.  `df.isin()` function is used to determine whether each element in the DataFrame is contained in parameters of the function. Tilde character is used to get the complement of the values. In this question, tilde is used to get the complement of values of customers that placed an order.

## Article Views I

Write a solution to find all the authors that viewed at least one of their own articles.

Return the result table sorted by `id` in ascending order.

```python
def article_views(views: pd.DataFrame) -> pd.DataFrame:
    result = views.loc[views['author_id'] == views['viewer_id'], ['author_id']].sort_values(['author_id'], ascending = True).drop_duplicates().rename(columns={'author_id': 'id'})
    return result[["id"]]
```

In this question, other than the simple and usual functions, I used `df.sort_values()` to meet the order requirements of the question. You can also see that I used the chaining method I mentioned in my previous article to make it seem better and save some storage.

## Invalid Tweets

Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is **strictly greater** than `15`.

Return the result table in **any order**.

```python
def invalid_tweets(tweets: pd.DataFrame) -> pd.DataFrame:
    return tweets.loc[tweets['content'].str.len() > 15, ['tweet_id']][['tweet_id']]
```

Different than the other similar questions, I used `Series.str.len()`. This function computes the length of each element in the Series/Index. I used it to meet the question’s requirements.

## **Calculate Special Bonus**

Write a solution to calculate the bonus of each employee. The bonus of an employee is `100%` of their salary if the ID of the employee is **an odd number** and **the employee's name does not start with the character** `'M'`. The bonus of an employee is `0` otherwise.

Return the result table ordered by `employee_id`.

```python
def calculate_special_bonus(employees: pd.DataFrame) -> pd.DataFrame:
    employees['bonus'] = 0

    employees.loc[(employees['employee_id'] % 2 != 0) & (~employees['name'].str.lower().str.startswith('m')), 'bonus'] = employees['salary']

    return employees[['employee_id','bonus']].sort_values('employee_id')
```

First of all, I set the all bonuses to 0 to change the bonuses that are required to be changed and keep the rest of it with the same value.

In this question, I used a converter function. `Series.str.lower()`function makes a string lowercase. I added this function to the chain because one of the test cases had a character ‘m’ instead of ‘M’. Therefore, I wanted to check for both lowercase and uppercase ‘m’. With `Series.str.startswith(char)`, I checked the first character of the employee names and calculated bonuses according to that.

## **Fix Names in a Table**

Write a solution to fix the names so that only the first character is uppercase and the rest are lowercase.

Return the result table ordered by `user_id`.

```python
def fix_names(users: pd.DataFrame) -> pd.DataFrame:
    users.name = users.name.str.lower().str.capitalize()

    return users[['user_id', 'name']].sort_values('user_id')
```

This is a simple one. First, I converted all words to lowercase with `Series.str.lower()` and capitalized the first letter with `Series.str.capitalize()`.

## Find Users With Valid E-Mails

Write a solution to find the users who have **valid emails**.

A valid e-mail has a prefix name and a domain where:

- **The prefix name** is a string that may contain letters (upper or lower case), digits, underscore `'_'`, period `'.'`, and/or dash `'-'`. The prefix name **must** start with a letter.
- **The domain** is `'@leetcode.com'`.

Return the result table in **any order**.

```python
def valid_emails(users: pd.DataFrame) -> pd.DataFrame:
    return users.loc[users['mail'].str.contains(r'^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\.com$')]
```

To be honest, I got some help from ChatGPT 😃. From the question, I realized that I need to check values whether they fit into a pattern. Regular expressions (regex) are the best way to do it efficiently. To do the comparison, I looked for a function and found that `Series.str.contains()` accepts regex, along with the other types of parameters. After that, I asked ChatGPT to generate a regex to meet the valid email requirements and used it in this function.

## Patients With a Condition

Write a solution to find the patient_id, patient_name, and conditions of the patients who have Type I Diabetes. Type I Diabetes always starts with `DIAB1` prefix.

Return the result table in **any order**.

```python
def find_patients(patients: pd.DataFrame) -> pd.DataFrame:
    return patients.loc[patients['conditions'].str.contains(r'(^|\s)DIAB1')]
```

Again, there is a question with regex. But I realized that I need to use regex after my first submission. I thought that I just needed to find a single word. Therefore, I simply used `"DIAB1"` with `Series.str.contains()`. However, in one of the test cases, there is a word “SADIAB1” that returns true for the function but not the word that question asks for. Therefore, I converted it to regex and added “(^|\s)” part which means that this word is at the beginning or there is a space before that.

## **Nth Highest Salary**

Write a solution to find the `nth` highest salary from the `Employee` table. If there is no `nth` highest salary, return `null`.

```python
def nth_highest_salary(employee: pd.DataFrame, N: int) -> pd.DataFrame:
    try:
        if N < 1:
            raise ValueError("Manually raising a ValueError")
        salary = employee.drop_duplicates(subset=['salary']).sort_values(by=['salary'], ascending = False).reset_index(drop=True).iloc[N-1]["salary"].astype(int)
        return pd.DataFrame([salary], columns=[f'getNthHighestSalary({N})'])
    except:
       return pd.DataFrame([np.nan], columns=[f'getNthHighestSalary({N})'])
```

In this question, I used  a try-except block for some test cases. There are 2 ways that the question’s test cases can cause an exception: There are entries or high salaries in data frame less than the n value or n value is smaller than 1. I used the try-except block and an if statement to eliminate these possibilities and also used NumPy to enter null value in the returned data frame to match the test cases.

## **Second Highest Salary**

Write a solution to find the second highest salary from the `Employee` table. If there is no second highest salary, return `null (return None in Pandas)`.

```python
def second_highest_salary(employee: pd.DataFrame) -> pd.DataFrame:
    try:
        salary = employee.drop_duplicates(subset=['salary']).sort_values(by=['salary'], ascending = False).reset_index(drop=True).iloc[1]["salary"].astype(int)
        return pd.DataFrame([salary], columns=['SecondHighestSalary'])
    except:
       return pd.DataFrame([np.nan], columns=['SecondHighestSalary'])
```

This question is similar to previous one. Instead of arbitrary one, n value is 2 in all cases. Therefore I just need to use the same logic with try-except in case of there are no 2nd highest salary in data frame.

## Department Highest Salary

Write a solution to find employees who have the highest salary in each of the departments. Return the result table in any order.

```python
def department_highest_salary(employee: pd.DataFrame, department: pd.DataFrame) -> pd.DataFrame:
    max_salary_by_department = employee.groupby('departmentId')['salary'].max()

    entries_with_max_salary = employee[employee.apply(lambda x: x['salary'] == max_salary_by_department[x['departmentId']], axis=1)]

    df_merged = pd.merge(entries_with_max_salary, department, left_on='departmentId', right_on='id', how='left')

    columns_order = ['name_y', 'name_x', 'salary']

    return df_merged[columns_order].rename(columns={'name_y': 'Department', 'name_x': 'Employee', 'salary': 'Salary'})
```

In this question, I used a lambda function which is an anonymous function that we can pass in instantly without defining a name or anything like a full traditional function. First, by using `df.groupby()` and `max()` function, I gathered the values of highest salaries of each department. Then, by using a lambda function, I matched and merged the salaries, the employees who get that salary and the departments of those employees. I returned the answer from the result of this merge.

## Rank Scores

Write a solution to find the rank of the scores. The ranking should be calculated according to the following rules:

- The scores should be ranked from the highest to the lowest.
- If there is a tie between two scores, both should have the same ranking.
- After a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no holes between ranks.

Return the result table ordered by `score` in descending order.

```python
def order_scores(scores: pd.DataFrame) -> pd.DataFrame:
    scores['rank'] = scores['score'].rank(method = 'dense', ascending = False)
    return scores.sort_values(by = 'score', ascending = False)[['score', 'rank']]
```

In this question, I used the `rank()` function to assign ranks to the scores. I used ‘dense’ parameter in the `rank()` function.  ‘Dense’ is like ‘min’, but rank always increases by 1 between groups.

By setting the method parameter to `'dense'` and sorting the DataFrame by score in descending order, I achieved the desired ranking.

## Delete Duplicate Emails

Write a solution to **delete** all duplicate emails, keeping only one unique email with the smallest `id`.

For SQL users, please note that you are supposed to write a `DELETE` statement and not a `SELECT` one.

For Pandas users, please note that you are supposed to modify `Person` in place.

After running your script, the answer shown is the `Person` table. The driver will first compile and run your piece of code and then show the `Person` table. The final order of the `Person` table **does not matter**.

```python
def delete_duplicate_emails(person: pd.DataFrame) -> None:
    person.sort_values(by=['id'], inplace=True)
    person.drop_duplicates(subset=['email'], inplace=True)
```

In this question, I sorted the DataFrame by ID to ensure that for duplicate emails, the one with the smallest ID remains. Then, I dropped duplicate emails with `df.drop_duplicates()`. It keeps only the first occurence, which is the one with the smallest ID. `‘inplace=True’` parameter makes it modify the DataFrame in place.

## **Rearrange Products Table**

Write a solution to rearrange the `Products` table so that each row has `(product_id, store, price)`. If a product is not available in a store, do **not** include a row with that  `product_id`  and `store` combination in the result table.

Return the result table in **any order**.

```python
def rearrange_products_table(products: pd.DataFrame) -> pd.DataFrame:
    return pd.melt(products, id_vars=['product_id'], value_vars=['store1', 'store2', 'store3'], var_name='store', value_name='price').dropna()
```

This question just requires a simple “melting” process which I mentioned in my previous article. 

I used `pd.melt()` function to reshape the table. This function stacks the 'store1', 'store2', and 'store3' columns into a single 'store' column while keeping 'product_id' as an identifier. After reshaping, I dropped any rows with missing prices using `df.dropna()`, ensuring that only products available in stores are included in the result.

## **Count Salary Categories**

Write a solution to calculate the number of bank accounts for each salary category. The salary categories are:

- `"Low Salary"`: All the salaries **strictly less** than `$20000`.
- `"Average Salary"`: All the salaries in the **inclusive** range `[$20000, $50000]`.
- `"High Salary"`: All the salaries **strictly greater** than `$50000`.

The result table **must** contain all three categories. If there are no accounts in a category, return `0`.

Return the result table in **any order**.

```python
def count_salary_categories(accounts: pd.DataFrame) -> pd.DataFrame:
    accounts['category'] = 'Low Salary'
    accounts.loc[(accounts['income'] >= 20000) & (accounts['income'] <= 50000), 'category'] = 'Average Salary'
    accounts.loc[accounts['income'] > 50000, 'category'] = 'High Salary'
    accounts = accounts.groupby(by=['category']).size().reset_index(name='accounts_count')
    res = pd.DataFrame({'category':['Low Salary', 'Average Salary', 'High Salary']})
    res = res.merge(accounts, how='left', on='category')
    res.loc[res['accounts_count'].isnull(), 'accounts_count'] = 0
    return res
```

In this question, I separated the entries into different salary categories. First, I created a column in DataFrame called ‘category’ and added ‘Low Salary’ value for all entries. Then, by using conditions, I changed the ‘category’ values according to the ‘income’ value. Finally, to meet the desired result, I grouped the entries and reset the index under ‘accounts_count’ name.

To return, I created a DataFrame in desired format and merged it with ‘accounts’ DataFrame. After adding 0 value to empty categories, I returned the DataFrame.

If you are looking for a one line solution:

```python
def count_salary_categories(accounts: pd.DataFrame) -> pd.DataFrame:
    return pd.DataFrame({'category':['Low Salary','Average Salary','High Salary'], 'accounts_count':[len(accounts[accounts['income'] < 20000]),
     len(accounts[(accounts['income'] >= 20000) & (accounts['income'] <= 50000)]), len(accounts[accounts['income'] > 50000])]})

```

## **Find Total Time Spent by Each Employee**

Write a solution to calculate the total time **in minutes** spent by each employee on each day at the office. Note that within one day, an employee can enter and leave more than once. The time spent in the office for a single entry is `out_time - in_time`.

Return the result table in **any order**.

```python
def total_time(employees: pd.DataFrame) -> pd.DataFrame:

    employees['total_time'] = employees['out_time'] - employees['in_time']

    employees = employees.groupby(['event_day', 'emp_id'])['total_time'].sum().reset_index(name = 'total_time')
    return employees.rename(columns = {'event_day' : 'day'})
```

First, I calculated the time spent by each employee for each entry by subtracting the 'in_time' from the 'out_time' and saved these values under ‘total_time’. Then, I grouped the DataFrame by 'event_day' and 'emp_id' and got the sum of the total time spent by each employee on each day using `groupby()`** and `sum()`. Finally, I reset the index and renamed the columns to meet desired format.

## **Game Play Analysis I**

Write a solution to find the **first login date** for each player.

Return the result table in **any order**.

```python
def game_analysis(activity: pd.DataFrame) -> pd.DataFrame:

    activity = activity.groupby('player_id')['event_date'].min().reset_index()

    return activity.rename(columns={'event_date':'first_login'})
```

This question was very simple. Instead of integers, I grouped entries according to the minimum value of dates by using min(). With a small renaming, I returned the desired DataFrame.

## **Number of Unique Subjects Taught by Each Teacher**

Write a solution to calculate the number of unique subjects each teacher teaches in the university.

Return the result table in **any order**.

```python
def count_unique_subjects(teacher: pd.DataFrame) -> pd.DataFrame:
    teacher.drop_duplicates(subset=["teacher_id", "subject_id"], inplace=True)
    teacher = teacher.groupby(by=["teacher_id"])[['subject_id']].count().reset_index()
    return teacher.rename(columns={"subject_id":"cnt"})
```

First of all, I dropped the duplicates according to the ‘teacher_id’ and ‘subject_id’ values to get rid of the effect of ‘dept_id’ value because with this value and duplicates are gone, I can group the entries and count them according to different teachers and number of subjects they teach. After this, I returned desired DataFrame with a small renaming.

## **Classes More Than 5 Students**

Write a solution to find all the classes that have **at least five students**.

Return the result table in **any order**.

```python
def find_classes(courses: pd.DataFrame) -> pd.DataFrame:
    courses = courses.groupby(by=["class"], as_index=False)[["student"]].count()
    courses = courses[courses["student"] >= 5]
    return courses.drop(columns=["student"])
```

First off all, I grouped the entries according to the classes to get the count of students in them. Then, I selected entries where number of students in the class is equal to or more than 5. After this step, I simply dropped the ‘student’ column and found the classes that have at least 5 students.

## **Customer Placing the Largest Number of Orders**

Write a solution to find the `customer_number` for the customer who has placed **the largest number of orders**.

The test cases are generated so that **exactly one customer** will have placed more orders than any other customer.

```python
def largest_orders(orders: pd.DataFrame) -> pd.DataFrame:
    result = orders.groupby(by=["customer_number"], as_index=False)[["order_number"]].count()
    result = result.sort_values(by=["order_number"],ascending=False).reset_index(drop=True)

    return result.drop(columns=["order_number"]).head(1)
```

First, I grouped the entries by 'customer_number' and counted the number of orders for each customer using `groupby()` and `count()`.

Then, I sorted the result in descending order based on the count of orders to identify the customer with the largest number of orders. I used ‘ascending=False’ parameter to sort in descending order. After this, I reset the index, drop the ‘order_number’ column since it is not asked and got the first entry of the DataFrame by using head(1) function.

At the end of question, there is a follow up:  What if more than one customer has the largest number of orders, can you find all the `customer_number` in this case?

In such case, I would follow the same procedure and get the first entry with head(1) to find the largest number of orders. Then I would use that value to return customer numbers with that number of orders.

## **Group Sold Products By The Date**

Write a solution to find for each date the number of different products sold and their names.

The sold products names for each date should be sorted lexicographically.

Return the result table ordered by `sell_date`.

```python
def categorize_products(activities: pd.DataFrame) -> pd.DataFrame:
    activities = activities.groupby(['sell_date'],as_index=False)
    activities = activities.agg({'product':[lambda x: x.nunique(), lambda x: ','.join(sorted(x.unique()))]})
    activities.columns = ['sell_date','num_sold','products']
    return activities.sort_values('sell_date')
```

In this question, I used 2 lambda functions to apply specific functions to all entries. First, I grouped the activities DataFrame by 'sell_date' using `groupby()` and `agg()` to apply multiple aggregation functions. In the aggregation, I calculated the number of unique products sold with `lambda x: x.nunique()` and concatenated the names of the unique products sorted lexicographically with `lambda x: ','.join(sorted(x.unique())).` nunique() counts the number of distinct elements in specified axis and unique() returns unique values.

After a renaming and sorting by ‘sell_date’ value, I returned the desired DataFrame. 

## **Daily Leads and Partners**

For each `date_id` and `make_name`, find the number of **distinct** `lead_id`'s and **distinct**  `partner_id`'s.

Return the result table in **any order**.

```python
def daily_leads_and_partners(daily_sales: pd.DataFrame) -> pd.DataFrame:
    daily_sales = daily_sales.groupby(by=["date_id", "make_name"]).nunique().reset_index()
    return daily_sales.rename(columns={"lead_id":"unique_leads", "partner_id":"unique_partners"})
```

First, I grouped the **`daily_sales`** DataFrame by 'date_id' and 'make_name' using **`groupby()`** to aggregate the counts of distinct values. In the aggregation, I used **`nunique()`** to calculate the number of distinct **`lead_id`'**s and **`partner_id`'**s for each group. Finally, I reset the index and renamed the columns to 'unique_leads' and 'unique_partners' to reflect the counts correctly.

## **Actors and Directors Who Cooperated At Least Three Times**

Write a solution to find all the pairs `(actor_id, director_id)` where the actor has cooperated with the director at least three times.

Return the result table in **any order**.

```python
def actors_and_directors(actor_director: pd.DataFrame) -> pd.DataFrame:
    actor_director = actor_director.groupby(by=["actor_id","director_id"]).count().reset_index()
    actor_director = actor_director[actor_director["timestamp"] >= 3]

    return actor_director[["actor_id", "director_id"] ]
```

In this question, I used groupby() for 2 columns at the same time since the count of same and different pairs are asked. After grouping, I filtered the entries that have count smaller than 3 and returned the DataFrame.

## **Replace Employee ID With The Unique Identifier**

Write a solution to show the **unique ID** of each user, If a user does not have a unique ID replace just show `null`.

Return the result table in **any** order.

```python
def replace_employee_id(employees: pd.DataFrame, employee_uni: pd.DataFrame) -> pd.DataFrame:
    result = pd.merge(employees, employee_uni, how="outer")
    return result[["unique_id","name"]].dropna(subset=["name"])
```

First of all, I merged ‘employees’ and ‘employee_uni’ DataFrames. Notice that I used ‘how=outer’ parameter that is for union of DataFrames. In this way, I am able to place null value for employees that don’t have unique ID.  Finally I returned the resulted DataFrame with name and unique ID. Notice that I used a dropna() function because of a test case that causes a ‘name’ value of some entries to be null after merge.

## **Students and Examinations**

Write a solution to find the number of times each student attended each exam.

Return the result table ordered by `student_id` and `subject_name`.

```python
def students_and_examinations(students: pd.DataFrame, subjects: pd.DataFrame, examinations: pd.DataFrame) -> pd.DataFrame:

    examinations = examinations.groupby(['student_id', 'subject_name']).agg(attended_exams=('subject_name', 'count')).reset_index()

    df = pd.merge(students, subjects, how = 'cross').sort_values(by = ['student_id' , 'subject_name'])

    df = pd.merge( df, examinations, how = 'left', on = ['student_id', 'subject_name'])

    df['attended_exams'] = df['attended_exams'].fillna(0)
    return df[['student_id', 'student_name', 'subject_name', 'attended_exams']]
```

First of all, I grouped and aggregated entries in ‘examinations’ DataFrame to create a column of counts of exams that each student took on each topic. Then, I merged ‘students’ and ‘subjects’  with ‘how=cross’ parameter to take the cross product of these DataFrames. Finally, I merged these 2 modified DataFrames with ‘how=left’ parameter to conduct a left outer join. I added the value 0 instead of null values and returned the resulted DataFrame.

## Managers with at Least 5 Direct Reports

Write a solution to find managers with at least **five direct reports**.

Return the result table in **any order**.

```python
def find_managers(employee: pd.DataFrame) -> pd.DataFrame:
    df = employee.groupby(['managerId'])['id'].count().reset_index()
    df = df.loc[df['id']>=5,['managerId']]
    df = employee.loc[employee['id'].isin(df['managerId']),['name']]

    return df
```

First, I grouped the entries of ‘employee’ DataFrame and count the ‘id’ values to find the number of direct reports of each manager. I filtered out the entries with counts less than 5 and I compared each manager ID in ‘employee’ DataFrame with entries in modified DataFrame which contains the managers with at least 5 direct reports. At the end, I returned the result of this comparison.

## **Sales Person**

Write a solution to find the names of all the salespersons who did not have any orders related to the company with the name **"RED"**.

Return the result table in **any order**.

```python
def sales_person(sales_person: pd.DataFrame, company: pd.DataFrame, orders: pd.DataFrame) -> pd.DataFrame:
    df = company.merge(orders, how='left')
    df = df.drop(df[df.name != 'RED'].index)

    df = sales_person.loc[sales_person['sales_id'].isin(df['sales_id']) == False,['name']]
    return df
```

First of all, I merged the ‘company’ and ‘orders’ DataFrames and from this merged DataFrame, I dropped the entries that aren’t related to the company with the name ‘RED’. Then, I compared the sales person IDs with the entries that are related to company RED. If the sales person is not matched, then it will be saved in the result DataFrame.