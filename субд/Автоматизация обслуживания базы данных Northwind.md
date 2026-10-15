## Введение

Проверьте наличие pg admin на пк, если его нет - установите с базами данных. (путь например: ``D:\Папка для Хранения\Прога + таблицы``)

> Галочку на Stack Builder убрать и нажать finish

 ## 1 Этап - Установка

 переходим в servers/postgres/query tool

 <img width="756" height="474" alt="image" src="https://github.com/user-attachments/assets/ea7aed02-f6ba-4d03-a84f-efb9a8ddae98" />

установите базу данных nortwind если у вас нет: ``fill_in_northwind.sql``

<img width="838" height="757" alt="image" src="https://github.com/user-attachments/assets/a80c0661-73cd-4702-991d-5ca87fa61635" />

# 2 Этап - Решение задач 

## 1. Создание журнала обслуживания
Создайте схему auto_maintenance и в ней таблицу maintenance_log. Для таблицы используйте следующие поля и ограничения:

| Поле	| Тип данных |	Требование |
|---|---|---|
|log_id|	INTEGER|	Автонумерация, первичный ключ|
|event_time|	TIMESTAMP	|Обязательное поле|
|operation|	TEXT	|Обязательное поле|
|status|	TEXT|	Обязательное поле|
|description|	TEXT|	Обязательное поле|

```bash
CREATE SCHEMA auto_maintenance;

CREATE TABLE auto_maintenance.maintenance_log (
    log_id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    event_time TIMESTAMP NOT NULL,
    operation TEXT NOT NULL,
    status TEXT NOT NULL,
    description TEXT NOT NULL
);
```

> Пояснение: GENERATED ALWAYS AS IDENTITY создаёт номера записей; PRIMARY KEY обозначает первичный ключ; NOT NULL запрещает пустые значения. event_time будет заполнять триггер.

## 2. Создание процедуры
Создайте в auto_maintenance процедуру analyze_public_tables без параметров. Она должна выполнить ANALYZE для public.products, затем добавить в maintenance_log запись: operation = ANALYZE, status = OK, description = products. Поле event_time в процедуре не заполняйте.

```bash
CREATE PROCEDURE auto_maintenance.analyze_public_tables()
LANGUAGE plpgsql
AS $$
BEGIN
    ANALYZE public.products;

    INSERT INTO auto_maintenance.maintenance_log
        (operation, status, description)
    VALUES ('ANALYZE', 'OK', 'products');
END;
$$;
```

> Пояснение: ANALYZE обновляет статистику таблицы products для планировщика запросов. INSERT добавляет одну строку в журнал, не указывая event_time.

## 3. Создание триггера
Создайте триггерную функцию и триггер для maintenance_log: при добавлении записи в event_time автоматически устанавливаются текущие дата и время.

```bash
CREATE FUNCTION auto_maintenance.set_event_time()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
    NEW.event_time := CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$;

CREATE TRIGGER trg_maintenance_log_event_time
BEFORE INSERT ON auto_maintenance.maintenance_log
FOR EACH ROW
EXECUTE FUNCTION auto_maintenance.set_event_time();
```

> Пояснение: BEFORE INSERT запускает функцию до вставки каждой строки; NEW — добавляемая запись. Триггер задаёт event_time до проверки ограничения NOT NULL.

## 4. Проверка работы
При пустом журнале один раз вызовите процедуру. Выведите все поля из maintenance_log. Должна появиться одна запись с данными ANALYZE / OK / products, автоматически заполненными log_id и event_time.

```bash
CALL auto_maintenance.analyze_public_tables();

SELECT log_id, event_time, operation, status, description
FROM auto_maintenance.maintenance_log
ORDER BY log_id;
```

> Ожидаемый результат при первом вызове: одна запись. log_id = 1; event_time — дата и время вызова; operation = ANALYZE; status = OK; description = products. При повторном CALL добавится ещё одна строка.

---

### 3 Этап - Заполнение бланка

Ответ вы делаете скриншотом ``win + shift +s`` и заполняете в бланк, пример скрина:

<img width="861" height="793" alt="image" src="https://github.com/user-attachments/assets/25f51aa6-09ad-4c08-8175-6da61a8b373d" />
