# API Облака для партнеров

## Список API
* [API Облака для партнеров: изменение формата/справочника схемы экстра поля товаров в магазине](#001)
* [API Облака для партнеров: получение формата/справочника схемы экстра поля товаров в магазине](#002)
* [API Облака для партнеров: добавление конкретных значений экстра в товары в магазине](#003)
* [API Облака для партнеров: получение конкретных значений экстра товаров в магазине](#004)

<a name="001"></a>
### API Облака для партнеров: изменение формата/справочника схемы экстра поля товаров в магазине

Запрос:
```
POST api/v1/inventpries/stores/0000-2222-4444-1111/products/schemas
 -H "x-auth-token: токен-пользователя-приложения
 -d {
   dictionaries: [
     {
       id: Countries
       values: {
         RUS: "Russia"
         AUT: "Austria" 
       }
       type: static
     }
   ]
   textField: {
     type: text
     editable: true
     regexp: "[a-Z]+"
   }
   numberField: {
     type: number
     editable: true
   }
   dictionaryField: {
     type: select
     dictionary: Countries
     editable: false
   }
 }	
```
Ответ:
```
status: 200 - ok
status: 400 - error
```

<a name="002"></a>
### API Облака для партнеров: получение формата/справочника схемы экстра поля товаров в магазине

Запрос:
```
GET api/v1/inventpries/stores/0000-2222-4444-1111/products/schemas
	-H "x-auth-token: токен-пользователя-приложения
```
Ответ:
```
status: 200 - ok
status: 400 - error
```

<a name="003"></a>
### API Облака для партнеров: добавление конкретных значений экстра в товары в магазине

Запрос:
```
POST api/v1/inventpries/stores/0000-2222-4444-1111/products/extras
	-H "x-auth-token: токен-пользователя-приложения
	-d {
		productUuid1 : {/*валидный json объект*/}
		productUuid2 : {...}
		productUuid2 : {...}
	}
```
Ответ:
```
status: 200 - ok
status: 400 - error
```

<a name="004"></a>
### API Облака для партнеров: получение конкретных значений экстра товаров в магазине

Запрос:
```
GET ???
```
Ответ:
```
status: 200 - ok
status: 400 - error
```
