# Решения задач на Codewars / LeetCode

## Информация о студенте
- **Самохин Константин Эдуардович:** 
- **Группа: НЭб-241** 
- **Платформа: Codewars** 
- **Ник на платформе: Konstantin 03** 

## Статистика
- **Всего решено:** 8
- **Codewars:** 8 задач (из них: 8 kyu: 0, 7 kyu: 6, 6 kyu: 2)

---

## Решенные задачи

### Задача 1
- **Sum of odd numbers** 
- **Платформа:** Codewars
- **https://www.codewars.com/kata/55fd2d567d94ac3bc9000064/train/python** 
- **7kyu** 
- **Given the triangle of consecutive odd numbers:
#            1
#         3     5
#    7     9    11
#  13    15    17    19
# 21    23    25    27    29
...
#Calculate the sum of the numbers in the nth row of this triangle (starting at index 1) e.g.: (Input --> Output)** 
- **def row_sum_odd_numbers(n):
    a = 0
    cnt = 0
    d = 0
    for i in range(n):
        a = i + a
    d = a * 2 + 1
    for b in range(n):
        cnt = cnt + d
        d = d + 2
    return cnt
print(row_sum_odd_numbers(3))**
```python
# test python code
```
Подход:

Сложность:

* Время:

* Память:

Комментарий:

### Задача 2
- **Sum of odd numbers** 
- **Codewars**
- **https://www.codewars.com/kata/5502c9e7b3216ec63c0001aa/train/python** 
- **7kyu** 
- **#The Western Suburbs Croquet Club has two categories of membership, Senior and Open. They would like your help with an application form that will tell prospective members which category they will be placed.
#To be a senior, a member must be at least 55 years old and have a handicap greater than 7. In this croquet club, handicaps range from -2 to +26; the better the player the lower the handicap.
#Input
#Input will consist of a list of pairs. Each pair contains information for a single potential member. Information consists of an integer for the person's age and an integer for the person's handicap.

#Output
#Output will consist of a list of string values (in Haskell and C: Open or Senior) stating whether the respective member is to be placed in the senior or open category.** 
- **def open_or_senior(data):
        output = []
        for i in range(len(data)):
            if  ((data[i][0] >= 55) and (data[i][1] > 7)):
                output.append("Senior")
            else:
                output.append("Open")
        return output**
```python
# test python code
```
Подход:

Сложность:

* Время:

* Память:

Комментарий:


```
<-- Добавьте остальные задачи по аналогии -->
```


## Заключение
```
<-- Опишите, что нового вы узнали, какие трудности возникли, какие задачи понравились больше всего -->
```
