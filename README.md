# py_lab2.1
# Лабораторная работа 2.1 Создание и использование пользовательских функций. Работа с рекурсивными функциями
## Цель работы
Научиться работать с функциями и подпрограммами в Python, а также исследовать их преимущества, особенности применения и возможные недостатки. Закрепить навыки создания, вызова и применения различных видов функций, включая глобальные, локальные, рекурсивные и анонимные.

# Вариант 12
## № 2.1.12
В вагоне-купе имеется некоторое количество купе, в каждом из которых по 4 места.
Разработчик хранит информацию о занятости одного купе в виде словаря:
{1: 'м', 2: None, 3: None, 4: 'ж'}
где:
- ключ определяет номер места (нечетные номера - нижние места, четные -
верхние);
- значение может быть одно из трех: None , "м" и "ж" , если место не
занято, занято мужчиной или женщиной соответственно.


Информация о занятости всего вагона хранится как список указанных словарей.
Определите:


- список полностью свободных купе;
- список свободных мест в вагоне;
- список свободных нижних или верхних мест;
- список свободных мест в купе с исключительно мужской компанией;
- список свободных мест в купе с исключительно женской компанией.

## Решение
```# № 2.1.12
data = [
    {1: 'м', 2: 'м', 3: 'м', 4: 'ж'},
    {1: 'ж', 2: 'м', 3: 'ж', 4: 'ж'},
    {1: 'ж', 2: 'ж', 3: 'ж', 4: 'ж'},
    {1: 'м', 2: 'м', 3: 'м', 4: 'м'},
    {1: None, 2: None, 3: None, 4: None},
    {1: 'м', 2: None, 3: None, 4: 'ж'},
    {1: None, 2: None, 3: None, 4: None},
    {1: 'м', 2: 'м', 3: None, 4: 'м'},
    {1: 'ж', 2: None, 3: None, 4: 'ж'}
]


def vacant_compartments(data):
    """Вернуть список полностью свободных купе. Нумерация купе идет с 1.

    Параметры:
        - data (list of dict): данные о занятости мест в вагоне.

    Результат:
        list of int."""
    result = []
    for i, compartment in enumerate(data, 1):
        # Проверяем, что все места в купе свободны (None)
        if all(seat is None for seat in compartment.values()):
            result.append(i)
    return result


def vacant_seats(data, compartments_condition=None, seat_condition=None):
    """Вернуть список свободных мест при соблюдении условий
    'compartments_condition' и 'seat_condition'.
    Нумерация купе и мест идет с 1.

    Параметры:
        - data (list of dict): данные о занятости мест в вагоне;
        - compartments_condition (function):
          функция c 1 параметром (словарь - купе), возвращающая True/False;
        - seat_condition (function):
          функция c 2 параметрами (номер места, значение),
          возвращающая True/False.

    Результат:
        list of tuple: список кортежей вида (номер купе, номер места)."""
    result = []
    for compartment_num, compartment in enumerate(data, 1):
        # Проверяем условие для купе (если задано)
        if compartments_condition is None or compartments_condition(compartment):
            for seat_num, seat_value in compartment.items():
                # Место свободно и удовлетворяет условию для места
                if seat_value is None and (seat_condition is None or seat_condition(seat_num, seat_value)):
                    result.append((compartment_num, seat_num))
    return result


def is_same_sex_and_vacant(compartment, sex):
    """Вернуть True, если в купе 'compartment' есть свободные места,
    а остальные пассажиры только пола 'sex'.

    Параметры:
        - compartment (dict): данные о купе;
        - sex (str): пол ("м" или "ж").

    Результат:
        bool."""
    # Проверяем, что все занятые места заняты пассажирами указанного пола
    occupied_seats = [value for value in compartment.values() if value is not None]
    vacant_seats = [value for value in compartment.values() if value is None]

    # Есть свободные места и все занятые места заняты пассажирами нужного пола
    return len(vacant_seats) > 0 and all(s == sex for s in occupied_seats)


# список полностью свободных купе
print(vacant_compartments(data))
# список свободных мест в вагоне
print(vacant_seats(data))
# список свободных нижних мест
print(vacant_seats(data, seat_condition=lambda seat, value: seat % 2 != 0))
# список свободных верхних мест
print(vacant_seats(data, seat_condition=lambda seat, value: seat % 2 == 0))
# список свободных мест в купе с исключительно мужской компанией
print(vacant_seats(data, lambda x: is_same_sex_and_vacant(x, "м")))
# список свободных мест в купе с исключительно женской компанией
print(vacant_seats(data, lambda x: is_same_sex_and_vacant(x, "ж")))
```
Результат:
<img width="1046" height="152" alt="image" src="https://github.com/user-attachments/assets/656cb86f-4f85-4baf-99fd-de8df2b4a1f4" />


## Вывод
В ходе выполнения работы мы освоили основы работы с функциями и подпрограммами в Python, изучили их ключевые особенности, преимущества и ограничения. Были закреплены навыки проектирования, вызова и практического применения различных типов функций — включая глобальные и локальные, рекурсивные. Это позволило глубже понять, как функции способствуют структурированию кода, повышению его читаемости, повторному использованию и упрощению отладки.
