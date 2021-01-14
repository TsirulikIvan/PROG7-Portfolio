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
Отказался пушиться на гитхаб, но на локале вроде все окей, потому [ссылка на репо](https://github.com/TsirulikIvan/PROG7-LR4)
### [ЛР 4](https://github.com/TsirulikIvan/PROG7-LR4)
### [ЛР 5](https://github.com/TsirulikIvan/PROG7-LR5)
### [ЛР 6](https://github.com/TsirulikIvan/PROG7-LR6)
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
#### [Решение](https://repl.it/@RandiSPB/RoyalChartreuseMozbot#main.py)
```python
from concurrent.futures import ThreadPoolExecutor

def matrix_multiply(X, Y):
    assert len(X) == len(Y), 'Попытка перемножить вектора разной длины'
    return [[sum(a * b for a, b in zip(i, j)) for j in zip(*Y)] for i in X]

A = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
B = [(9, 8, 7), (6, 5, 4), (3, 2, 1)]


def run_in_threads(func, args, n_th=2):
    with ThreadPoolExecutor(max_workers=n_th) as executor:
        future = executor.submit(func, *args)
    return future.result()

try:
  print(run_in_threads(matrix_multiply, (A, B), len(A)))
except AssertionError as error:
  print(error)
```
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
```python
import csv
import queue
import random
import threading

def write_header(filename: str='data.csv'):
    with open(filename, 'w') as csvfile:
        fieldnames = ['Process number', 'value']
        csv_writer = csv.writer(
            csvfile, delimiter=';', quotechar='"', quoting=csv.QUOTE_MINIMAL
            )
        csv_writer.writerow(fieldnames)


def write_to_file(a, b, n_job=None, filename: str='data.csv', n_iter: int=2):
    with open(filename, 'a') as csvfile:
        csv_writer = csv.writer(
            csvfile, delimiter=';', quotechar='"', quoting=csv.QUOTE_MINIMAL
            )
        for i in range(n_iter):
            n = random.randint(a, b)
            csv_writer.writerow([str(n_job), str(n)])
    return n


def writing_more(a, b, *, filename: str='data.csv', n_jobs: int=2, n_iter: int=100):
    que = queue.Queue()
    step = (b - a) / n_jobs
    threads = []
    result = []
    write_header(filename)
    for i in range(n_jobs):
        thread = threading.Thread(
            target=lambda q, x, y, i: q.put(write_to_file(x, y, i)), args=(que, a + step * i, a + step * (i + 1), i)
        )
        thread.start()
        print('thread {} started'.format(i))
        threads.append(thread)

    for th in threads:
        print('thread joined')
        th.join()

    while not que.empty():
        result.append(que.get())
    
    return result


s = writing_more(0, 200)
```
### ИСР3.1
#### Постановка задачи
Разработать быстрый асинхронный чат с использованием итераторов/генераторов

1. Исправьте ошибку, при которой в области отображения сообщений (textarea), дублируются сообщения пользователя.
2. Избавьтесь в коде на клиенте от eval, реализовав обработку сообщений на клиенте с помощью метода parse объекта JSON.
3. Сделайте рефакторинг на сервере и храните сообщения в объекте типа deque модуля collections, общее количество сообщений, хранимых на сервере = 50.
4. Добавьте новое поле в объект message, чтобы каждый пользователь имел уникальный идентификатор, который можно было бы использовать для отправки ему персональных сообщений, привязывать их к этому идентификатору.
5. Реализовать подсчет сообщений пользователя, итерацию по ним с использованием итераторов/генераторов для этого:
   1. Создать новый маршрут "/stat", где будет отображаться статистика всех сообщений и статистика по каждому пользователю.
   2. Создать шаблон страницы stat.html с таблицей:
#### [Решение](https://github.com/TsirulikIvan/iterchat)
