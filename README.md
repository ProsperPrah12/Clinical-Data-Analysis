# Documenting Sales project work

### Project Over view 

This project analyzes sales data to uncover insights on salespeople, products, and countries. It explores trends over time, identifies top performers, and highlights key revenue drivers. The analysis uses Python (pandas, matplotlib, numpy) for data manipulation and visualization.

### Data Sources

Sale Data: The primary dataset used for this analysis is the "clinical_data.csv" file, containing detail information about patient diagnosis.

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
  df = pd.read_csv('data.csv')
  df['Date'] = pd.to_datetime(df['Date'], format='mixed')
  print(df.to_string())
```
5. Data formatting

### Exploratory Data Analysis (EDA)
EDA involve the exploring clinical data to answer key questions, such as;

- Which diseases are more frequent in males and females?
- What is the proportion of new, old, and recurring cases?
- Do certain diseases occur more in specific age groups?
- What are the most common diagnoses among patients?
