## Анализ данных о сердечно-сосудистых заболеваниях, (поиск инсайтов, составление рекомендаций стейкхолдерам, построение предиктивной модели классификации наличия заболевания).

ЗАДАЧИ ИССЛЕДОВАНИЯ

Исследование проводится на основании данных, полученных по результатам диагностических обследований пациентов с признаками и симптомами сердечно-сосудистых заболеваний при их обращении в медицинские учреждения. 
Задачами исследования являются:
    
    * выявление влияния поведенческих факторов на параметры диагностических признаков и риск возникновения ССЗ.
    
    * выявление взаимосвязи объективных физических признаков (пол, возраст, рост, вес) с диагностическими признаками 
    и риском возникновения ССЗ.
    
    * определение признаков, имеющих статистически значимую взаимосвязь с наличием ССЗ, построение модели прогнозирования
    возникновения ССЗ на основе имеющихся данных, оценка ее точности.
    
ОПИСАНИЕ ДАННЫХ

Датасет сформирован по результатам диагностических обследований и содержит 70 000 записей. 
Признаки можно разделить на следующие типы:

    1. Фактические данные:
        • gender - пол, категориальный признак, 1 - женский, 2 - мужской 1 
        • age - возраст, количественный признак, указан в днях.
        • height - рост, количественный признак, указан в см.
        • weight - вес, количественный признак, указан в кг.
    
    2. Поведенческие факторы:
        • smoke - курение, категориальный признак, 1 - да, 0 - нет.
        • alco - употребление алкоголя, категориальный признак, 1 - да, 0 - нет.
        • active - физическая активность, категориальный признак, 1 - да, 0 - нет.  
    
    3. Диагностические данные:
        • ap_hi - систолическое кровяное давление, количественный признак.
        • lo_hi - диастолическое кровяное давление, количественный признак.
        • cholesterol - уровень холестерина в крови, категориальный признак, 1 - норма, 2 - выше нормы, 3 - значительно выше нормы.
        • gluc - уровень сахара в крови, категориальный признак, 1 - норма, 2 - выше нормы, 3 - значительно выше нормы.

Целевая переменная cardio - сердечно-сосудистое заболевание, категориальный признак, 1 - да, 0 - нет.
В датасете нет пропусков данных, все признаки заполнены. 

ПОДГОТОВКА ДАННЫХ

Данные признаков height, weight, ap_hi, ap_lo, судя по минимальным и максимальным значениям, требуют корректировки.
![image](https://user-images.githubusercontent.com/94550891/209708328-3eaa3a47-baf1-4bfc-bada-d350036af55b.png)

Для указанных выше признаков определим границы выбросов и проведем очистку данных. Рассчитаем границы выбросов, исходя из методики 
межквартильного диапазона с коэффициентом 1.5.

В итоге удалили 1 463 строк.

В данных содержится признак вес weight, играющий существенную роль в причинах возникновения ССЗ. Однако, абсолютное значение 
не всегда объективно, т.к. вес связан с ростом человека. Поэтому в медицине чаще используется относительный показатель 
Индекс массы тела (ИМТ) (англ. body mass index (BMI)).  ИМТ позволяет оценить степень соответствия массы человека и его роста и, 
тем самым, косвенно, оценить, является ли масса недостаточной, нормальной, избыточной (ожирение). Есть несколько методик расчета, 
возьмем самую распространенную (индекс Кетле):

 ИМТ = вес (кг) / (рост(м)) ** 2

Введем признак BMI, рассчитанный по данной формуле. 

АНАЛИЗ ДАННЫХ

После очистки данных итоговый датасет содержит информацию о 68 537 пациентах в возрасте от 39 до 64 лет. 
Половой состав: женщины - 65%, мужчины - 35%. Доля пациентов с диагностированным ССЗ составляет 49%. 
Заболеваемость среди мужчин и женщин практически одинакова: женщины - 49%, мужчины - 50%.
Рассмотрим заболеваемость в различных группах пациентов:

*Поведенческие признаки.* Большинство пациентов не курят, не употребляют алкоголь, физически активны. При этом заболеваемость среди них практически такая же, как в противоположной группе и в среднем по всем пациентам около 49%. Можно предположить, что в имеющихся данных влияние этих признаков на возникновение ССЗ будет небольшим.

*Диагностические признаки и ИМТ.* По отдельным признакам (холестерин, сахар) большинство пациентов имеют нормальные показатели, но при этом высокие уровни заболеваемости. Границы нормального артериального давления довольно условны, взяты исходя из возрастного состава пациентов в диапазоне 120 - 145. Высокий уровень заболеваемости в этой группе может быть связан с неблагоприятными показателями по другим признакам. Хотя доля каждого отдельного признака достаточно высокая, доля пациентов, имеющих нормальные показатели по всем этим признакам, составляет 4.9%. А высокий уровень заболеваемости может быть связан с наличием других факторов, не присутствующих в имеющихся данных. Большинство пациентов (95.1%) имеют хотя бы один признак, не соответствующий норме.

ИССЛЕДОВАНИЕ ЗАВИСИМОСТЕЙ МЕЖДУ ПРИЗНАКАМИ

Исследуем возможные зависимости между признаками:
  
        • Взаимосвязь целевого признака cardio с другими признаками.
    
        • Взаимосвязь поведенческих и диагностических признаков. 
      
        • Взаимосвязь веса с поведенческими и диагностическими признаками

 Для этого рассчитаем корреляцию между признаками и оценим статистическую значимость этой зависимости. Для расчета корреляции воспользуемся следующими методами:
        
        • Матрица корреляции Пирсона - для числовых признаков.
        
        • Хи-квадрат статистика, тест Фишера - для категориальных признаков.
        
        • Бисериальная корреляция - для числовых и категориальных признаков.
При оценке статистической значимости показателей исходим из уровня значимости alfa = 0.05.

1. Взаимосвязь целевого признака cardio с другими признаками.
Определим корреляционную связь и статистическую значимость для дальнейшего использования при построении предиктивной модели.

    1.1. Диагностические показатели - cardio. 
Поскольку систолическое и диастолическое давление - два связанных признака и возникновение ССЗ связано, в первую очередь, с уровнем систолического давления, то в дальнейшем анализе будем рассматривать только этот признак.

Анализ показывает корреляционную зависимость возникновения ССЗ от значений диагностических параметров, подтверждается их статистическая значимость, т.е. P-value < alfa.: 

![image](https://user-images.githubusercontent.com/94550891/209711389-5cebdf6e-899f-442f-85d9-4bbfa6938b69.png)

![image](https://user-images.githubusercontent.com/94550891/209711408-76f2e23b-0e3f-44b0-8910-dc469eb48531.png)

![image](https://user-images.githubusercontent.com/94550891/209711424-5d3cb64e-93c8-4c8a-9043-d2b3877e082b.png)

![image](https://user-images.githubusercontent.com/94550891/209711434-869bc367-aa03-4221-9ae5-53fda7cff2a2.png)

    1.2. Объективные показатели (ИМТ, возраст, пол) - cardio.

Подтверждается статистическая значимость коэффициентов корреляции между ИМТ, возрастом и целевым признаком.

![image](https://user-images.githubusercontent.com/94550891/209712192-cd351264-be86-4829-83f7-8f6466dd6a27.png)

![image](https://user-images.githubusercontent.com/94550891/209712232-8c23aaad-bf7b-457a-904d-6592f506f6e9.png)

![image](https://user-images.githubusercontent.com/94550891/209712253-aa114c69-9d2a-49bb-8a7b-593e78854736.png)

![image](https://user-images.githubusercontent.com/94550891/209712272-3782c667-1c1e-438d-beeb-8f878cbf7806.png)

Влияние пола не является статистически значимым (p-value = 0.06). 

    1.3. Поведенческие факторы - cardio.

Корреляция присутствует, хотя значения невысокие. Статистическая значимость подтверждается. Отрицательные коэффициенты корреляции признаков smoke и alco c целевым признаком свидетельствуют о том, что в исследуемом наборе данных у пациентов, курящих и употребляющих алкоголь, меньшая склонность к ССЗ.

![image](https://user-images.githubusercontent.com/94550891/209712462-a7683295-59f1-4703-9a17-bad638e35fe8.png)

2. Взаимосвязь поведенческих и диагностических признаков.
Поведенческие признаки smoke и alco имеют положительные коэффициенты корреляции с диагностическими показателями, т.е. у пациентов, курящих и употребляющих алкоголь, более высокие уровни давления и холестерина (исключение: smoke - gluc), что, в свою очередь, способствует возникновению ССЗ.

![image](https://user-images.githubusercontent.com/94550891/209712539-5e5adffb-e6e8-4939-9c52-22028fb40692.png)

![image](https://user-images.githubusercontent.com/94550891/209712559-33e30778-08a6-499b-9ee3-6dec88c5818b.png)

![image](https://user-images.githubusercontent.com/94550891/209712576-82b0f2a8-b44a-406a-be24-c6ee5df95ee3.png)

3. Взаимосвязь ИМТ с поведенческими и диагностическими признаками.
Из объективных признаков рассмотрим отдельно взаимосвязь веса с поведенческими и диагностическими признаками. Вес во многом зависит от образа жизни, питания, а излишний вес (ожирение) является одной из причин возникновения различных заболеваний, в т.ч. сердечно-сосудистых и приводит к изменению различных диагностических признаков.
Подтверждается положительная корреляция ИМТ с диагностическими признаками:

![image](https://user-images.githubusercontent.com/94550891/209712633-b7399f14-762c-4331-a5eb-018f411cce3c.png)

Коэффициенты корреляции ИМТ с поведенческими признаками имеют небольшие значения, но связь статистически значима.

![image](https://user-images.githubusercontent.com/94550891/209712667-6d299a9a-1c8a-4e01-b70a-c299e19b8deb.png)

В части физической активности можно однозначно сказать, что большая активность способствует снижению веса. Отрицательный коэффициент корреляции с признаком smoke подтверждает факт увеличения веса у бросивших курить. В части алкоголя сложно определить причинную связь: алкоголь способствуют увеличению веса или просто среди полных людей больше употребляющих алкоголь.

ПОСТРОЕНИЕ ПРЕДИКТИВНОЙ МОДЕЛИ

Для решения задачи прогнозирования возникновения ССЗ (целевой признак cardio) воспользуемся несколькими моделями классификации и сравним их качество, используя функцию roc_auc_score.
Также построим для каждой модели Матрицу путаницы, рассчитаем показатели. Для данной тематики важным показателем является Recall (чувствительность), который показывает сколько заболевших определила модель из общего количества, имеющих заболевание  

    1. Логистическая регрессия.
Изменение набора зависимых признаков незначительно влияет на точность модели. При наборе признаков: BMI, ap_hi, cholesterol, gluc, smoke, alco, active, age_years.

![image](https://user-images.githubusercontent.com/94550891/209713054-06164097-7fc4-4600-a0e4-8577ed5bd38f.png)

![image](https://user-images.githubusercontent.com/94550891/209713080-7f8ab405-8250-4b21-8507-3b16bd3f35ce.png)

    2. Варьирование гиперпараметров модели.
Используем класс GridSearchCV для варьирования гиперпараметров “С” и “penalty” модели Логистической регрессии. Последующее обучение модели с параметрами С = 2, penalty = l2 не дали улучшения точности предсказания. Чувствительность ухудшилась.

![image](https://user-images.githubusercontent.com/94550891/209713125-4db948a4-2e89-4562-a76d-d90111d46efa.png)

![image](https://user-images.githubusercontent.com/94550891/209713139-2194a4ad-0fa4-432e-8fb3-762cc889e731.png)

    3. Бэггинг для логистической регрессии.
Точность предсказания, чувствительность не меняются

![image](https://user-images.githubusercontent.com/94550891/209713202-43c98285-a14e-4b67-a794-d0c3e1747ac2.png)

![image](https://user-images.githubusercontent.com/94550891/209713218-99ed3b48-df86-42ac-b2e6-c39255e58e3b.png)

    4. Случайный лес.
Увеличение точности на обучающей выборке, на тестовой практически не изменилась. Чувствительность увеличилась.

![image](https://user-images.githubusercontent.com/94550891/209713242-9f716684-f3c4-4899-bcd6-d546e405d34f.png)

![image](https://user-images.githubusercontent.com/94550891/209713252-9ae13d41-a93b-48df-b533-301240dc4c16.png)

    5. Градиентный бустинг.
Увеличение точности на обучающей выборке, на тестовой не изменилась. Чувствительность увеличилась.

![image](https://user-images.githubusercontent.com/94550891/209713282-1fcc1f17-9081-47d8-b086-2f5d795704d0.png)

![image](https://user-images.githubusercontent.com/94550891/209713289-40017bed-39a9-40a4-b8db-1ab777f7d8ad.png)

Разница между моделями не столь существенна в части точности предсказания на тестовой выборке 0.726 - 0.729, на обучающей более значимая 0.726 - 0.742. 

Чувствительность 0.664 - 0.705, т.е. модель в лучшем случае определяет только 70% пациентов фактически имеющих заболевание. В то же время показатель правильности определения среди пациентов, не имеющих заболевание  Специфичность : specificity имеет более высокие значения и изменяется в диапазоне 0.782 - 0.789. 

Разница в значениях данных показателей связана с величиной порогового значения функций активации моделей. В задаче прогнозирования заболеваний лучше ошибиться, спрогнозировав заболевание у здорового человека (в дальнейшем отменить при медицинских обследованиях), чем не определить у действительно болеющего.
