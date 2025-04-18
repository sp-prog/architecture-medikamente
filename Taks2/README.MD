# Категории данных

1. Медицинские
2. Финансовые
3. Персональные


# Описание архитектуры

Сервисы изначально необходимо релизовывать с возможностью работы в кластере. Для лучшего контроля данных и простой масштабируемости необходимо выбрать архитектуру микросервисов: у каждого сервиса своя БД. Каждый сервис обслуживает свою часть предметной области. По заданию компания согласилась выделить необходимое финансирование на работы, т.е. предполагается, что финансирование безгранично.

Для обеспечения конфидециальности необходимо:
- внедрить систему аудита действий пользователей в сети. Такие инструменты должны быть в Windows Server 2022
- закрыть доступ к файловым хранилищам всем категориям пользователей, кроме сетевого администратора. Сетевой администратор должен иметь возможность доступка с серверу только под своей учетно записью.
- развернуть сервер 1С и переключить продукты 1С файлового режима на сетевой. Настроить доступ к продуктам 1С только по личным учетным записям
- подключить Informatica, которая умеет обучаться, ко всем источникам данных: почтовый сервер, файловые хранилища, базы данных
- добвыить систему тегирования найденных данных Informatica Data Catalog.
- вндерить систему авторизации по тегам и ролям. Для этого можно совместить Keycloak с Active Directory
- для работы с расписанием врачам, сотрудникам ресепшена и пользователся создать систему управления расписанием. Для каждой группы пользователей выделить свою группу экзепляров приложения. Это позволить уменьшить влияние одних групп пользователей на других
- все данные по сети необходимо передавать через протоколы TSL, HTTPS или SSL
- файлы пользовательских данных должны шифроваться в местах хранения
- данные медицинских карт в БД также должны шифроваться. Это может усложнить настройку тегирования. Возможно понядобится нанять высоковалифицированных разработчиков
- для контроля персональных данных пациента при передаче между сервисами расписаний и сервисом пациентов можно воспользоваться Apache Nifi
- любые данные, которыми в последствии может управлять пользователь (удалять), необходимо так же шифровать и хранить ключи расшифровки в отдельных таблицаз. При запросе пользователя на удаление ключ шифрования необходимо удалить из таблицы. Для контроля доступа к данным пользователю в дальнейшем можно предоставить возможность маркировать тегами свои данные. Эти теги будут автоматически учитываться Keyckoak при предоставлении доступов или Apache Nifi при передаче данных между сервисами. Данным будут маскироваться

# Сроки хранения

Необходимо собирать только те персональные и данные, которые необходимы для достижения первоначальной цели их сбора.
Если цели достигнуты или необходимость в данных отпала, то они должны быть уничтожены в течении 30 дней.
Тот же срок дейстувет при отзыве согласия на обратку ПДН
При нарушении принципов обработки ПДН данные необходимо уничтожить в течении 10 дней

Медицинские данные храняться от 1 года до 25 лет в зависимости от категории данных согласно "<Письмо> Минздрава России от 07.12.2015 N 13-2/1538 "О сроках хранения медицинской документации""