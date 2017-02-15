# API Терминала для партнеров

## Список API
* [API Терминала для партнеров: получить итоговый чек при переходе к оплате](#001)
* [API Терминала для партнеров: разделить чек по позициям на несколько чеков (группы печати)](#002)
* [API Терминала для партнеров: работа с локальным хранилищем данных](#003)
* [API Терминала для партнеров: получить все подключенные пинпады](#004)
* [API Терминала для партнеров: оплатить мультичек на пинпаде](#005)

<a name="001"></a>
### API Терминала для партнеров: получить итоговый чек при переходе к оплате

Для получения всего чека используем:
```
 receipt.getReceipt()
 он возвращает строку в JSON формате
 {
    "receiptData": {
        "totalSum": "218.50",
        "discountPercents": "0.000000",
        "totalSumWithoutDiscount": "218.50",
        "positionDiscountSum": "0.00",
        "positionsCount": "4",
        "extraData": "{}"
    },
    "receiptPositions": [{
        "uuid": "070efd1b-4f53-401a-b5c1-cb31b5e9072d",
        "type": "NORMAL",
        "code": "1",
        "measure": "шт",
        "price": "56",
        "priceWithDiscount": "56",
        "quantity": "1"
    }, {
        "uuid": "9d2fdacd-969c-41f2-b360-a5ebbddcbe9e",
        "type": "NORMAL",
        "code": "131",
        "measure": "шт",
        "price": "30.5",
        "priceWithDiscount": "30.5",
        "quantity": "5"
    }, {
        "uuid": "7c916c19-756d-4b3d-806e-63c090080a3d",
        "type": "NORMAL",
        "code": "129",
        "measure": "шт",
        "price": "9",
        "priceWithDiscount": "9",
        "quantity": "1"
    }, {
        "uuid": "9dd36300-647e-4863-81b2-eb9591bc599b",
        "type": "NORMAL",
        "code": "127",
        "measure": "шт",
        "price": "1",
        "priceWithDiscount": "1",
        "quantity": "1"
    }]
}
```

<a name="002"></a>
### API Терминала для партнеров: разделить чек по позициям на несколько чеков (группы печати)

Группы печати, где для задания группы печати на позицию, используется метод:
```
edited 
receipt.addPositionPrintGroup(JSON String)
 в качестве параметра
 var addPositionPrintGroup = {
    "uuid": "e82a113b-0d76-424f-9ad5-c6595ca57770", -- uuid товара
    "code": "4", -- код товара
    "printGroupId" : "4dc2-3fcd", -- id группы печати
    "printGroupIsFiscal" : true, -- признак фискальности
    "printGroupOrgName" : "Andrew", -- имя группы
    "printGroupOrgInn": "88005553535", -- ИНН
    "printGroupPaymentSum" : "123.00" --Сумма товаров в группе
};
edited
```

<a name="003"></a>
### API Терминала для партнеров: работа с локальным хранилищем данных

Storage – система хранения данных в формате ключ – значение

Для работы с API необходимо в манифесте приложения указать:
```
capabilities:
 - storage
```

Объект для работы с API вызывается функцией:
```
var storage = require('storage')
```

Далее используются две функции:

* Сохранение данных:
```
storage.set(key, value)
```
Функция возвращает boolean: true в случае успешного сохранения, false если произошла ошибка.
key и value – строковые переменные

* Получение данных:
```
storage.get(key)
```
Функция возвращает строковую переменную ранее записанную в хранилище, либо null, если значение не было найдено.
```
key – строковая переменная
```

<a name="004"></a>
### API Терминала для партнеров: получить все подключенные пинпады
`В процессе разработки Терминала`

<a name="005"></a>
### API Терминала для партнеров: оплатить мультичек на пинпаде
```
receipt.addPaymentOperation(JSON String)
```

Принимает на вход принимает JSON, который описывает структуру оплаты для разбиения общего чека на платежи:
```
var paymentOperation = {
  "uuid": "1ef45g5", //юид оплаты
  "deviceUUID": "smth", //юид устройства для оплаты
  "paymentSum" : "123.00", //сумма платежа
  "isCashless" : true, //признак картой/нал
  "printGroups" : [ //список групп печати в оплате
  {
      "printGroupId" : "4dc2-3fcd",
      "printGroupIsFiscal" : true,
      "printGroupOrgName" : "Andrew",
      "printGroupPaymentSum" : "123.00",
      "printGroupOrgInn": "88005553535"
   }
  ]
};
```
