# 📊 Базовый SQL

Создана БД, которая хранит информацию о венчурных фондах и инвестициях в компании-стартапы. Эта база данных основана на датасете [Startup Investments](https://www.kaggle.com/datasets/justinas/startup-investments). Необходимо проанализировать данные о фондах и инвестициях и написать запросы к базе.

## ER-diagram
<details>
<summary>
Раскрыть
</summary>
	
<kbd><img src="https://user-images.githubusercontent.com/81607668/139528817-09766746-e26b-4aa6-9465-bd5cc58a34cc.png" alt="Image" width="750" height="480"></kbd>
  
`acquisition` - содержит информацию о покупках одних компаний другими. 

<kbd><img width="659" alt="image" src="https://user-images.githubusercontent.com/81607668/139528238-dade7402-2d4e-4a66-911e-ad1bd949f2ed.png"></kbd>

`company` - содержит информацию о компаниях-стартапах. 

<kbd><img width="531" alt="image" src="https://user-images.githubusercontent.com/81607668/139528127-64ad1eff-abdb-4e3d-add5-47477ee0d84d.png"></kbd>

`education` - хранит информацию об уровне образования сотрудников компаний.

<kbd><img width="527" alt="image" src="https://user-images.githubusercontent.com/81607668/139528283-926c7663-bdfb-4649-ab37-8c2ca814f47d.png"></kbd>

`fund` - хранит информацию о венчурных фондах.

<kbd><img width="330" alt="image" src="https://user-images.githubusercontent.com/81607668/139528155-b1c44957-d369-41af-a9d7-3da3928f0564.png"></kbd>

`funding_round` - содержит информацию о раундах инвестиций. 

<kbd><img width="792" alt="image" src="https://user-images.githubusercontent.com/81607668/139528190-361da1c5-6512-4df7-860e-e0fdbd45f097.png"></kbd>

`investment` - содержит информацию об инвестициях венчурных фондов в компании-стартапы.

<kbd><img width="427" alt="image" src="https://user-images.githubusercontent.com/81607668/139528173-eb792b62-c98a-47aa-bd80-bfa5c432063b.png"></kbd>

`people` - содержит информацию о сотрудниках компаний-стартапов.

<kbd><img width="427" alt="image" src="https://user-images.githubusercontent.com/81607668/139528173-eb792b62-c98a-47aa-bd80-bfa5c432063b.png"></kbd>

</details>
 
***

### 📌 Q1: Посчитайте, сколько компаний закрылось.
```sql
select 
  count(distinct id) 
from 
  company 
where 
  status = 'closed';
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q2: Отобразите количество привлечённых средств для новостных компаний США. Используйте данные из таблицы company. Отсортируйте таблицу по убыванию значений в поле funding_total .
```sql
select 
  funding_total 
from 
  company 
where 
  country_code = 'USA' 
  and category_code = 'news' 
order by 
  funding_total desc;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q3: Найдите общую сумму сделок по покупке одних компаний другими в долларах. Отберите сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.
```sql
select 
  sum(price_amount) 
from 
  acquisition 
where 
  term_code = 'cash' 
  and extract(
    year 
    from 
      acquired_at
  ) in (2011, 2012, 2013);
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q4: Отобразите имя, фамилию и названия аккаунтов людей в твиттере, у которых названия аккаунтов начинаются на 'Silver'.
```sql
select 
  first_name, 
  last_name, 
  twitter_username 
from 
  people 
where 
  twitter_username like 'Silver%'
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q5: Выведите на экран всю информацию о людях, у которых названия аккаунтов в твиттере содержат подстроку 'money', а фамилия начинается на 'K'.
```sql
select 
  * 
from 
  people 
where 
  twitter_username like '%money%' 
  and last_name like 'K%'
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q6: Для каждой страны отобразите общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируйте данные по убыванию суммы.
```sql
select 
  country_code, 
  sum(funding_total) 
from 
  company 
group by 
  country_code 
order by 
  2 desc;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q7: Составьте таблицу, в которую войдёт дата проведения раунда, а также минимальное и максимальное значения суммы инвестиций, привлечённых в эту дату.Оставьте в итоговой таблице только те записи, в которых минимальное значение суммы инвестиций не равно нулю и не равно максимальному значению.
```sql
select 
  funded_at, 
  max(raised_amount), 
  min(raised_amount) 
from 
  funding_round 
group by 
  funded_at 
having 
  min(raised_amount) != 0 
  and min(raised_amount) != max(raised_amount);
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q8: Создайте поле с категориями: Для фондов, которые инвестируют в 100 и более компаний, назначьте категорию high_activity. Для фондов, которые инвестируют в 20 и более компаний до 100, назначьте категорию middle_activity. Если количество инвестируемых компаний фонда не достигает 20, назначьте категорию low_activity. Отобразите все поля таблицы fund и новое поле с категориями.
```sql
select 
  *, 
  case when invested_companies >= 100 then 'high_activity' when invested_companies >= 20 
  and invested_companies < 100 then 'middle_activity' when invested_companies < 20 then 'low_activity' end 
from 
  fund;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q9: Для каждой из категорий, назначенных в предыдущем задании, посчитайте округлённое до ближайшего целого числа среднее количество инвестиционных раундов, в которых фонд принимал участие. Выведите на экран категории и среднее число инвестиционных раундов. Отсортируйте таблицу по возрастанию среднего.
```sql
SELECT 
  CASE WHEN invested_companies >= 100 THEN 'high_activity' WHEN invested_companies >= 20 THEN 'middle_activity' ELSE 'low_activity' END AS activity, 
  round(
    avg(investment_rounds), 
    0
  ) 
FROM 
  fund 
group by 
  activity 
order by 
  2;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q10: Проанализируйте, в каких странах находятся фонды, которые чаще всего инвестируют в стартапы. Для каждой страны посчитайте минимальное, максимальное и среднее число компаний, в которые инвестировали фонды этой страны, основанные с 2010 по 2012 год включительно. Исключите страны с фондами, у которых минимальное число компаний, получивших инвестиции, равно нулю. Выгрузите десять самых активных стран-инвесторов. Отсортируйте таблицу по среднему количеству компаний от большего к меньшему, а затем по коду страны в лексикографическом порядке.
```sql
select 
  country_code, 
  min(invested_companies), 
  max(invested_companies), 
  avg(invested_companies) 
from 
  fund 
where 
  extract(
    year 
    from 
      founded_at
  ) between 2010 
  and 2012 
group by 
  country_code 
having 
  min(invested_companies) != 0 
order by 
  4 desc, 
  1 
limit 
  10;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q11: Отобразите имя и фамилию всех сотрудников стартапов. Добавьте поле с названием учебного заведения, которое окончил сотрудник, если эта информация известна.
```sql
select 
  p.first_name, 
  p.last_name, 
  e.instituition 
from 
  people p 
  left join education e on e.person_id = p.id
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q12: Для каждой компании найдите количество учебных заведений, которые окончили её сотрудники. Выведите название компании и число уникальных названий учебных заведений. Составьте топ-5 компаний по количеству университетов.
```sql
select 
  c.name, 
  count(distinct e.instituition) 
from 
  company c 
  join people p on p.company_id = c.id 
  join education e on e.person_id = p.id 
group by 
  c.name 
order by 
  2 desc 
limit 
  5;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q13: Составьте список с уникальными названиями закрытых компаний, для которых первый раунд финансирования оказался последним.
```sql
select 
  distinct c.name 
from 
  company c 
  join funding_round f on f.company_id = c.id 
where 
  c.status = 'closed' 
  and f.is_first_round = 1 
  and f.is_last_round = 1
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q14: Составьте список уникальных номеров сотрудников, которые работают в компаниях, отобранных в предыдущем задании.
```sql
with t0 as (
  select 
    distinct c.id 
  from 
    company c 
    join funding_round f on f.company_id = c.id 
  where 
    c.status = 'closed' 
    and f.is_first_round = 1 
    and f.is_last_round = 1
) 
select 
  distinct p.id 
from 
  t0 
  join people p on t0.id = p.company_id;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q15: Составьте таблицу, куда войдут уникальные пары с номерами сотрудников из предыдущей задачи и учебным заведением, которое окончил сотрудник.
```sql
WITH t0 AS (
  SELECT 
    DISTINCT c.id 
  FROM 
    company c 
    JOIN funding_round f ON f.company_id = c.id 
  WHERE 
    c.status = 'closed' 
    AND f.is_first_round = 1 
    AND f.is_last_round = 1
) 
SELECT 
  DISTINCT p.id, 
  e.instituition 
FROM 
  t0 
  JOIN people p ON t0.id = p.company_id 
  join education e on p.id = e.person_id
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q16: Посчитайте количество учебных заведений для каждого сотрудника из предыдущего задания. При подсчёте учитывайте, что некоторые сотрудники могли окончить одно и то же заведение дважды.
```sql
WITH t0 AS (
  SELECT 
    DISTINCT c.id 
  FROM 
    company c 
    JOIN funding_round f ON f.company_id = c.id 
  WHERE 
    c.status = 'closed' 
    AND f.is_first_round = 1 
    AND f.is_last_round = 1
) 
SELECT 
  p.id, 
  count(e.instituition) 
FROM 
  t0 
  JOIN people p ON t0.id = p.company_id 
  join education e on p.id = e.person_id 
group by 
  p.id
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q17: Дополните предыдущий запрос и выведите среднее число учебных заведений (всех, не только уникальных), которые окончили сотрудники разных компаний. Нужно вывести только одну запись, группировка здесь не понадобится.
```sql
WITH t0 AS (
  SELECT 
    DISTINCT c.id 
  FROM 
    company c 
    JOIN funding_round f ON f.company_id = c.id 
  WHERE 
    c.status = 'closed' 
    AND f.is_first_round = 1 
    AND f.is_last_round = 1
), 
t1 as (
  SELECT 
    p.id, 
    count(e.instituition) cnt 
  FROM 
    t0 
    JOIN people p ON t0.id = p.company_id 
    join education e on p.id = e.person_id 
  group by 
    p.id
) 
select 
  avg(cnt) 
from 
  t1;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q18: Напишите похожий запрос: выведите среднее число учебных заведений (всех, не только уникальных), которые окончили сотрудники Facebook*.
```sql
WITH t0 AS (
  SELECT 
    DISTINCT c.id 
  FROM 
    company c 
  WHERE 
    c.name = 'Facebook'
), 
t1 as (
  SELECT 
    p.id, 
    count(e.instituition) cnt 
  FROM 
    t0 
    JOIN people p ON t0.id = p.company_id 
    join education e on p.id = e.person_id 
  group by 
    p.id
) 
select 
  avg(cnt) 
from 
  t1;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q19: Составьте таблицу из полей: name_of_fund — название фонда; name_of_company — название компании; amount — сумма инвестиций, которую привлекла компания в раунде. В таблицу войдут данные о компаниях, в истории которых было больше шести важных этапов, а раунды финансирования проходили с 2012 по 2013 год включительно.
```sql
SELECT 
  f.name as name_of_fund, 
  c.name as name_of_company, 
  fr.raised_amount as amount 
FROM 
  fund as f 
  INNER JOIN investment as i ON f.id = i.fund_id 
  INNER JOIN funding_round as fr ON i.funding_round_id = fr.id 
  INNER JOIN company as c ON fr.company_id = c.id 
WHERE 
  c.milestones > 6 
  and EXTRACT(
    YEAR 
    FROM 
      CAST(fr.funded_at as timestamp)
  ) BETWEEN 2012 
  and 2013;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q20: Выгрузите таблицу, в которой будут такие поля: название компании-покупателя; сумма сделки; название компании, которую купили; сумма инвестиций, вложенных в купленную компанию; доля, которая отображает, во сколько раз сумма покупки превысила сумму вложенных в компанию инвестиций, округлённая до ближайшего целого числа. Не учитывайте те сделки, в которых сумма покупки равна нулю. Если сумма инвестиций в компанию равна нулю, исключите такую компанию из таблицы.
```sql
SELECT 
  c.name AS acquiring_company, 
  a.price_amount, 
  c2.name AS acquired_company, 
  c2.funding_total, 
  ROUND(
    a.price_amount / c2.funding_total
  ) AS part 
FROM 
  acquisition AS a 
  JOIN company AS c ON c.id = a.acquiring_company_id 
  JOIN company AS c2 ON c2.id = a.acquired_company_id 
WHERE 
  a.price_amount <> 0 
  AND c2.funding_total <> 0 
ORDER BY 
  a.price_amount DESC, 
  c2.name 
LIMIT 
  10;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q21: Выгрузите таблицу, в которую войдут названия компаний из категории social, получившие финансирование с 2010 по 2013 год включительно. Проверьте, что сумма инвестиций не равна нулю. Выведите также номер месяца, в котором проходил раунд финансирования.
```sql
SELECT 
  c.name, 
  EXTRACT(
    Month 
    FROM 
      CAST(fr.funded_at as timestamp)
  ) 
FROM 
  company as c 
  INNER JOIN funding_round as fr ON c.id = fr.company_id 
WHERE 
  c.category_code = 'social' 
  and EXTRACT(
    YEAR 
    FROM 
      CAST(fr.funded_at as timestamp)
  ) BETWEEN 2010 
  and 2013 
  AND fr.raised_amount != 0;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q22: Отберите данные по месяцам с 2010 по 2013 год, когда проходили инвестиционные раунды. Сгруппируйте данные по номеру месяца и получите таблицу, в которой будут поля: номер месяца, в котором проходили раунды; количество уникальных названий фондов из США, которые инвестировали в этом месяце; количество компаний, купленных за этот месяц; общая сумма сделок по покупкам в этом месяце.
```sql
WITH month_fund AS (
  SELECT 
    EXTRACT(
      MONTH 
      FROM 
        fr.funded_at
    ) AS MONTH, 
    COUNT(DISTINCT f.name) AS count_of_fund 
  FROM 
    funding_round AS fr 
    LEFT JOIN investment AS i ON i.funding_round_id = fr.id 
    LEFT JOIN fund AS f ON i.fund_id = f.id 
  WHERE 
    EXTRACT(
      YEAR 
      FROM 
        fr.funded_at
    ) BETWEEN 2010 
    AND 2013 
    AND f.country_code = 'USA' 
  GROUP BY 
    MONTH
), 
month_acquired AS (
  SELECT 
    EXTRACT(
      MONTH 
      FROM 
        acquired_at
    ) AS MONTH, 
    COUNT(acquired_company_id) AS count_of_acquired, 
    SUM(price_amount) AS sum_of_acquired 
  FROM 
    acquisition 
  WHERE 
    EXTRACT(
      YEAR 
      FROM 
        acquired_at
    ) BETWEEN 2010 
    AND 2013 
  GROUP BY 
    MONTH
) 
SELECT 
  month_fund.month, 
  month_fund.count_of_fund, 
  month_acquired.count_of_acquired, 
  month_acquired.sum_of_acquired 
FROM 
  month_fund 
  JOIN month_acquired ON month_fund.month = month_acquired.month;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

### 📌 Q23: Составьте сводную таблицу и выведите среднюю сумму инвестиций для стран, в которых есть стартапы, зарегистрированные в 2011, 2012 и 2013 годах. Данные за каждый год должны быть в отдельном поле. Отсортируйте таблицу по среднему значению инвестиций за 2011 год от большего к меньшему.
```sql
WITH a as (
  SELECT 
    country_code, 
    AVG(funding_total) as avg_2011 
  FROM 
    company 
  WHERE 
    EXTRACT(
      YEAR 
      FROM 
        CAST(founded_at as timestamp)
    ) = 2011 
  GROUP BY 
    country_code
), 
b as (
  SELECT 
    country_code, 
    AVG(funding_total) as avg_2012 
  FROM 
    company 
  WHERE 
    EXTRACT(
      YEAR 
      FROM 
        CAST(founded_at as timestamp)
    ) = 2012 
  GROUP BY 
    country_code
), 
c as (
  SELECT 
    country_code, 
    AVG(funding_total) as avg_2013 
  FROM 
    company 
  WHERE 
    EXTRACT(
      YEAR 
      FROM 
        CAST(founded_at as timestamp)
    ) = 2013 
  GROUP BY 
    country_code
) 
SELECT 
  a.country_code, 
  a.avg_2011, 
  b.avg_2012, 
  c.avg_2013 
FROM 
  a 
  INNER JOIN b ON a.country_code = b.country_code 
  INNER JOIN c ON a.country_code = c.country_code 
ORDER BY 
  a.avg_2011 DESC;
```

<img width="294" alt="image" src="https://user-images.githubusercontent.com/81607668/171318070-1151876c-31a6-40ea-ab20-4ac26a8d3623.png">

***