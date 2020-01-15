## Capstone Project 1: Data Wrangling

**[Google link](https://docs.google.com/document/d/1EBtoL05Ir64pk1aDLzz0nKp1YVikfVR0fGMQv5lTsqg/edit?usp=sharing)**

Steps:

1. Create a Google Doc (1-2 pages) describing the data wrangling steps you took to clean the dataset. Include answers to these questions in your submission:

2. What kind of cleaning steps did you perform?
 - Drop unnecessary columns
 - Apply datetime format
 - Calculate 24 month customer lifetime revenue by using order table based on item order data set
 - Get first order revenue per customer based on item order data set
 - Create dataset on customer_id level: contains all first order infomation + first order reveue +  24 month lifetime revenue

3. How did you deal with missing values, if any?
 - Del rows since only 2 missing values for each column

4. Were there outliers, and how did you handle them?
- Keep outliers since those are extreme values - some customers submitted large orders
- Anomaly: del customers (8 rows) with 24 month LTV lower than Zero
           Customers canceled orders or get refunds of the orders.

5. Submit a link to the document.

6. Discuss it with your mentor at the next call.

7. Revise and resubmit if needed.

8. Convert the final document to a .pdf and add it to your GitHub repository for this project. This document will eventually become part of your milestone report.


Code:
```python
import pandas as pd
pd.set_option('display.max_columns', None)
pd.plotting.register_matplotlib_converters()
import numpy as np
import datetime
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('ggplot')


import seaborn as sns
sns.set(rc={'figure.figsize':(16.7,6.27)})


from plotly.offline import init_notebook_mode, iplot # plotly offline mode
init_notebook_mode(connected=True) 
import plotly.graph_objs as go # plotly graphical object

import os
# print(os.listdir("../input"))

import warnings            
warnings.filterwarnings("ignore") 

from pandas import datetime
import datetime

# Basic  EDA
from sklearn.externals.six import StringIO
import statsmodels.api as sm
from scipy import stats

customer=pd.read_csv('https://drive.google.com/uc?export=download&id=1wv4GrazmaBO4wd6FuNay7pillVkUzzs7')

order=pd.read_csv('/Users/lylay/Desktop/spring board/bb_order.csv')

# drop cols
cols_to_drop=['item_id', 'Unnamed: 22','Unnamed: 21','p5','p7','p3','p2','p6']
order.drop(cols_to_drop, axis=1, inplace=True)

# apply datetime format
order['order_date'] =  pd.to_datetime(order['order_date'], infer_datetime_format=True)
order2=order.groupby(['customer_id'])['order_date'].min().reset_index(name='min_order_date')
order=pd.merge(order, order2, on='customer_id')
order['cut_date_24m']= order.min_order_date + datetime.timedelta(days=730)

# Calculate 24 month lifetime revenue and create column: revenue_24m
df1=order.groupby('customer_id').apply(lambda x: x[(x['order_date'] >= x['min_order_date']) & (x['order_date'] <= x['cut_date_24m'])]['revenue'].sum())
df1=df1.reset_index(name='revenue_24m')      

#Create first order info table for each customer based on order data sets
first_order_info= order.loc[order.order_date==order.min_order_date][['customer_id','ga_order_id','page_title','product','item_origin','revenue','facility_name','quantity',
                                                                     'p1','p4', 'shipping_method','address_flag', 'min_order_date']]

# rename column 'revenue' into 'first_order_revenue_item' since the df contains customer info of first order only
first_order_info.rename(columns={'revenue':'first_order_revenue_item'}, inplace=True)

#add 24month ltv and first order revenue to the customer dataframe
customer2=customer.merge(first_revenue, on='customer_id').merge(df1, on='customer_id')
customers=customer2.merge(first_order_info[['customer_id','min_order_date']], on='customer_id')

# drop cols
cols_to_drop=['Unnamed: 32','Unnamed: 33','Unnamed: 34','fo_custom_flag','first_order_source','first_h_item_id','first_order_id',
            'fo_ga_channel','fo_ffr_flag','fo_quote_flag','postal_code','registered_date', 'email_address','count_of_lifetime_orders',
        'count_of_lifetime_items']
customers.drop(cols_to_drop, axis=1, inplace=True)

# drop customer's 24m LTV (revenue_24m) less than ZERO (only 8 rows so it won't affect the whole data set)
customer_df=customers[customers.revenue_24m >= 0]

```