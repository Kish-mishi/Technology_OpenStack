## Курс: Технология OpenStack. Основы программирования и конфигурирования.  
## Группа: Анциферова Татьяна, Ахметжанов Ренат, Корчагина Дарья, Резванов Владислав  
## Лабораторная работа 3  
## Дата создания: 17.03.2024  
---
# Работа с Openstack API
## Ход работы
### 1.Пробрасываем 5000 и 8774 порты для дальнейшей работы.
![ports](pictures/1-.jpg)


### 2.Запрашиваем у Keystone токен для дальнейшей работы. Нужное значение лежит в заголовке (header)- x-subject-token.
Для обращения к API используем расширение для браузера  [RestMan](https://chromewebstore.google.com/detail/restman/ihgpcfpkpmdcghlnaofdmjkoemnlijdi).   
Обращаемся к API Keystone по ссылке, запрос POST:  [https://localhost:5000/v3/auth/tokens](https://localhost:5000/v3/auth/tokens).  

![link](pictures/3.jpg)

Используем аутентификацию по паролю с указанием скоупа (в нашем случае project = admin):

```
  "auth": {
    "identity": {
      "methods": [
        "password"
      ],
      "password": {
        "user": {
          "name": "admin",
          "domain": {
            "id": "default"
          },
          "password": "4a1e531917dc4365"
        }
      }
    },
    "scope": {
      "project": {
        "name": "admin",
        "domain": {  "id": "default"  }
      }
    }
  }
}
```
### 3. Проверяем, что токен рабочий: запрашиваем эндпоинт Nova (просматриваем существующие ВМ), передав в заголовках полученный x-subject-token и x-auth-token (они идентичны).
Передаем токены:

![token](pictures/2.jpg)

Обращаемся к API Nova по ссылке, запрос GET:  [https://localhost:8774/v2.1/servers](https://localhost:8774/v2.1/servers).  

![link](pictures/3.jpg)

Получаем существующие виртуальные машины:

![link](pictures/5.jpg)

### 4. Создаем новую ВМ через Nova API (порт 8774).

Обращаемся к API Nova по ссылке, запрос POST:  [https://localhost:8774/v2.1/servers](https://localhost:8774/v2.1/servers).  

Пишем запрос:
```
 {
  "server":{
    "name": "test2",
    "imageRef": "3c7b78b8-327e-44cc-9fcc-f9372cb32574",
    "flavorRef": "13d32344-a906-4e98-bee6-66e6ff1c23de",
    "networks":[
      {
        "uuid":"44577df6-ae1c-4648-a2c8-1d26de9402c7"
      }
    ],
    "security_groups":[
      {
        "name": "2709c55e-e930-47dc-a834-6b6d6af8c464"
      }
    ]
  }
}
```

![ВМ](pictures/6.jpg)

Получаем:

![created_ВМ](pictures/7.jpg)

В результате видим новую ВМ - test2 в списке наших виртуальных машин:

![horizon_created_ВМ](pictures/8.jpg)

## Вопросы:
### 1.Какие протоколы тунеллирования использует Neutron?

### 2.Можно ли заменить Cinder, например, CEPH-ом? Для чего если да, почему если нет?



