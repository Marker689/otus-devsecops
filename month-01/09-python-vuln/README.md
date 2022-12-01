# Домашнее задание

Поиск уязвимостей в python-коде.

## Цель:
Научиться находить распространенные недостатки в python-коде. Познакомиться с механизмами безопасности в django.


## Описание/Пошаговая инструкция выполнения домашнего задания:
### Часть 1.
Для приведенного фрагмента кода необходимо указать его недостаток, а также написать рекомендации по его устранению. Например, у такого кода:
```python
with connection.cursor() as cursor:
cursor.execute(""" SELECT admin FROM users WHERE username = '%s' """ % username)
````
Недостаток заключается в том, что введенные пользователем данные не проверяются и "as is" передаются в базу данных. В качестве мер противодействия можно либо корректно обрабатывать пользовательский ввод, либо использовать библиотеки, которые экранируют все опасные символы, что является более предпочтительным вариантом.


Фрагмент кода 1:
```python
#!/usr/bin/env python3
from flask import Flask
from mod_api import mod_api
app = Flask('vuln_app')
app.config['SECRET_KEY'] = 'F0cUzh8BgYJSLXAU8qDmClM0dE8GJTpsiyVEl3BCqQMCABp1U$f%'
app.register_blueprint(mod_api, url_prefix='/api')
```
Фрагмент кода 2:
```python
from flask_wtf import Form
from wtforms import TextField
class LoginForm(Form):
    username = TextField('username')
    password = TextField('password')
```
Фрагмент кода 3:
```python
def post(self):
    username = self.get_argument('username')
    password = self.get_argument('password').encode('utf-8')
    email = self.get_argument('mail')
    try:
        username = username.lower()
        email = email.strip().lower()
        user = User({'username': username, 'password': password, 'email': email, 'date_joined': curtime()})
        user.validate()
        save_user(self.db_conn, user)

    except Exception:

        return self.render_template("success_create.html")
```
### Часть 2.
Изучить модель безопасности django.
Ответить на вопросы.

Какие наиболее распространенные атаки предотвращает django?
Описать, как устроено управление пользовательскими сессиями в django




# Ответ

## Часть 1.
### Фрагмент кода 1:

Найденные недостатки:

- секретный ключ передается в открытом виде,
- хардкод для имени экземпляра Flask (**'vuln_app'**)

В качестве мер противодействия могу предложить следующее:

- спрятать секретный ключ в переменные окружения.
- вместо 'vuln_app' использовать переменную **'\_\_name\_\_'**

### Фрагмент кода 2:

Недостаток заключается в том, что класс **TextField** отсутствует в библиотеке **wtforms**. Помимо этого пароль не маскируется.
В качестве мер противодействия нужно использовать другие классы для "логина" и "пароля", а именно **StringField** и **PasswordField**

### Фрагмент кода 3:

Недостатки данного фрагмента кода заключаются в следующем:

- пароль записывается в явном виде,
- при вызове исключения вызывается шаблон страницы о "удачном создании пользователя" (**success_create.html**)
- получение аргументов из класса **self** выполнятся вне блока **try**


В качестве мер противодействия:

- стоит отказаться от хранения пароля в открытом виде и хранить лишь хэш пароля,
- перенести получение аргументов из класса **self** в блок **try**
- при исключении необходимо вызывать шаблон страницы о "неудачном создании пользователя" (**failed_create.html**)
- Дополнительно, возможно стоит экранировать переменные username и email, чтобы избежать дополнительных уязвимостей