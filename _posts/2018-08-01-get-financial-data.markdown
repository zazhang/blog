# How to Get Financial Data from Web?

## Tushare

[*Tushare*](http://tushare.org/index.html) is an open source data API that mainly provide Chinese stock and macroeconomic data. The data API combines Shanghai Stock Exchange, Shenzhen Stock Exchange, Sina, and Tencent Financial News. 

To use *Tushare*, we can simply do `pip install tushare`. 

To access individual stock, one can do following,

``` python
import tushare as ts

ts.get_hist_data('600001') # 600001 is the ticker of the stock
```

More usage can be found at [here](http://tushare.org/index.html).

## Pandas-datareader

[*Pandas-datareader*](https://pandas-datareader.readthedocs.io/en/latest/) is a data API for global markets. It can be installed via `pip install pandas-datareader`. Note, in order to use *pandas-datareader*, we need to have *Pandas* installed.

*Pandas-datareader* uses different sources to get financial data, such as Google Finance, Yahoo Finance, etc.

For example, to access Apple stock via Google Finance， we can do
``` python
import pandas_datareader.data as web
import datetime

start = datetime.datetime(2010,1,1)
end = datetime.datetime(2018,1,1)
f = web.DataReader('AAPL','google',start,end)
```

Currently, Google Finance ia not stable. Other data access may be used.

## Quandl

[*Quandl*](https://www.quandl.com) provides both free and premium financial data. 

If an API is provided (an url like this: `https://www.quandl.com/api/v3/datasets/WIKI/FB/data.csv?api_key=YOURAPIKEYHERE
`, by substituting the api_key with your own API key), paste the url in a web browser, you can download the data directly from *Quandl*.


--------

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>