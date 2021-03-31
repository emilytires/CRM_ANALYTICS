# CRM_ANALYTICS

What will we learn here?
    
In this section, we will analyze important metrics related to crm analytics such as level based persona, rfm metrics, customer lifetime value calculation, customer lifetime prediction.

1- Level Based Customer Segmentation : differentiates customers by their country, device type, gender and bucketization of age then grouping customers with the price value level into individual segments that can be distinctly targeted. It is one of the basic ways we can segment customers without using machine learning models.

  Final table features:
  
    customers_level_based (include country, device, gender and categorical age)
    
    price
    
 2- RFM Segmentation : these metrics are important indicators of a customer’s behavior because frequency and monetary value affects a customer’s lifetime value, and recency affects retention, a measure of engagement.
    
  Final table features:
  
    Recency : Time since last  order
    
    Frequency : Total number of transactions
    
    Monetary : Total price
    
    RecencyScore (using qcut functions, we will segment Recency) 1: worst , 2, 3, 4, 5: best
    
    FrequencyScore (using qcut functions, we will segment Frequency) 1: worst , 2, 3, 4, 5: best
    
    MonetaryScore (using qcut functions, we will segment Monetary) 1: worst , 2, 3, 4, 5: best
    
    RFM_SCORE ( string(RecencyScore) + string(FrequencyScore) + string(MonetaryScore))
    
    
    555 => Champions ....
    111 => Hibernating

3- Customer LifeTime Value Calculation : If you want your business to acquire and retain highly valuable customers, then it's essential that your team learns what customer lifetime value is and how to calculate it.

  Final table features:
  
    total_transaction : how many transactions each customer makes
    
    total_unit : how many orders each customer placed
    
    total_price : how much money each customer spent
    
    avg_order_value
    
    purchase_frequency
    
    profit_margin
    
    customer_value
    
    cltv_c
    
    SCALED_CLTV_C : scaling of cltv_c (range 1-100)
    
    segment : segment customers using the qcut function in the scaled_cltv_c

  # Calculation of Customer Lifetime Metrics
  
  cltv['avg_order_value'] = cltv["total_price"] / cltv['total_transaction']
  
  cltv["purchase_frequency"] = cltv['total_transaction'] / cltv.shape[0]
  
  repeat_rate = cltv[cltv["total_transaction"] > 1].shape[0] / cltv.shape[0]
  
  churn_rate = 1 - repeat_rate
  
  cltv['profit_margin'] = cltv['total_price'] * 0.05
  
  cltv['customer_value'] = cltv['avg_order_value'] * cltv["purchase_frequency"]
  
  cltv['cltv_c'] = (cltv['customer_value'] / churn_rate) * cltv['profit_margin']
    
  4- Customer LifeTime Value Prediction : This is statistical way to learn your customer segments. According to CLV allows us to focus on the long-term results, not the short-term profits.
  
  The modelling & evaluation process is going to be the following:
  
   Fit and evaluate BG/NBD model for frequency prediction (Beta Geometric/Negative Binomial Distribution)
   
   Fit and evaluate Gamma-Gamma model for monetary value prediction
   
   Combine 2 models into CLV model and compare to baseline
   
   Refit the model on the entire dataset
   
   Final table features:
   
    Recency_cltv_p
    
    T (Tenure) : customer age
    
    Frequency 
    
    Monetary_avg : rfm_from_uk["Monetary"] / rfm_from_uk["Frequency"]
    
    Recency_weekly_p : Recency_cltv_p/7
    
    T_weekly : Tenure/7
    
    clv  : time window. It can be anything like 3, 6, 12, 24 months. We will go ahead 6 months
    
    cltv_scaled : scaling of clv (range 1-100)
    
    cltv_p_segment : segment customers using the qcut function in the cltv_scaled
    
