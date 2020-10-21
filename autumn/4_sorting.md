# Основы алгоритмов и структур данных: алгоритмы сортировки

В рамках лекции №6 мы уже рассмотрели несколько видов сортировок. Перечислим их еще раз.

1. **Сортировка вставками (insertion sort)**

- Перебираются элементы в неотсортированной части массива.
- Каждый элемент вставляется в отсортированную часть массива на то место, где он должен находиться. 

Ассимптотическая сложность: <img src="https://i.upmath.me/svg/O(N%5E2)" alt="O(N^2)" />

[![](http://img.youtube.com/vi/ROalU379l3U/0.jpg)](http://www.youtube.com/watch?v=ROalU379l3U "")

> [Сортировки вставками (habr.com)](https://habr.com/ru/post/415935/)

Код на языке Python 3:

```python
def insertion_sort(in_arr):
    out_arr = in_arr[:]
    for idx, key in enumerate(out_arr[1:], 1):
        j = idx - 1
        while key < out_arr[j] and j >= 0:
            out_arr[j+1] = out_arr[j]
            j-=1
        out_arr[j+1] = key
    return out_arr
    
arr_0 = [3, 1, 7, 2, 4]
arr_1 = insertion_sort(arr_0)
print(arr_1) 
```

2. **Пузырьковая сортировка (bubble sort)**

Элементы сравниваются попарно и, если порядок в паре неверный, выполняется перестановка элементов в паре.

Ассимптотическая сложность: <img src="https://i.upmath.me/svg/O(N%5E2)" alt="O(N^2)" />

[![](http://img.youtube.com/vi/lyZQPjUT5B4/0.jpg)](http://www.youtube.com/watch?v=lyZQPjUT5B4 "")

> [Пузырьковая сортировка и все-все-все (habr.com)](https://habr.com/ru/post/204600/)

Код на языке Python 3:

```python

def bubble_sort(in_arr):
    out_arr = in_arr[:]
    for i in range(len(out_arr) - 1):
        for j in range (len(out_arr) - 1 - i):
            if out_arr[j] > out_arr[j +1]:
                out_arr[j], out_arr[j +1] = out_arr[j +1], out_arr[j]
    return out_arr

arr_0 = [3, 1, 7, 2, 4]
arr_1 = bubble_sort(arr_0)
print(arr_1) 

```
3. **Пирамидальная сортировка (heap sort)**

- Из сортируемого массива длинною <img src="https://i.upmath.me/svg/N" alt="N" /> составляется двоичная куча: <img src="https://i.upmath.me/svg/O(N%5Clog_2N)" alt="O(N\log_2N)" />.
- Наименьший (или наибольший) элемент берется из корня: <img src="https://i.upmath.me/svg/O(1)" alt="O(1)" />.
- Двоичная куча перестраивается, чтобы сохранить свои свойства: <img src="https://i.upmath.me/svg/O(%5Clog_2N)" alt="O(\log_2N)" />.
- Два предыдущих пункта повторяются <img src="https://i.upmath.me/svg/N" alt="N" /> раз.

Ассимптотическая сложность: <img src="https://i.upmath.me/svg/O(N%5Clog_2N)" alt="O(N\log_2N)" />

[![](http://img.youtube.com/vi/Xw2D9aJRBY4/0.jpg)](http://www.youtube.com/watch?v=Xw2D9aJRBY4 "")

> Пример пирамидальной сортировки с использованием двоичной min-кучи в документации модуля `heapq`: 
>
> https://docs.python.org/3/library/heapq.html#basic-examples

Рассмотрим еще несколько алгоритмов сортировки, использующиеся в самых распространенных программных реализациях.

## Стандартная библиотек языка С: быстрая сортировка (quick sort)

Быстрая сортировка входит в стандартнуую библиотеку языка С и реализована в рамках функции **[qsort()](https://www.opennet.ru/man.shtml?topic=qsort&category=3&russian=0)**.

Если кратко, то алгоритм делит массив на две части и далее рекурсивно вызывает сам себя для их сортировки. Для более подробного объяснения см.: 
- Стивенс, Р., 2016. Алгоритмы. Разработка и применение. М.: Издательство «Э. - с. 148-154
- Седжвик, Р., 2001. Фундаментальные алгоритмы на С++. К.: Диасофт.  - с. 299-330

Ассимптотическая сложность: 
- в среднем <img src="https://i.upmath.me/svg/O(N%5Clog_2N)" alt="O(N\log_2N)" />;
- в худшем случае <img src="https://i.upmath.me/svg/O(N%5E2)" alt="O(N^2)" />.

[![](http://img.youtube.com/vi/ywWBy6J5gz8/0.jpg)](http://www.youtube.com/watch?v=ywWBy6J5gz8 "")


Алгоритм не требует дополнительной памяти. Не является [стабильной](https://ru.wikipedia.org/wiki/%D0%A3%D1%81%D1%82%D0%BE%D0%B9%D1%87%D0%B8%D0%B2%D0%B0%D1%8F_%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0) сортировкой.

## Сортировка связных списков (сортировка слиянием)
