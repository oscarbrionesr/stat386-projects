---
layout: post
title:  "Investing for the Future: Using the S&P 500 index data to find good companies to invest in"
date:   2022-10-20
author: Oscar Briones Ramirez
description: Money well invested brings peace of mind and safety. This post shows how to collect S&P 500 index data from a couple of websites
image: assets/images/invest.jpeg
---

# Introduction

Saving money is a very basic principle of personal finance everyone should follow to be safe for any unexpected events. However, investing money is even better. 

Inflation causes our money's buying power to decrease year after year, and the money we keep in our bank accounts does not grow over time, even when we have it saved up in a interest-based savings account, the annual percentage yield is usually lower that 1%, which means even though we have money saved up, we won't be able to afford the same things with it in the future.
This is why is important to, not only save money, but invest it so that its value not only holds up to inflation, but even increases, and allows us to be ready for emergencies and even retirement. There are many ways to invest money in, however, this post will talk about investing in the stock market. 

![Figure](https://github.com/oscarbrionesr/stat386-projects/raw/main/assets/images/invest2.jpeg)

## How to invest

A good rule of thumb is to invest in the S&P 500 index, which represents 500 of the biggest companies in the U.S. Investing in this index, has historically yielded returns of around 10% annually. However, it is also possible to invest in the individual companies in this index, some of which, have yielded up to 30% or even 50% annually.


## Let's look at the data

I used the S%P 500 data found in the Stock Market MBA and Liberated Stock Trader websites. In this post, I explain how I used the pandas to get the data.

The purpose of this post is to collect the data from the companies in the S&P 500, and look at indicators such market cap, number of employees, priece to earnings ratio, etc. in order to see if there are correlations that can allow us to see what individual companies would be good to invest in.

 I determined that it was allowable to get the data from these sites because the purpose of these websites is to provide financial literacy for the public so that they can be better equiped to make good financial decisioins for their future.

A later post will perform EDA to figure out if there are individual companies worth investing in.

*Complete code and data can be found in [this GitHub repo](https://github.com/oscarbrionesr/sp500-web-scraping).*

---

# Data Collection

I used Python and the Requests package to perform get requests directly to the websites.

# Step 1: Create requests from websites

1. Import the requests package:

```
import requests
```


2. Assign websites' urls to variables and perform request:

```
url = "https://www.liberatedstocktrader.com/sp-500-companies-list-by-number-of-employees/"
url2 = "https://stockmarketmba.com/stocksinthesp500.php"

r = requests.get(url).content
r2 = requests.get(url2).content
```

# Step 2: Create dataframes and clean the data

1. Create dataframes out of the information requested from the websites:

```
df = pd.read_html(r)[2]
df2 = pd.read_html(r2)[0]
```

2. Get dataframes ready to merge them in a later step:

The first dataframe needs to move the 1st row up, to become the header. The second dataframe just needs a column name to be changed in order to match the first dataframe
```
new_header = df.iloc[0]
df = df[1:]
df.columns = new_header

df2 = df2.rename(columns={'Symbol': 'Ticker'})

```

# Step 3: Merging and cleaning the data

1. Merge both dataframes with the "Ticker" column in common

```
temp_df = pd.merge(df, df2, on="Ticker")
```

2. Remove unwanted columns and rename for easy reading.
I decided I want to have the following columns for future analysis: Ticker,	Company,	Sector,	# Employees,	Market cap,	Dividend yield,	PE Ratio

```
sp500 = temp_df.drop(['Description', 'Category2', 'Category3', 'GICS Sector', 'Action', 'Price to book value', 'Price to TTM sales'], axis=1)

sp500 = sp500.rename(columns={'Company_x': 'Company', 'Sector_x': 'Sector', 'Price to TTM earnings': 'PE Ratio'})

```

# step 4: Export the data as .csv

```
sp500.to_csv('s&p500.csv')
```

Finally, here is my data:

![Figure](https://github.com/oscarbrionesr/stat386-projects/raw/main/assets/images/spdata.jpeg)


The resulting DataFrame contains 478 rows with information on publicly traded companies of interest (not all the columns are shown).


# A note on pre-generated data versus curated data

It might be useful to go to the websites and look at the data there, however, for purpuses of further analysis, I deemed it useful to collect the data from both websites and put it together.
However, it might be worth taking a look at the readily available data in the websites to see if there is already data that serves more general purposes.

---

# Conclusion

In this post I showed how to collect data from multiple websites to create a custom dataset. Particularlly, I am interested in looking at number of employees, market cap, dividend yield, PE ratios, and a few more, in order to show in future posts how to analyze and work with this data to find insights on companies worth investing in.

Let me know if you have any suggestions or questions on this data in the comments below!
