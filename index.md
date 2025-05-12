[Git repo](https://github.com/anpac20/sales_predictive_analysis)

## 1. Business hypothesis  
In this project, I build a predictive model using real sales data from Favorita stores in Ecuador to estimate future sales volume. The goal is to simulate a real-world forecasting scenario where multiple data sources are combined to drive operational planning and commercial decisions.

Key questions:

- Can sales be accurately predicted using available store and transaction-level data?
- How much value do features like previous sales, store metadata, and oil prices add to model accuracy?

---

## 2. Results and recommendations  

**Use lag features for forecasting**  
The model without lag features (using only promotions, store info, and oil prices) reached an RMSE of **446.94**. When sales history was included (1-day and 7-day lag), error dropped to **~200**, highlighting the importance of historical demand patterns in retail forecasting.

**Smoothed oil prices improve signal quality**  
Using 7-day and 30-day moving averages for oil prices led to more stable model behavior. Daily oil price noise can hinder performance, especially when used as a macroeconomic proxy.

**Visual inspection validates prediction patterns**  
Plots comparing actual vs predicted sales (smoothed over 7 days) reveal that the model captures overall trends, even when fine-grain fluctuations are harder to predict. This makes the approach valuable for medium-term planning (stock, campaigns, logistics).

---

## 3. Data  
The dataset comes from a Kaggle competition and contains daily sales across stores and product families. Additional data includes store metadata and oil prices, all of which are used to simulate the external and internal variables a business may have access to.

The main files used are:

- `train.csv`: Daily sales per store and product family
- `stores.csv`: Store metadata (type, cluster, city, state)
- `oil.csv`: Daily oil prices (proxy for economic conditions)

Dates range from 2013 to 2017. Forecasting was done using data up to mid-2017 for training, with the final months reserved for validation.

---

## 4. Feature Engineering  
I enriched the dataset with multiple feature types:

- **Temporal**: day of week, month, weekend flag  
- **Categorical**: encoded store location and type  
- **Macroeconomic**: daily oil price, 7- and 30-day rolling averages  
- **Lag features**: sales per store-family lagged by 1 and 7 days

This structured approach allowed the model to learn from past patterns while incorporating context from store and market conditions.

---

## 5. Model and Training  
A Random Forest Regressor was trained using scikit-learn with 20 estimators. The model was evaluated with and without lag features using RMSE as the metric.

Split strategy:

- **Train set:** dates before June 2017  
- **Validation set:** dates from June 2017 onward

This simulates a realistic time-based validation used in production forecasting environments.

---

## 6. Visual Evaluation  

The high volume of daily sales data makes raw prediction plots difficult to interpret due to noise and extreme spikes. To address this, I applied a two-step smoothing strategy:
![pred_valid](images/pred_valid.png)

Daily sales prediction vs. actual values, smoothed with a daily rolling average to visualize trends over time:
![pred_valid_daily](images/pred_valid_daily.png)


This view confirms that the model captures the overall dynamics of sales behavior and offers an interpretable way to communicate accuracy to business stakeholders.
