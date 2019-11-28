## Проект автоматизированного тестирования для Voximplant Engine
---

**Автор:** *@MigelDS*

**Стек:** *JavaScript (ES6), Jest, Axios (REST)*

---

### Содержание:

| Каталог | Содержит                                                     |
| :------ | ------------------------------------------------------------ |
| /config | Различные конфигурации для запуска тестов на различном окружении (internal, prod) |
| /src    | Исходники проекта (сценарии, полезные самописные утилиты, константы и т.п.) |
| /tests  | Автотесты для Voxengine                                      |

---

## Как запустить автотесты:

Существует несколько способов это сделать.

**Самый быстрый и простой способ:**

1. Перейти на вкладку [***CI/CD -> pipelines***](https://git.zingaya.com/tests/voxengine_AT/pipelines)

2. Нажать на ***Run Pipeline***

3. Выбрать ветку для тестирования. 

> **NOTE:** *По умолчанию выбрана ветка **master***

4. Начать тестирование, нажав кнопку ***Run Pipeline***

5. Дождаться выполнения тестов. Тестирование состоит из двух этапов

   1. Проверка ***eslint***. Необходима для поддержания единого style-guide'a в проекте. 

      > **NOTE:** *Как разработчику, вам этот шаг не интересен*

   2. Непосредственный ***test***. Выполнение тестов происходит на данном этапе.

6. По итогу выполнения всех шагов pipeline'а у вас появится возможность изучить отчёт прогона тестов. 

   > **NOTE:** *Отчет в виде HTML будет ожидать вас в **Job artifacts**, достаточно просто скачать архив и открыть **report.html***
   >
   > **NOTE:** *Функционал доступа к отчету по ссылка на данный момент в разработке*

**Другой способ запуска** *(в случае наличия прав):*

1. Скачать проект себе локально через ***git clone***

2. В папке с проектом выполнить ***yarn*** для подвязки зависимостей

   > **NOTE:** *Убедитесь в наличии утилиты **yarn** на вашей ОС*

3. Выполнить команду:

   1. ***yarn test-prod***: для тестирования окружения productionЦ
   2. ***yarn test-internal:*** для тестирования окружения internal

4. Получить отчет:

   1. В терминале IDE, 
   2. Либо открыв ***report.html*** в каталоге ***public***

---

**Покрытие тестами:**

На данный момент времени ведутся работы по визуализации процента покрытия всего функционала автотестами.

Процесс визуализации можно отслеживать на [***mindmeister***](https://mm.tt/1366767423?t=9SnP0KS7yg) карте

---

По всем вопросам можно обращаться к @MigelDS

