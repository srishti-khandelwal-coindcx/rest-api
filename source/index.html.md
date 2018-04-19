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

<aside class="warning">All the API calls use POST method. Parameters are to be passed as JSON in the request body</aside>

# Authentication

> To authorize, use this code:

```ruby

```

```python

```

```shell

```

```javascript

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

`POST /exchange/users/balances`

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

## New order
Use this endpoint to place a new order on the exchange

### HTTP Request

`POST /exchange/orders/create`

### Parameters

Parameter Name | Required | Value | Description
--------- | ------- 



