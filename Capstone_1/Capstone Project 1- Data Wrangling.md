## Capstone Project 1: Data Wrangling

**[Google link](https://docs.google.com/document/d/1EBtoL05Ir64pk1aDLzz0nKp1YVikfVR0fGMQv5lTsqg/edit?usp=sharing)**

Steps:

1. What kind of cleaning steps did you perform?

 - Apply datetime format
 ```python
order_info['order_date'] =  pd.to_datetime(order_info['order_date'], infer_datetime_format=True)
order_info["min_order_date"] = order_info.groupby(["uid"])['order_date'].transform(min)
order_info["recent_order_date"] = order_info.groupby(["uid"])['order_date'].transform(max)
```
  - Calculate 12 month customer lifetime revenue by using order table based on item order data set
 ```python
 df1=order_info.groupby('uid').apply(lambda x: x[(x['order_date'] >= x['min_order_date']) & (x['order_date'] <= x['cutoff_12m_date'])]['order_revenue'].sum())
 df1=df1.reset_index(name='revenue_12m')
 ```
 - create target column repeat_purchase
```python
customers['repeat_purchase'] = ~(customers.order_count_12m < 2)
```
 - Drop unnecessary columns
 
 ```python
 cols_to_drop=['landing_site_ref','tags', 'utm_term','utm_content','landing_site',
         'utm_campaign','utm_medium','utm_source','referring_site']
 df_customer.drop(cols_to_drop, axis=1, inplace=True)
```

2. How did you deal with missing values, if any?
 - filling null based on business definitions
 ```python
 cols_to_fill = ['utm_campaign', 'utm_medium','utm_source', 'referring_site','AE', 'Dotdigital', 'Spotify']
 for i in cols_to_fill:
     customers[i+'_known'] = customers[i].notnull()

 cols_to_drop=['landing_site_ref','tags', 'utm_term','utm_content','landing_site',
         'utm_campaign','utm_medium','utm_source','referring_site','AE', 'Dotdigital', 'Spotify']
 customers.drop(cols_to_drop, axis=1, inplace=True)

 customers['province_code'].fillna("unknown", inplace = True) 
 customers['country_code'].fillna("unknown", inplace = True) 

```

3. Were there outliers, and how did you handle them?
- Keep outliers since those are extreme values - some customers submitted large orders
- Anomaly: del customers (8 rows) with 24 month LTV lower than Zero
           Customers canceled orders or get refunds of the orders.

