# Cookie

Итак, рассмотрим такую штуку как куки. Иногда в тасках бывают подсказки, к примеру, какие-нибудь картинки с изображением печенек. Знающие английский поймут почему. Например, на StepCTF в одном из web тасков была картинка с Кунг-фу Пандой, которая ела печеньки:)

Куки  — небольшой фрагмент данных, отправленный веб-сервером и хранимый на компьютере пользователя. Веб-клиент (обычно веб-браузер) всякий раз при попытке открыть страницу соответствующего сайта пересылает этот фрагмент данных веб-серверу в составе HTTP-запроса. Применяется для сохранения данных на стороне пользователя, на практике обычно используется для:

 1. Аутентификации пользователя
 2. Хранения персональных предпочтений и настроек пользователя
 3. Отслеживания состояния сеанса доступа пользователя
 4. Ведения статистики о пользователях

Куки используются веб-серверами для различения пользователей и хранения данных о них.
К примеру, если вход на сайт осуществляется при помощи куки, то после ввода пользователем своих данных на странице входа, куки позволяют серверу запомнить, что пользователь уже идентифицирован, и ему разрешён доступ к соответствующим услугам и операциям.
Многие сайты также используют куки для сохранения настроек пользователя. Эти настройки могут использоваться для персонализации, которая включает в себя выбор оформления и функциональности. Например, Википедия позволяет авторизованным пользователям выбрать дизайн сайта. Поисковая система Google позволяет пользователям (в том числе и не зарегистрированным в ней) выбрать количество результатов поиска, отображаемых на одной странице.
Куки также используются для отслеживания действий пользователей на сайте. Как правило, это делается с целью сбора статистики, а рекламные компании на основе такой статистики формируют анонимные профили пользователей, для более точного нацеливания рекламы.



**Работа кук**
Запрашивая страницу, браузер отправляет веб-серверу короткий текст с HTTP-запросом. Например, для доступа к странице http://www.example.org/index.html, браузер отправляет на сервер www.example.org следующий запрос:

> GET /index.html HTTP/1.1 Host: www.example.org

браузер→сервер

Сервер отвечает, отправляя запрашиваемую страницу вместе с текстом, содержащим HTTP-ответ. Там может содержаться указание браузеру сохранить куки:

> HTTP/1.1 200 OK Content-type: text/html Set-Cookie: name=value  
> (содержимое страницы)

браузер←сервер

Строка Set-cookie отправляется лишь тогда, когда сервер желает, чтобы браузер сохранил куки. В этом случае, если куки поддерживаются браузером и их приём включён, браузер запоминает строку name=value (имя = значение) и отправляет её обратно серверу с каждым последующим запросом. Например, при запросе следующей страницы http://www.example.org/spec.html браузер пошлёт серверу www.example.org следующий запрос:

> GET /spec.html HTTP/1.1 Host: www.example.org Cookie: name=value
> Accept: */*


браузер→сервер
Этот запрос отличается от первого запроса тем, что содержит строку, которую сервер отправил браузеру ранее. Таким образом, сервер узна́ет, что этот запрос связан с предыдущим. Сервер отвечает, отправляя запрашиваемую страницу и, возможно, добавив новые куки.
Значение куки может быть изменено сервером путём отправления новых строк Set-Cookie: name=newvalue. После этого браузер заменяет старое куки с тем же name на новую строку.
Куки также могут устанавливаться программами на языках типа JavaScript, встроенными в текст страниц, или аналогичными скриптами, работающими в браузере. В JavaScript для этого используется объект document.cookie. Например, document.cookie = "temperature=20" создаст куки под именем «temperature» и значением 20.
Рассмотрим небольшой пример решения таска:
При решении таска использовалось расширение Chrome под названием "EditThisCookie"
Открыв таск, видим:
![enter image description here](img/cookie1.png)
После того, как мы решили предыдущий таск с GET-запросом, админы решили улучшить безопасность и не давать права, используя GET-запрос. Хмм, возможно, стоит проверить куки:
![enter image description here](img/cookie2.png)
В куках мы видим очень заманчивое поле "isAdmin"
![enter image description here](img/cookie25.png)
Видим, что значение 0. Стоит попробовать поменять 0 на 1
![enter image description here](img/cookie3.png)
Сохраняем изменения и перезагружаем страницу и получаем результат
![enter image description here](img/cookie4.png)
Получили флаг - таск решен.
