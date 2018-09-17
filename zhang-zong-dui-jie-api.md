---
description: 新火版
---

# 转运API

{% api-method method="post" host="http://www.freshingstock.com/login" path=" " %}
{% api-method-summary %}
Login & obtain token
{% endapi-method-summary %}

{% api-method-description %}
这个方法允许 获取用户的 pk 和 token，后续用于api 交互
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="username" type="string" required=true %}
FS username
{% endapi-method-parameter %}

{% api-method-parameter name="password" type="string" required=true %}
FS password
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
    "STATUS": "SUCCESS",
    "pk": 15,
    "token": "ca1a52129s5f11e8ba1512bdd61b176c"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Could not find a cake matching this query.
{% endapi-method-response-example-description %}

```javascript
{
    "message": "Ain't no cake like that."
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="http://www.freshingstock.com/searemoval/sea\_removal " path=" " %}
{% api-method-summary %}
Create Removal shipment
{% endapi-method-summary %}

{% api-method-description %}
每次需要海运货物到新火仓库时，需要创建一个对应海运批次
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="param" type="string" required=true %}
转换为 json string  
  
  
  
 {   
         "why": "api\_create\_removal\_shipment",  
         "ship-type": "custom\_ship\_out"  
 }  
{% endapi-method-parameter %}

{% api-method-parameter name="token" type="string" required=true %}
{token}
{% endapi-method-parameter %}

{% api-method-parameter name="pk" type="string" required=true %}
{pk}
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
identity 表示已经创建了一个标志位6KA1T 的海运批次  
{% endapi-method-response-example-description %}

```javascript
{
    "status": true,
    "data": {
        "status": "created",
        "id": 2,
        "identity": "6KA1I"
    },
    "request_param": {
        "why": "create_removal_shipment",
        "ship-type": "custom_ship_out"
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="http://www.freshingstock.com/searemoval/sea\_removal" path=" " %}
{% api-method-summary %}
delete\_removal\_shipment
{% endapi-method-summary %}

{% api-method-description %}
删除在创建状态的removal shipment
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="param" type="string" required=true %}
转换为json string  
{   
"why": "api\_delete\_removal\_shipment",   
"ship-type": "custom\_ship\_out",  
 "identity": "6KA1I"  
 }
{% endapi-method-parameter %}

{% api-method-parameter name="token" type="string" required=true %}
{token}
{% endapi-method-parameter %}

{% api-method-parameter name="pk" type="string" required=true %}
{pk}
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="http://www.freshingstock.com/searemoval/sea\_removal" path=" " %}
{% api-method-summary %}
update\_removal\_shipment\_detail
{% endapi-method-summary %}

{% api-method-description %}
填入海运批次信息
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="param" type="string" required=true %}
转换为json string  
  
参数见下面**【param】**  
{% endapi-method-parameter %}

{% api-method-parameter name="token" type="string" required=true %}
{token}
{% endapi-method-parameter %}

{% api-method-parameter name="pk" type="string" required=true %}
{pk}
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

![](.gitbook/assets/image%20%285%29.png)

#### 【param】

```javascript

{
    'why': 'api_update_removal_shipment_detail',
    'removal_shipment_identity': '6KA1I',
    'details': [{
            'ship-type': 'custom_ship_out',
            'extra-info': '{}',
            'height(cm)': 30,
            'width(cm)': 30,
            'length(cm)': 30,
            'identity-barcode': '1Zxxxx1',
            'box-qty': 1,
            'weight(kg)': 20
            }, {
            'ship-type': 'custom_ship_out',
            'extra-info': '{}',
            'height(cm)': 30,
            'width(cm)': 30,
            'length(cm)': 30,
            'identity-barcode': '1Zxxxx2',
            'box-qty': 1,
            'weight(kg)': 21
            }, {
            'ship-type': 'custom_ship_out',
            'extra-info': '{}',
            'height(cm)': 30,
            'width(cm)': 30,
            'length(cm)': 30,
            'identity-barcode': '1Zxxxx3',
            'box-qty': 1,
            'weight(kg)': 22
            }, {
            'ship-type': 'custom_ship_out',
            'extra-info': '{}',
            'height(cm)': 30,
            'width(cm)': 30,
            'length(cm)': 30,
            'identity-barcode': '1Zxxxx4',
            'box-qty': 1,
            'weight(kg)': 23
            }, {
            'ship-type': 'custom_ship_out',
            'extra-info': '{}',
            'height(cm)': 30,
            'width(cm)': 30,
            'length(cm)': 30,
            'identity-barcode': '1Zxxxx5',
            'box-qty': 1,
            'weight(kg)': 24
            }]
}
```

{% api-method method="post" host="http://www.freshingstock.com/searemoval/sea\_removal " path=" " %}
{% api-method-summary %}
update\_package\_label
{% endapi-method-summary %}

{% api-method-description %}
提供按箱 提供package label
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="param" type="string" required=true %}
转换为jsonstr   
参加下面【param】
{% endapi-method-parameter %}

{% api-method-parameter name="token" type="string" required=true %}
{token}
{% endapi-method-parameter %}

{% api-method-parameter name="pk" type="string" required=true %}
{pk}
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

```javascript
==========================================================================
上传label

{
    "why": "api_update_package_label",
    "ship-type": "custom_ship_out",
    "action": "update_or_create",
    
    #如果shipment-id 有指定，但是系统未检索到，会返回错误
    #如果shipment-id 未指定，系统会先检索提供的fba-shipment-id 是否存在对应的shipment，
    ##如果存在，则是update 信息
    ##如果不存在，则会创建shipment，并将提供的fba-shipment-id 作为shipment 信息一部分存储
    #如果shipment-id 和 fba-shipment-id 都未指定，系统自动创建shipment, 但是fba-shipment-id为空
    
    "shipment-id": "6",
    "fba-shipment-id": "FBAxxx1", 
    
    "identity-barcode-list": [
        "1Zxxxx1",
        "1Zxxxx2",
        "1Zxxxx3",
        "1Zxxxx4",
        "1Zxxxx5"
    ],
    "PdfDocument": "encoded_string",
    "PdfType": "PackageLabel_4x6_2_page"
}

正常会返回 shipment-id, fba-shipment-id

============================================================================
删除shipment

{
    "why": "api_update_package_label",
    "ship-type": "custom_ship_out",
    "action": "delete",
    #系统会根据shipment-id或者fba-shipment-id 来查找符合的shipemnt 并删除
    "shipment-id": "6",
    "fba-shipment-id": "FBAxxx1",  
}


```

![](.gitbook/assets/image%20%286%29.png)

{% api-method method="post" host="http://www.freshingstock.com/searemoval/sea\_removal" path=" " %}
{% api-method-summary %}
obtain\_inbound\_status
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="param" type="string" required=true %}
转换为json string  
参考下面**\[param\]**
{% endapi-method-parameter %}

{% api-method-parameter name="token" type="string" required=true %}
{token}
{% endapi-method-parameter %}

{% api-method-parameter name="pk" type="string" required=true %}
{pk}
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

```javascript
#param 格式
{
    "action": "by_removal_shipment",
    "ship-type": "custom_ship_out",
    "why": "api_obtain_inbound_status",
    "identity": "LJYA9"
}


目前status 初步定义为
created， 上传尚未收到
received， 收到货未入库
audit，    已入库
exception - has missing or damanage    入库异常 - 严重受损或未收到货
gone，错误的信息

#response 响应例子
{
    "request_param": {
        "action": "by_removal_shipment",
        "ship-type": "custom_ship_out",
        "why": "api_obtain_inbound_status",
        "identity": "LJYA9"
    },
    "details": [{
            "status": "created",
            "box_qty": 1,
            "identity-barcode": "1Zxxxx1",
            "last_modify_time": "2018-09-17 19:10:12+00:00"
        }, {
            "status": "created",
            "box_qty": 1,
            "identity-barcode": "1Zxxxx2",
            "last_modify_time": "2018-09-17 19:10:14+00:00"
        }, {
            "status": "created",
            "box_qty": 1,
            "identity-barcode": "1Zxxxx3",
            "last_modify_time": "2018-09-17 19:10:15+00:00"
        }, {
            "status": "created",
            "box_qty": 1,
            "identity-barcode": "1Zxxxx4",
            "last_modify_time": "2018-09-17 19:10:17+00:00"
        }],
    "identity": "LJYA9"
}
```

