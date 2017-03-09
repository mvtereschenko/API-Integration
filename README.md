# API Облака для терминала

## Список API
* [1. Изменение значений экстра продуктов для магазина из терминала в Облако](#001)
* [2. Получение значений экстра продуктов для магазина на терминал из Облака](#002)
* [3. Получение схемы экстра продуктов для магазина на терминал из Облака](#003)

<a name="001"></a>
### 1. Изменение значений экстра продуктов для магазина из терминала в Облако

Запрос:
https://srv41.market.local/core/handler/api/v1/inventories/products/extras/events/update
```
req:
curl -X GET 
-H "X-Device-ID: 352398080126057"                       //заголовок просавляетс не на кассе
-H "X-User-ID: 02-900000000000001"                      //заголовок просавляетс не на кассе
-H "X-Shop-UUID: 20160913-8486-4027-800D-47417330D159"  //заголовок просавляетс не на кассе

"https://srv41.market.local/core/handler/api/v1/inventories/products/extras/events/update" -d

 [ 
  {
    timestamp: ...дата-как-обычно... ,
    extra: {                                                             
      "uuid": "832dd968-1249-45c5-8432-b2e1f41c27a0",
      "app": "test-app",                                          
      "key": {                                                    
        "uuid": "PRODUCT-UUID-200",                               
        "store": "20161209-3002-4054-80FD-F41228042BAF"           
      },                                                          
      "scheme": {                                                 
        "value": "20",                                            
        "fieldId": "SOME-DICTIONARY-FIELD-UUID"                   
      },                                                          
      "data": {                                                   
        "FullName": "Новое улучшенное значение"                
      },                                                          
    }                                                             

  }
]
```
Статусы ответов:
```
body {}
200 Ok
400 неверный пейлоад
500 сервак не доступен
```

<a name="002"></a>
### 2. Получение значений экстра продуктов для магазина на терминал из Облака

Запрос:
https://srv41.market.local/core/handler/api/v1/inventories/products/extras
```
curl -X GET -H "X-EVO-MARKET-ENTRYPOINT: INTERNAL" -H "X-Device-ID: 352398080126057" -H   
"X-User-ID: 02-900000000000001" -H "X-Shop-UUID: 20160913-8486-4027-800D-47417330D159"  
"https://srv41.market.local/core/handler/api/v1/inventories/products/extras"  

resp:
[
  {
    "uuid": "832dd968-1249-45c5-8432-b2e1f41c27a0",
    "app": "test-app",
    "key": {
      "uuid": "b57dfde8-eb29-4ab5-aac9-e608a2e07ee5",
      "store": "20160913-8486-4027-800D-47417330D159"
    },
    "scheme": {
      "value": "20",
      "fieldId": "SOME-DICTIONARY-FIELD-UUID"
    },
    "data": {
      "FullName": "Organization Twenty FullName"
    }
  }
]
```

<a name="003"></a>
### 3. Получение схемы экстра продуктов для магазина на терминал из Облака

Запрос: https://srv41.market.local/core/handler/api/v1/inventories/products/schemes
```
curl -X GET -H "X-EVO-MARKET-ENTRYPOINT: INTERNAL" -H "X-Device-ID: 352398080126057"   
-H "X-User-ID: 02-900000000000001" -H "X-Shop-UUID: 20160913-8486-4027-800D-47417330D159"  
"https://srv41.market.local/core/handler/api/v1/inventories/products/schemes"  

resp:
[
  {
    "uuid": "product-scheme-uuid",
    "appId": "test-product-extra-app1",
    "items": [
      {
        "type": "TEXT_FIELD",
        "title": "text field title",
        "editable": false,
        "regexp": null,
        "uuid": "SOME-TEXT-FIELD-UUID",
        "data": null
      },
      {
        "type": "DICTIONARY_FIELD",
        "editable": true,
        "title": "Dictionary field title",
        "uuid": "SOME-DICTIONARY-FIELD-UUID",
        "items": [
          {
            "data": {},
            "title": "Something title",
            "value": "20"
          },
          {
            "data": {},
            "title": "Special title",
            "value": "20"
          }
        ]
      }
    ]
  }
]
```
