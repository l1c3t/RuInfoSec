<h1 align="center"> Mordor Environments</h1> 
<h2 align="center"> Erebor</h2>

![1]()

Эта среда Mordor была разработана для репликации очень маленькой сети с необходимыми устройствами для сбора информации о состязательной деятельности. Эта среда представляет собой среду Windows, в которой SilkETW работает на всех конечных точках для сбора телеметрии ETW через журнал событий.
### Дизайн сети

![2]()

### Конечные точки для пользователей

Платформа |	Версия |	Цель |	FQDN | IP адрес |	Основной пользователь |
---------|:---------|:----------|:------|:----------|------------|
Windows	| Win 2019 | DC |	HFDC01.erebor.com |	10.0.1.5	|Administrator |
Windows	| Win 10 |	Client |	HR001.erebor.com	| 10.0.1.106 | bbaggins |
Windows	| Win 10 |	Client |	IT001.erebor.com	| 10.0.1.105 | toakenshield |
Windows |	Win 10 |	Client |	ACCT001.erebor.com |	10.0.1.100	|	kfili |
Windows	| Win 2019	| Log Collector |	WECServer.erebor.com |	10.0.1.102 |	Administrator |
Windows |	Win 2019 |	File Server	| FILE001.erebor.com |	10.0.1.103 |	Administrator |
Linux |	Ubuntu 18 |	Data Analysis |	HELK |	10.0.1.6	|	ubuntu |
Linux	| Ubuntu 18 |	Red Team C2	| RTO	| 10.0.1.8	| ubuntu |
### Информация о пользователях Windows

Имя |	Фамилия	| Sam |	Отдел |	Должность |	Пароль |	Идентификатор |
---------|---------|----------|------|----------|------------|-----------|
Norah |	Martha |	nmartha |	Human Resources |	HR Director	|S@l@m3!123 |	Users |
Pedro	| Gustavo |	pgustavo |	IT Support	| CIO	| W1n1!2019	| Domain | Admins |
Lucho	| Rodriguez |	lrodriguez | Accounting	| VP |	T0d@y!2019 |	Users |
Sysmon |	MS |	sysmonsvc |	IT Support |	Service Account	| Buggy!1122 |	Users |
Administrator ||	Administrator| | |	P1ls3n!	| Users

### Информация о пользователе HELK
Вы можете обновить пароль HELK в [файле параметров HELK](https://github.com/hunters-forge/Blacksmith/blob/master/aws/mordor/cfn-parameters/erebor/helk-server-parameters.json), используемом для развёртывания среды. Этот файл размещён в проекте [Blacksmith](https://github.com/hunters-forge/Blacksmith), поскольку он является официальным репозиторием для всех шаблонов, используемых для развертывания в любой среде Mordor. 
- Имя пользователя по умолчанию: helk
- Пароль по умолчанию: hunt1ng!
### Собранные источники данных
Сервисная конфигурация SilkETW:
- https://github.com/hunters-forge/Blacksmith/blob/master/aws/mordor/cfn-files/configs/erebor/erebor_SilkServiceConfig.xml

Я прикрепил изображение ниже, чтобы показать вам, как SilkETW использует модель ETW для включения событий ETW для сбора, фильтрации и приёма через журнал событий.

![3]()

- Он считывает содержимое предоставленной конфигурации SilkServiceConfig.xml, чтобы определить поставщиков ETW, которые необходимо включить, и с какими фильтрами.
- Позволяет определить провайдеров ETW, определённых в файле конфигурации.
- Он создаёт сеансы трассировки событий и подписывает их на провайдеров ETW.
- Потребляет события из сессий трассировки в реальном времени.
- Записывает потребляемые события в журнал событий SilkService Log в режиме реального времени.
### Развёртывание среды
[Проект Blacksmith](https://blacksmith.readthedocs.io/en/latest/) отвечает за развёртывание этой среды. Поэтому вы можете следовать инструкциям, приведённым [здесь](https://blacksmith.readthedocs.io/en/latest/mordor_shire.html).