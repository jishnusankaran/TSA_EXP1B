# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 21/11/26

### AIM:
To perform regular differencing, seasonal adjustment, and log transformation on the New Car Sales dataset to convert non-stationary data into stationary data.
### ALGORITHM:
1.Import the required libraries such as Pandas, NumPy and Matplotlib.

2.Read the New Car Sales dataset using Pandas.

3.Convert Year and Month into a datetime format.

4.Calculate total monthly car sales by grouping the data.

5.Perform regular differencing using the monthly sales data.

6.Perform seasonal adjustment using seasonal decomposition.

7.Apply log transformation to the sales data.

8.Perform regular and seasonal differencing on the log-transformed data.

9.Plot all six graphs and display the overall comparison And Display the Result.
### PROGRAM:
```

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from statsmodels.tsa.seasonal import seasonal_decompose

data = pd.read_csv('/content/New Car Sales by Make.csv')

data['Date'] = pd.to_datetime(dict(
    year=data['Year'],
    month=data['Month'],
    day=1
))

monthly_sales = data.groupby('Date')['Quantity'].sum().sort_index()

monthly_sales = monthly_sales.asfreq('MS')

data_ts = pd.DataFrame(monthly_sales)
data_ts.columns = ['Total_Sales']

print("Original Data:")
print(data_ts.head())

data_ts['sales_diff'] = data_ts['Total_Sales'].diff()

result = seasonal_decompose(
    data_ts['Total_Sales'],
    model='additive',
    period=12
)

data_ts['sales_sea_diff'] = (
    data_ts['Total_Sales'] - result.seasonal
)

data_ts['sales_log'] = np.log(data_ts['Total_Sales'])

data_ts['sales_log_diff'] = (
    data_ts['sales_log'].diff()
)

data_ts['sales_log_seasonal_diff'] = (
    data_ts['sales_log_diff'].diff(12)
)

plt.figure(figsize=(16, 18))

plt.subplot(6, 1, 1)
plt.plot(data_ts['Total_Sales'], label='Original Sales')
plt.legend(loc='best')
plt.title('Original New Car Sales')
plt.xlabel('Year')
plt.ylabel('Total Cars Sold')

plt.subplot(6, 1, 2)
plt.plot(data_ts['sales_diff'], label='Regular Differencing')
plt.legend(loc='best')
plt.title('Regular Differencing')
plt.xlabel('Year')
plt.ylabel('Change in Sales')

plt.subplot(6, 1, 3)
plt.plot(data_ts['sales_sea_diff'], label='Seasonal Adjustment')
plt.legend(loc='best')
plt.title('Seasonal Adjustment')
plt.xlabel('Year')
plt.ylabel('Seasonally Adjusted Sales')

plt.subplot(6, 1, 4)
plt.plot(data_ts['sales_log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')
plt.xlabel('Year')
plt.ylabel('Log(Sales)')

plt.subplot(6, 1, 5)
plt.plot(data_ts['sales_log_diff'],
         label='Log Transformation and Regular Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular Differencing')
plt.xlabel('Year')
plt.ylabel('Difference of Log Sales')

plt.subplot(6, 1, 6)
plt.plot(data_ts['sales_log_seasonal_diff'],
         label='Log + Regular + Seasonal Differencing')
plt.legend(loc='best')
plt.title('Log Transformation, Regular and Seasonal Differencing')
plt.xlabel('Year')
plt.ylabel('Differenced Log Sales')

plt.tight_layout()
plt.show()

data_ts.plot(
    kind='line',
    figsize=(16, 8),
    title='Overall View of New Car Sales'
)

plt.xlabel('Year')
plt.ylabel('Values')
plt.show()

print("\nOverall Results:")
print(data_ts.tail())

print("\nDataset Information:")
print("Total Months:", len(data_ts))
print("Total Car Manufacturers:", data['Make'].nunique())
print("Total Cars Sold:", data['Quantity'].sum())

```

### OUTPUT:
ORIGINAL DATA:
<img width="1311" height="226" alt="Screenshot 2026-11-21 082333" src="https://github.com/user-attachments/assets/fdc5ba99-ebcc-4318-88c2-d8b398343cc4" />


REGULAR DIFFERENCING:
<img width="1310" height="242" alt="Screenshot 2026-11-21 082423" src="https://github.com/user-attachments/assets/b8ec2182-5895-4b63-81a8-21560045b32b" />


SEASONAL ADJUSTMENT:
<img width="1308" height="236" alt="Screenshot 2026-11-21 082506" src="https://github.com/user-attachments/assets/1d76b5a0-a3d2-4a76-8e4d-99c584678754" />


LOG TRANSFORMATION:
<img width="1314" height="237" alt="Screenshot 2026-11-21 082553" src="https://github.com/user-attachments/assets/55482c38-f637-49aa-895f-4f953cef22cc" />


LOG TRANSFORMATION + REGULAR DIFFERENCING:
<img width="1315" height="241" alt="Screenshot 2026-11-21 082620" src="https://github.com/user-attachments/assets/f1dfdf22-7fa9-4538-b7bb-2eecdfab38b3" />



LOG TRANSFORMATION + REGULAR DIFFERENCE + SEASONAL DIFFERENCE:
<img width="1310" height="244" alt="Screenshot 2026-11-21 082638" src="https://github.com/user-attachments/assets/906ace9c-c828-4a0d-89cf-f54b449eb2c9" />

<img width="1115" height="575" alt="Screenshot 2026-11-21 082708" src="https://github.com/user-attachments/assets/0ea4d0c0-8fe4-4db6-9c6d-10e7da2e3d48" />


### RESULT:
**RESULT:**
Thus, we have successfully implemented Python code for converting non-stationary data into stationary data using the New Car Sales dataset. Regular differencing, seasonal adjustment, log transformation, and seasonal differencing were performed, and the corresponding graphs and overall results were displayed successfully.
