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

## Tushare Pro

*Tushare Pro*是 *Tushare* 最新的升级版，提供更快更稳定的数据支持。当前， *Tushare Pro*正处在数据转移中，很多数据暂时无法提供。

*Tushare Pro*需要用户注册并利用token接入其数据库。数据的使用免费，但有权限限制，权限通过参与活动等方式获得积分提升。

当前，*Tushare Pro*不支持分钟级数据提取。（20181013）

数据调用如下：
```python
import tushare as ts
ts.set_token('your token')

pro = ts.pro_api() # initiate the api

# alternatively, you can do
pro = ts.pro_api('your token')

# to get daily price info
df = get_price(ts_code='600000.SH', start_date='20181009', end_date='20181010')
```

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

## Wind

*Wind*数据提取有流量限制，达到限制后7*24小时后恢复。期间可以使用其他未达到流量的方法（e.g. 数据浏览器达到限制，可使用API或者 *Excel* 插件）或者其他人账号。

*Wind*读取数据（e.g.读取多只股票多日交易情况）可能失败，因为单个query要求的数据量太大，无法通过网络传输，可以拆分成多个query来实现，例如一只股票一只股票的读取。

### Wind应用
*Wind* Windows版应用提供很强大的数据提取功能，可以直接在应用中查询数据并提取成 *Excel*。其中几个重要的单元如下：

#### 数据浏览器

数据浏览器提供各类指标及标的的选择，通过指定提取的时间片段可以取得所需数据。

#### 行情序列

行情序列支持单一股票的逐日行情和分钟行情。其中分钟行情仅支持近3年，如需更多请联系Wind管理员购买。

### Wind Excel插件

*Wind Excel*插件将以上功能整合进 *Excel*。但是单次query的量不可太大，过大的数据量会导致 *Excel* 停止响应。

### Wind Python API

*Wind Python* API需要通过 *Wind* App安装。安装后可以直接通过脚本语言提取数据，具有方便快捷的特点。提取数据时的语句可以通过 *Wind 代码提取器*(WindNavigator.exe)获得。

## JoinQuant

*JoinQuant*提供免费1年的数据试用，*JQData*。*JQData*提供2005年至今的各类数据包括分钟级数据。

## Level 2

各大数据服务商均付费提供Level2交易数据。Level 2数据为公开数据，包括quotes，trades，stock info，order，transaction等信息。


--------

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>