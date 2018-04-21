---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the CoinDCX API!
<aside class="notice">The base URL for all the API calls is `https://api.coindcx.com` </aside>

# Public endpoints

## Ticker
### HTTP Request
`GET /exchange/ticker`

> Response

```json
[
  {
    "market": "REQBTC",
    "change_24_hour": "-1.621",
    "high": "0.00002799",
    "low": "0.00002626",
    "last_price": "0.00002663",
    "bid": "0.00002663",
    "ask": "0.00002669",
    "timestamp": 1524211224
  }
]
```

### Definitions

<ul>
  <li>bid - Highest bid offer in the orderbook</li>
  <li>ask - Highest ask offer in the orderbook</li>
  <li>high - 24 hour high</li>
  <li>low - 24 hour low</li>
  <li>timestamp - Time when this ticker was generated</li>
</ul>

<aside>A ticker is generated once every second</aside>


## Markets
### HTTP Request
`GET /exchange/v1/markets`

> Respose:

```json
[
  "SNTBTC",
  "TRXBTC",
  "TRXETH"
  .
  .
]
```

Returns an array of strings of currently active markets.


## Markets details
### HTTP Request

`GET /exchange/v1/markets_details`

> Response:

```json
[
  {
    "coindcx_name":               "SNTBTC",
    "base_currency_short_name":   "BTC",
    "target_currency_short_name": "SNT",
    "target_currency_name":       "Status",
    "base_currency_name":         "Bitcoin",
    "min_quantity":               1,
    "max_quantity":               1000000,
    "min_notional":               0.001,
    "base_currency_precision":    8,
    "target_currency_precision":  0,
    "step":                       1,
    "order_types":                ["market_order", "limit_order"]
  }
]
```
       

### Definitions

<ul>
  <li>min_quantity - It is the minimum quantity of target currency (SNT) for which an order may be placed</li>
  <li>max_quantity - It is the maximum quantity of target currency (SNT) for which an order may be placed</li>
  <li>min_notional - It is the minimum amount of base currency (BTC) for which an order may be placed</li>
  <li>base_currency_precision - Number of decimals accepted for the base currency</li>
  <li>target_currency_precision - Number of decimals accepted for the target currency</li>
  <li>step - It is the minimum increament accepted for the target currency</li>
</ul>


## Trades
### HTTP request
`GET /exchange/v1/trades/:market`

> Response

```json
[
  {
    "p":  0.00001693,
    "q":  394,
    "T":  1521476030955.09,
    "m":  false
  }
}
```

### Path parameters
Name           |Required| Example     
---------------|---- |---------------
market         | Yes |  SNTBTC 

This API provides with a sorted list of most recent 30 trades.
### Definitions
<ul>
  <li>m stands for whether the trade is market maker or not.</li>
  <li>p is the trade price</li>
  <li>q is the quantity</li>
  <li>T is the timestamp of trade</li>
</ul>


## Order book
### HTTP request
`GET /exchange/v1/books/:market`

### Path parameters
Name           |Required| Example     
---------------|---- |---------------
market         | Yes |  SNTBTC 

```json
{  
  "asks":{
    "0.00160900":"23.79000000",
    "0.00161000":"300.85",
    "0.00161400":"11.25",
    "0.00161500":"101.82",
    "0.00161700":"222.37000000"
    ,
    ,
    ,
  },
  "bids":{  
    "0.00160400":"24.24000000",
    "0.00160300":"7.63",
    "0.00160100":"917.51000000",
    "0.00159900":"40.8",
    "0.00159700":"6"
    ,
    ,
    ,
  }
```


<aside class="warning">This end point returns unsorted objects of bids and asks. They must be sorted locally at your end</aside>



### 


# Authentication

<aside class="warning">All the Authenticated API calls use POST method. Parameters are to be passed as JSON in the request body</aside>

> To authorize, use this code:

```ruby
  require 'net/http'
  require 'uri'
  require 'json'
  require 'openssl'



  secret = "Your secret key"
  key = "Your API key"


  payload = {
    "side" : "buy",
    "order_type" : "limit_order",
    "price_per_unit": 0.00001724,
    "market" : "SNTBTC",
    "total_quantity" : 100
  }.to_json

  signature = OpenSSL::HMAC.hexdigest(OpenSSL::Digest.new('sha256'), secret, payload)


  headers = {
    'Content-Type' => 'application/json',
    'X-AUTH-APIKEY' => key, 
    'X-AUTH-SIGNATURE' => signature
  }

  uri = URI.parse("https://api.coindcx.com/exchange/v1/orders/create")

  https = Net::HTTP.new(uri.host, uri.port)
  https.use_ssl = true
  request = Net::HTTP::Post.new(uri.path, headers)

  request.body = payload

  response = https.request(request)
```

```python
  import hmac
  import hashlib
  import base64
  import json

  secret = "Your secret key"
  key = "Your API key"

  body = {
    "side" : "buy",
    "order_type" : "limit_order",
    "price_per_unit": 0.00001724,
    "market" : "SNTBTC",
    "total_quantity" : 100
  }

  json_body = json.dumps(body, separators=(',', ':'))

  signature = hmac.new(secret, json_body, hashlib.sha256).hexdigest()

  url = "https://api.coindcx.com/exchange/v1/orders/create"

  headers = {
    'Content-Type': 'application/json', 
    'X-AUTH-APIKEY': key, 
    'X-AUTH-SIGNATURE': signature
  }

  response = requests.post(url, data=json_body, headers=headers)

```

```shell

```

```javascript
const crypto = require('crypto')
const request = require('request')
key = "Your api key"
secret = "Your secret key"
body = {
  "side" : "buy",
  "order_type" : "limit_order",
  "price_per_unit": 0.00001724,
  "market" : "SNTBTC",
  "total_quantity" : 100
}

const payload = new Buffer(JSON.stringify(body)).toString();

const signature = crypto
  .createHmac('sha256', secret)
  .update(payload)
  .digest('hex')

const options = {
  url: "https://api.coindcx.com/exchange/v1/orders/create",
  headers: {
    'X-AUTH-APIKEY': key,
    'X-AUTH-SIGNATURE': signature
  },
  json: true,
  body: body
}

request.post(
  options,
  function(error, response, body) {
    console.log('body:', body)
  }
)
```

> Make sure to replace API key and API secret with your own.

The authentication procedure is as follows:
<ul>
  <li>The payload is the parameters object, JSON encoded</li>
`payload = parameters-object -> JSON encode`
  <br><br>
  <li>The signature is the hex digest of an HMAC-SHA256 hash where the message is your payload, and the secret key is your API secret.</li>
`signature = HMAC-SHA256(payload, api-secret).digest('hex')`
</ul>
<br>
 <p>After this, You will have to add following headers into all the authenticated requests</p>

Header Name | Value
--------- | ------- 
X-AUTH-APIKEY| your-api-key
X-AUTH-SIGNATURE| signature

<aside class="notice">
You must replace <code>your-api-key</code> and <code>signature</code> with your personal API key and generated signature respectively.
</aside>

# User

## Get balances

```ruby

```

```python

```

```shell

```

```javascript

```

> Response:

```json
[
  {
    "currency": "BTC",
    "balance": 1.167,
    "locked_balance": 2.1
  }
]
```

> Locked balance is the balance currently being used by an open order

This endpoint retrieves account's balances.

### HTTP Request

`POST /exchange/v1/users/balances`

<!-- ### Query Parameters -->

<!-- <aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->

<!-- ## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete -->

# Order
Enum definitions for the purpose of order are as follows:

Name      |   Values
----------|------------
side      |   buy, sell
order_type|   market_order, limit_order



## New order

```ruby

```

```python

```

```shell

```

```javascript

```

> Response:

```json
{  
   "orders":[  
     {  
        "id":"ead19992-43fd-11e8-b027-bb815bcb14ed",
        "market":"TRXETH",
        "order_type":"limit_order",
        "side":"buy",
        "status":"open",
        "fee_amount":0.0000008,
        "fee":0.1,
        "total_quantity":2,
        "remaining_quantity":2.0,
        "avg_price":0.0,
        "price_per_unit":0.00001567,
        "created_at":"2018-04-19T18:17:28.022Z",
        "updated_at":"2018-04-19T18:17:28.022Z"
     }
   ]
}
```


Use this endpoint to place a new order on the exchange

### HTTP Request

`POST /exchange/v1/orders/create`

### Parameters

Name           |Required| Example    | Description
---------------|---- |---------------|-------
market         | Yes | ETHBTC        | The trading pair
total_quantity | Yes | 1.101         | Quantity to trade 
price_per_unit | No  | 0.082         | Price per unit (not required for market order)
side           | Yes | buy           | Specify buy or sell
order_type     | Yes | market_order  | Order Type


##  Order status
```ruby

```

```python

```

```shell

```

```javascript

```

> Response:

```json 
{  
  "id":"ead19992-43fd-11e8-b027-bb815bcb14ed",
  "market":"TRXETH",
  "order_type":"limit_order",
  "side":"buy",
  "status":"open",
  "fee_amount":0.0000008,
  "fee":0.1,
  "total_quantity":2,
  "remaining_quantity":2.0,
  "avg_price":0.0,
  "price_per_unit":0.00001567,
  "created_at":"2018-04-19T18:17:28.022Z",
  "updated_at":"2018-04-19T18:17:28.022Z"
}
```


Use this endpoint to fetch status of any order

### HTTP Request

`POST /exchange/v1/orders/status`

### Parameters

Name           |Required| Example    | Description
---------------|---- |---------------|-------
id             | Yes |  ead19992-43fd-11e8-b027-bb815bcb14ed       | The ID of the order


##  Active orders
```ruby

```

```python

```

```shell

```

```javascript

```

> Response:

```json 
[
  {  
    "id":"ead19992-43fd-11e8-b027-bb815bcb14ed",
    "market":"TRXETH",
    "order_type":"limit_order",
    "side":"buy",
    "status":"open",
    "fee_amount":0.0000008,
    "fee":0.1,
    "total_quantity":2,
    "remaining_quantity":2.0,
    "avg_price":0.0,
    "price_per_unit":0.00001567,
    "created_at":"2018-04-19T18:17:28.022Z",
    "updated_at":"2018-04-19T18:17:28.022Z"
  }
]
```


Use this endpoint to fetch active orders

### HTTP Request

`POST /exchange/v1/orders/active_orders`

### Parameters

Name           |Required| Example    | Description
---------------|---- |---------------|-------
market         | Yes  |    SNTBTC     |
side           | No  |    buy        |


##  Active orders count
```ruby

```

```python

```

```shell

```

```javascript

```

> Response:

```json 
 { count: 1, status: 200 }
```


Use this endpoint to fetch active orders count

### HTTP Request

`POST /exchange/v1/orders/active_orders_count`

### Parameters

Name           |Required| Example    | Description
---------------|---- |---------------|-------
market         | Yes  |    SNTBTC     |
side           | No  |    buy        |



##  Cancel all
```ruby

```

```python

```

```shell

```

```javascript

```

> Response:

```json 

```


Use this endpoint to cancel multiple active orders in a single API call

### HTTP Request

`POST /exchange/v1/orders/cancel_all`

### Parameters

Name           |Required| Example    | Description
---------------|---- |---------------|-------
market         | Yes  |    SNTBTC     |
side           | No  |    buy        |

Sending side param is optional. You may cancel all the sell orders of SNTBTC by sending
<br>
<code>{market: "SNTBTC", side  : "sell"}</code>


Or you may cancel all your orders in SNTBTC market by sending
<br>
<code>{market: "SNTBTC"}</code>



##  Cancel
```ruby

```

```python

```

```shell

```

```javascript

```

> Response:

```json 

```


Use this endpoint to cancel an active orders

### HTTP Request

`POST /exchange/v1/orders/cancel`

### Parameters

Name           |Required| Example    | Description
---------------|---- |---------------|-------
id             | Yes |  ead19992-43fd-11e8-b027-bb815bcb14ed       | The ID of the order


# API call limits
We have rate limits in place to facilitate availability of our resources to a wider set of people. Typically you can place around 4 orders per second. The exact number depends on the server load.
In aggregate, you may call `https//api.coindcx.com` not more than 10 times per second.

