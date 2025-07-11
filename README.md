# Проект: Оценка риска ДТП

## Описание проекта

Каршеринговая компания рассматривает возможность внедрения системы, оценивающей риск дорожно-транспортного происшествия (ДТП) на выбранном маршруте. Система должна предупреждать водителя о повышенном риске до начала поездки, используя исторические данные о происшествиях.

Проект реализует первичную модель оценки рисков на основе открытых данных ДТП, с акцентом на предсказание вины автомобилистов.

## Постановка задачи

- Разработать модель, предсказывающую риск участия в ДТП.
- Оценить влияние различных факторов (время года, тип дороги, транспортное средство).
- Сделать вывод о возможности создания полноценной системы оценки водительского риска.
- Подготовить рекомендации по дополнительному оснащению ТС (например, датчики, камеры).

## Описание данных

Используются SQL-таблицы, содержащие:

- **collisions** — данные об инцидентах (время, место, тяжесть).
- **parties** — информация об участниках ДТП (тип ТС, виновность, травмы).
- **vehicles** — данные об автомобилях.

Фильтрация:

- Используются только события с участием автомобилей (`unit_type = car`).
- Исключены события с легкими повреждениями (`SCRATCH`).
- Использованы только записи до **2012 года**.

## Как решалась задача

- Загрузка таблиц из SQL-базы.
- Первичный анализ: изучены размеры таблиц и связи по `case_id`.
- Очищены данные: исключены записи вне 2012 года и малозначимые типы повреждений.
- Выполнен статистический анализ: сезонные колебания, временные пики, распределение по дням недели.
- Построена модель предсказания виновности (`at_fault`) с учётом признаков: погода, месяц, тип дороги и др.
- SHAP-анализ — для определения важнейших признаков.

## Использованные технологии

- `Python`, `Pandas`, `Seaborn`, `Matplotlib`
- `SQL`
- `Sklearn`, `CatBoostClassifier`, `LogisticRegression`, `RandomForest`
- `SHAP`

## Результаты

- Обнаружена сильная сезонная и погодная зависимость количества ДТП.
- Тип дороги, месяц и возраст водителя оказались одними из ключевых факторов.
- Модель `CatBoostClassifier` достигла удовлетворительной точности в определении виновности (`roc_auc = 66` `f1 = 63`).
- Предложены меры по улучшению: сбор более полных данных, использование GPS-трекинга, внедрение телеметрии.

## Выводы

- Предсказание риска ДТП на маршруте — задача, потенциально решаемая с помощью ML.
- Исторические данные позволяют выявить закономерности, но для высокой точности нужны дополнительные источники: поведенческие данные, скорость, дорожная обстановка.
- Разработка MVP системы возможна, но требует дополнения датчиков и телеметрии.
