---
title: AgraTek Switching API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - python
  - php

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to AgraTek Switching API Reference.

We have language bindings in Python, and PHP! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

System specifications:

1. The format used is ```json-rpc 2.0```
2. Access method with ```post``` request
3. ```method``` is a method that will be accessed

* Sandbox: [http://dev.agratek.id/api/merchant](http://dev.agratek.id/api/merchant)
* Production: [http://agratek.id/api/merchant](http://agratek.id/api/merchant)

# Authentication

> use following code to generate your signature:

```php
<?php
function json_rpc_header($userid, $password){
  date_default_timezone_set('UTC');
  $inttime = strval(time()-strtotime('1970-01-01 00:00:00'));
  $value = $userid . $inttime;
  $key = $password;
  $signature = hash_hmac('sha256', $value, $key, true);
  $signature64 = base64_encode($signature);
  headers = {
    'userid':$userid,
    'signature':$ignature64,
    'key':$inttime
  }
  return headers
}
?>
```

```python
def json_rpc_header(userid, password):
  utc_date = datetime.utcnow()
  start_date = datetime.strptime('19700101 000000', '%Y%m%d %H%M%S')
  tot_seconds = int((utc_date-start_date).total_seconds())
  value = "%s&%s" % (str(userid),str(tot_seconds))
  key = str(password)
  signature = hmac.new(key, msg=value, digestmod=hashlib.sha256).digest()
  signature64 = base64.encodestring(signature).replace('\n', '')
  headers = {
    'userid':userid,
    'signature':signature64,
    'key':inttime
    }
  return headers
```

Every HTTP POST request must contain following HTTP Header:

header | sample data
----- | -----------
```userid``` | 000121
```signature``` | wtl7nDjOGWqVFS/oESUbEoOyQWyJItLIFWMZvASYpmQ=
```key``` | 1424087283

```userid``` is Merchant's user ID.

```signature``` is a combination of merchant's ```userid``` and ```password```, encrypted using **HMAC-256** encryption. This is to improve security.

```key``` is current timestamp in UNIX format.

<aside class="notice">
You must generate your signature to be include in your request header. On the right side is example script to generate a signature.
</aside>

# IP Whitelist

Any incoming request to AgraTek will be validated by IP address, AgraTek can accept one or more IP address to be whitelisted.

# Send Your Request

> send your request in json format:

```json
{
  "jsonrpc": "2.0",
  "method": "set_antrian",
  "params": data,
  "id":1
}
```

All request must be send in ```http post``` using ```json-rpc 2.0``` format.


## Header

> header in json format:

```json
headers = {
  "userid": "admin",
  "key": 1442495331,
  "signature": "ix/9W9AV38NbOBxPMxMRUl8moiYmnC1nUlbou0WmrZ8="
}
```

Sample of http post request header for:

* userid      = admin
* password    = admin
* timestamp   = '1442495331'
* signature64 = 'ix/9W9AV38NbOBxPMxMRUl8moiYmnC1nUlbou0WmrZ8='


## Data

```python
record        = [
    {'kode':'001'},
    {'kode':'002'}
  ]
data          = {"data": record}
headers       = json_rpc_header('admin','$2a$10$EjDrW6Fk0g5')
requests      = {
  "jsonrpc": "2.0",
  "method": "set_antrian",
  "params": data,
  "id":1
}
jsondata      = json.dumps(data, ensure_ascii=False)
results       = requests.post(rpc_url, data=jsondata, headers=headers)
dict_results  = json.loads(rows.text)
```

```php
<?php
?>
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc":"2.0",
  "method":"get_todo_detail",
  "params":{
    "token":"1234567890123457",
    "data":[{"kode": "001"}]
  },
  "id": 19
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
ID | The ID of the kitten to delete

