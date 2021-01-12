# PROG7-Portfolio
## Список ЛР
### ЛР 1(https://github.com/herzenuni/sem7-task1-TsirulikIvan)
### ЛР 2
#### Постановка задачи
```python
# Написание программы для численного интегрирования площади под кривой. 
def integrate(f, a, b, *, n_iter=1000):
        pass
        # float  с точностью 10^-8

# или можно так: 
#  def integrate(f, a, b, n_iter=1000):
#        pass
#        # float  с точностью 10^-8

# а и b - диапазон интегрирования
# f - функция (например, sin, cos, tan, ...) # может быть любая функция из библиотеки math

if __name__ == '__main__':
    pass
```
#### Решение
```python
from typing import Callable


def integrate(f: Callable[[float], float],
 a: float, b: float, n_iter: int = 1000,
  accuracy: int = 8) -> float:
  return round(sum(tuple(map(f, tuple(a + float((b - a) / n_iter) * n for n in range(n_iter))))) * float((b - a) / n_iter), accuracy)
```
### ЛР 3
#### Постановка задачи
Выполните инструкцию, опубликованную ранее и настройте выгрузку собственного Lektor-сайта в репозиторий, где настроена работа сервиса GitHub Pages. В ответе предоставьте ссылку на опубликованный сайт. 

Например, nzhukov.github.io
#### Решение
### ЛР 4
#### Постановка задачи
#### Решение
### ЛР 5
#### Постановка задачи
#### Решение
### ЛР 6
#### Постановка задачи
#### Решение
## Список ИСР и ВСР
### ИСР1.1
#### Постановка задачи
#### Решение
### ИСР1.2
#### Постановка задачи
#### Решение
### ВСР1.1
#### Постановка задачи
#### Решение
