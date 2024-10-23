###  Fitness Hall Web Application


![image](https://github.com/user-attachments/assets/cfc2cc08-817e-46a0-970b-6d9f7b6fe0ad)



Состав команды:
- Кириллова Мария - 3 курс ИВТ-22-22
- Степанишвили Анна - 3 курс ИВТ-22-22
- Соловьев Илья - 3 курс ИВТ-22-22

##
- [Fitness Hall Web Application](#Start)
   - [Постановка задачи](#Task)
      - [Модель угроз](#Threats)
      - [Модель нарушителя](#Intruder)
   - [Архитектура (до кибериммунизации)](#Architecture1)
      - [Негативные сценарии](#NegativeScenarios)
      - [Позитивные сценарии](#PositiveScenarios)
   - [Архитектура (после кибериммунизации)](#Architecture2)
      - [Решение](#Decision)
##

## <a name="Task"> Постановка задачи</a>
Необходимо создать безопасную среду для пользования веб-приложением фитнес зала.
Обеспечить защиту личных и медицинских данных клиентов.
Обеспечить безопасность финансовых операций.
Предотвратить несанкционированный доступ и мошенничество.
Создать надежное взаимодействие между клиентами и персоналом фитнес зала.

## <a name="Threats">Модель угроз</a>
![export](https://github.com/user-attachments/assets/02a122c2-70e7-416b-bd0f-ceb3b639c976)
## <a name="Intruder">Модель нарушителя</a>
![export (2)](https://github.com/user-attachments/assets/33a3c8c2-7659-4064-8955-39fcf3e04381)


## <a name="Purposes">Цели безопасности</a>
1. Защита личной информации пользователей.
2. Предотвращение мошеннических действий и фальсификации данных. 
3. Обеспечение безопасности финансовых транзакций и предотвращение финансовых потерь.

## <a name="Task"> Архитектура
Архитектура веб-приложения для фитнес-зала должна быть масштабируемой, надежной и обеспечивать удобный пользовательский опыт.  Ниже представлена архитектура с использованием микросервисной модели, которая позволяет отдельному функционалу развиваться независимо и масштабироваться по мере необходимости.

**1. Клиентский интерфейс (Frontend):**

* **Технологии:** React, Angular, Vue.js, или аналогичные фреймворки для создания динамического и интерактивного интерфейса.  Использование SPA (Single Page Application) обеспечивает плавную навигацию и улучшенный пользовательский опыт.
* **Компоненты:**
    * **Регистрация/Авторизация:**  Обращения к микросервисам для проверки данных и создания учетных записей.  Использование OAuth 2.0 для интеграции с социальными сетями.
    * **Запись на занятия:**  Календарь занятий, фильтрация по типу, времени и тренеру, возможность выбора и бронирования мест.
    * **Просмотр абонементов:**  Список активных абонементов, их условия, даты действия.
    * **Профиль пользователя:**  Изменение данных, контактная информация, оплаченные абонементы.
    * **Взаимодействие с персоналом:**  Система сообщений, чаты, возможность задавать вопросы и получать ответы.
    * **Оплата:**  Интеграция с платежными системами (Stripe, PayPal).
    * **Мобильное приложение (необязательно):**  Для удобства доступа к функционалу с мобильных устройств.
* **Связь с бэкендом:**  Использование REST API для получения и отправки данных.

**2. Микросервисный бэкенд (Backend):**

* **Технологии:** Node.js с Express.js или Python с Flask/Django, Go, Java Spring Boot.  Выбор зависит от потребностей и опыта команды.
* **Микросервисы:**
    * **Пользователи:**  Управление учетными записями пользователей, авторизация, аутентификация.
    * **Занятия:**  Управление расписанием, типами занятий, тренерами, местами. Поддержка гибких настроек расписания, включая возможность добавления или удаления занятий в реальном времени.
    * **Абонементы:**  Управление абонементами, их типами, ценами, условиями. Поддержка различных типов абонементов и гибкой системы скидок.
    * **Платежи:**  Обработка платежей, интеграция с платежными системами.  Важно обеспечить безопасность платежных транзакций.
    * **Персонал:**  Управление данными о тренерах, их расписание, квалификация.
    * **Сообщения:**  Система сообщений для взаимодействия клиентов с персоналом.
    * **Отчеты и аналитика:**  Обработка данных о посещаемости, продаже абонементов, спросе на занятия.
* **База данных:**  PostgreSQL, MySQL, MongoDB (для гибких запросов).  Выбор зависит от структуры данных и требований к производительности.  Важно обеспечить надежное хранение данных и их безопасность.
* **API Gateway:**  Контроллер внешнего доступа к микросервисам, обеспечивающий безопасность, маршрутизацию и контроль.

![image](https://github.com/user-attachments/assets/259b7842-b7a7-42aa-bf03-678f1771b219)

# Негативные сценарии работы

1. Получение доступа к адресам, телефонам, платежным данным и деталям заказа:
Злоумышленник находит уязвимость в API Gateway, которая позволяет ему получить доступ к незащищенным запросам к микросервисам. Он подделывает запрос, например, от имени клиента, и получает информацию о заказах, адресах, телефонах и платежных данных. Backend микросервисы не проверяют аутентификацию или авторизацию достаточным образом.
Последствия: Кража конфиденциальной информации, мошеннические платежи, подмена заказов, кража личности.

2. Создание фальшивых электронных писем или веб-сайтов, имитирующих маркетплейсы:
Злоумышленник создает фальшивый веб-сайт или электронное письмо, имитирующее интерфейс маркетплейса. Пользователь, попав на этот сайт, вводит свои данные (включая платежную информацию), которые затем крадутся злоумышленником. API Gateway не проверяет подлинность веб-сайтов или электронных писем.
Последствия: Кража конфиденциальной информации, мошеннические платежи, подмена заказов, репутационные потери.

3. Недостаточная защита баз данных или слабая кибербезопасность на стороне продавца:
Злоумышленник ищет и находит уязвимость в базе данных, связанной с заказом, например, через SQL-инъекцию. Он получает доступ к базе данных, копирует данные пользователей и платежную информацию. Система безопасности на стороне продавца не обеспечивает надлежащую защиту от вторжений.
Последствия: Кража конфиденциальной информации, мошеннические платежи, подмена заказов, репутационные потери, возможные штрафы от регуляторов.

4. Поддельные или вредоносные платежные страницы:
Злоумышленник создает поддельный сайт оплаты, который выглядит как оригинальный. Пользователь, перейдя по ссылке из фишингового письма, вводит свои данные на поддельной странице. API Gateway не имеет механизма проверки подлинности платежных страниц.
Последствия: Кража платежных данных, мошеннические платежи, подмена заказов, репутационные потери, возможные штрафы.

5. Риск вмешательства или краж во время транспортировки заказов:
Злоумышленник перехватывает данные о доставке, например, через уязвимости в системах отслеживания. Или, в физическом смысле, ворует посылку, содержащую товары. Система не предусматривает надежной защиты данных о доставке.
Последствия: Потеря товара, мошеннические действия, репутационные потери.

6. Использование уязвимых или небезопасных приложений или программ:
Пользователь использует уязвимое приложение. Злоумышленник может загрузить вредоносный код, который украдет данные с устройства пользователя. Система не предусматривает механизм проверки безопасности используемых приложений.
Последствия: Кража конфиденциальной информации, мошеннические платежи, установка вредоносного ПО на устройства пользователей, репутационные потери.

Общие последствия для архитектуры:

-Потеря доверия клиентов: Все эти сценарии могут привести к потере доверия пользователей к сервису.

-Финансовые потери: Мошенничество и утечка информации могут привести к существенным финансовым потерям для компании.

-Репутационные потери: Утечка данных и мошенничество могут значительно навредить репутации компании.

-Юридические проблемы: В зависимости от законодательства, компания может столкнуться с юридическими проблемами.

Эти сценарии подчеркивают важность многоуровневой безопасности, включая защиту на frontend, API Gateway, backend микросервисах, в хранилище данных, а также на стороне клиента. 


# Положительные сценарии 

1. Безопасная и надежная обработка платежей:
Пользователь успешно оплачивает абонемент. Система использует надежные платежные шлюзы (например, Stripe, PayPal) и шифрование данных. API Gateway и Payment Service прошли аудит безопасности и соответствуют всем нормативным требованиям. Пользователь получает подтверждение оплаты и доступ к услугам.
Результат: Успешная транзакция, удовлетворенный пользователь, защита финансовых данных.

2. Удобное и эффективное взаимодействие с персоналом:
Клиент задает вопрос тренеру через систему сообщений. Система уведомляет тренера о новом сообщении, и он отвечает клиенту в течение установленного времени. Пользователь получает своевременный и вежливый ответ, что улучшает его опыт использования приложения.
Результат: Быстрое и качественное обслуживание клиентов, повышенная удовлетворенность, улучшение отношений клиент-персонал.

3. Безопасный и надежный доступ к данным:
Клиент авторизуется в приложении, используя надежные механизмы аутентификации. Система проверяет подлинность пользователя и предоставляет ему доступ к личному профилю и истории заказов. Пользователь уверен, что его данные защищены и конфиденциальны.
Результат: Безопасный и надежный доступ к личной информации, защита от несанкционированного доступа, уверенность пользователя.

4. Удобная и эффективная система бронирования:
Клиент успешно записывается на тренировку в нужное время и день. Система отображает доступные слоты и позволяет выбрать наиболее подходящий для клиента вариант. Пользователь быстро и легко находит удобное время для занятий.
Результат: Удобство и эффективность, удовлетворенный пользователь, эффективный менеджмент расписания, высокая загрузка занятий.

5. Эффективное управление заказами и доставкой:
Клиент оформляет заказ на дополнительное оборудование. Система автоматически отправляет уведомление о статусе заказа и его доставке. Клиент получает товар в срок и в соответствии с условиями.
Результат: Эффективное и надежное управление заказами, удовлетворенный пользователь, своевременная доставка.

6. Масштабируемость и надежность:
В период высокого спроса приложение обрабатывает большое количество запросов без задержек. Система масштабируется в соответствии с потребностями, обеспечивая непрерывную работу и доступность для всех клиентов.
Результат: Высокая доступность сервиса, удовлетворенность клиентов, эффективное обслуживание большого количества пользователей.

7. Интеграция с другими системами:
Приложение успешно интегрировано с системой оплаты. Пользователь может оплатить тренировки и другие услуги быстро и безопасно.
Результат: Удобство и эффективность, удовлетворенность клиентов, расширение функционала.

Общие позитивные сценарии:

-Повышенная эффективность работы: Автоматизация процессов, сокращение времени на выполнение задач.

-Улучшение клиентского опыта: Удобство, персонализированный подход, быстрая поддержка.

-Повышение прибыли: Улучшение эффективности продаж, сокращение потерь.

Эти позитивные сценарии демонстрируют, как архитектура может работать в идеальном случае. Важно понимать, что позитивные сценарии должны быть дополнены анализом потенциальных угроз и рисков, чтобы обеспечить надежность и безопасность приложения в реальных условиях.

# Архитектура(после киберимммунизации)
![image](https://github.com/user-attachments/assets/14f17b8b-9f33-49a7-942a-ad558ce0d2c2)

Решения для негативных сценариев работы архитектуры веб-приложения фитнес-зала, должны быть многоуровневыми и охватывать все этапы работы приложения.

1. Получение доступа к адресам, телефонам, платежным данным и деталям заказа:
Использовать надежные методы аутентификации (например, OAuth 2.0, JWT) и авторизации. Регулярно проверять API-ключи и tokens. Шифрование данных в хранилище и во время передачи. Внедрить строгий контроль доступа к данным на уровне микросервисов. Регулярный аудит безопасности и пентесты. Использовать брандмауэр на уровне API Gateway.

2. Создание фальшивых электронных писем или веб-сайтов, имитирующих маркетплейсы:
Использовать SSL/TLS для защиты коммуникаций. Внедрить механизмы проверки подлинности веб-сайтов и электронных писем (например, проверка сертификатов, валидации доменных имен). Обучить пользователей распознавать фишинг. Использовать двухфакторную аутентификацию. Использовать анти-фишинг инструменты.

3. Недостаточная защита баз данных или слабая кибербезопасность на стороне продавца:
Использовать надежные базы данных с современными системами безопасности. Регулярные обновления баз данных и операционной системы. Внедрить систему мониторинга и логгирования базы данных. Использовать сильные пароли и многофакторную аутентификацию для доступа к базе данных. Проводить регулярные аудиты баз данных, использовать брандмауэры и системы обнаружения вторжений.

4. Поддельные или вредоносные платежные страницы:
Использовать надежные платежные шлюзы (например, Stripe, PayPal). Регулярно обновлять платежные шлюзы и библиотеки. Использовать механизмы проверки подлинности платежных страниц, сравнивая URL, доменные имена и сертификаты. Внедрить систему мониторинга подозрительных платежных операций.

5. Риск вмешательства или краж во время транспортировки заказов:
Использовать надежные системы отслеживания заказов с шифрованием данных. Использовать методы шифрования и аутентификации для защиты данных доставки. Сотрудничать с надежными службами доставки.

6. Использование уязвимых или небезопасных приложений или программ:
Регулярно обновлять все используемые приложения и библиотеки. Использовать инструменты для обнаружения уязвимостей. Обучать пользователей о безопасности приложений и программ. Внедрить систему мониторинга и логгирования на уровне устройств пользователей.

Общие решения:

-Регулярный аудит безопасности: Проводить регулярные аудиты системы на предмет уязвимостей.

-Обучение сотрудников: Обучать сотрудников о мерах безопасности и недопущении угроз.

-Мониторинг системы: Использовать системы мониторинга и логгирования для выявления и устранения проблем.

-Безопасная разработка: Придерживаться принципов безопасной разработки на всех этапах жизненного цикла ПО.

Важно понимать, что решения для негативных сценариев должны быть адаптированы к конкретной архитектуре и функциональности приложения. Необходимо проводить детальный анализ рисков и разрабатывать соответствующие меры предотвращения. Постоянный мониторинг и регулярные обновления безопасности являются ключевыми аспектами обеспечения надежности и безопасности приложения

