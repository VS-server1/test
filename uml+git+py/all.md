## Решение задания 1. Use Case (UseCase.png)

Вставьте код в PlantUML ( Упорядочить ->  Вставить -> Расширенные -> PlantUML ), нажмите «Вставить», затем Файл → Экспорт как → PNG под именем UseCase.png. **ИЛИ НАРИСУЙТЕ ВРУЧНУЮ.(РЕКОМЕНДУЕТСЯ ВИЗУАЛЬНО ЗАПОМНИТЬ**

```bash
@startuml
left to right direction
actor "Покупатель" as P
rectangle "Список покупок" {
  usecase "Посмотреть список" as UC1
  usecase "Добавить товар" as UC2
  usecase "Отметить купленным" as UC3
  usecase "Удалить товар" as UC4
}
P -- UC1
P -- UC2
P -- UC3
P -- UC4
@enduml
```

<img width="1920" height="991" alt="image" src="https://github.com/user-attachments/assets/e1b734c8-9df9-47bb-9dc6-b4737b7e5b02" />

**результат**

<img width="541" height="517" alt="image" src="https://github.com/user-attachments/assets/bd8433cf-1b1d-413f-881f-e7a765fa422c" />

## Решение задания 2. Sequence (Sequence.png)

Точно также как и 1. Экспорт как → PNG под именем Sequence.png. 

```bash
@startuml
actor Покупатель
participant Программа
participant Список
Покупатель -> Программа: Добавить товар
Программа --> Покупатель: Введите название
Покупатель -> Программа: Название
alt название введено
  Программа -> Список: Сохранить товар
  Список --> Программа: Товар сохранён
  Программа --> Покупатель: Товар добавлен
else название пустое
  Программа --> Покупатель: Ошибка: пустое название
end
@enduml
```

<img width="660" height="548" alt="image" src="https://github.com/user-attachments/assets/ed39beda-b2e8-408f-86ba-4755f7bb0b1e" />

## Решение задания 3. Программа main.py

```python
items = []

def show_items(): print(items or "Список пуст")

def add_item():
    name = input("Товар: ")
    items.append(name) if name else print("Ошибка: пустое название")

def buy_item():
    try: items[int(input("Номер: ")) - 1] += " - куплено"
    except: print("Ошибка: нет такого номера")

def delete_item():
    try: items.pop(int(input("Номер: ")) - 1)
    except: print("Ошибка: нет такого номера")

while True:
    c = input("1 Список 2 Добавить 3 Купил 4 Удалить 5 Выход: ")
    if c == "1": show_items()
    elif c == "2": add_item()
    elif c == "3": buy_item()
    elif c == "4": delete_item()
    elif c == "5": break
    else: print("Ошибка: нет такого пункта")
```

## Решение задания 4. Проверка (Result.png)
Запустите python main.py и вводите по порядку:

```bash
1.	2, затем Молоко
2.	2, затем Хлеб
3.	2, затем Сыр
4.	1 — список из трёх товаров
5.	3, затем 2 — второй товар отмечен купленным
6.	4, затем 3 — третий товар удалён
7.	1 — итоговый список
Ожидаемый результат на последнем шаге:
['Молоко', 'Хлеб - куплено']
```
Сделайте скриншот окна с этим выводом и сохраните как Result.png.

## Решение задания 5. Git

В терминале, в папке проекта:

```bash
git config user.name "TestUser"
git config user.email "test@test.com"
```


```bash
git init
git add main.py
git commit -m "Создан список покупок"
git log --oneline
```

Второй коммит(необязательно а просто для контекста) делаете изменения и потом:

```bash
git add main.py
git commit -m "Создан список покупок2"
```


## Что сдавать
Папка «Фамилия_Группа_СписокПокупок» с пятью файлами и скриншотом истории Git:

•	UseCase.png

•	Sequence.png

•	main.py

•	Result.png

•	TestReport.docx

•	скриншот команды git log --oneline
