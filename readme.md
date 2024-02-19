# Machine Learning

- [Machine Learning](#machine-learning)
    - [Reading and Transforming data](#reading-and-transforming-data)
      - [How to read a csv file in to a dataframe?](#how-to-read-a-csv-file-in-to-a-dataframe)
      - [How to print df content, data types and find missing values?](#how-to-print-df-content-data-types-and-find-missing-values)
      - [How to drop df columns and fill default values for missing fields](#how-to-drop-df-columns-and-fill-default-values-for-missing-fields)
      - [How to split data for training and testing?](#how-to-split-data-for-training-and-testing)
      - [How to find outliers in data](#how-to-find-outliers-in-data)
      - [How to deal with the outliers?](#how-to-deal-with-the-outliers)


### Reading and Transforming data

#### How to read a csv file in to a dataframe?
```
import pandas as pd
df = pd.read_csv("AB_NYC_2019.csv")
```

#### How to print df content, data types and find missing values?
```
df.head() # show top few rows
df.info() # show df types

missing_values = df.isnull().sum()
```

#### How to drop df columns and fill default values for missing fields
```
colsToDrop = ["id", "host_name", "last_review", "reviews_per_month"]
df.drop(colsToDrop, axis=1)

df['price'].fillna(df['price'].mean()) # fill the missing price values with the mean price value
```

#### How to split data for training and testing?
```
features = df.drop('price', axis=1).values # x-axis
prices = df['price'].values # y-axis

# 20% testing, 80% training
x_train, x_test, y_train, y_test = train_test_split(features, prices, test_size=0.2, random_state=42)
```

#### How to find outliers in data
```
import matplotlib.pyplot as plt
import seaborn as sns

sns.boxplot(x="price", data=df)
```

#### How to deal with the outliers?

1. Use IQR (Interquartile Range).
   1. Sort the data in ascending order.
   2. Find the median.
   3. Find the Q1 and Q3 i.e. median on the left of the median and the right of the median respectively.
   4. IQR will be equal to Q3 - Q1.
   5. Any number below `Q1 - (1.5 * IQR) or above Q3 + (1.5 * IQR)` will be considered an outlier. **Note: We multiply by 1.5 because of Tukey's fences approach**
    ```
    quartile1 = df["price"].quantile(0.25)
    quartile3 = df["price"].quantile(0.75)
    IQR = quartile3 - quartile1
    
    cleaned_data = df[(df["price"] > Q1 - 1.5 * IQR) & (df["price"] < Q3 + 1.5 * IQR)]
    import seaborn as sns
    sns.boxplot(x="price", data=cleaned_data)
    ```