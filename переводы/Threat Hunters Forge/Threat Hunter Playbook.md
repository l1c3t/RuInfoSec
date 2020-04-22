#### Перевод статьи [Threat Hunter Playbook ⚔ + Mordor Datasets 📜 + BinderHub 🌎 = Open Infrastructure 🏗 for Open Hunts 🏹 💜](https://medium.com/threat-hunters-forge/threat-hunter-playbook-mordor-datasets-binderhub-open-infrastructure-for-open-8c8aee3d8b4)

##### Автор Роберто Родригес ([Roberto Rodriguez](https://medium.com/@Cyb3rWard0g))

![1]()

Прошло почти три года с тех пор, как я начал публично документировать обнаружения, и я всегда задавался вопросом: ***«Как я могу поделиться обнаружениями более практичным и интерактивным способом, чтобы любой человек в мире мог получить доступ, запустить и проверить каждую аналитику из одного и того же места?»***

Я не просто хотел поделиться статическим документом с правилами или запросами, которые любой может просто скопировать и вставить. Я хотел предоставить способ поощрения совместной работы и позволить другим запрашивать наборы данных, которые я использовал во время разработки аналитики. Кроме того, я подумал, что если бы я нашёл способ для других самостоятельно тестировать обнаружения без необходимости создания аналитической платформы. На самом деле, это помогло бы всем из любого уголка мира, у кого даже нет инфраструктуры, ресурсов или навыков.

В этой статье я покажу вам, как мне удалось интегрировать обнаружения из инициативы [Threat Hunter Playbook](https://github.com/hunters-forge/ThreatHunter-Playbook) и предварительно записанные наборы данных из [Mordor](https://mordor.readthedocs.io/en/latest/) с удивительным [проектом BinderHub](https://binderhub.readthedocs.io/en/latest/index.html), чтобы дать возможность сообществу и другим людям со всего мира интерактивно запускать каждое обнаружение и получать одинаковые результаты в общедоступной вычислительной среде, используя веб-браузер.

Я считаю, что прежде чем углубляться в технические аспекты того, как я это сделал, важно понять, что даёт каждый проект, помимо BinderHub. Для каждого из них существует большое количество документации, поэтому я предоставлю краткое резюме и ссылки, чтобы вы могли узнать о них подробнее.

## Что такое проект Mordor?

![2]()

[Проект Mordor](https://mordor.readthedocs.io/en/latest/) предоставляет предварительно записанные события безопасности, полученные имитационными состязательными техниками в виде файлов JavaScript Object Notation (JSON) для удобства использования. Предварительно записанные данные классифицируются по платформам, враждебным группам, тактике и техникам, определённым в Mitre [ATT&CK Framework](https://attack.mitre.org/). Официальный дескриптор Twitter — [@Mordor_Project](https://twitter.com/Mordor_Project).

Этот проект является частью сообщества [Threat Hunters Forge](https://twitter.com/HuntersForge), и начал я с него, чтобы поделиться наборами данных со своим братом Хосе Луисом Родригесом ([Jose Luis Rodriguez](https://medium.com/@cyb3rpandah)), проводя совместные исследования. Мы поняли, что каждый из нас тратит слишком много времени на моделирование одного и того же варианта состязательной техники, одновременно генерируя одни и те же события безопасности с точки зрения поведения.

Например, ***что происходит, когда злоумышленник запрашивает значения ключа реестра Windows?*** Либо, если это делается с помощью PowerShell или C#, злоумышленник вызовет несколько одинаковых событий, показанных ниже, с точки зрения события безопасности Windows. Конечно, только в том случае, если в [системном списке контроля доступа (SACL)](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-control-lists) объекта реестра установлен правильный [контроль доступа (ACE)](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-control-lists)😉.

![3]()

Мы решили, что если сможем стандартизировать нашу среду моделирования и задокументировать включённую телеметрию, один из нас сможет провести моделирование, собрать данные и поделиться ими с другими для дальнейшего анализа. Затем мы подумали о других, имеющих подобные ограничения или неспособных вообще смоделировать технику, поэтому решили поделиться с миром каждым набором данных, и именно так родился [репозиторий Mordor](https://github.com/hunters-forge/mordor)😈📜. Для дополнительной информации о том, как мы делаем снимки данных при моделировании состязательных техник или как используем предварительно записанный набор данных, вы можете перейти на [Вики-проект](https://mordor.readthedocs.io/en/latest/) или посмотреть следующее видео:

[![Watch the video]()

## Что представляет собой проект Threat Hunter Playbook?

![4]()

<h5 align="center">https://twitter.com/HunterPlaybook/status/1189908361367691266</h5>

[Threat Hunter Playbook](https://github.com/hunters-forge/ThreatHunter-Playbook) — это ещё одна инициатива сообщества [Threat Hunters Forge](https://twitter.com/HuntersForge), направленная на поиск стратегий и вдохновения новых обнаружений. Официальный дескриптор твиттера — [@HunterPlaybook](https://twitter.com/HunterPlaybook), который я использую для обмена Notebook'ами каждые две недели. Он предоставляет конкретные цепочки событий безопасности, которые можно использовать для разработки аналитики данных в предпочитаемом вами инструменте или формате запроса. Этот проект также следует структуре фреймворка [MITRE ATT&CK](https://attack.mitre.org/), классифицирующей посткомпромиссное поведение злоумышленника в тактических группах.

Кроме того, в проекте разработаны стратегии обнаружения в виде [интерактивных Notebook'ов](https://notebooks-forge.readthedocs.io/en/latest/jupyter.html), обеспечивающих простой и гибкий способ сохранения, копирования и визуализации аналитики и ожидаемого результата.

### Что такое Notebook?

Представляйте Notebook как документ, к которому вы можете получить доступ через веб-интерфейс, позволяющий сохранять input (т. е. живой код) и output (т. е. результаты выполнения кода / вывод оценённого кода) интерактивных сеансов, а также важные примечания. Такие примечания необходимы для объяснения методологии и шагов, предпринятых для выполнения конкретных задач (например, анализ данных).

![5]()

<h5 align="center">https://nbviewer.jupyter.org/github/hunters-forge/ThreatHunter-Playbook/blob/master/docs/content/notebooks/windows/06_credential_access/credential_access/WIN-191030201010.ipynb</h5>

Кроме того, каждый Notebook снабжён ссылками на предварительно записанные наборы данных из проекта [Mordor](https://mordor.readthedocs.io/en/latest/), чтобы подтвердить обнаружение конкретной анализируемой состязательной техники. Это очень полезная функция, так как она обеспечивает дополнительный контекст для обмена аналитическими данными.

## Наборы данных Mordor и Threat Hunter Playbook?

Я решил добавлять ссылки на наборы данных Mordor к каждому обнаружению. Надеюсь, это подтолкнёт других взглянуть на используемый набор данных и потенциально поможет мне улучшить логику обнаружения. Есть несколько событий, которые я, возможно, даже не учитывал при создании обнаружения (вообще 😆).

Например, для Notebook «[Удалённый интерактивный менеджер задач LSASS Dump](https://nbviewer.jupyter.org/github/hunters-forge/ThreatHunter-Playbook/blob/master/docs/content/notebooks/windows/06_credential_access/credential_access/WIN-191030201010.ipynb)» я считаю следующие источники данных полезными для разработки аналитики:

- [Событие безопасности 4778: сеанс был подключён к станции окон](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4778).
- [Sysmon 1: создание процесса](https://github.com/hunters-forge/OSSEM/blob/master/data_dictionaries/windows/sysmon/event-1.md).
- [Sysmon 10: доступ к процессу](https://github.com/hunters-forge/OSSEM/blob/master/data_dictionaries/windows/sysmon/event-10.md).
- [Sysmon 11: создание файла](https://github.com/hunters-forge/OSSEM/blob/master/data_dictionaries/windows/sysmon/event-11.md).

Однако, если взглянуть на метаданные, предоставляемые [набором данных Mordor](https://github.com/hunters-forge/mordor/blob/master/small_datasets/windows/credential_access/credential_dumping_T1003/remoteinteractive_taskmngr_lsass_dump.md), используемым для этого обнаружения, то можно увидеть, что есть другие события, которые могут быть интересны для улучшения обнаружения или создания дополнительной аналитики 😉. 

![6]()

## Что же я тогда пытаюсь решить?

Пока что я верю и надеюсь, что [обмен обнаружениями](https://twitter.com/HunterPlaybook) с помощью Notebook'ов и предоставлением ссылок на предварительно записанные наборы данных, использованные при разработке аналитики, полезен. В то время как кто-то захочет скопировать и вставить предоставленную аналитику, другие могут запустить проверочные тесты и углубиться в аналитику с точки зрения данных. Я полагаю, что обмен будет полезен и исследователям, которые только начинают работать в отрасли и, возможно, захотят перейти непосредственно к анализу данных и использовать аналитику, предоставленную в качестве ссылок или примеров для разработки будущих обнаружений.

Несмотря на то, что это звучит здорово (я надеюсь), для того, чтобы в полной мере использовать Notebook и подход к набору данных Mordor, необходимо выполнить следующие шаги:
- Скопировать репозиторий https://github.com/hunters-forge/ThreatHunter-Playbook.
- Запустить Jupyter Notebook Server, чтобы разместить на нём все Notebook'и, предоставленные проектом Threat Hunter Playbook.
- Установить библиотеки Python, такие как [PySpark](https://pypi.org/project/pyspark/) и [OpenHunt](https://pypi.org/project/openhunt/), на сервер Jupyter Notebook для запуска анализа данных в Notebook'ах.
- Скопировать репозиторий https://github.com/hunters-forge/mordor внутри каталога Notebook'ов или загрузить каждый набор данных в интерактивном режиме во время выполнения каждой аналитики.
- Распаковать каждый набор данных Mordor (это файлы .tar.gz).

Для тех, кто знаком с [контейнерами Docker](https://www.docker.com/resources/what-container), все вышеперечисленные шаги можно собрать в [файл Docker (Dockerfile)](https://docs.docker.com/engine/reference/builder/) и 💥 💥. Однако, несмотря на то, что установка докера-контейнера на вашем компьютере может быть очень простой задачей, не все обладают одинаковыми навыками. Поэтому я начал искать способы для автоматизации установки, но не смог найти простой способ поделиться своими исследованиями без каких-либо базовых знаний о Docker, vagrant или packer, AWS CloudFormation или любом другом способе установки чего-либо локально или через облачного провайдера. Я искал **подход с помощью единственного клика**. Так обстояли дела, пока я не прочитал о [проекте Binder](https://mybinder.readthedocs.io/en/latest/index.html) и его продукте [BinderHub](https://binderhub.readthedocs.io/en/latest/)😍.

## Войдите в проект Binder
>*Проект Binder — это открытое сообщество, которое позволяет создавать совместимые, интерактивные и воспроизводимые среды. Основной технический продукт, который создаёт сообщество, называется BinderHub, и одно из развёртываний существует на `mybinder.org`. Этот веб-сайт управляется проектом Binder как общественная служба, позволяющая другим людям легко делиться своей работой.*

## Что такое BinderHub?
>*Основная цель BinderHub — создание пользовательских вычислительных сред, которые  используются многими удалёнными пользователями. BinderHub позволяет конечному пользователю легко указать желаемую вычислительную среду из репозитория Git. Затем BinderHub обслуживает пользовательскую вычислительную среду по URL-адресу, к которому пользователи могут обращаться удалённо.*

## Как работает BinderHub?
Согласно [документам BinderHub](https://binderhub.readthedocs.io/en/latest/), он объединяет несколько сервисов для оперативного создания и регистрации образов Docker.

BinderHub связывает вместе:

- [JupyterHub](https://github.com/jupyterhub/jupyterhub) предоставляет масштабируемую систему для аутентификации пользователей и создания однопользовательских серверов Jupyter Notebook.
- [Repo2Docker](https://github.com/jupyter/repo2docker), который генерирует образ Docker, используя Git-репозиторий, размещённый в интернете.

Он использует следующие сервисы:

- **Облачный провайдер**, такой как Google Cloud, Microsoft Azure, Amazon EC2 и другие.
- **Kubernetes** для управления ресурсами в облаке.
- **Helm** для настройки и управления Kubernetes.
- **Docker** для использования контейнеров, которые стандартизируют вычислительные среды.
- **Пользовательский интерфейс BinderHub**, к которому пользователи могут получить доступ для указания Git-репозитория, который они хотят построить BinderHub для генерации изображений Docker, используя URL Git-репозитория.
- **Реестр Docker** (например, gcr.io), в котором размещаются образы контейнеров.
- **JupyterHub** для установки временных контейнеров для пользователей.

### Нужно ли мне создавать свой собственный BinderHub?
Вы можете создать собственное развёртывание BinderHub и запустить код в облаке, но команда Binder уже сделала это для вас на сервере BinderHub, работающем на `*[mybinder.org](https://mybinder.org/)*` в качестве общедоступного сервиса (бесплатно). Если вы перейдёте на этот сайт, просто укажите имя или URL-адрес хранилища, в котором находится **репозиторий Binder**.

![7]()

Вы можете разместить собственный репозиторий Binder в **Github**, **GitLab** и т. д., как показано ниже

![8]()

## Репозиторий Binder?

Согласно [документам Binder](https://mybinder.readthedocs.io/en/latest/introduction.html#what-is-a-binder), Binder (также называемый Binder-ready репозиторий) представляет собой репозиторий кода, который содержит как минимум две вещи:

1. Код или контент для запуска (например, Jupyter Notebooks).
2. Файлы конфигурации для вашей среды (т. е. Dockerfile).

### Что такое «Файлы конфигурации»?
Это файлы, используемые Binder для создания среды, необходимой для запуска вашего кода. Например, если я хочу построить сервер Jupyter Notebook, я должен сказать Binder, как это сделать. Список всех файлов конфигурации, доступных для создания среды, см. на [странице «Файлы конфигурации»](https://mybinder.readthedocs.io/en/latest/config_files.html#config-files). Кроме того, есть и другие способы. Увидеть остальные примеры репозиториев Binder можно [здесь](https://mybinder.readthedocs.io/en/latest/sample_repos.html).

Я использую [контейнеры Docker](https://www.docker.com/resources/what-container) для создания Notebook'ов Jupyter, поэтому я решил создать такой Notebook, чтобы поделиться своими Notebook'ами и исследованиями с другими. Также у меня есть целый проект, связанный с Jupyter Notebook и контейнерами Docker под названием [Notebooks Forge](https://github.com/hunters-forge/notebooks-forge). Этот проект посвящён созданию и предоставлению серверов `Notebooks` для операторов `Defensive` и `Offensive` (фиолетовый пост об этом скоро...😉).

## Репозиторий Binder для среды Threat Hunter Playbook с помощью Docker
Если вы хотите использовать Docker для своего собственного репозитория Binder, обязательно прочитайте [документы Binder](https://mybinder.readthedocs.io/en/latest/tutorials/dockerfile.html) и не забудьте про требования, указанные в документах.

Как я упоминал ранее, я создаю серверы Jupyter Notebook с помощью файлов Docker из проекта [Notebooks Forge](https://github.com/hunters-forge/notebooks-forge) и сохраняю встроенные образы Docker в [своём собственном общедоступном реестре Docker](https://hub.docker.com/u/cyb3rward0g). Вы можете просто скачать примеры и использовать их прямо сейчас.

![9]()

Для своего файла конфигурации Binder я беру образ Docker с именем [jupyter-pyspark](https://hub.docker.com/r/cyb3rward0g/jupyter-pyspark) из моего общедоступного реестра Docker и использую его в качестве основы для выполнения всех шагов, необходимых для интеграции проектов Threat Hunter Playbook и Mordor с конкретными требованиями, необходимыми для работы с развёртыванием BinderHub. Вы можете прочитать содержимое Dockerfile [здесь](https://github.com/hunters-forge/ThreatHunter-Playbook/blob/master/Dockerfile).

![10]()

Я создаю файл Docker (файл **Dockerfile**) и помещаю его в корень репозитория Threat Hunter Playbook GitHub, как показано ниже:

![11]()

## Как BinderHub создаёт среду Threat Hunter Playbook?
Есть несколько шагов, которые происходят в бэкэнде, но всё, что мне нужно сделать, это зайти на сайт https://mybinder.org/ и заполнить необходимую информацию, как показано ниже. Если вы заметите, как только я набираю URL-адрес моего репозитория GitHub, он выёт мне следующую ссылку Binder https://mybinder.org/v2/gh/hunters-forge/ThreatHunter-Playbook/master, чтобы я мог сразу же поделиться с другими.

![12]()

Кроме того, когда я нажимаю на выпадающий треугольнику в нижней части страницы, я получаю информацию о значке переплёта, который я мог использовать для своего [файла README Hunter Threat Hunter ](https://github.com/hunters-forge/ThreatHunter-Playbook/blob/master/README.md). Это может помочь автоматизировать начальные шаги.

![13]()

Наконец, всё, что мне нужно сделать, это нажать оранжевую кнопку **Launch**, и BinderHub начнёт сборку среды Jupyter Notebook из **Dockerfile**, который я создал в корне репозитория, включая все Notebook'и из Threat Hunter Playbook и предварительно записанные наборы данных из проекта Mordor.

Со значком Binder, я могу автоматизировать шаги для любого, кто получает доступ к репозиторию. Всё, что вам нужно сделать, это нажать кнопку «**launch binder**».

![14]()

### Но что происходит в бэкэнде?
После нажатия на значок Binder BinderHub разрешает [ссылку](https://github.com/hunters-forge/ThreatHunter-Playbook), и в бэкэнде происходит следующее:

![15]()

BinderHub проверяет, существует ли образ Docker в его реестре Docker.
- Если он не существует (при первой сборке), BinderHub создаёт модуль сборки Kubernetes, который использует [Repo2Docker](https://github.com/jupyter/repo2docker) для превращения репозитория GitHub в образ Docker с поддержкой Jupyter. Он берёт **[Dockerfile](https://github.com/hunters-forge/ThreatHunter-Playbook/blob/master/Dockerfile** в репозиторий, создаёт образ, передаёт его в реестр Docker BinderHub и сохраняет информацию реестра для дальнейшего использования.
- Если образ существует, BinderHub проверяет его актуальность. Если образ не обновлён, BinderHub снова использует [Repo2Docker](https://github.com/jupyter/repo2docker) для создания нового образа и обновления его в своем реестре Docker.

Если образ Docker существует и он актуален, BinderHub отправляет реестр образов Docker в JupyterHub.
- JupyterHub создаёт модуль Kubernetes для образа Jupyter Notebook.
- JupyterHub отслеживает активность пользователя и отсоединяет его после короткого периода бездействия.
А пока вы увидите следующий скрин.

![16]()

Как только все это произойдет, вам будет представлен интерфейс меню Jupyter Notebook. Там вы увидите папку, содержащую файлы Markdown и Notebook'и Jupyter из проекта Threat Hunter Playbook.

![17]()

Если вы хотите получить доступ к обнаружениям Windows как к Notebook'ам, вы можете перейти по следующему пути: `content/notebooks/windows`.

![18]()

Например, вы можете дважды щёлкнуть Notebook «**Удалённый интерактивный диспетчер задач LSASS Dump**» (Remote Interactive Task Manager LSASS Dump), и запустить в интерактивном режиме каждую  ячейку ввода. Первые из них просто текст Markdown.

![19]()

Если вы вернетёсь в главное меню Notebook'а и нажмёте на вкладку `Running`, то увидите, что Notebook на самом деле работает, и это не статический вид.

![20]()

Вы можете запустить каждую ячейку Notebook'а, просто нажав [SHIFT] + [ENTER] на клавиатуре. Ячейки на изображении ниже, делают следующее:
- Импорт библиотек Python.
- Запуск сеанса Spark.
- Чтение содержимого конкретного файла Mordor и возврат DataFrame.
- Показ DataFrame как временное представление Spark SQL для выполнения SQL-подобных запросов в верхней части.

![21]()

Этот Notebook был создан для участия в [CAR-2019–08–001(https://car.mitre.org/analytics/CAR-2019-08-001/)], поэтому я решил проверить предоставленную аналитику по набору данных Mordor, как показано ниже:
### Аналитический псевдокод CAR
```
files = search File:Create
lsass_dump = filter files where (
  file_name = "lsass*.dmp"  and
  image_path = "C:\Windows\*\taskmgr.exe")
output lsass_dump
```
### SQL-запрос для Threat Hunter Playbook 
```
SELECT `@timestamp`, computer_name, Image, TargetFilename, ProcessGuid
FROM mordor_file
WHERE channel = "Microsoft-Windows-Sysmon/Operational"
    AND event_id = 11
    AND Image LIKE "%taskmgr.exe"
    AND lower(TargetFilename) RLIKE ".*lsass.*\.dmp"
```
![22]()

Затем я решил добавить другие потенциальные обнаружения и начал объединять события Sysmon, чтобы добавить больше контекста к исходной аналитике.
```
SELECT o.`@timestamp`, o.computer_name, o.Image, o.LogonId, o.ProcessGuid, a.SourceProcessGUID, o.CommandLine
FROM mordor_file o
INNER JOIN (
    SELECT computer_name,SourceProcessGUID
    FROM mordor_file
    WHERE channel = "Microsoft-Windows-Sysmon/Operational"
        AND event_id = 10
        AND lower(TargetImage) LIKE "%lsass.exe"
        AND (lower(CallTrace) RLIKE ".*dbgcore\.dll.*" OR lower(CallTrace) RLIKE ".*dbghelp\.dll.*")
        ) a
ON o.ProcessGuid = a.SourceProcessGUID
WHERE o.channel = "Microsoft-Windows-Sysmon/Operational"
    AND o.event_id = 1
```
![23]()

Мне также нравится показывать, как можно объединять источники данных (например, события Sysmon и Windows Security) и выполнять совместные заявления на общих полях с уникальными значениями. В этом Notebook'е я показываю, как присоединить результаты первого Sysmon join, который я выполнил на изображении выше, к событию Windows Security [4778: сеанс был переподключен к Window Station](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4778) по значениям **LogonId**. Обнаружение переходит от локального к удалённому интерактивному поведению через [RDP](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-rdpbcgr/c330f4c8-896f-4d41-983c-aec97fbdbb90).

![24]()

Вот и всё! Теперь я могу обмениваться наборами данных и Notebooks'ами, позволяя другим **получать доступ, запускать и проверять каждую аналитику из одного места**!

![25]()

## На что это похоже на самом деле?
Если вы хотите увидеть процесс с помощью **одного клика мыши** в браузере, прежде чем попробовать, я собрал видео, показывающее, как легко любому человеку в мире получить доступ к Notebooks'ам с детектором и предварительно записанным наборам данных, а также возможность интерактивного запуска каждого аналитика через веб-браузер и открытую вычислительную среду, используя потрясающие возможности **BinderHub**.

ОБНОВЛЕНИЕ 12/18/19: структура папок в видео устарела. Тем не менее, концепция остаётся неизменной, и я считаю, что самое важное увидеть, как она работает.

[![Watch the video]()

Я поделился тем же видео в рамках беседы ATT&CKcon 2.0 с Хосе Луисом Родригесом ([Jose Luis Rodriguez](https://medium.com/@cyb3rpandah)), а также слайдами нашей презентации, приведёнными ниже:

 ![26]() **Роберто Родригес** ([Roberto Rodriguez](https://twitter.com/Cyb3rWard0g))
 @Cyb3rWard0g

 [@Cyb3rPandaH](https://twitter.com/Cyb3rPandaH) вместе со мной весело провели время [@MITREattack](https://twitter.com/MITREattack) [#ATTACKcon](https://twitter.com/hashtag/ATTACKcon?src=hash), делясь своими исследованиями Notebook'ов [@ProjectJupyter](https://twitter.com/ProjectJupyter), [@mybinderteam](https://twitter.com/mybinderteam), [@HunterPlaybook](https://twitter.com/HunterPlaybook) и [@Mordor_Project](https://twitter.com/Mordor_Project) [#ThreatHunting](https://twitter.com/hashtag/ThreatHunting?src=hash)
 Обсуждение: [youtube.com/watch?v=L3KxKA](https://www.youtube.com/watch?v=L3KxKAGSJp4&feature=youtu.be&t=7848)...
 Слайды: [speakerdeck.com/cyb3rward0g/re](https://speakerdeck.com/cyb3rward0g/ready-to-att-and-ck-bring-your-own-data-byod-and-validate-your-data-analytics)...
 Демонстрация BinderHub: [https://youtu.be/mQZFHbnDH4A](https://www.youtube.com/watch?v=mQZFHbnDH4A&feature=youtu.be)...

 [YouTube at](https://twitter.com/YouTube) 🏠 ‎@YouTube
 [![Watch the video]()

## Правила использования Binder
Несмотря на то, что BinderHub предоставляет общедоступную услугу BinderHub любому пользователю, существуют определённые указания, которым следует следовать. Подробнее об этом можно прочитать [здесь](https://mybinder.readthedocs.io/en/latest/user-guidelines.html).
- Threat Hunter Playbook может бытьвременно заблокирована в любое время, если она представляет собой нежелательное поведение, определённое командой Binder. Я рассматриваю проект как доказательство концепции, а также как способ демонстрации и обмена Notebook'ами обнаружения в результате открытого публичного исследования.
- Команда Binder не хочет, чтобы один репозиторий доминировал над всем трафиком в Binder, поэтому они установили максимальный лимит одновременных пользовательских сеансов, которые указывают на одну и ту же ссылку Binder. **Максимальное количество одновременных пользователей для данного репозитория составляет 100**.

Если у вас есть дополнительные вопросы, вы можете начать с просмотра раздела «[Часто задаваемые вопросы](https://mybinder.readthedocs.io/en/latest/faq.html#)» документации Binder. Очень полезно!
## Расширение возможностей сообщества
Я не видел, чтобы эта технология использовалась ранее для подобных случаев в сообществе Infosec, и я очень рад, что [Threat Hunter Playbook](https://github.com/hunters-forge/ThreatHunter-Playbook) - **первый публичный проект** в нашей отрасли, использующий [BinderHub](https://binderhub.readthedocs.io/en/latest/overview.html), чтобы делиться обнаружениями с другими людьми по всему миру и позволять каждому, у кого есть веб-браузер воспроизводить исследования с помощью [Notebook'ов](https://twitter.com/ProjectJupyter) и предварительно записанных наборов данных.

![27]()

## Будущая работа
- Я готовлю материал для нескольких семинаров и учебных занятий, используя все проекты, упомянутые в этой статье, так что следите за обновлениями!
- Создавайте официальный BinderHub для сообщества Infosec, чтобы делиться любыми исследованиями через Notebook (надеясь получить спонсора от сообщества).
- В этом году (2019) я смог сделать живую демонстрацию на саммите [SANS Threat Hunting Summit](https://www.sans.org/event/threat-hunting-and-incident-response-summit-2019/summit-agenda?msc=home) и [ATT&CKcon 2.0](https://www.mitre.org/attackcon-agenda), и это был первый раз, AFAIK, когда кто-то из аудитории смог интерактивно запустить и проверить аналитику во время презентации, не имея предварительно построенной инфраструктуры от меня или других людей, присутствующими на саммите. Я планирую сделать больше в 2020 году с более широкой аудиторией и поднять задание «Demo Gods» на новый уровень.

<h3 align="center">. . .</h3>

На этом всё! Надеюсь, вам понравилось! Любой отзыв приветствуется 😃. Также, если захотите продолжить разговоры об этой технологии и всех других проектах, упомянутых в этой статье, и быть в курсе будущих обучающих мероприятий, не стесняйтесь присоединиться к группе Hunters Forge Slack.

Автоматическое бесплатное приглашение: https://launchpass.com/threathunting

Кроме того, если вы находитесь в районе метро DC, не стесняйтесь присоединиться к встрече NOVA Threat Hunters Forge.

URL-адрес встречи: https://www.meetup.com/NOVA-Threat-Hunters-Forge/

## Ссылки на производительность и затраты BinderHub:
- [Дашборд Grafana в реальном времени](https://grafana.mybinder.org/d/3SpLQinmk/1-overview?orgId=1&refresh=1m).
- [Предпоследняя версия набора данных о затратах](https://github.com/jupyterhub/binder-data/tree/master/billing/data/proc).
- [Binder Notebook для показа стоимости за день](https://mybinder.org/v2/gh/jupyterhub/binder-billing/master?urlpath=lab/tree/analyze_data.ipynb).

## Остальные ссылки
https://docs.microsoft.com/en-us/windows/win32/secauthz/access-control-lists

https://mordor.readthedocs.io/en/latest/

https://mybinder.readthedocs.io/en/latest/index.html

https://binderhub.readthedocs.io/en/latest/

https://github.com/hunters-forge/ThreatHunter-Playbook

https://binderhub.readthedocs.io/en/latest/overview.html#what-happens-when-a-user-clicks-a-binder-link

https://mybinder.org/

https://www.youtube.com/watch?v=fZ8S1uI_hcA
