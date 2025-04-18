## Анализ результатов эксперимента по внедрению нового алгоритма рекомендации постов

### Цель
Пользователи были поделены на три группы:
- exp_group = 1 — всё по-старому
- exp_group = 2 — новая рекомендательная система
- exp_group = 0 — всё по-старому

Основная цель эксперимента — проверить гипотезу, что новый алгоритм приведет к увеличению CTR пользователей (метрика отношения количества лайков к общему количеству просмотра постов). Для этого нужно:
- Провести AA-тест для проверки, что группы, на которые поделены пользователи, равнозначны
- Провести AB-тест

### Cтек
Python, pandas, numpy, mathplotlib, seaborn, pandahouse, SQL, ClickHouse, statsmodels, scipy

### Структура проекта
- `CH.py`— в модуле реализован класс Getch, объект которого подключается к базе данных и отправляет запрос, используя в качестве параметров для подключения переменные окружения
- `AA_test.ipynb` — ноутбук с реализацией AA-теста
- `AB_test.ipynb` — ноутбук с реализацией AB-теста

### Этапы реализации
1. Выгрузка данных из БД
2. Реализация АА-теста для проверки, что система сплитования работает корректно (ключевая метрика не отличается между группами пользователей 0 и 1). Для AA-теста данные берутся в дни с `2024-08-23` по `2024-08-29`
3. Сравнение метрики CTR между группой 2 и группой 1 с помощью статистических методов. Для AB-теста данные берутся в дни с `2024-08-30` по `2024-09-05`.
4. Статистический вывод

### Описание данных
Все данные находятся в табличном виде в ClickHouse

**feed_actions**
|Название атрибута|Тип атрибута|
|-|--------|
| user_id | UInt32 |
| post_id | UInt32 |
| action | String |
| time | DATETIME |
| gender | Int8 |
| age | Int16 |
| country | String |
| city | String |
| os | String |
| source | String |
| exp_group | Int8 |

### Результат
С помощью статистических тестов (теста Манна-Уитни, Пуассоновского бутстреп и t-теста поверх бакетного преобразования) выявил статистически значимые различия между целевой и контрольной группами. Показал, что распределение СTR в целевой группе имеет бимодальную форму, что говорит о разделении пользователей внутри группы на две подгруппы. 
