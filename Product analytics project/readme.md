## Использованные библиотеки: pandas, matplotlib, plotly

## Описание проекта
В роли продуктового аналитика необходимо рассчитать метрики на базе данных о поведении пользователей в игровом приложении, дать рекомендации о доп. метриках для изучения и необходимых для их исследования доп. данных.
Также ответить на 2 теоретических вопроса по продуктовой аналитике

## Задача
Рассчитать следующие метрики:

- Retention и время, которое ученики проводят в приложении.
- Как ученики переходят с уровня на уровень.
- Метрики монетизации для всей когорты
- Где ученики совершают больше всего платежей?
- Где ученики совершают первые платежи?

## Теоретические вопросы (ответы - в файле проекта)
- Дайте определение ARPPU. В результате изменений в продукте ARPPU снизился. Это хорошо или плохо?
- Предположим, что в результате обновления дизайна продукта вы наблюдаете увеличение среднего времени нахождения пользователей в приложении, но уровень конверсии в покупку снизился. Как вы будете анализировать данное изменение и какие рекомендации вы предложите команде продукта?
  
## Общие выводы

**Предварительная обработка:**\
Получили 4 датафрейма `sessions`, `payments`, `users` и `levels`
- Изучили данные и предобработали их: привели к необходимым типам, проверили на пропуски, дубликаты, добавили необходимые столбцы, подготовили данные к анализу

**Анализ**
- изучили интервалы регистрации пользователей (11-21.05.2023 включительно) и интервалы работы в приложении (сессии 11-30.05.2023 включительно)
- исходя из этих данных, установили дату наблюдения 31.05.2023 и горизонт анализа 10 дней, таким образом, были изучены все предоставленные данные
- изучили время,проводимое в приложении, установили, что:

        - Всего пользователей, воспользовавшихся приложением после регистрации - 31070 человек
        - Из них у 521 пользователя суммарное время сессий составило 0.0 сек
        - Пользователи всего провели в приложении за время наблюдений 74645.84 часов
        - Среднее время пользователя в приложении за время наблюдений 2.4 часа
        - Медианное время пользователя за время наблюдений 0.44 часа
        - Максимальное суммарное время сессий одного пользователя за время наблюдений 143.91 часов
        - Минимальное время сессий пользователя за время наблюдений 0.0 сек
        
- разделили пользователей на когорты (по признаку - "день регистрации"), исследовали для них время, проводимое в приложении и удержание (Retention), установили, что 

        - средний размер когорты 2824 человек
        - основные регистрации пришлись на 15-18 мая (будние дни): ~ 25 из 31 тыс. зарегистрировавшихся
        - в день регистрации приложением начинает пользоваться около 2/3 пользователей
        - в первый день пользователи проводят максимальное время в приложении (в среднем от 20 до 50 минут на пользователя, в зависимости от когорты)
        - на второй день lifetime у всех когорт отмечается значительное сокращение времени пребывания (в 2-2.5 раза)
        - к концу периода наблюдений (10 день lifetime) среднее время пребывания в приложении - менее 10 минут для всех когорт
        - разброс удержания к концу горизонта анализа достаточно существенный, почти в 2 раза - от 0.13 до 0.24 от исходного количества в когорте
        - "лучшими" с точки зрения удержания в пределах горизонта анализа можно считать когорты от 17 и 18.05.: многочисленные (2 и 3 место по числу подключившихся пользователей), с плавно сокращающимся retention в течение периода анализа и итоговым retention > 0.2        
- визуализировали динамику регистраций пользователей и удержание покогортно

**Изучение бизнес-метрик**:
- изучили LTV покогортно, визуализировали
- определили **ARPU = 0.016**
- определили **ARPPU = 1.607**
- определили **Paying share (конверсию в платящих пользователей) = 1.003%**
- определили **DAU = 5110 человек**
- определили **WAU = 10411 человек**
- определили **Sticky factor = 49.09%**

**Изучение взаимодействия пользователей с приложением (прохождение уровней)**:
- изучили и визуализировали воронку прохождения пользователями уровней, определили, что:

        заметна относительная лёгкость перехода с уровня на уровень вплоть до 8го (% переходящих не опускается ниже 80)
        чуть менее половины пользователей приложения дошли в исследуемом периоде только до 5го уровня
        треть пользователей дошла до 8го уровня
        до максимального, 21 уровня, дошёл 1 пользователь
        
    - первая оплата была произведена уже на первом уровне, но это единичный user_id (возможно, ошибка в данных?)
    - некоторое количество первых оплат происходит на первых уровнях, нарастая к 4-5 уровню
    - самое большое число пользователей (>80) первую оплату производит на 8 уровне
    - максимальные по размеру и по количеству оплат - уровни приложения (в порядке убывания) - 8, 9, 10
    - единичная оплата на первом уровне весьма крупная - по размеру превосходит сумму оплат на большинстве уровней
    
**Рекомендуемые для отслеживания бизнес-метрики:**
- к числу основных метрик для отслеживания в приложении отнесём **ARPU (LTV), ARPPU, Paying share (конверсию), Retention_rate**

- также необходимо знать **ROI (возврат на инвестиции)**, что поможет оценить эффективность платёжной модели приложения, оправданность затрат на привлечение, сравнить требуемый уровень основных метрик с фактическим
- для расчёта и анализа ROI необходимо собирать и передавать для анализа **стоимость привлечения (и удержания) пользователей, с разбивкой по источникам привлечения**

- дополнительно полезно собрать и изучить **данные о типе устройств/операционных системах/регионах пользователей**, чтобы производить анализ бизнес-метрик, уточнённых по этим параметрам

