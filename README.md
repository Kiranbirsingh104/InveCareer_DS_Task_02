# project
# data
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

# Input data files are available in the read-only "../input/" directory
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory
import os
for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))
        import pandas as pd 
store = pd.read_csv("/kaggle/input/superstore-sales/superstore_final_dataset (1).csv",encoding='latin-1')
store.head()
step 1: load the data
import numpy as np
store = store.rename(columns={'Order_ID':"OrderID",'Sub_Category':"Product",'Sales':'Price',"Order_Date":'OrderDate','Customer_ID':'CustomerID'})
store["Quantity"]=np.random.poisson(lam=3)
store.head()
data = store[['OrderID','Product','Category','Price','Quantity','OrderDate','CustomerID']]
df = data.iloc[:500]
df.tail()
# Step 2: Data Cleaning and Preprocessing
# Convert OrderDate to datetime
df.isnull().sum()
#usedwhereduplicatevale
df.duplicated().sum()
import pandas as pd 
#df['OrderDate'] = pd.to_datetime(format="%d/%m/%Y")
# Step 3: Sales Trends Analysis
#sales over trends
import matplotlib.pyplot as plt
sales_trends=df.groupby('OrderDate').agg({'Price':'sum'}).reset_index()
# Line plot for sales trends over time
plt.figure(figsize=(14, 7))
plt.plot(sales_trends['OrderDate'], sales_trends['Price'])
plt.title('Sales Trends Over Time')
plt.xlabel('Date')
plt.ylabel('Total Sales')
plt.show()
# Step 4: Product Price Distribution
# Descriptive statistics
price_stats = df['Price'].describe()
print(price_stats)
# Price distribution visualization
df['Price'].plot(kind='hist', bins=30, figsize=(14,7))
plt.title('Product Price Distribution')
plt.xlabel('Price')
plt.ylabel('Frequency')
plt.show()
# Step 5: Customer Spending Habits
# Total spending per customer
customer_spending = df.groupby('CustomerID').sum()['Price']
# Spending distribution visualization
customer_spending.plot(kind='hist', bins=50, figsize=(14, 7))
plt.title('Customer Spending Distribution')
plt.xlabel('Total Spending')
plt.ylabel('Frequency')
plt.show()
# Step 6: Customer Segmentation (RFM Analysis)
import datetime as dt
# Aggregate data on customer level
rfm_df = df.groupby('CustomerID').agg({
   'Price':'sum'}).reset_index()
# Rename columns
rfm_df.rename(columns={
    'OrderDate': 'Recency',
    'OrderID': 'Frequency',
    'Price': 'Monetary'
}, inplace=True)
# Step 7: Insights Generation
# Print top spenders
rfm_df.sort_values(by='Monetary', ascending=False).head()
# Optimize Product Offerings
best_selling_products = df.groupby('Product').sum()['Price'].sort_values(ascending=False).head(10)
best_selling_products.head()
# Suggest marketing strategies
# Example: Target high-value customers with special offers
high_value_customers = rfm_df[rfm_df['Monetary'] > rfm_df['Monetary'].quantile(0.75)]
high_value_customers.head()
# Optimize Product Offerings
best_selling_products = df.groupby('Product').sum()['Price'].sort_values(ascending=False).head(10)
best_selling_products.head()
# Suggest marketing strategies
# Example: Target high-value customers with special offers
high_value_customers = rfm_df[rfm_df['Monetary'] > rfm_df['Monetary'].quantile(0.75)]
high_value_customers.head()
# Step 8: Optional - Save the cleaned and aggregated data for future use
df.to_csv('cleaned_ecommerce_data.csv',index=False)
rfm_df.to_csv('rfm_analysis.csv',index=True)
