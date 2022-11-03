# 📊 Базовый SQL

Создана БД, которая хранит информацию о венчурных фондах и инвестициях в компании-стартапы. Эта база данных основана на датасете [Startup Investments](https://www.kaggle.com/datasets/justinas/startup-investments). Необходимо проанализировать данные о фондах и инвестициях и написать запросы к базе.

## ER-diagram

![ER DB](https://user-images.githubusercontent.com/62104692/199701294-c0488e79-7985-4463-87f9-358a02c1d8af.png)

✏️ **`acquisitions`** - содержит информацию о покупках одних компаний другими. 

![acquisitions](https://user-images.githubusercontent.com/62104692/199701352-6315796c-07cd-4a56-84f6-ae0a9b5b5078.png)
<details>
<summary> см. описание табл </summary>
	
* первичный ключ `id` — идентификатор или уникальный номер покупки;
* внешний ключ `acquiring_company_id` — ссылается на таблицу company — идентификатор компании-покупателя, то есть той, что покупает другую компанию;
* внешний ключ `acquired_company_id` — ссылается на таблицу company — идентификатор компании, которую покупают;
* `term_code` — способ оплаты сделки:
* `cash` — наличными;
* `stock` — акциями компании;
* `cash_and_stock` — смешанный тип оплаты: наличные и акции.
* `price_amount` — сумма покупки в долларах;
* `acquired_at` — дата совершения сделки;
* `created_at` — дата и время создания записи в таблице;
* `updated_at` — дата и время обновления записи в таблице.

</details>

------
✏️ **`company`** - содержит информацию о компаниях-стартапах. 
	
![offices](https://user-images.githubusercontent.com/62104692/199702494-f98440e2-432e-4b20-9ee8-01be16226b10.png)
<details>
<summary> см. описание табл </summary>
	
* первичный ключ `id` — идентификатор, или уникальный номер компании;
* `name` — название компании;
* `category_code` — категория деятельности компании, например:
* `news` — специализируется на работе с новостями;
* `social` — специализируется на социальной работе.
* `status` — статус компании: acquired — приобретена; operating — действует; ipo — вышла на IPO; closed — перестала существовать.
* `founded_at` — дата основания компании;
* `closed_at` — дата закрытия компании, которую указывают в том случае, если компании больше не существует;
* `domain` — домен сайта компании;
* `twitter_username` — название профиля компании в твиттере;
* `country_code` — код страны, например, USA для США, GBR для Великобритании;
* `investment_rounds` — число раундов, в которых компания участвовала как инвестор;
* `funding_rounds` — число раундов, в которых компания привлекала инвестиции;
* `funding_total` — сумма привлечённых инвестиций в долларах;
* `milestones` — количество важных этапов в истории компании;
* `created_at` — дата и время создания записи в таблице;
* `updated_at` — дата и время обновления записи в таблице.

</details>

------
	
✏️ **`education`** - хранит информацию об уровне образования сотрудников компаний.
	
![degrees](https://user-images.githubusercontent.com/62104692/199702167-0b839208-bca1-413d-8702-6d5219e2107b.png)
<details>
<summary> см. описание табл </summary>

* первичный ключ `id` — уникальный номер записи с информацией об образовании;
* внешний ключ `person_id` — ссылается на таблицу people — идентификатор человека, информация о котором представлена в записи;
* `degree_type` — учебная степень;
* `instituition` — учебное заведение, название университета;
* `graduated_at` — дата завершения обучения, выпуска;
* `created_at` — дата и время создания записи в таблице;
* `updated_at` — дата и время обновления записи в таблице.
	
</details>

------
✏️ **`fund`** - хранит информацию о венчурных фондах.
	
![funds](https://user-images.githubusercontent.com/62104692/199701705-a1978cbb-c812-495b-bc27-45d11b0a5cc2.png)
<details>
<summary> см. описание табл </summary>

* первичный ключ `id` — уникальный номер венчурного фонда;
* `name` — название венчурного фонда;
* `founded_at` — дата основания фонда;
* `domain` — домен сайта фонда;
* `twitter_username` — профиль фонда в твиттере;
* `country_code` — код страны фонда;
* `investment_rounds` — число инвестиционных раундов, в которых фонд принимал участие;
* `invested_companies` — число компаний, в которые инвестировал фонд;
* `milestones` — количество важных этапов в истории фонда;
* `created_at` — дата и время создания записи в таблице;
* `updated_at` — дата и время обновления записи в таблице.	
	
</details>

------	
✏️ **`funding_round`** - содержит информацию о раундах инвестиций. 
	
![funding_rounds](https://user-images.githubusercontent.com/62104692/199701470-4c9e4a05-7092-4b70-9cc1-eeb00587b69c.png)
<details>
<summary> см. описание табл </summary>

* первичный ключ `id` — уникальный номер инвестиционного раунда;
* внешний ключ `company_id` — ссылается на таблицу company — уникальный номер компании, участвовавшей в инвестиционном раунде;
* `funded_at` — дата проведения раунда;
* `funding_round_type` — тип инвестиционного раунда, например:
* `venture` — венчурный раунд;
* `angel` — ангельский раунд;
* `series_a` — раунд А.
* `raised_amount` — сумма инвестиций, которую привлекла компания в этом раунде в долларах;
* `pre_money_valuation` — предварительная, проведённая до инвестиций оценка стоимости компании в долларах;
* `participants` — количество участников инвестиционного раунда;
* `is_first_round` — является ли этот раунд первым для компании;
* `is_last_round` — является ли этот раунд последним для компании;
* `created_at` — дата и время создания записи в таблице;
* `updated_at` — дата и время обновления записи в таблице.
	
</details>

------	
✏️ **`investment` - содержит информацию об инвестициях венчурных фондов в компании-стартапы.
	
![investments](https://user-images.githubusercontent.com/62104692/199701616-cf80803e-f3bb-4344-baca-99e4f18ef049.png)
<details>
<summary> см. описание табл </summary>

* первичный ключ `id` — уникальный номер инвестиции;
* внешний ключ `funding_round_id` — ссылается на таблицу funding_round — уникальный номер раунда инвестиции;
* внешний ключ `company_id` — ссылается на таблицу company — уникальный номер компании-стартапа, в которую инвестируют;
* внешний ключ `fund_id` — ссылается на таблицу fund — уникальный номер фонда, инвестирующего в компанию-стартап;
* `created_at` — дата и время создания записи в таблице;
* `updated_at` — дата и время обновления записи в таблице.	
	
</details>

------	
✏️ **`people`** - содержит информацию о сотрудниках компаний-стартапов.
	
![people](https://user-images.githubusercontent.com/62104692/199701538-4088f172-b9fb-47c6-ba82-e4484a86fa0d.png)
<details>
<summary> см. описание табл </summary>

* первичный ключ `id` — уникальный номер сотрудника;
* `first_name` — имя сотрудника;
* `last_name` — фамилия сотрудника;
* внешний ключ `company_id` — ссылается на таблицу company — уникальный номер компании-стартапа;
* `twitter_username` — профиль сотрудника в твиттере;
* `created_at` — дата и время создания записи в таблице;
* `updated_at` — дата и время обновления записи в таблице.
	
</details>

------


### 📌 Q1: 
Посчитайте, сколько компаний закрылось.
```sql
SELECT Count(DISTINCT id)
FROM   company
WHERE  status = 'closed'; 
```

### 📌 Q2: 
Отобразите количество привлечённых средств для новостных компаний США. Используйте данные из таблицы company. Отсортируйте таблицу по убыванию значений в поле funding_total.
```sql
SELECT funding_total
FROM   company
WHERE  country_code = 'USA'
       AND category_code = 'news'
ORDER  BY funding_total DESC;
```

### 📌 Q3: 
Найдите общую сумму сделок по покупке одних компаний другими в долларах. Отберите сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.
```sql
SELECT Sum(price_amount)
FROM   acquisition
WHERE  term_code = 'cash'
       AND Extract(year FROM acquired_at) IN ( 2011, 2012, 2013 ); 
```

### 📌 Q4: 
Отобразите имя, фамилию и названия аккаунтов людей в твиттере, у которых названия аккаунтов начинаются на 'Silver'.
```sql
SELECT first_name,
       last_name,
       twitter_username
FROM   people
WHERE  twitter_username LIKE 'Silver%;'
```

### 📌 Q5: 
Выведите на экран всю информацию о людях, у которых названия аккаунтов в твиттере содержат подстроку 'money', а фамилия начинается на 'K'.
```sql
SELECT *
FROM   people
WHERE  twitter_username LIKE '%money%'
       AND last_name LIKE 'K%'; 
```

### 📌 Q6: 
Для каждой страны отобразите общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируйте данные по убыванию суммы.
```sql
SELECT country_code,
       Sum(funding_total)
FROM   company
GROUP  BY country_code
ORDER  BY 2 DESC; 
```

### 📌 Q7: 
Составьте таблицу, в которую войдёт дата проведения раунда, а также минимальное и максимальное значения суммы инвестиций, привлечённых в эту дату.Оставьте в итоговой таблице только те записи, в которых минимальное значение суммы инвестиций не равно нулю и не равно максимальному значению.
```sql
SELECT funded_at,
       Max(raised_amount),
       Min(raised_amount)
FROM   funding_round
GROUP  BY funded_at
HAVING Min(raised_amount) != 0
       AND Min(raised_amount) != Max(raised_amount); 
```

### 📌 Q8: 
Создайте поле с категориями: Для фондов, которые инвестируют в 100 и более компаний, назначьте категорию high_activity. Для фондов, которые инвестируют в 20 и более компаний до 100, назначьте категорию middle_activity. Если количество инвестируемых компаний фонда не достигает 20, назначьте категорию low_activity. Отобразите все поля таблицы fund и новое поле с категориями.
```sql
SELECT *,
       CASE
         WHEN invested_companies >= 100 THEN 'high_activity'
         WHEN invested_companies >= 20
              AND invested_companies < 100 THEN 'middle_activity'
         WHEN invested_companies < 20 THEN 'low_activity'
       END
FROM   fund;
```

### 📌 Q9: 
Для каждой из категорий, назначенных в предыдущем задании, посчитайте округлённое до ближайшего целого числа среднее количество инвестиционных раундов, в которых фонд принимал участие. Выведите на экран категории и среднее число инвестиционных раундов. Отсортируйте таблицу по возрастанию среднего.
```sql
SELECT CASE
         WHEN invested_companies >= 100 THEN 'high_activity'
         WHEN invested_companies >= 20 THEN 'middle_activity'
         ELSE 'low_activity'
       END AS activity,
       Round(Avg(investment_rounds), 0)
FROM   fund
GROUP  BY activity
ORDER  BY 2; 
```

### 📌 Q10: 
Проанализируйте, в каких странах находятся фонды, которые чаще всего инвестируют в стартапы. Для каждой страны посчитайте минимальное, максимальное и среднее число компаний, в которые инвестировали фонды этой страны, основанные с 2010 по 2012 год включительно. Исключите страны с фондами, у которых минимальное число компаний, получивших инвестиции, равно нулю. Выгрузите десять самых активных стран-инвесторов. Отсортируйте таблицу по среднему количеству компаний от большего к меньшему, а затем по коду страны в лексикографическом порядке.
```sql
SELECT country_code,
       Min(invested_companies),
       Max(invested_companies),
       Avg(invested_companies)
FROM   fund
WHERE  Extract(year FROM founded_at) BETWEEN 2010 AND 2012
GROUP  BY country_code
HAVING Min(invested_companies) != 0
ORDER  BY 4 DESC,
          1
LIMIT  10; 
```


### 📌 Q11: 
Отобразите имя и фамилию всех сотрудников стартапов. Добавьте поле с названием учебного заведения, которое окончил сотрудник, если эта информация известна.
```sql
SELECT p.first_name,
       p.last_name,
       e.instituition
FROM   people p
       LEFT JOIN education e
              ON e.person_id = p.id;
```


### 📌 Q12: 
Для каждой компании найдите количество учебных заведений, которые окончили её сотрудники. Выведите название компании и число уникальных названий учебных заведений. Составьте топ-5 компаний по количеству университетов.
```sql
SELECT c.name,
       Count(DISTINCT e.instituition)
FROM   company c
       JOIN people p
         ON p.company_id = c.id
       JOIN education e
         ON e.person_id = p.id
GROUP  BY c.name
ORDER  BY 2 DESC
LIMIT  5; 
```


### 📌 Q13: 
Составьте список с уникальными названиями закрытых компаний, для которых первый раунд финансирования оказался последним.
```sql
SELECT DISTINCT c.NAME
FROM   company c
       JOIN funding_round f
         ON f.company_id = c.id
WHERE  c.status = 'closed'
       AND f.is_first_round = 1
       AND f.is_last_round = 1; 
```


### 📌 Q14: 
Составьте список уникальных номеров сотрудников, которые работают в компаниях, отобранных в предыдущем задании.
```sql
WITH t0
     AS (SELECT DISTINCT c.id
         FROM   company c
                JOIN funding_round f
                  ON f.company_id = c.id
         WHERE  c.status = 'closed'
                AND f.is_first_round = 1
                AND f.is_last_round = 1)
SELECT DISTINCT p.id
FROM   t0
       JOIN people p
         ON t0.id = p.company_id;
```


### 📌 Q15:
Составьте таблицу, куда войдут уникальные пары с номерами сотрудников из предыдущей задачи и учебным заведением, которое окончил сотрудник.
```sql
WITH t0
     AS (SELECT DISTINCT c.id
         FROM   company c
                JOIN funding_round f
                  ON f.company_id = c.id
         WHERE  c.status = 'closed'
                AND f.is_first_round = 1
                AND f.is_last_round = 1)
SELECT DISTINCT p.id,
                e.instituition
FROM   t0
       JOIN people p
         ON t0.id = p.company_id
       JOIN education e
         ON p.id = e.person_id; 
```


### 📌 Q16: 
Посчитайте количество учебных заведений для каждого сотрудника из предыдущего задания. При подсчёте учитывайте, что некоторые сотрудники могли окончить одно и то же заведение дважды.
```sql
WITH t0
     AS (SELECT DISTINCT c.id
         FROM   company c
                JOIN funding_round f
                  ON f.company_id = c.id
         WHERE  c.status = 'closed'
                AND f.is_first_round = 1
                AND f.is_last_round = 1)
SELECT p.id,
       Count(e.instituition)
FROM   t0
       JOIN people p
         ON t0.id = p.company_id
       JOIN education e
         ON p.id = e.person_id
GROUP  BY p.id; 
```


### 📌 Q17: 
Дополните предыдущий запрос и выведите среднее число учебных заведений (всех, не только уникальных), которые окончили сотрудники разных компаний. Нужно вывести только одну запись, группировка здесь не понадобится.
```sql
WITH t0
     AS (SELECT DISTINCT c.id
         FROM   company c
                JOIN funding_round f
                  ON f.company_id = c.id
         WHERE  c.status = 'closed'
                AND f.is_first_round = 1
                AND f.is_last_round = 1),
     t1
     AS (SELECT p.id,
                Count(e.instituition) cnt
         FROM   t0
                JOIN people p
                  ON t0.id = p.company_id
                JOIN education e
                  ON p.id = e.person_id
         GROUP  BY p.id)
SELECT Avg(cnt)
FROM   t1; 
```


### 📌 Q18: 
Напишите похожий запрос: выведите среднее число учебных заведений (всех, не только уникальных), которые окончили сотрудники Facebook*.
```sql
WITH t0
     AS (SELECT DISTINCT c.id
         FROM   company c
         WHERE  c.NAME = 'Facebook'),
     t1
     AS (SELECT p.id,
                Count(e.instituition) cnt
         FROM   t0
                JOIN people p
                  ON t0.id = p.company_id
                JOIN education e
                  ON p.id = e.person_id
         GROUP  BY p.id)
SELECT Avg(cnt)
FROM   t1; 
```


### 📌 Q19: 
Составьте таблицу из полей: name_of_fund — название фонда; name_of_company — название компании; amount — сумма инвестиций, которую привлекла компания в раунде. В таблицу войдут данные о компаниях, в истории которых было больше шести важных этапов, а раунды финансирования проходили с 2012 по 2013 год включительно.
```sql
SELECT f.NAME           AS name_of_fund,
       c.NAME           AS name_of_company,
       fr.raised_amount AS amount
FROM   fund AS f
       INNER JOIN investment AS i
               ON f.id = i.fund_id
       INNER JOIN funding_round AS fr
               ON i.funding_round_id = fr.id
       INNER JOIN company AS c
               ON fr.company_id = c.id
WHERE  c.milestones > 6
       AND Extract(year FROM Cast(fr.funded_at AS TIMESTAMP)) BETWEEN 2012 AND
           2013;
```


### 📌 Q20: 
Выгрузите таблицу, в которой будут такие поля: название компании-покупателя; сумма сделки; название компании, которую купили; сумма инвестиций, вложенных в купленную компанию; доля, которая отображает, во сколько раз сумма покупки превысила сумму вложенных в компанию инвестиций, округлённая до ближайшего целого числа. Не учитывайте те сделки, в которых сумма покупки равна нулю. Если сумма инвестиций в компанию равна нулю, исключите такую компанию из таблицы.
```sql
SELECT c.name                                   AS acquiring_company,
       a.price_amount,
       c2.name                                  AS acquired_company,
       c2.funding_total,
       Round(a.price_amount / c2.funding_total) AS part
FROM   acquisition AS a
       JOIN company AS c
         ON c.id = a.acquiring_company_id
       JOIN company AS c2
         ON c2.id = a.acquired_company_id
WHERE  a.price_amount <> 0
       AND c2.funding_total <> 0
ORDER  BY a.price_amount DESC,
          c2.name
LIMIT  10; 
```


### 📌 Q21: 
Выгрузите таблицу, в которую войдут названия компаний из категории social, получившие финансирование с 2010 по 2013 год включительно. Проверьте, что сумма инвестиций не равна нулю. Выведите также номер месяца, в котором проходил раунд финансирования.
```sql
SELECT c.NAME,
       Extract(month FROM Cast(fr.funded_at AS TIMESTAMP))
FROM   company AS c
       INNER JOIN funding_round AS fr
               ON c.id = fr.company_id
WHERE  c.category_code = 'social'
       AND Extract(year FROM Cast(fr.funded_at AS TIMESTAMP)) BETWEEN 2010 AND
           2013
       AND fr.raised_amount != 0; 
```


### 📌 Q22: 
Отберите данные по месяцам с 2010 по 2013 год, когда проходили инвестиционные раунды. Сгруппируйте данные по номеру месяца и получите таблицу, в которой будут поля: номер месяца, в котором проходили раунды; количество уникальных названий фондов из США, которые инвестировали в этом месяце; количество компаний, купленных за этот месяц; общая сумма сделок по покупкам в этом месяце.
```sql
WITH month_fund
     AS (SELECT Extract(month FROM fr.funded_at) AS MONTH,
                Count(DISTINCT f.NAME)           AS count_of_fund
         FROM   funding_round AS fr
                LEFT JOIN investment AS i
                       ON i.funding_round_id = fr.id
                LEFT JOIN fund AS f
                       ON i.fund_id = f.id
         WHERE  Extract(year FROM fr.funded_at) BETWEEN 2010 AND 2013
                AND f.country_code = 'USA'
         GROUP  BY month),
     month_acquired
     AS (SELECT Extract(month FROM acquired_at) AS MONTH,
                Count(acquired_company_id)      AS count_of_acquired,
                Sum(price_amount)               AS sum_of_acquired
         FROM   acquisition
         WHERE  Extract(year FROM acquired_at) BETWEEN 2010 AND 2013
         GROUP  BY month)
SELECT month_fund.month,
       month_fund.count_of_fund,
       month_acquired.count_of_acquired,
       month_acquired.sum_of_acquired
FROM   month_fund
       JOIN month_acquired
         ON month_fund.month = month_acquired.month; 
```


### 📌 Q23: 
Составьте сводную таблицу и выведите среднюю сумму инвестиций для стран, в которых есть стартапы, зарегистрированные в 2011, 2012 и 2013 годах. Данные за каждый год должны быть в отдельном поле. Отсортируйте таблицу по среднему значению инвестиций за 2011 год от большего к меньшему.
```sql
WITH a
     AS (SELECT country_code,
                Avg(funding_total) AS avg_2011
         FROM   company
         WHERE  Extract(year FROM Cast(founded_at AS TIMESTAMP)) = 2011
         GROUP  BY country_code),
     b
     AS (SELECT country_code,
                Avg(funding_total) AS avg_2012
         FROM   company
         WHERE  Extract(year FROM Cast(founded_at AS TIMESTAMP)) = 2012
         GROUP  BY country_code),
     c
     AS (SELECT country_code,
                Avg(funding_total) AS avg_2013
         FROM   company
         WHERE  Extract(year FROM Cast(founded_at AS TIMESTAMP)) = 2013
         GROUP  BY country_code)
SELECT a.country_code,
       a.avg_2011,
       b.avg_2012,
       c.avg_2013
FROM   a
       INNER JOIN b
               ON a.country_code = b.country_code
       INNER JOIN c
               ON a.country_code = c.country_code
ORDER  BY a.avg_2011 DESC; 
```
