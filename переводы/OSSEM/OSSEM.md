#### Перевод статьи [OSSEM](https://github.com/hunters-forge/OSSEM/blob/master/README.md)

<h1 align="center">OSSEM</h1>

Open Source Security Events Metadata (OSSEM) — это общественный проект, основной задачей которого является документирование и стандартизация журналов событий безопасности из различных источников данных и операционных систем. События безопасности документируются в формате словаря и могут использоваться в качестве справочника для таких проектов, как ThreatHunter-Playbook, при этом источники данных сопоставляются с аналитикой данных, используемой для подтверждения обнаружения состязательных методов. Кроме того, проект предоставляет общую информационную модель (CIM), которая используется дата-инженерами во время процедур нормализации данных, чтобы позволить аналитикам по безопасности запрашивать и анализировать данные по различным источникам данных. Наконец, проект также предоставляет документацию о структуре и связях, определённых в конкретных источниках данных, чтобы облегчить развитие анализа данных.

<img src="/переводы/OSSEM/Pictures/OSSEM_logo.png" width=300>

# Цели

* Определить и совместно использовать общую информационную модель в целях улучшения стандартизации данных и преобразования журналов регистрации событий безопасности.
* Определить и совместно использовать структуру данных и отношений, определённых в журналах событий безопасности.
* Предоставить сообществу подробную информацию в формате словаря о журналах регистрации событий безопасности.
* Дополнительно узнать о журналах событий безопасности (Windows, Linux и MacOS).
* Получить удовольствие и призадуматься о структуре данных в своём SIEM в вопросах обнаружения!

# Структура проекта

4 основные папки:

* [**Общая информационная модель (CIM)**](./common_information_model):
  * Облегчает нормализацию наборов данных, предоставляя стандартный способ разбора журналов событий безопасности.
  * Организована определёнными объектами, связанными с журналами событий, и более подробно определена в [Словарях данных](./data_dictionaries).
  * Определения каждой организации и её соответствующих названий полей в основном представляют собой общие описания, которые могли бы помочь и ускорить процедуры разбора журналов регистрации событий.
* [**Словари данных**](./data_dictionaries/):
  * Содержит конкретную информацию о журналах событий безопасности, организованных операционной системой, и соответствующих наборах данных.
  * Каждый словарь описывает один журнал событий и имена соответствующих полей событий.
  * Разница между папкой [Общая информационная модель](./common_information_model) и словарями данных заключается в том, что в CIM определения полей более общие, в то время как в словаре данных определение имени каждого поля уникально для конкретного журнала событий.
* [**Модель данных обнаружения**](./detection_data_model):
  * Сосредоточена на определении требуемых данных в виде объектов данных и взаимосвязей между собой, которые необходимы для облегчения создания аналитики данных и подтверждения обнаружения враждебных методов.
  * Это обусловлено удивительной работой MITRE с их проектом [CAR Analytics](https://car.mitre.org/wiki/Main_Page).
  * Информация, необходимая для каждого объекта данных, извлекается из обектов, определённых в [Общей информационной модели](./common_information_model).
* [**Источники данных ATTACK**](./attack_data_sources):
  * Сосредоточена на документировании источников данных, предложенных или связанных с методиками, определёнными в [Корпоративной матрице](https://attack.mitre.org/wiki/Technique_Matrix).
  * Кроме того, здесь источники данных будут сопоставляться с конкретными объектами данных, определёнными в части проекта [Модель данных обнаружения](./detection_data_model), с основной целью создания связи между методами, источниками данных и аналитикой данных.

# Текущий статус: альфа

Проект находится в альфа-стадии, это означает, что содержание находится в разработке. Мы приветствуем любые отзывы и предложения по улучшению проекта.

# Проекты с использованием OSSEM

* [HELK](https://github.com/Cyb3rWard0g/HELK) в настоящее время обновляет свои конвейерные конфигурации.

# Ресурсы

* [Готовы на охоту? Сначала покажите свои данные!](https://cyberwardog.blogspot.com/2017/12/ready-to-hunt-first-show-me-your-data.html).
* [Что нового в Windows 10: 1507 и 1511 версии](https://docs.microsoft.com/en-us/windows/whats-new/whats-new-windows-10-version-1507-and-1511#bkmk-lsass).
* [Скачать события аудита безопасности для Windows (Электронная таблица)](https://www.microsoft.com/en-us/download/details.aspx?id=50034).
* [Расширенные настройки политики аудита безопасности](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/advanced-security-audit-policy-settings).
* [Мониторинг активного каталога на наличие признаков компромисса](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise#audit-account-management).
* [Рекомендации по политике в области аудита](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
* [Используйте переадресацию событий Windows для помощи в обнаружении вторжения](https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection).
* [Минимальный рекомендуемый минимум аудиторской политики](https://docs.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection#a-href-idbkmk-appendixaaappendix-a---minimum-recommended-minimum-audit-policy).
* [Windows ITPro Docs: защита от угроз](https://github.com/MicrosoftDocs/windows-itpro-docs/tree/master/windows/security/threat-protection).

# Автор

* Роберто Родригес (Roberto Rodriguez) [@Cyb3rWard0g](https://twitter.com/Cyb3rWard0g)

# Участники

* Хосе Луис Родригес (Jose Luis Rodriguez) [@Cyb3rPandaH](https://twitter.com/Cyb3rPandaH)
* Нейт Гуагенти (Nate Guagenti) [@neu5ron](https://twitter.com/neu5ron)
* Фред Фрей (Fred Frey) [@FryGuy2600](https://twitter.com/FryGuy2600)
* Рикардо Диас (Ricardo Dias) [@hxnoyd](https://twitter.com/hxnoyd)
