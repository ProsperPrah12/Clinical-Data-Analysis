# Documenting Sales project work

### Project Over view 

This project analyzes sales data to uncover insights on salespeople, products, and countries. It explores trends over time, identifies top performers, and highlights key revenue drivers. The analysis uses Python (pandas, matplotlib, numpy) for data manipulation and visualization.

### Data Sources

Sale Data: The primary dataset used for this analysis is the "sale.csv" file, containing detail information about sales.

### Tools Used

- Python (pandas, matplotlib, numpy)

### Data Cleaning/ Preparation 

In the initial preparation phase, I perform the folowing tasks;
1. Data loading and inspection
2. Cleaning empty cells
```python
   import pandas as pd
   data = pd.read_csv("sales.csv")
   data.dropna(inplace = True)
   print(data.to_string())
```
3. Cleaning wrong format
```python
   import pandas as pd
   Data= pd.read_csv('sales.csv')
   Data['Date'] = pd.to_datetime(Data['Date'], format='mixed')
   print(Data.to_string())
```
5. Removing Duplicates
 ```python
    import pandas as pd
    Data = pd.read_csv("sales.csv")
    print(Data.duplicated().to_string())
 ```
### Exploratory Data Analysis (EDA)
 EDA involve the exploring clinical data to answer key questions, such as;

- Create a pivot table showing total sales by Country and Product.
- Analyze seasonality: Do certain products sell better in particular months?
- Build a correlation analysis between Amount and Boxes Shipped.
- Identify the top 3 salespeople in each country by sales amount.
- Forecast: Based on trends, estimate the next month’s sales using simple time-series methods.

### Data Analysis

1. Create a pivot table showing total sales by Country and Product.

```python
      import pandas as pd
      import numpy as np
      import matplotlib.pyplot as plt
      
      # Load the data
      df = pd.read_csv("sales.csv")
      
      # Create pivot table
      pivot_table = pd.pivot_table(
          df,
          values="Amount",
          index="Country",
          columns="Product",
          aggfunc="sum",
          fill_value=0
      )
      
      
      # Plot heatmap using numpy + matplotlib
      plt.figure(figsize=(14, 8))
      
      # Convert pivot table to numpy array
      data = pivot_table.values
      
      # Show the heatmap
      plt.imshow(data, cmap="YlGnBu", aspect="auto")
      
      # Add colorbar
      plt.colorbar(label="Total Sales Amount")
      
      # Set ticks and labels
      plt.xticks(np.arange(len(pivot_table.columns)), pivot_table.columns, rotation=90)
      plt.yticks(np.arange(len(pivot_table.index)), pivot_table.index)
      
      # Add title
      plt.title("Total Sales by Country and Product (Heatmap with NumPy + Matplotlib)", fontsize=14)
      
      plt.tight_layout()
      plt.show()
   ```
![Chat 1](https://github.com/user-attachments/assets/c3950298-5724-4827-9c29-e26cc1c7c13d)


2. Analyze seasonality: Do certain products sell better in particular months?

   ```python
      import pandas as pd 
      import numpy as np
      import matplotlib.pyplot 
      
      # Load the file 
      df = pd.read_csv("sales.csv", encoding = 'latin-1')
      
      # Remove any non-numeric characters (like commas, $, £)
      df["Amount"] = df["Amount"].replace(r"[^\d.]", "", regex=True).astype(float)
      
      # Convert Date column to datetime
      df["Date"] = pd.to_datetime(df["Date"], errors="coerce")
      
      # Extract month name from Date
      df["Month"] = df["Date"].dt.month_name()
      
      # Group by Product and Month
      monthly_sales = df.groupby(["Product", "Month"])["Amount"].sum().reset_index()
      
      # Pivot for better visualization
      pivot_seasonality = monthly_sales.pivot(index="Month",  columns="Product",  values="Amount").fillna(0)
      
      month_order = [
          "January", "February", "March", "April", "May", "June",
          "July", "August", "September", "October", "November", "December"
      ]
      pivot_seasonality = pivot_seasonality.reindex(month_order)

      # plot
      plt.figure(figsize=(14, 8))
      for product in pivot_seasonality.columns:
      plt.plot(pivot_seasonality.index, pivot_seasonality[product],marker ="o" label=product)
      
      plt.title("Seasonality of Product Sales", fontsize=16)
      plt.xlabel("Month")
      plt.ylabel("Total Sales Amount")
      plt.xticks(rotation=45)

      # Put legend outside
      plt.legend(bbox_to_anchor=(1.05, 1), loc="upper left")  
      plt.tight_layout()
      plt.show()
   ```
![Chat 2](https://github.com/user-attachments/assets/f27da1d1-8f3f-4e3e-8a9b-beb1f5acac54)

3. Forecast: Based on trends, estimate the next month’s sales using simple time-series methods.
   
   ```python
      import pandas as pd
      import matplotlib.pyplot as plt
      
      # Ensure Date is datetime
      df["Date"] = pd.to_datetime(df["Date"])
      
      # Clean Amount column
      df["Amount"] = df["Amount"].replace(r"[^\d.]", "", regex=True).astype(float)
      
      # Aggregate sales by month
      monthly_sales = df.groupby(df["Date"].dt.to_period("M"))["Amount"].sum().reset_index()
      monthly_sales["Date"] = monthly_sales["Date"].dt.to_timestamp()
      
      # Calculate 3-month moving average
      monthly_sales["3_MA"] = monthly_sales["Amount"].rolling(window=3).mean()
      
      # Forecast next month = last available 3-month average
      forecast_next = monthly_sales["3_MA"].iloc[-1]
      print("Forecast for next month (Moving Average):", forecast_next)
      
      # plot
      plt.figure(figsize=(10, 6))
      plt.plot(monthly_sales["Date"], monthly_sales["Amount"], marker="o", label="Actual Sales")
      plt.plot(monthly_sales["Date"], monthly_sales["3_MA"], marker="x", linestyle="--", label="3-Month Moving Avg")
      
      # Extend forecast
      next_month = monthly_sales["Date"].iloc[-1] + pd.DateOffset(months=1)
      plt.scatter(next_month, forecast_next, color="red", label="Forecast (Next Month)")
      
      plt.title("Monthly Sales with Forecast")
      plt.xlabel("Month")
      plt.ylabel("Sales Amount")
      plt.legend()
      plt.grid(True)
      plt.show()
   ```
![Chat 5](https://github.com/user-attachments/assets/575f59ab-a42c-4094-b844-1028a416fae6)

4. Identify the top 3 salespeople in each country by sales amount.

  ```python
     import pandas as pd
      import numpy as np
      
      # Load dataset
      df = pd.read_csv("sales.csv", encoding="latin-1")
      
      # Clean Amount column
      df["Amount"] = df["Amount"].replace(r"[^\d.]", "", regex=True).astype(float) 
      
      # Group by Country and Salesperson, sum the sales
      sales_by_person = df.groupby(["Country", "Sales Person"])["Amount"].sum().reset_index()
      
      # Rank salespeople within each country
      sales_by_person["Rank"] = sales_by_person.groupby("Country")["Amount"].rank(method="first", ascending=False)
      
      # Get top 3 salespeople per country
      top3_salespeople = sales_by_person[sales_by_person["Rank"] <= 3].sort_values(["Country", "Rank"])
      
      print(top3_salespeople)
   ```
![Chat 4](https://github.com/user-attachments/assets/7665b490-08dc-4b31-aa9c-a3d74bd5cc0d)

5. Build a correlation analysis between Amount and Boxes Shipped.
  ```python
     import pandas as pd
      import matplotlib.pyplot as plt
      
      # Ensure Date is datetime
      df["Date"] = pd.to_datetime(df["Date"])
      
      # Clean Amount column
      df["Amount"] = df["Amount"].replace(r"[^\d.]", "", regex=True).astype(float)
      
      # Aggregate sales by month
      monthly_sales = df.groupby(df["Date"].dt.to_period("M"))["Amount"].sum().reset_index()
      monthly_sales["Date"] = monthly_sales["Date"].dt.to_timestamp()
      
      # Calculate 3-month moving average
      monthly_sales["3_MA"] = monthly_sales["Amount"].rolling(window=3).mean()
      
      # Forecast next month = last available 3-month average
      forecast_next = monthly_sales["3_MA"].iloc[-1]
      print("Forecast for next month (Moving Average):", forecast_next)
      
      # plot
      plt.figure(figsize=(10, 6))
      plt.plot(monthly_sales["Date"], monthly_sales["Amount"], marker="o", label="Actual Sales")
      plt.plot(monthly_sales["Date"], monthly_sales["3_MA"], marker="x", linestyle="--", label="3-Month Moving Avg")
      
      # Extend forecast
      next_month = monthly_sales["Date"].iloc[-1] + pd.DateOffset(months=1)
      plt.scatter(next_month, forecast_next, color="red", label="Forecast (Next Month)")
      
      plt.title("Monthly Sales with Forecast")
      plt.xlabel("Month")
      plt.ylabel("Sales Amount")
      plt.legend()
      plt.grid(True)
      plt.show()
  ```
![Chat 3](https://github.com/user-attachments/assets/5e9a98c8-750a-472c-815d-e4a7a8f1ac84)

### Results
1. Sales by Country and Product
    - A pivot table revealed variations in sales performance across different countries and products. This provided insights into which products dominate        in specific markets.
      
2. Seasonality of Sales
   - Monthly sales aggregation showed that certain products exhibit seasonal trends, performing better in particular months. These patterns can help            with planning inventory and marketing strategies.
     
3. Correlation Between Amount and Boxes Shipped
   - The correlation coefficient between sales amount and boxes shipped was -0.0188. Its indicates almost no linear relationship, suggesting that higher        sales revenue is not necessarily linked to higher shipment volumes.
     
4. Top Salespeople by Country
   - Ranking salespeople by sales amounts identified the top three performers in each country and highlight regional leaders and provide benchmarks             for performance evaluation.
     
5. Sales Forecasting
   - Using a 3-month moving average, the forecast for next month’s sales was estimated. This simple time-series method offers a baseline prediction that        can be improved with more advanced forecasting models.

### Recommendations

- Focus marketing campaigns and inventory planning around products that show strong seasonal demand.
- Adjust pricing strategies and introduce product bundles to increase overall revenue per transaction.
- Recognize top-performing salespeople and share their strategies to improve performance across teams.
- Invest in more advanced forecasting models to anticipate sales trends with higher accuracy.
- Implement real-time dashboards to monitor sales and support faster, data-driven decision-making.

### Limitations 

1. The analysis is based solely on the provided dataset, which may not capture external factors such as market competition or economic conditions.
2. Sales data was aggregated at the monthly level, limiting insights into weekly or daily fluctuations.
3. Certain business drivers, such as marketing spend, customer demographics, or promotional activities, were not included in the dataset.
4. While correlations between variables were analyzed, correlation does not imply causation, and deeper statistical modeling may be required.
