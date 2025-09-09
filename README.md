# Documenting Sales project work

### Project Over view 

This project analyzes sales data to uncover insights on salespeople, products, and countries. It explores trends over time, identifies top performers, and highlights key revenue drivers. The analysis uses Python (pandas, matplotlib, numpy) for data manipulation and visualization.

### Data Sources

Sale Data: The primary dataset used for this analysis is the "sale.csv" file, containing detail information about sale.

### Tools Used

- Python (pandas, matplotlib, numpy)

### Data Cleaning/ Preparation 

In the initial preparation phase, I perform the folowing tasks;
1. Data loading and inspection
2. Cleaning empty cells
```python
   import pandas as pd
   data = pd.read_csv("sale.csv")
   data.dropna(inplace = True)
   print(data.to_string())
```
3. Cleaning wrong format
```python
  import pandas as pd
   Data= pd.read_csv('sale.csv')
   Data['Date'] = pd.to_datetime(Data['Date'], format='mixed')
   print(Data.to_string())
```
5. Removing Duplicates
   ```python
      import pandas as pd
      Data = pd.read_csv("sale.csv")
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

- Create a pivot table showing total sales by Country and Product.

```python
      import pandas as pd
      import numpy as np
      import matplotlib.pyplot as plt
      
      # Load the data
      df = pd.read_csv("sale.csv")
      
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
- Analyze seasonality: Do certain products sell better in particular months?

  
