# PROG7-Portfolio
## Список ЛР
### [ЛР 1](https://github.com/herzenuni/sem7-task1-TsirulikIvan)
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
### ЛР 5
### ЛР 6
## Список ИСР и ВСР
### ИСР1.1-1.2
#### Постановка задачи
```python
import threading

# Примитивный вариант перемножения матриц - это когда матрица представляет вектор-строку или вектор-столбец.

# В этом случае функцию matrix_multiply можно написать так:


def matrix_multiply(X, Y):

    return sum(list(map(lambda x, y: x * y, X, Y)))


# простой вариант выполнения этого ИСР - это случай перемножения двух векторов друг на друга
# для выполнения задания требуется учесть ограничения, накладываемые размерностью, длина таких векторов должна быть равна т.е. len(X) == len(Y)

# В коде ниже приведен пример вычисления произведения векторов с помощью потоков (Threads)

print(matrix_multiply((1, 2, 3), (9, 8, 7)))

t = threading.Thread(
    target=matrix_multiply, args=((1, 2, 3), (4, 5, 6))).start()


# однако результат никуда не помещается, поэтому будет затерт сборщиком мусора. 

# логично будет использовать модуль concurrent.futures (см. пример в конспекте лекции), чтобы получать результат как только он будет вычислен

# при решении более сложной задачи, когда у нас будет использоваться не вектор-строка или столбец, а полноценная матрица, представленная например так: 

A = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
B = [(9, 8, 7), (6, 5, 4), (3, 2, 1)]

# в matrix_multiply будет необходимо написать очередь, в которую будут помещаться пары строк и столбцов, которые необходимо будет перемножить. 
# Например, если дана квадратная матрица размера 2, то перемножение 1 строки на 1 столбец будет выполняться в первом потоке, 1 строки на 2 столбец - во втором и т.д. по очереди

def run_in_threads(n_th=2):
    for i in range(n_th):
        threading.Thread(target=matrix_multiply, args=(A, B)).start()


# run_in_threads()
```
#### Решение
### ИСР2.1-2.2
#### Постановка задачи
```python
import random
from concurrent.futures import ProcessPoolExecutor, as_completed
from functools import partial

# Код ниже реализует вывод в файл значений, возвращаемых функцией randint при этом в файл мы также записываем номер процесса, который в него записал значения.

# Для успешного выполнения СР требуется переписать функцию write_to_file, используя модуль csv https://docs.python.org/3/library/csv.html чтобы сформировать файл в требуемом формате.
# В качестве разделителя использовать ";", обязательно вывести шапку "№ of process; RANDOM VALUE; " остальные настройки произвольные.

# Для выполнения ИСР 2 требуется переписать функцию writing_more и использовать не ProcessPoolExecutor а класс, предполагающий использовать потоков.


def write_to_file(f, a, b, n_job=None, n_iter=100):

    for i in range(n_iter):
        with open(f, mode='a') as file:
            n = random.randint(a, b)
            file.writelines(
                ["# of process: " + str(n_job) + " " + str(n) + '\n'])

    return n


def writing_more(f, a, b, *, n_jobs=2, n_iter=200):

    executor = ProcessPoolExecutor(max_workers=n_jobs)
    spawn = partial(
        executor.submit, write_to_file, 'output.txt', n_iter=n_iter // n_jobs)

    step = (b - a) / n_jobs

    fs = [
        spawn(a + i * step, a + (i + 1) * step, n_job=i) for i in range(n_jobs)
    ]
    return sum(f.result() for f in as_completed(fs))


s = writing_more('output.txt', 0, 200)

print(s)
```
#### Решение
### ВСР1.1
#### Постановка задачи
#### Решение
