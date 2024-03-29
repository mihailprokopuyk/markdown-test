## Проект автоматизированного тестирования для Voximplant Engine

**Автор:** *@MigelDS*

**Стек:** *JavaScript (ES6), Jest, Axios (REST)*

---

### Содержание:

1. Как запустить автотесты
2. Анализ отчетов
3. Покрытие тестами

| Каталог | Содержит                                                     |
| :------ | ------------------------------------------------------------ |
| /config | Различные конфигурации для запуска тестов на различном окружении (internal, prod) |
| /src    | Исходники проекта (сценарии, полезные самописные утилиты, константы и т.п.) |
| /tests  | Автотесты для Voxengine                                      |

---

## Как запустить автотесты:

Существует несколько способов это сделать.

### **Самый быстрый и простой способ: (*Запуск всех тестов*)**

1. Перейти на вкладку [***CI/CD -> pipelines***](https://git.zingaya.com/tests/voxengine_AT/pipelines)

2. Нажать на ***Run Pipeline***

3. Выбрать ветку для тестирования. 

> **NOTE:** *По умолчанию выбрана ветка **master***

4. Начать тестирование, нажав кнопку ***Run Pipeline***

5. Дождаться выполнения тестов. Тестирование состоит из двух этапов

   1. Проверка ***eslint***. Необходима для поддержания единого style-guide'a в проекте. 

      > **NOTE:** *Как разработчику, вам этот шаг не интересен*

   2. Непосредственный ***test***. Выполнение тестов происходит на данном этапе.

6. По итогу выполнения всех шагов pipeline'а, у вас появится возможность изучить отчёт прогона тестов. 

   > **NOTE:** *Отчет в виде HTML будет ожидать вас в **Job artifacts**, достаточно просто скачать архив и открыть **report.html***
   >
   > **NOTE:** *Функционал доступа к отчету по ссылке на данный момент в разработке*

### **Другой способ запуска** *(в случае наличия прав):*

1. Скачать проект себе локально через ***git clone***

2. В папке с проектом выполнить ***yarn*** для подвязки зависимостей

   > **NOTE:** *Убедитесь в наличии утилиты **yarn** на вашей ОС*

3. Выполнить команду:

   1. ***yarn test-prod***: для тестирования окружения production
   2. ***yarn test-internal:*** для тестирования окружения internal

4. Получить отчет:

   1. В терминале IDE, 
   2. Либо открыв ***report.html*** в каталоге ***public***

5. Запуск индивидуального теста:

   ​	***jest testname.test.js*** из корня проекта


### **Запуск отдельных тестов:**

Для запуска отдельных автотестов необходимо запустить gitlab pipeline с переменной TEST и значением, равным имени фаила автотеста для запуска. Можно перечислить сколь угодно имен, все они будут запущены.

***Пример:*** вам необходимо запустить автотест createASRParams.test.js. Вам необходимо:

1. Перейти на вкладку [***CI/CD -> pipelines***](https://git.zingaya.com/tests/voxengine_AT/pipelines)

2. Добавить в поле "Input variable key" имя переменной ***TEST*** (регистр важен)

3. В поле Input variable value необходимо написать имена тех тестовых фаилов, которые вы хотите запустить. Расширения .test.js, равно как и путь до фаила не важен - Jest найдет тестовый фаил самостоятельно.

   > NOTE: в качестве примера createASRParams.test.js. Для его запуска надо создать переменную TEST, значение которой будет createASRParams
   >
   > ![launch](https://storage.yandexcloud.net/atdata/launchTests.png)

4. Начать тестирование, нажав кнопку ***Run Pipeline***.

5. В результате вы получите результат прогона необходимых тестов:

   ![results](https://storage.yandexcloud.net/atdata/results.png)

---

## Анализ отчетов:

Для анализа прохода вам необходимо заполучить отчет в любом виде (*как это сделать было описано выше*). В конечном итоге будет список автотестов. 

Как правило, во избежание дублирования кода, практически каждый автотест параметризирован, и отличается друг от друга входными данными. 

> Как пример: тестирование URLPlayer на различные аудиоформаты. В данном случае ***тест*** и ***сценарий*** ***параметризированы***, потому что они не меняются из раза в раз, меняются лишь входные данные - формат аудиофаила для проигрывания.

#### Отчет содержит в себе:

* Название фаила теста ( */tests/player/differentAudioExtension.test.js* )
  * Описание того, что должен делать тест ( *URL Player with different extentions [формат]* )
  * Шаги прохождения теста, как правило:
    * Загрузка сценария в приложение ( *should load* )
    * Исполнение сценария и получение обратного результата ( *should run scenario and do something good* )
    * Оценка результата и сравнение с ожидаемым результатом

В случае ***успешного*** прохождения теста, в отчете он будет иметь ***зеленый*** цвет. 

В случае, когда тест по какому-либо поводу ***не был пройден***, он будет иметь, соответственно, ***красный*** цвет.

> **NOTE:** *Большое количество времени уделяется на улучшение взаимодействия между конечным пользователем и автотестом, поэтому постоянно разрабатываются различные механизмы по сообщению об ошибки в отчетах для того, чтобы было интуитивно понятно, где и что именно работает не корректно.*

### Приведем пример анализа. 

Имеется следующий не пройденный автотест:

 ```javascript
 player/markerNegative.test.js
 
 Players markers (negative) scenario
 
     should get correct scenario work result    15.508s
     failed
 
     Error: Failed on waiting for results.     
     Got 3 from scenario, expected 2 by test
 
     at wait (/builds/tests/voxengine_AT/src/utils/helpers.js:85:11)    
     at runMicrotasks (<anonymous>)   
     at processTicksAndRejections (internal/process/task_queues.js:93:5)    
     at Object.<anonymous> 
 ```

 1. Необходимо понять, в каком именно тесте произошла ошибка ( *Players markers (negative) scenario - в данном случае ошибка в тесте, проверяющая негативные сценарии маркеров*).

 2. Изучить предмет тестового сценария в [***confluence***](https://zingaya.atlassian.net/wiki/spaces/TES/pages/753959023/voxengine+AT). 

    > *В данном случае сценарий ставит маркер с правой стороны в середину фаила, так же сценарий ставит негативный маркер, который выходит далеко за пределы длины фаила. Срабатывать должен в обоих случаях*. Сценарий возвращает количество раз, когда маркер сработал.

 3. Зная, что сценарий возвращает количество отработанных маркеров и что их должно быть два, видим, что сценарий вернул ***ответ 3***, в то время как тест ожидает, что ***ответом будет 2***. Вывод: сценарий отработал некорректно, необхоидмо изучать сценарий / логи панели.

Еще пример:

```javascript
player/differentTTS.test.js

TTS from different services [ google ] › should run successfully and get results

    Request failed with status code 404

Или

player/loopOption.test.js

Loop option for URL player › should run successfully and get results
	Error: Failed on waiting for results. 
    
    Got undefined from scenario, expected true by test
    
```

Как правило, такого рода ошибки появлюятся в результате того, что ***тест не дождался ответа от сценария***.

Это обозначает, что сценарий к моменту окончания автотеста либо еще не завершен, соответственно, возвращает еще результат ***undefined***, либо ответ так и не был получен, а это значит, что сценарий не был пройден.

В случае ***ошибки 404*** у теста фактически нет соединения со сценарием в панели. Это может обозначать то, что сценарий завершился раньше положенного, и автотест еще не успел получить результаты, а сценарий уже исполнил Voxengine.terminate() (***или не запускался вовсе***). В данном случае ошибка может быть как на стороне автотестов, так и на стороне сценария.

---

**Покрытие тестами:**

На данный момент времени ведутся работы по визуализации процента покрытия всего функционала автотестами.

Процесс визуализации можно отслеживать на [***mindmeister***](https://mm.tt/1366767423?t=9SnP0KS7yg) карте. Так же, есть документ в [***confluence***](https://zingaya.atlassian.net/wiki/spaces/TES/pages/753959023/voxengine+AT#%D0%9F%D0%BE%D0%BA%D1%80%D1%8B%D1%82%D0%B8%D0%B5-%D0%B0%D0%B2%D1%82%D0%BE%D1%82%D0%B5%D1%81%D1%82%D0%BE%D0%B2%3A)

---

По всем вопросам можно обращаться к @MigelDS
