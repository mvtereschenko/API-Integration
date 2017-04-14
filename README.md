# API Облака для терминала

* [Список API Облака для партнеров:](#0)

    * [1. Изменение схемы продуктов для магазина](#1)
    * [2. Получение схемы продуктов для магазина](#2)
    * [3. Удаление схемы продуктов для магазина](#3)
    * [4. Добавление конкретных значений экстра в товары для магазина](#4)
    * [5. Получение конкретных значений экстра товаров в магазине](#5)
    * [6. Удаление конкретных значений экстра товаров в магазине](#6)
    * [7. Получение документов чека и их экстра данных](#7)

    <a name="0"></a>
    ## API Облака для партнеров
      
    В вашем кабинете на dev.evotor.ru нужно зарегистрировать приложение "Название приложения".  
    Для разработки нужен uuid приложения, uuid приложения можно посмотреть в адресной строке.  
    Также нужно прописать сервер приложения https:название приложения.название компании.ru, пока в кабинете разработчика не появится эта возможность.  
    
    Если присутствует необходимость серверной интеграции, тогда нужно предварительно авторизовывать пользовательские запросы.  
    Для этого нужен пользовательский токен. Чтобы его получить - нужно поддержать нотацию API:  
    https://api.evotor.ru/docs/?url=/docs/v1/evotor-to-partner.json#!/Управление_учетными_записями
    
    <a name="1"></a>
    ### 1. Изменение схемы продуктов для магазина
    
    У товара в магазине для одного приложения может быть одно экстра-поле внутри него любой валидный json (например поле с массивом объектов которые могут вам понадобиться на терминале).  
    У того же товара в том же магазине, но для другого приложения эстра поле будет другим. Когда к апи обращаются с токеном приложения то GET и POST будет влиять на экстраполя того приложения с токеном которого обращаются.  
    
    В 1 приложении должна быть только 1 схема, которая покрывает все экстра данного приложения. В перспективе можно будет менять схему для нескольких приложений (тогда пригодиться массив по appId, но пока это ограничено на уровне прав доступа по токену в запросе, который имеет право изменять только соответствующую схему приложения).  
    
    Запрос:
    ```
    POST api/v1/inventories/stores/{store-uuid}/products/schemes  
    Host: https://api.evotor.ru  
    Content-Type: application/json;charset=utf-8
    X-Authorization: {токен}
  
        [  {
            "uuid": "{uuid-схемы-продуктов-в-магазине}",
            "appId": "{uuid-приложения-в-маркетплейсе}",
            "items": [{
                    "title": "Произвольный заголовок поля ввода текста",
                    "editable": false,
                    "regexp": "\\w+", // валидный регексп
                    "uuid": "{uuid-поля-1}",
                    "type": "TEXT_FIELD",
                    "data": {} // валидный JSON с произвольными данными доступными из JS на терминале
                },
                {
                    "editable": true,
                    "title": "Произвольный заголовок поля выбора из списка",
                    "uuid": "{uuid-поля-2}",
                    "items": [{
                            "data": {}, // валидный JSON с произвольными данными доступными из JS на терминале
                            "title": "Заголовок первого варианта выбора",
                            "value": "10"
                        },
                        {
                            "data": {}, // валидный JSON с произвольными данными доступными из JS на терминале
                            "title": "Special title",
                            "value": "20"
                        }
                    ],
                    "type": "DICTIONARY_FIELD"
                }
            ]
        }]
    ```
    Статусы ответов:
    ```
    200 - Успех
    400 - Ошибка "Bad Request"
    401 - Ошибка "Unauthorized"
    ```
    
    В пример запроса описаны 2 поля, где 1 поле - текстовое поле, где title = "Произвольный заголовок поля ввода текста" и 2 поле - выбор из списка, где title = "Произвольный заголовок поля выбора из списка".  
    Где items.title - это заголовок поля и заголовок окна при выборе одновременно, а items.items.title - это заголовок одного значения из списка возможных предопределенных значений.  
    Добавлять значения "экстра полей" для всех productUuid можно как одним POST запросом, так и по частями, например по 100 позиций.
    
    <a name="2"></a>
    ### 2. Получение схемы продуктов для магазина
    
    Запрос:
    ```
    GET api/v1/inventories/stores/{store-uuid}/products/schemes  
    Host: https://api.evotor.ru  
    Content-Type: application/json;charset=utf-8
    X-Authorization: {токен}
    ```
    Статусы ответов:
    ```
    200 - Успех
    400 - Ошибка "Bad Request"
    401 - Ошибка "Unauthorized"
    ```

    <a name="3"></a>
    ### 3. Удаление схемы продуктов для магазина
    
    Запрос: надо добавить

    Пример 1: Удалить схемы по списку уидов схем

    ```
    POST /api/v1/inventories/stores/{storeUuid}/products/schemes/delete  
    Host: https://api.evotor.ru  
    Content-Type: application/json;charset=utf-8  
    X-Authorization: {токен}
    
    [   
      {scheme-uuid-1},
      {scheme-uuid-2}
    ]
    ```
    Пример 2: Удалить все схемы для товаров в магазине
    ```
    POST /api/v1/inventories/stores/{storeUuid}/products/schemes/delete
    Host: https://api.evotor.ru
    Content-Type: application/json;charset=utf-8
    X-Authorization: {токен}
    
    []
    ```
    Статусы ответов:
    ```
    200 - Успех
    400 - Ошибка "Bad Request"
    401 - Ошибка "Unauthorized"
    ```
    
    <a name="4"></a>
    ### 4. Добавление конкретных значений экстра в товары для магазина
    
    Запрос: 
    ```
    POST /api/v1/inventories/stores/{store-uuid}/products/extras 
    Host: https://api.evotor.ru  
    Content-Type: application/json;charset=utf-8
    X-Auth-Token: {токен}
    
        [{
            "uuid": "{uuid-экстра-поля-1}",
            "appId": "{uuid-приложения-в-маркетплейсе}",
            "key": {
                "uuid": "{uuid-продукта-связанного-с-экстра-полем}",
            },
            "scheme": { // Секция данных привязки значения экстра поля к контролу отображаемому на терминале
                        // (не требуется если экстра данные не нужно отобржатаь в меню терминал)
                "value": "20",
                "fieldId": "{{uuid-поля-из-схемы-отображения-продукта}"
            },
            "data": {}, // валидный JSON с произвольными данными доступными из JS на терминале
        }]
    
    ```
    Статусы ответов:
    ```
    200 - Успех
    400 - Ошибка "Bad Request"
    401 - Ошибка "Unauthorized"
    ```
    
    <a name="5"></a>
    ### 5. Получение конкретных значений экстра товаров в магазине
    
    Запрос:
    ```
    GET /api/v1/inventories/stores/{store-uuid}/products/extras HTTP/1.1
    Host: https://api.evotor.ru
    Content-Type: application/json;charset=utf-8"
    X-Authorization: {токен}
    ```
    Ответ:
    ```
    [ {
      "uuid" : "6eea4588-b3ac-4d78-9ede-c7b4fe26de55",
      "appId" : "9cb441a5-52b2-46fa-b40d-f71bff3eb63e",
      "name" : null,
      "key" : {
        "uuid" : "6eea4588-b3ac-4d78-9ede-c7b4fe26de55",
        "store" : null
      },
      "scheme" : {
        "value" : "a52fde7b-d2d6-443e-966f-1bcfbaff03f1",
        "fieldId" : "90144b1b-1560-409b-a90e-0cc89afd0830"
      },
      "data" : {
        "depts" : [ {
          "title" : "Женский отдел",
          "value" : "444b4584-633b-4e5e-af25-59dc583dd4f5"
        }, {
          "title" : "Туристический",
          "value" : "6d7b1b6e-d029-4a77-8cf1-3cf5239525cb"
        }, {
          "title" : "Детский отдел",
          "value" : "a52fde7b-d2d6-443e-966f-1bcfbaff03f1"
        }, {
          "title" : "Бытовая химия",
          "value" : "b71b0ae7-fa59-4cba-bd47-35ec351a543e"
        } ]
      }
    }]
    ```
    Статусы ответов:
    ```
    200 - Успех
    400 - Ошибка "Bad Request"
    401 - Ошибка "Unauthorized"
    ```

    <a name="6"></a>
    ### 6. Удаление конкретных значений экстра товаров в магазине
    
    Пример 1: Удалить экстры по списку уидов экстр
    ```
    POST /api/v1/inventories/stores/{storeUuid}/products/extras/delete
    Host: https://api.evotor.ru
    Content-Type: application/json;charset=utf-8
    X-Authorization: {токен}
    
    [   
      {extra-uuid-1},
      {extra-uuid-2}
    ]
    ```
    Пример 2: Удалить вес экстры для товаров в магазине
    ```
    POST /api/v1/inventories/stores/{storeUuid}/products/extras/delete
    Host: https://api.evotor.ru
    Content-Type: application/json;charset=utf-8
    X-Authorization: {токен}
    
    []
    ```
    Статусы ответов:
    ```
    200 - Успех
    400 - Ошибка "Bad Request"
    401 - Ошибка "Unauthorized"
    ```
        
    <a name="7"></a>
    ### 7. Получение документов чека и их экстра данных
    
    Запрос: https://test-api.evotor.ru/api/v1/inventories/stores/{store-uuid}/documents?deviceUuid  
    ```
    GET /api/v1/inventories/stores/{store-uuid}/documents?deviceUuid  
    Host: https://api.evotor.ru  
    Content-Type: application/json;charset=utf-8
    accept-encoding = [gzip]
    X-Authorization: {токен}
    
    [{
        "uuid": "5285ab1b-2ecb-4614-87f7-99b1486a04fe",
        "type": "OPEN_SESSION",
        "deviceId": "352398080047279",
        "deviceUuid": "8ffbe447-79d0-4fd9-823a-6da96d7f0be9",
        ...стандарнтые поля модели документа Эвотор...
        "extras": {
            "{application1-uuid}": {
                валидный_джейсон_со_значениями_эстра,
                application1 - uuid соответсвующий user - application1 - token - у
            }
        }
    }]
    ```
    Статусы ответов:
    ```
    200 - Успех
    400 - Ошибка "Bad Request"
    401 - Ошибка "Unauthorized"
    ```

