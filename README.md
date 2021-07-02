# Overnight vs Intraday Blog

All of our analysis and results can be found in the blog on our [website](https://extremistanresearch.com). In this README, we will broadly explain our code, as   well as our thought process in our methodology. 
  
For this blog, we got all our data from Yahoo Finance using the yfinance library for python.


## Program Methodology

After importing the necessary libraries and defining some lists that would be used later, we imported our data from Yahoo Finance.
```
data = yf.download("IJH", start="1993-01-01", end="2021-05-10", interval = "1d")
```
After that, we created a list for the close and open price from the data.
```
# Create a list for the close price
for row in data["Close"]:
    if row != "":
        close_price.append(float(row))

# Create a list for the open price
for row in data["Open"]:
    if row != "":
        open_price.append(float(row))
```
We then looped through these lists, calculating the difference both overnight and intraday for each day in our data. To find intraday, we simply used i+1 for both the close and the open. For overnight we used i+1 for the open and i for the close since we want yesterday's close price with today's open.
```
# Calculate the intraday and overnight difference for each day on the list
for i in range(len(close_price) - 1):
    dif_intra.append(close_price[i + 1] / open_price[i + 1])
    dif_overnight.append(open_price[i + 1] / close_price[i])
```
After this comes the most important part of the program. Instead of just graphing the daily change, we want to graph the cumulative increase factor. To do this, we have to take the second day's change, and multiply it by the first day's change. Then we multiply the third days change by the value we got from the first and second. We continue in this fashion until we get to the last day, which is the last day's change multiplied by every orevious day's change. Here is the code for that:
```
# Give variables starting values
dif_intra_total = 1
dif_intra_list = []
# Loop the each item in the intraday list
for item in dif_intra:
    # Multiply the current value to the previos value
    dif_intra_total = dif_intra_total * item
    # Add this to the new intraday list
    dif_intra_list.append(dif_intra_total)
```
The process is the exact same for the overnight data, so I will not show the code for that

After that, all we had to do was graph our data and save it.
  




