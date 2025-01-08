# Домашнее задание - Работа с уровнями изоляции транзакции в PostgreSQL
### 1. Создание виртуальной машины и настройка SSH
- Создана виртуальная машина с дефолтными параметрами.
 ![image](https://github.com/user-attachments/assets/9543d9aa-3f9c-404b-9901-bc3f7a47b1e9)


### 2. Установка PostgreSQL
 Установлен PostgreSQL на виртуальной машине.
![image](https://github.com/user-attachments/assets/06aea8cf-9729-423c-bb87-cb29d27eb270)


### 3. Создание таблицы и данных
- Выполнены команды для создания таблицы и вставки данных:
  ```sql
  create table persons(
      id serial,
      first_name text,
      second_name text
  );
  insert into persons(first_name, second_name) values('ivan', 'ivanov');
  insert into persons(first_name, second_name) values('petr', 'petrov');
  commit;

4. Проверка уровня изоляции
Уровень изоляции по умолчанию:
```show transaction isolation level;```

Результат: Read Committed.

### Тестирование уровней изоляции

Уровень Read Committed
Сессия 1: Начата транзакция и добавлена запись:


```insert into persons(first_name, second_name) values('sergey', 'sergeev');```

Сессия 2: Попытка сделать SELECT:

```select * from persons;```
-Результат: Запись не видна, так как транзакция в первой сессии не завершена (Read Committed позволяет видеть только завершённые изменения).

Сессия 1: Транзакция завершена:


```commit;```

Сессия 2: Повторный SELECT:


```select * from persons;```
- Результат: Запись видна, так как транзакция в первой сессии завершена.


Обе сессии: Уровень изоляции изменён:


```set transaction isolation level repeatable read;```


Сессия 1: Добавлена запись:
```insert into persons(first_name, second_name) values('sveta', 'svetova');```

Сессия 2: Попытка сделать SELECT:


```select * from persons;```
- Результат: Запись не видна, так как Repeatable Read фиксирует снимок данных на момент начала транзакции.

Сессия 1: Транзакция завершена:


```commit;```
Сессия 2: Повторный SELECT:


```select * from persons;```

- Результат: Запись не видна, так как снимок данных не обновляется в рамках одной транзакции (Repeatable Read).

Сессия 2: Завершение транзакции:


```commit;```

Сессия 2: Новый SELECT:


```select * from persons;```
- Результат: Запись видна, так как новая транзакция фиксирует обновлённый снимок данных.

### Выводы
Уровень Read Committed позволяет видеть изменения сразу после завершения транзакции.
Уровень Repeatable Read изолирует изменения, сделанные другими транзакциями, до завершения текущей.
