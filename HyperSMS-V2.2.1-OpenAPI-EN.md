# HyperSMS V2.2.1 OpenAPI documentation

## 0 The Version change notes

### v2.0.0
1. Support Login API
2. Provide static data description

### v2.1.2
1. Provide SMS related OpenAPI documentation

### v2.2.1
1. Add 0 version change notes
2. Provide product item API
3. Delete "Appendix C" 

## 1 Overview

### 1.1 Terminology introduction
Noun | Description | Remarks
:----: |:---- |:---
app | Application access side | HyperSMS OpenAPI application access side
appId | Application ID | Provided after account opening
appKey | Application Key | Provided after account opening
appSecret | Application Password | Provided after account opening
token | server token | Generated after calling the login API
callbackApproveUrl |Receipt address of approve status | Provided by the access party before opening an account
callbackSendUrl | Sending status receipt address | Provided by the accessing party before opening an account


### 1.1 Account opening process
 
1. The access party provides two return receipt addresses, callbackApproveUrl/callbackSendUrl

2. Apply for account opening in the HyperSMS platform, and generate appId/appKey/appSecret and provide it to the accessing party


### 1.2 API call process
1. The application access party calls the login API to obtain the token through appId/appSecret

2. By specifying token in the header; then call other APIs as needed

3. The sign value needs to be specified when calling in each business API, and the calculation process is detailed in "Appendix G"

4. If the business data needs to be approved, the approve status will be callback through the "callbackApproveUrl" address

5. After the sending task is processed, the DR and Retrieve status will be callback through the "callbackSendUrl" address

### 1.3 Unified access entrance:

- Test-ENV: http://hyperfast.vn-vtt-test.corporfountain.com/hyper_fast/
- Prod-ENV: https://openapi.viettel.hypersms.vn/v2/

Note: The above address may change, please confirm with the service party!

## 2 Standards and specifications

### 2.1 Communication protocol

HTTPS/HTTP

### 2.2 Request method

All API use the POST[raw] method uniformly, and the data format is uniformly JSON

### 2.3 Character encoding
Unified to UTF-8 character set encoding

### 2.4 API format standard

#### 2.4.1 Headers standard format
``` JSON
{
 "Content-Type":"application/json",
 "appId": 0101010,
 "timestamp": 1621503576000,
 "token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxMDAwMDE2ODMiLCJpYXQiOjE2MjE0OT"
}
```
Description:

Parameters |Required or not|Description
:----: |:---- |:---
Content-Type |Optional |Request type, currently only supports application/json
appId |Required |Access party application ID
token |Required |Token after login, empty string if not logged in
timestamp |Required | Current UNIX timestamp (milliseconds)

#### 2.4.2 Request Body Standard Format
``` JSON
{
 "key1": "Value1",
 "key2": "Value2",
 "timestamp": 1621503576000,
 "sign": "600e5d07f445be432d86d9271fb1df9b1"
}
```
Description:

Parameter |Required or not |Description
:----: |:---- |:---
< key1 > | | Refer to the actual API definition format requirements
sign |Required | Sign value of this request
timestamp |Required | Current UNIX timestamp (milliseconds)

#### 2.4.3 Response Body standard format
``` JSON
{
 "code": "100",
 "msg": "api.response.code.success"
 "msgArgs": null,
 "data": {
    "code": "123123123"
  }
}
```
Description:

Parameter |Required or not |Description
:----: |:---- |:---
code |Required |Response status
msg |Required |Response message;
msgArgs | Optional | Response message extended description;
data |Required |Response return value (Note: different API return value types are different);


## 3. Authentication related API

### 3.1 Login API
- **API description:** The access party obtains the global token through the login API
- **API address:**  app/login

#### 3.1.1 Request parameters
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;appId |string | Required | Access party application ID
&emsp;appSecret |string | Required | Access party password


Example request:

``` JSON
{
  "appId":"18520322032",
  "appSecret":"acb000000"
}

```


#### 3.1.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;expire |long |Required |Expiration time (in seconds)
&emsp;token |string |Required |Access token

Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "expire": 604800,
    "Token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJzdWIiOiIxMDAwMDE2ODMiLCJpYXQiOjE2MjE1NjQ0OTAsImV4cCI6MTYyMjE2OTI5MH0.O1sqK5jk-wI2DQNOsunZ5cU0x9369cTKxItmfPRHA57Kk5ynhSoWnV5YyysX65l4LqsUKITEBUWnZ78h-UdrJA"
  }
}
```

### 3.2 Product item API (Supported by v2.2.1)

- **API description:** Get all the product items under the login account.
- **API address:** app/product/item/query

#### 3.2.1 Request parameters
Parameter name					|Type		|Required or not |Description 
:----					|:---		|:------ |:---
&emsp;code				|string		| Optional	| Product item code
&emsp;name			    |string		| Optional | Product item name
&emsp;type              |int        | Optional | Product type; see "Appendix B" for details
&emsp;currPage          |int | Required | Current page number, default value is 1
&emsp;pageSize          |int | Required | The number of items per page, the default value is 10
&emsp;sign              |string | Required | Signature See "Appendix G" for details
&emsp;timestamp         |long |Required |Current UNIX timestamp (milliseconds)


请求示例：

``` JSON
{
  "code":"group_SMS_1",
  "name":"",
  "type":1,
  "currPage": 1,
  "pageSize": 1,
  "timestamp":1621503576000,
  "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```


#### 3.2.2 返回结果

Parameter name				|Type		|Required or not |Description 
:----						|:---		|:------	|:---	
code |int |Required |Response code See: "Appendix A"
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;totalCount |int |Required |Total number
&emsp;totalPage |int |Required |Total Pages
&emsp;list |array |Optional |Query results
&emsp;&emsp;code				    |string		|Required		| Product item code
&emsp;&emsp;name				|string		| Required	| Product item name
&emsp;&emsp;type			|int		| Required | Product type; see "Appendix B" for details

示例：

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "totalCount": 5,
    "totalPage": 2,
    "currPage": 1,
    "pageSize": 10,
    "list": [
       {
          "code":"group_SMS_1",
          "name":"",
          "type":1,
       }
    ]
  }
}
```


## 4. Brandname related API

### 4.1 New brand API
- **API description:** New brand information
- **API address:** app/brandname/add

#### 4.1.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;brandName |string | Required | Brand name
&emsp;description |string | Optional | Description
&emsp;certified |string | Optional | Certificate URL


Example request:

``` JSON
{
  "brandName":"Google",
  "description":" Description ",
  "certified":"http://www.google.cn/aaaqeqeqw.gif",
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 4.1.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Brand code


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "code": "1212300"
  }
}
```

### 4.2 Paging query brand API
- **API description:** Query brand information, support paging
- **API address:** app/brandname/query

#### 4.2.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;brandName |string | Optional | Brand name
&emsp;code |string | Optional | Brand code
&emsp;currPage |int | Required | Current page number, default value is 1
&emsp;pageSize |int | Required | The number of items per page, the default value is 10


Example request:

``` JSON
{
 "brandName":"",
 "code":"",
 "currPage": 1,
 "pageSize": 1,
 "timestamp": 1621503576000,
 "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 4.2.2 Return results

Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
code |int |Required |Response code See: "Appendix A"
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;totalCount |int |Required |Total number
&emsp;totalPage |int |Required |Total Pages
&emsp;list |array |Optional |Query results
&emsp;&emsp;code |string |Required |Brand code
&emsp;&emsp;brandName |string | Required | Brand name
&emsp;&emsp;certified |string | Optional | Certificate URL
&emsp;&emsp;createTime |long | Optional | Create timestamp (milliseconds)
&emsp;&emsp;status |int | Required | Data status See: "Appendix D"


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "totalCount": 100,
    "totalPage": 2,
    "currPage": 1,
    "pageSize": 1,
    "list": [
       {
          "brandName":"bName",
          "code":"XXX13123",
          "brandName": "http://www.google.cn/aaaqeqeqw.gif",
          "createTime": 1621503576000,
          "status": 1
       }
    ]
  }
}
```

### 4.3 Query Brand Details API
- **API description:** Query brand details, including the latest approve information, etc.
- **API address:** app/brandname/info

#### 4.3.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string | Required | Brand code


Example request:

``` JSON
{
   "code":"001aaab",
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 4.3.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Brand code
&emsp;brandName |string | Required | Brand name
&emsp;certified |string | Optional | Certificate URL
&emsp;createTime |long | Optional | Create timestamp (milliseconds)
&emsp;status |int | Required | Data status See "Appendix D" for details
&emsp;approve |object | Optional | Approve information
&emsp;&emsp;status |int | Optional | Approve status See "Appendix E" for details
&emsp;&emsp;opinion |string | Optional | Approve opinion


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
      "brandName":"bName",
      "code":"XXX13123",
      "brandName": "http://www.google.cn/aaaqeqeqw.gif",
      "createTime": 1621503576000,
      "status": 1,
      "approve": {
         "status": 0,
         "opinion": "pass"
      }
  }
}
```

### 4.4 Brand invalidation API
- **API description:** Collocation of brand data offline is invalid
- **API address:** app/brandname/invalid

#### 4.4.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string | Required | Brand code


Example request:

``` JSON
{
  "code":"001aaab",
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 4.4.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": null
}

```



## 5. Template related API

### 5.1 New template API
- **API description:** New template information
- **API address:** app/template/add

#### 5.1.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;type |int | Required |Product type; see "Appendix B" for details
&emsp;brandNameCode |string | Required | Brand code
&emsp;productItemCode |string | Required | The belonging product item; see "3.2" for details
&emsp;title |string | Required | Template title
&emsp;content |string | Required | SMS type is normal text,HyperSMS type is a collection of multiple materials.(Supports up to 5 dynamic parameters)
&emsp;&emsp;type			|string		| Optional | Material type (text/image/video/audio). HyperSMS type is required.
&emsp;&emsp;content			|string		| Optional | Material value. HyperSMS type is required.



Request example:

- **SMS Normal Template**

``` JSON
{
  "type":2,
  "brandNameCode":"001aaab",
  "productItemCode":"SMS_AD_GROUP_1",
  "title":"temp title",
  "content":"Hi, My name is HAHA",
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

- **SMS Dynamic parameter template**

``` JSON
{
  "type":2,
  "brandNameCode":"001aaab",
  "productItemCode":"SMS_AD_GROUP_1",
  "title":"temp title",
  "content":"Hi, My name is {0}",
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

- **HyperSMS Normal Template** 

``` JSON
{
  "type":1,
  "brandNameCode":"001aaab",
  "productItemCode":"SMS_AD_GROUP_1",
  "title":"temp title",
  "content":"[{\"type\": \"text\", \"content\": \"My name is HAHA, I am 18 years old\"}, {\"type\": \"image\", \"content\": \"http://hypersms-th.cdn.corporfountain.com/sms_agent_temp/1/5f617a1dd8596.png\"}, {\"type\": \"video\", \"content\": \"http://hypersms-th.cdn.corporfountain.com/sms_agent_temp/100001555/5fc493029991c.mp4\"}, {\"type\": \"audio\", \"content\": \"http://hypersms-th.cdn.corporfountain.com/sms_agent_temp/100001555/5fc7396abcf04.mp3\"}]",
  "timestamp":1621503576000,
  "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

- **HyperSMS Dynamic parameter template** 

``` JSON
{
  "type":2,
  "brandNameCode":"001aaab",
  "productItemCode":"SMS_AD_GROUP_1",
  "title":"temp title",
  "content":"[{\"type\": \"text\", \"content\": \"My name is {0}\"}, {\"type\": \"image\", \"content\": \"{1}\"}, {\"type\": \"video\", \"content\": \"{2}\"}, {\"type\": \"audio\", \"content\": \"{3}\"}]",
  "timestamp":1621503576000,
  "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 5.1.2 Return result

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Template code


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "code": "1212300"
  }
}
```


### 5.2 Edit template API
- **API description:** Edit template information
- **API address:** app/template/edit

#### 5.2.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string |Required |Template code
&emsp;type |int | Optional |Product type; see "Appendix B" for details
&emsp;brandNameCode |string | Optional | Brand code
&emsp;productItemCode |string | Optional | The belonging product item; see "3.2" for details
&emsp;title |string | Optional | Template title
&emsp;content |string | Optional | Template content (support up to 5 dynamic parameters {index} format)


Request example:

``` JSON
{
  "code":"1212300",
  "title":"New Temp title",
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```


#### 5.2.2 Return result

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Template code


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "code": "1212300"
  }
}
```


### 5.3 Paging query template API
- **API description:** Query template data according to specified conditions, support paging
- **API address:** app/template/query

#### 5.3.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string |Optional |Template code
&emsp;type |int | Optional |Product type; see "Appendix B" for details
&emsp;brandNameCode |string | Optional | Brand code
&emsp;productItemCode |string | Optional | The belonging product item; see "3.2" for details
&emsp;title |string | Optional | Template title (support fuzzy check)
&emsp;currPage |int | Required | Current page number, default value is 1
&emsp;pageSize |int | Required | The number of items per page, the default value is 10


Example request:

``` JSON
{
  "brandNameCode":"",
  "code":"",
  "currPage": 1,
  "pageSize": 1,
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 5.3.2 Return results

Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
code |int |Required |Response code See: "Appendix A"
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;totalCount |int |Required |Total number
&emsp;totalPage |int |Required |Total Pages
&emsp;list |array |Optional |Query results
&emsp;&emsp;code |string |Required |Template code
&emsp;&emsp;type |int | Required |Template type; see "Appendix B" for details
&emsp;&emsp;brandNameCode |string | Required | Brand code
&emsp;&emsp;productItemCode |string | Optional | The product item belongs to; see "3.2" for details
&emsp;&emsp;title |string | Optional | Template title
&emsp;&emsp;createTime |long | Optional | Create timestamp (milliseconds)
&emsp;&emsp;status |int | Required | Data status See "Appendix D" for details


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "totalCount": 100,
    "totalPage": 2,
    "currPage": 1,
    "pageSize": 1,
    "list": [
       {
          "type":2,
          "brandNameCode":"001aaab",
          "productItemCode":"SMS_AD_GROUP_1",
          "title":"temp title",
          "createTime": 1621503576000,
          "status": 1
       }
    ]
  }
}
```


### 5.4 Template Detail API
- **API description:** Query template details according to the template code, including review information
- **API address:** app/template/info

#### 5.4.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string |Required |Template code


Request example:

``` JSON
{
  "code":"001aaab",
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```


#### 5.4.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Template code
&emsp;type |int | Required |Template type; see "Appendix B" for details
&emsp;brandNameCode |string | Required | Brand code
&emsp;productItemCode |string | Required | The belonging product item; see "3.2" for details
&emsp;title |string | Required | Template title
&emsp;content |string | Required | Template content
&emsp;createTime |long | Required | Create timestamp (milliseconds)
&emsp;status |int | Required | Data status See "Appendix D" for details
&emsp;approve |object | Optional | Approve information
&emsp;&emsp;status |int | Required | Approve status See "Appendix E" for details
&emsp;&emsp;opinion |string | Optional | Approve opinion


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
      "type":2,
      "brandNameCode":"001aaab",
      "productItemCode":"SMS_AD_GROUP_1",
      "title":"temp title",
      "createTime": 1621503576000,
      "status": 1,
      "content":"Hi, My name is HAHA"
      "approve": {
         "status": 0,
         "opinion": "pass"
      }
  }
}
```

### 5.5 Template invalidation API
- **API description:** The template data is offline and invalidated
- **API address:** app/template/invalid

#### 5.3.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string | Required | Template code


Example request:

``` JSON
{
  "code":"001aaab",
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 5.3.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": null
}

```



## 6. Send Send related API

### 6.1 Sending API
- **API description:** Send a single number
- **API address:** app/send

#### 6.1.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;type |int | Required |Product type; see "Appendix B" for details
&emsp;templateCode |string | Optional | HyperSMS template code
&emsp;smsTemplateCode |string | Required | SMS template code
&emsp;subId |string | Required | Send number
&emsp;param |array | Optional | Set of dynamic parameter values ​​Note: The order should correspond to the dynamic parameter (index) in the template
&emsp;encodeType |int | Optional | Optional (0: plaintext, 1: operator encryption) The default is 0


Example request:

- **Normal Template**

``` JSON
{
  "type":2,
  "smsTemplateCode":"001aaab",
  "subId":"091313123123",
  "encodeType": 0,
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

- **Dynamic parameter template**

``` JSON
{
  "type":2,
  "smsTemplateCode":"001aaab",
  "subId":"091313123123",
  "param": ["0","2"]
  "encodeType": 0,
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```


#### 6.1.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Send code
&emsp;status |int |Optional |Send status (1: normal; 0: failed)
&emsp;msg |string |Optional |Send details


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "code": "160139858512181547",
    "status": 1
    "msg": null
  }
}
```


### 6.2 Batch sending API
- **API description:** Send multiple numbers (only 500 numbers are currently supported)
- **API address:** app/send/batch

#### 6.2.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;type |int | Required |Product type; see "Appendix B" for details
&emsp;templateCode |string | Optional | HyperSMS template code
&emsp;smsTemplateCode |string | Required | SMS template code
&emsp;subList |array | Required | Send multiple numbers
&emsp;&emsp;subId |string | Required | Send number
&emsp;&emsp;param |array | Optional | Set of dynamic parameter values, ​​Note: The order should correspond to the dynamic parameter {index} in the template
&emsp;&emsp;encodeType |int | Optional | Optional (0: plaintext, 1: operator encryption) The default is 0


Example request:

- **General Template**

``` JSON
{
  "type":2,
  "smsTemplateCode":"001aaab",
  "subList":[
    {
      "subId":"091313123121"
    },
    {
      "subId":"091313123122"
    }
  ],
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```

- **Dynamic parameter template**

``` JSON
{
  "type":2,
  "smsTemplateCode":"001aaab",
  "subList":[
    {
       "subId":"091313123121",
       "param": ["0","2"]
    },
    {
      "subId":"091313123122",
      "param": ["0","2"]
    }
  ],
  "timestamp": 1621503576000,
  "sign": "600e5d07f445be432d86d9271fb1df9b1"
}

```


#### 6.2.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;sendList |array |Required |&nbsp;
&emsp;&emsp;code |string |Required |Send code
&emsp;&emsp;status |int |Optional |Send status (1: normal; 0: failed)
&emsp;&emsp;msg |string |Optional |Send details


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "sendList":[
        {
          "code": "160139858512181547",
          "status": 1,
          "msg": null
        },
        {
          "code": "160139858512181548",
          "status": 1,
          "msg": null
      }
    ]
  }
}
```

## 7. Campaign related API

### 7.1 Create campaign API
- **API description:** Create campaign task information and sending rules
- **API address:** app/campaign/add

#### 7.1.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;type |int | Required |Product type; see "Appendix B" for details
&emsp;templateCode |string | Optional | HyperSMS template code
&emsp;smsTemplateCode |string | Required | SMS template code
&emsp;name |string | Required | Campaign name
&emsp;description |string | Optional | Campaign description
&emsp;property |object | Optional | Campaign description
&emsp;&emsp;startTime |string | Required | Campaign start time (format yyyy-MM-dd HH:mm:ss)
&emsp;&emsp;endTime |string | Required | Campaign end time (format yyyy-MM-dd HH:mm:ss)
&emsp;&emsp;sendPeriod |array | Optional | Limit the set of sending intervals
&emsp;&emsp;&emsp;start |string | Required | Time period start (format HH:mm:ss)
&emsp;&emsp;&emsp;end |string | Required | End of time period (format HH:mm:ss)
&emsp;&emsp;subList |array | Required |&nbsp; Up to 500 numbers
&emsp;&emsp;&emsp;subId |string | Required | Send number
&emsp;&emsp;&emsp;encodeType |int | Optional | Optional (0: plaintext, 1: operator encryption) The default is 0


Example request:

``` JSON
{
    "type":2,
    "smsTemplateCode":"001aaab",
    "name":"Campaign Test",
    "description":"description ",
    "property":{
       "startTime": "2021-05-21 12:12:12",
       "endTime": "2021-05-22 12:12:12",
        "sendPeriod":[
            {
                "start":"12:12:12",
                "end":"15:12:12"
            }
        ],
        "subList":[
            {
                "code":"160139858512181547",
                "status":1,
                "msg":null
            },
            {
                "code":"160139858512181548",
                "status":1,
                "msg":null
            }
        ]
    },
    "timestamp":1621503576000,
    "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 7.1.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Campaign code


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "code": "1212300"
  }
}
```



### 7.2 Edit campaign API
- **API description:** Edit campaign tasks and activity rules
- **API address:** app/campaign/edit

#### 7.2.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string |Required |Campaign code
&emsp;type |int | Optional |Product type; see "Appendix B" for details
&emsp;templateCode |string | Optional | HyperSMS template code
&emsp;smsTemplateCode |string | Optional | SMS template code
&emsp;name |string | Optional | Campaign name
&emsp;description |string | Optional | Campaign description
&emsp;property |object | Optional | Campaign description
&emsp;&emsp;startTime |string | Optional | Campaign start time (format yyyy-MM-dd HH:mm:ss)
&emsp;&emsp;endTime |string | Optional | Campaign end time (format yyyy-MM-dd HH:mm:ss)
&emsp;&emsp;sendPeriod |array | Optional | Limit the set of sending intervals
&emsp;&emsp;&emsp;start |string | Optional | Time period start (format HH:mm:ss)
&emsp;&emsp;&emsp;end |string | Optional | End of time period (format HH:mm:ss)
&emsp;&emsp;subList |array | Optional |&nbsp; Up to 500 numbers
&emsp;&emsp;&emsp;subId |string | Optional | Send number
&emsp;&emsp;&emsp;encodeType |int | Optional | Optional (0: plaintext, 1: operator encryption) The default is 0


Example request:

``` JSON
{
    "code":"1212300",
    "type":2,
    "smsTemplateCode":"001aaab",
    "name":"Campaign Test",
    "description":"description ",
    "property":{
       "startTime": "2021-05-21 12:12:12",
       "endTime": "2021-05-22 12:12:12",
        "sendPeriod":[
            {
                "start":"12:12:12",
                "end":"15:12:12"
            }
        ],
        "subList":[
            {
                "code":"160139858512181547",
                "status":1,
                "msg":null
            },
            {
                "code":"160139858512181548",
                "status":1,
                "msg":null
            }
        ]
    },
    "timestamp":1621503576000,
    "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 7.2.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Campaign code


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "code": "1212300"
  }
}
```

### 7.3 Paging query campaign API
- **API description:** Query activity data according to specified conditions, support paging
- **API address:** app/campaign/query

#### 7.3.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string |Optional |Campaign code
&emsp;type |int | Optional |Product type; see "Appendix B" for details
&emsp;templateCode |string | Optional | HyperSMS template code
&emsp;smsTemplateCode |string | Optional | SMS template code
&emsp;name |string | Optional | Campaign name (support fuzzy search)
&emsp;currPage |int | Required | Current page number, default value is 1
&emsp;pageSize |int | Required | The number of items per page, the default value is 10


Example request:

``` JSON
{
  "smsTemplateCode":"",
  "code":"",
  "currPage": 1,
  "pageSize": 1,
  "timestamp":1621503576000,
  "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 7.3.2 Return results

Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
code |int |Required |Response code See: "Appendix A"
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;totalCount |int |Required |Total number
&emsp;totalPage |int |Required |Total Pages
&emsp;list |array |Required |Query results
&emsp;&emsp;code |string |Required |Campaign code
&emsp;&emsp;type |int | Required |Product type; see "Appendix B" for details
&emsp;&emsp;templateCode |string | Optional | HyperSMS template code
&emsp;&emsp;smsTemplateCode |string | Optional | SMS template code
&emsp;&emsp;name |string | Required | Campaign name
&emsp;&emsp;description |string | Optional | Campaign description
&emsp;&emsp;createTime |long | Required | Create timestamp (milliseconds)
&emsp;&emsp;status |int | Required | Data status See "Appendix D" for details


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
    "totalCount": 100,
    "totalPage": 2,
    "currPage": 1,
    "pageSize": 1,
    "list": [
       {
          "code": "1212300",
          "type": 2,
          "smsTemplateCode": "001aaab",
          "name": "Campaign Test",
          "description": "description ",
          "createTime": 1621503576000,
          "status": 1
       }
    ]
  }
}
```


### 7.4 Campaign Details API
- **API description:** Query event details according to the campaign code, including basic event information, rule information, sending target and approved information
- **API address:** app/campaign/info

#### 7.4.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string |Required |Campaign code


Request example:

``` JSON
{
    "code":"001aaab",
    "timestamp":1621503576000,
    "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```


#### 7.4.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;
&emsp;code |string |Required |Campaign code code
&emsp;type |int | Required |Product type; see "Appendix B" for details
&emsp;templateCode |string | Optional | HyperSMS template code
&emsp;smsTemplateCode |string | Optional | SMS template code
&emsp;name |string | Required | Campaign name
&emsp;description |string | Optional | Campaign description
&emsp;createTime |long | Required | Create timestamp (milliseconds)
&emsp;status |int | Required | Campaign data status See "Appendix D" for details
&emsp;property |object | Optional | Campaign rule description
&emsp;&emsp;startTime |string | Optional | Campaign start time (format yyyy-MM-dd HH:mm:ss)
&emsp;&emsp;endTime |string | Optional | Campaign end time (format yyyy-MM-dd HH:mm:ss)
&emsp;&emsp;sendPeriod |array | Optional | Limit the set of sending intervals
&emsp;&emsp;&emsp;start |string | Optional | Time period start (format HH:mm:ss)
&emsp;&emsp;&emsp;end |string | Optional | End of time period (format HH:mm:ss)
&emsp;approve |object | Optional | Approve information
&emsp;&emsp;status |int | Required | Approve status See "Appendix E" for details
&emsp;&emsp;opinion |string | Optional | Approve opinion
&emsp;sendList |array |Required |Send number information
&emsp;&emsp;code |string |Required |Send code
&emsp;&emsp;status |int |Optional |Send status (1: normal; 0: failed)
&emsp;&emsp;msg |string |Optional |Send details

Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": {
      "code": "1212300",
      "type": 2,
      "smsTemplateCode": "001aaab",
      "name": "Campaign Test",
      "description": "description ",
      "createTime": 1621503576000,
      "status": 1
      "property": {
        "startTime": "2021-05-21 12:12:12",
        "endTime": "2021-05-22 12:12:12",
         "sendPeriod":[
            {
              "start": "12:12:12",
              "end": "15:12:12"
            }
         ]
       },
      "approve": {
         "status": 0,
         "opinion": "pass"
      },
      "sendList":[
         {
           "code": "160139858512181547",
           "status": 1,
           "msg": null
         },
         {
           "code": "160139858512181548",
           "status": 1,
           "msg": null
         }
      ]
  }
}
```

### 7.5 Campaign invalidation API
- **API description:** Invalidated Campaign (Note: Activities in progress cannot be invalidated)
- **API address:** app/campaign/invalid

#### 7.5.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string | Required | Campaign code


Example request:

``` JSON
{
    "code":"001aaab",
    "timestamp":1621503576000,
    "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 7.5.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": null
}

```


### 7.6 Cancel Campaign task API
- **API description:** Cancel the active task in execution
- **API address:** app/campaign/cancel

#### 7.6.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string | Required | Campaign code


Example request:

``` JSON
{
    "code":"001aaab",
    "timestamp":1621503576000,
    "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 7.6.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": null
}

```


### 7.7 Pause campaign task API
- **API description:** Pause the Campaign task that is being executed
- **API address:** app/campaign/pause

#### 7.7.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string | Required | Campaign code


Example request:

``` JSON
{
    "code":"001aaab",
    "timestamp":1621503576000,
    "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 7.7.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": null
}

```



### 7.8 Resume campaign task API
- **API description:** Resume campaign tasks that are being suspended
- **API address:** app/campaign/resume

#### 7.8.1 Request parameters
  
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;code |string | Required | Campaign code


Example request:

``` JSON
{
    "code":"001aaab",
    "timestamp":1621503576000,
    "sign":"600e5d07f445be432d86d9271fb1df9b1"
}

```

#### 7.8.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;
data |object |Required |&nbsp;


Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
  "data": null
}

```



## 8. Callback related API

### 8.1 Approve Status callback API
- **API description:** The data that needs to be approval, when the approve status changes, the callback will be received through the callbackApproveUrl provided by the access party
- **API address:** Provided by the accessing party

#### 8.1.1 Request parameters
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;type |int | Required | Callback type 1: Brandname 2: Template 3: Campaign
&emsp;code |string | Required | Callback code
&emsp;approve |object | Required | Callback review information
&emsp;&emsp;status |int | Required | Approve status See "Appendix E" for details
&emsp;&emsp;opinion |string | Optional | Approve opinion


Example request:

``` JSON
{
  "sign": "afsdfasdf2asdfasdf",
  "timestamp": 1621503576000,
  "type": 1,
  "code":"acb000000",
  "approve": {
     "status": 1,
     "opinion": "pass"
  }
}

```


#### 8.1.2 Return results

Parameter name | Type | Required or not | Description
:---- |:--- |:------ |:---
code |int |Required |Response code, see "Appendix A" for details
msg |string |Required |&nbsp;

Example:

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
}
```



### 8.2 Send and download status callback API
- **API description:** After the sending task is executed, the sending and downloading status will be callback through the callbackSendUrl specified by the access party
- **API address:** Provided by the accessing party

#### 8.2.1 Request parameters
Parameter name |Type |Required or not |Description
:---- |:--- |:------ |:---
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp |long |Required |Current UNIX timestamp (milliseconds)
&emsp;sign |string | Optional | Signature See "Appendix G" for details
timestamp	 |long	|Required   |Current UNIX timestamp (milliseconds)
&emsp;type				|int		| Required	| Callback type 0:DR 1:Retrieve
&emsp;code			|string		| Required | Callback code
&emsp;status			|int		| Required | Receipt status See "Appendix F" for details


Example request:

``` JSON
{
  "sign": "afsdfasdf2asdfasdf",
  "timestamp": 1621503576000,
  "type": 1,
  "code":"acb000000",
  "status": 1
}

```


#### 8.2.2 Return results

Parameter name						|Type		|Required or not	|Description 
:----						|:---		|:------	|:---	
code |int |Required |Response code, see "Appendix A" for details
msg							|string		|Required		|&nbsp;

Example request：

``` JSON
{
  "code": 0,
  "msg": "api.response.code.success",
  "msgArgs": null,
}
```



## Appendix

### Appendix A Response code description

Code	|Msg        |Remarks
:----	|:---                  |:---
0		|api.response.code.success   |The request is successful
&emsp;  |&emsp;                    |&emsp;
-1		|api.response.code.fail  |The request is failure
-1		|api.response.code.illegal |Illegal request
-1		|api.response.code.unknownError |Server internal exception
-1		|api.response.code.paramSignError |Signature error
&emsp;  |&emsp;                    |&emsp;
100		|api.response.code.paramError    |Parameter error
100		|api.response.code.paramTypeError |Parameter type error
100		|api.response.code.paramNotEmpty  |The parameter is empty
100		|api.response.code.paramTokenInvalid  |Invalid parameter token
&emsp;  |&emsp;                    |&emsp;
200		|api.response.code.user.accountNotExist |appId or appSecret is failure
200		|api.response.code.user.accountHasLocked | Account locked

### Appendix B: Description of product type code

Code		|Remarks
:----	|:---
1		| HyperSMS
2		| SMS


### Appendix D: Description of data status code
Code		|Remarks  
:----	|:---
0		| invalid
1		| valid

### Appendix E: Approve status code description

Code		|Remarks  
:----	|:---
0		| Reject
1		| Pass
2		| Waiting for Approval

### Appendix F description of sending status code


Type |Code	|Remarks 
:---- |:----	|:---
DR |1		| Success
DR |2		| Failure
DR |3		| Processing
Retrieve |1		| Success
Retrieve |2		| Failure
Retrieve |3		| Processing

### Appendix G signature rules

- Before the caller initiates the request, generate the key-value pair of the request parameters
- Sort by parameter key (ascending dictionary)
- Splice the parameter set in the "&key=value" format, and add appKey
- Finally get the sign value through MD5

Code example(java)：
``` JAVA
public static String createSign(Map<String, String> params, String appKey){
    StringBuilder sb = new StringBuilder();
    Map<String, String> sortParams = new TreeMap<>(params);
    for (Map.Entry<String, String> entry : sortParams.entrySet()) {
        String key = entry.getKey();
        if(StringUtils.isBlank(entry.getValue())){
            continue;
        }
        String value = entry.getValue().trim();
        if (StringUtils.isNotEmpty(value)){
            sb.append("&").append(key).append("=").append(value);
        }
    }
    String stringA = sb.toString().replaceFirst("&","");
    String stringSignTemp = stringA + appKey;
    String signValue = Md5Encrypt.md5(stringSignTemp);
    return signValue;
}

```