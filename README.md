# US_StocksAnalysis-Using-Rsqlite

Stock Grades
Setup
## recommended packages
library(dplyr)
library(tidyr)
library(ggplot2)
library(gridExtra)
library(knitr)
library(scales)
Connect to the us_stocks database using RSQLite.

Query the fundamentals stocks for common stocks with recent price history. Save as output variable as ‘stocks’.

Start with provided query and add applicable joins
JOIN to the tickers to table t for common stock. Use the like function to filter for ticker name with match lower(t.name) ‘%common stock%’.
JOIN to the temporary table p on symbol for stocks with a recent price history.
Notice the where clause is Download_Date IN (‘2021-03-17’,‘2020-03-09’) for most recent fundamentals and last year fundamentals
CREATE STORED VARIABLES FOR QUESTION 1
with p as (
select distinct symbol
FROM prices
WHERE prices.begins_at = '2020-04-01'
)

select f.Download_Date, f.symbol, f.pe_ratio, f.dividend_yield, f.market_cap
from fundamentals f
/*
JOIN TO ADDITIONAL TABLES HERE
*/
WHERE f.Download_Date IN ('2021-03-17','2020-03-09')
Create a variable called “grade” to represent a composite score of the percentiles from 3 variables including price earning ratio (25% weight), dividend yield (25% weight) and year over year percent change in market capitization (50% weight). Print a kable of the top 5 stocks by grade with columns symbol, 3 composite variables, 3 percent rank variables and the grade.
Hints to create compsite score
Clean price earning ratio
CREATE the first histogram, p1, of price earning for question 2
Assign all pe_ratio > 150 to 150 to set all high pe ratios to the same low percentile
Assign all missing (NA) pe_ratio to 200 to set all companies with negative earnings to same even lower percentile
CREATE the second histogram, p2, of price earning for question 2
CREATE the side by side plots with grid.arrange function for question 2
Clean dividend yeild
Assign all missing (NA) dividend_yield to 0
Clean up market capitization for percent change
Convert Download_Date as date
Create a variable previous_market_cap. Recommend using dplyr function lag() after arranging by download date and grouping by symbol
Filter for stocks with available values for market capitization and previous_market_cap, so no missing market capitization values. For example, !is.na(market_cap) & !is.na(previous_market_cap)
Create a variable market_cap_percent_change with (market_cap - previous_market_cap)/previous_market_cap
Filter stocks for Download_Date == as.Date(‘2021-03-17’)
Create the grade composite score with percentiles for the 3 variable use dplyr percent_rank function
Before using rank function recommend ungroup(stocks) or convert back to regular data frame with data.frame(stocks)
Create Rank1 as 1 - percent rank for the pe_ratio since lower is better
Create Rank2 as percent rank for dividend yeild since high is better
Create Rank3 as percent rank for market_cap_percent_change assuming higher is better
Create Grade as .25Rank1 + .25Rank2 + .5*Rank3
Arrange by desc(Grade) and prink kable of the top 5 grades with the symbol, pe_ratio, dividend_yield, market_cap_percent_change, Rank1, Rank2, Rank3, Grade selected
Questions
Answer in complete sentences with solutions created with inline code.

How many stocks (unique symbols) were available from 2020-03-09 compored to 2021-03-17? Please indicate the total stocks from both dates along with the difference.

How much data was cleaned up for the pe_ratio? Please indicate the total amound of values adjusted to 150 and the total amount of missing values adjusted to 200. Show the impact of data cleaning with side by side histograms.

Based on the top 100 stocks by grade composite, what is the average market capitization increase?

Based on the top 100 stocks by grade composite, what stock would you pick? Why? Based choice one of the 3 variables such as max dividend yield or min price earning ratio.
