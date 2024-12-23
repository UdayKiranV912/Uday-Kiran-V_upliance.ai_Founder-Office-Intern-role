# User Behavior, Cooking Preferences, and Order Trends Analysis

## Objective:
The objective of this assignment is to analyze datasets related to user behavior, cooking preferences, and order trends. You will work with three datasets: `UserDetails`, `CookingSessions`, and `OrderDetails`. The tasks include:
- Cleaning and merging the data
- Analyzing the relationship between cooking sessions and user orders
- Identifying popular dishes
- Exploring demographic factors that influence user behavior
- Creating visualizations to showcase key insights
- Writing a report summarizing findings and business recommendations

## Datasets:
1. **UserDetails**: Contains user information (e.g., age, location, favorite meal, total orders).
2. **CookingSessions**: Contains details of cooking sessions, such as the dish prepared, session rating, and duration.
3. **OrderDetails**: Contains order-related data, including order status, amount, and session ID.

## Libraries Required:
```bash
pip install pandas matplotlib seaborn numpy
1. Import Libraries
python
Copy code
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
2. Load the Datasets
python
Copy code
# Load the datasets
user_details = pd.read_csv('UserDetails.csv')
cooking_sessions = pd.read_csv('CookingSessions.csv')
order_details = pd.read_csv('OrderDetails.csv')
3. Data Cleaning
Remove any duplicate rows.
Handle missing values.
Convert date columns to datetime format.
Ensure correct data types for each column (e.g., integer, float).
python
Copy code
# Remove duplicates
user_details.drop_duplicates(inplace=True)
cooking_sessions.drop_duplicates(inplace=True)
order_details.drop_duplicates(inplace=True)

# Handle missing values
user_details.fillna(method='ffill', inplace=True)
cooking_sessions.fillna(method='ffill', inplace=True)
order_details.fillna(method='ffill', inplace=True)

# Convert date columns to datetime
user_details['Registration Date'] = pd.to_datetime(user_details['Registration Date'])
cooking_sessions['Session Start'] = pd.to_datetime(cooking_sessions['Session Start'])
cooking_sessions['Session End'] = pd.to_datetime(cooking_sessions['Session End'])
order_details['Order Date'] = pd.to_datetime(order_details['Order Date'])
4. Merging Data
Merge the three datasets based on common columns such as User ID and Session ID.
python
Copy code
# Merge datasets
merged_data = pd.merge(order_details, cooking_sessions, on='Session ID', how='left')
merged_data = pd.merge(merged_data, user_details, on='User ID', how='left')
5. Analyzing the Relationship Between Cooking Sessions and User Orders
Investigate the number of orders per user.
Analyze the relationship between meal types and session ratings.
python
Copy code
# Analyze the relationship between session ratings and user orders
order_ratings = merged_data.groupby(['User ID', 'Meal Type']).agg({'Rating': 'mean', 'Amount (USD)': 'sum'}).reset_index()
6. Identifying Popular Dishes
Find the most frequently ordered dishes.
Calculate the average rating for each dish.
python
Copy code
# Most popular dishes based on order count
popular_dishes = merged_data['Dish Name'].value_counts().reset_index()
popular_dishes.columns = ['Dish Name', 'Order Count']

# Average ratings for each dish
average_ratings = merged_data.groupby('Dish Name')['Rating'].mean().reset_index()
7. Demographic Analysis
Analyze how factors like age, location, and favorite meal influence user behavior.
python
Copy code
# Age distribution of users
age_distribution = user_details['Age'].describe()

# Favorite meal vs total orders
meal_order_analysis = user_details.groupby('Favorite Meal')['Total Orders'].sum().reset_index()

# Location vs total orders
location_order_analysis = user_details.groupby('Location')['Total Orders'].sum().reset_index()
8. Visualizations
Create visualizations to explore relationships between different variables.
python
Copy code
# Pie chart for meal types ordered by time of day
meal_time_dist = merged_data['Time of Day'].value_counts()
meal_time_dist.plot(kind='pie', autopct='%1.1f%%', figsize=(6, 6), colors=['skyblue', 'orange', 'green'])
plt.title('Percentage of Orders by Time of Day')
plt.ylabel('')
plt.show()

# Bar plot for popular dishes
sns.barplot(x='Order Count', y='Dish Name', data=popular_dishes, palette='Set2')
plt.title('Most Popular Dishes')
plt.show()

# Scatter plot for age vs total orders
sns.scatterplot(x='Age', y='Total Orders', data=user_details, hue='Favorite Meal')
plt.title('Age vs Total Orders by Favorite Meal')
plt.show()
9. Report Summary
Based on the analysis, summarize key findings and provide business recommendations. Consider:

Which dishes are most popular.
The influence of meal type on order frequency.
How demographic factors like age and location affect ordering behavior.
Possible marketing strategies for targeted promotions based on user preferences.
Conclusion:
The insights drawn from this analysis can guide business decisions, such as targeted advertising and menu customization, and help optimize user experience by identifying key preferences.