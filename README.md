# G2M-Cab-Investment-Firm
Consolidated database and implemented data cleaning process for taking a corporate investment decision (Go-2-Market). Analysed market share and profit to provide investors with a solution. Using Matplotlib and Seaborn, demonstrated profit percentages for several business proposals. 

## Project Introduction
  XYZ is a private firm in The United States of America. The company is considering for expansion of their services and has decided to go ahead with the G2M strategy. The company has got business proposals from 2 companies. 
  
  Objective : 
  - To provide actionable insights to help XYZ firm in identifying the right company for making investment.
  - The Analysis has been done with the following procedures: 
  - Understanding data and its structure,
  - Estimating number of trips, identifying top users by demographics, and profit for both the companies,
  - Identifying the most popular and lucrative cab service,
  - Investment recommendations.
  
  ## Contents
- Convert & Load the dataset
- Get an Overview about the data e.g. shape, information and datatypes of the dataset. 
- Data cleaning (if required), check for missing values or delete unwated columns.
- Changing datatypes, making master data.
- Factorizing the data using Pandas
- Data Visualization, show correlation matrix

## Required Packages EDA
- numpy
- pandas
- seaborn
- matplotlib
- datetime

## Importing Packages
```
import numpy as np 
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import datetime
```

## Reading and Manipulating Data
```
cab_data = pd.read_csv(r"G2M Cab Investment Firm\Cab_Data.csv")
transactionID_data = pd.read_csv(r"G2M Cab Investment Firm\Transaction_ID.csv")
customer_ID = pd.read_csv(r"G2M Cab Investment Firm\Customer_ID.csv")
city_data = pd.read_csv(r"G2M Cab Investment Firm\City.csv")
```
## Previewing data and datatypes
### cab_data.head()
```
	Transaction ID	Date of Travel	Company	City	KM Travelled	Price Charged	Cost of Trip
0	10000011	08/01/2016	Pink Cab	ATLANTA GA	30.45	370.95	313.635
1	10000012	06/01/2016	Pink Cab	ATLANTA GA	28.62	358.52	334.854
2	10000013	02/01/2016	Pink Cab	ATLANTA GA	9.04	125.20	97.632
3	10000014	07/01/2016	Pink Cab	ATLANTA GA	33.17	377.40	351.602
4	10000015	03/01/2016	Pink Cab	ATLANTA GA	8.73	114.62	97.776
```
### transactionID_data.head()
```
	Transaction ID	Customer ID	Payment_Mode
0	10000011	29290	Card
1	10000012	27703	Card
2	10000013	28712	Cash
3	10000014	28020	Cash
4	10000015	27182	Card
```
### customer_ID.head()
```
Customer ID	Gender	Age	Income (USD/Month)
0	29290	Male	28	10813
1	27703	Male	27	9237
2	28712	Male	53	11242
3	28020	Male	23	23327
4	27182	Male	33	8536
```
### city.head()
```
	City	Population	Users
0	NEW YORK NY	8,405,837	302,149
1	CHICAGO IL	1,955,130	164,468
2	LOS ANGELES CA	1,595,037	144,132
3	MIAMI FL	1,339,155	17,675
4	SILICON VALLEY	1,177,609	27,247
```
### dataset info
```
cab_data.info()
transactionID_data.info()
customer_ID.info()
city.head.info()
```
#### cab_data.info()
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 359392 entries, 0 to 359391
Data columns (total 7 columns):
 #   Column          Non-Null Count   Dtype  
---  ------          --------------   -----  
 0   Transaction ID  359392 non-null  int64  
 1   Date of Travel  359392 non-null  object 
 2   Company         359392 non-null  object 
 3   City            359392 non-null  object 
 4   KM Travelled    359392 non-null  float64
 5   Price Charged   359392 non-null  float64
 6   Cost of Trip    359392 non-null  float64
dtypes: float64(3), int64(1), object(3)
memory usage: 19.2+ MB
```
#### transactionID_data.info()
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 440098 entries, 0 to 440097
Data columns (total 3 columns):
 #   Column          Non-Null Count   Dtype 
---  ------          --------------   ----- 
 0   Transaction ID  440098 non-null  int64 
 1   Customer ID     440098 non-null  int64 
 2   Payment_Mode    440098 non-null  object
dtypes: int64(2), object(1)
memory usage: 10.1+ MB
```
#### customer_ID.info()
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 49171 entries, 0 to 49170
Data columns (total 4 columns):
 #   Column              Non-Null Count  Dtype 
---  ------              --------------  ----- 
 0   Customer ID         49171 non-null  int64 
 1   Gender              49171 non-null  object
 2   Age                 49171 non-null  int64 
 3   Income (USD/Month)  49171 non-null  int64 
dtypes: int64(3), object(1)
memory usage: 1.5+ MB
```
#### city_data.info()
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 20 entries, 0 to 19
Data columns (total 3 columns):
 #   Column      Non-Null Count  Dtype 
---  ------      --------------  ----- 
 0   City        20 non-null     object
 1   Population  20 non-null     object
 2   Users       20 non-null     object
dtypes: object(3)
memory usage: 608.0+ bytes
```
#### changing datatypes ad merging data
```
cab_data['Date of Travel'] = pd.to_datetime(cab_data['Date of Travel'])
```
```
master_data = cab_data.merge(transactionID_data, on = 'Transaction ID').merge(customer_ID, on = 'Customer ID').merge(city_data, on = 'City')
```
### Previewing master data
```
Transaction ID	Date of Travel	Company	City	KM Travelled	Price Charged	Cost of Trip	Month	Year	Customer ID	Payment_Mode	Gender	Age	Income (USD/Month)	Population	Users
0	10000011	2016-08-01	Pink Cab	ATLANTA GA	30.45	370.95	313.6350	8	2016	29290	Card	Male	28	10813	814,885	24,701
1	10351127	2018-07-21	Yellow Cab	ATLANTA GA	26.19	598.70	317.4228	7	2018	29290	Cash	Male	28	10813	814,885	24,701
2	10412921	2018-11-23	Yellow Cab	ATLANTA GA	42.55	792.05	597.4020	11	2018	29290	Card	Male	28	10813	814,885	24,701
3	10000012	2016-06-01	Pink Cab	ATLANTA GA	28.62	358.52	334.8540	6	2016	27703	Card	Male	27	9237	814,885	24,701
4	10320494	2018-04-21	Yellow Cab	ATLANTA GA	36.38	721.10	467.1192	4	2018	27703	Card	Male	27	9237	814,885	24,701
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
359387	10307228	2018-03-03	Yellow Cab	WASHINGTON DC	38.40	668.93	525.3120	3	2018	51406	Cash	Female	29	6829	418,859	127,001
359388	10319775	2018-04-13	Yellow Cab	WASHINGTON DC	3.57	67.60	44.5536	4	2018	51406	Cash	Female	29	6829	418,859	127,001
359389	10347676	2018-06-07	Yellow Cab	WASHINGTON DC	23.46	331.97	337.8240	6	2018	51406	Card	Female	29	6829	418,859	127,001
359390	10358624	2018-02-08	Yellow Cab	WASHINGTON DC	27.60	358.23	364.3200	2	2018	51406	Cash	Female	29	6829	418,859	127,001
359391	10370709	2018-08-30	Yellow Cab	WASHINGTON DC	34.24	453.11	427.3152	8	2018	51406	Card	Female	29	6829	418,859	127,001
359392 rows × 16 columns
```
### Data Visualization
- Number of Transactions per month
#### Pink Cab
<img src="https://user-images.githubusercontent.com/101534066/196304087-fb70dd5e-60c8-4163-9206-99a0342dc2a8.png" width="70%" height="60%">
#### Yellow Cab
<img src="https://user-images.githubusercontent.com/101534066/196304508-9a12a6cc-f8a3-4d82-b443-c79a2ee66493.png" width="70%" height="60%">

#### Number of Transactions for Pink and Yellow Cab throughout the year
<img src="https://user-images.githubusercontent.com/101534066/196305469-d8430d2f-e6c1-4a90-9207-6125f13985aa.png" width="70%" height="60%">

#### Pink and Yellow Cab Users overview
<img src="https://user-images.githubusercontent.com/101534066/196304629-88a252af-bb10-412a-9629-501f8286fece.png" width="40%" height="40%">

#### Users' Preference by demographics
<img src="https://user-images.githubusercontent.com/101534066/196305565-66da8252-7fc3-4aaa-807b-0fbee92a039c.png" width="50%" height="50%">
<img src="https://user-images.githubusercontent.com/101534066/196305644-e72ada62-8fde-4f6d-9d7e-72b9e8164269.png" width="50%" height="50%">

#### Users Summary by City
<img src="https://user-images.githubusercontent.com/101534066/196305730-34cced2a-bab9-4263-91a8-5bd6dfe3b88c.png" width="70%" height="70%">

#### Comparing KM Travelled with price charged for both companies in each city.
<img src="https://user-images.githubusercontent.com/101534066/196305852-def8c6b3-a24f-41d9-bed5-ed286f96f044.png" width="70%" height="60%">
<img src="https://user-images.githubusercontent.com/101534066/196305930-78bb7d14-970c-4485-9eda-4c04c2de71e3.png" width="70%" height="60%">

#### Comparing average profit for both companies by city
<img src="https://user-images.githubusercontent.com/101534066/196306139-42378f00-a577-44cb-bb79-0d4316479377.png" width="70%" height="70%">

#### Average Profit 
<img src="https://user-images.githubusercontent.com/101534066/196306279-f0b0e28b-b95e-4180-b8dc-49cf255a7def.png" width="40%" height="60%">
<img src="https://user-images.githubusercontent.com/101534066/196306332-6dd56a03-9d7c-49c0-b5aa-f854b29239a9.png" width="40%" height="60%">

### Recommendations
- We have evaluated both the cab companies on following factors and have concluded that Yellow Cab Company is better for investment than Pink Cab Company. 
- Number of transactions: Both the companies have higher number of transactions in the months of October, November, and December. But as observed Yellow Cab has 3x more customers throughout the year. 
- Market Share: Almost 75% of the cab users prefer Yellow Cab Company over Pink Cab Company. 
- Age wise Reach: Yellow cab has higher customers in all age groups and the most popular among middle aged group. 
- Gender wise Reach: Yellow cab has significantly higher number of both male and female users. Although male cab service users are 15% higher than female cab service users. 
- Average Price charged by KM: Yellow cab average charge per KM is almost 3 times the average charge of Pink cab. 
- Average Profit: Yellow Cab company has approximately 2.5 times higher average profit than Pink Cab company. 

**Based on the data analysed, Yellow Cab company is recommended to XYZ company for investment.**
