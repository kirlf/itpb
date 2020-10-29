# Основы алгоритмов и структур данных: алгоритмы сортировки

В рамках лекции №6 мы уже рассмотрели несколько видов сортировок. Перечислим их еще раз.

1. **Сортировка вставками (insertion sort)**

- Перебираются элементы в неотсортированной части массива.
- Каждый элемент вставляется в отсортированную часть массива на то место, где он должен находиться. 

Ассимптотическая сложность: <img src="https://i.upmath.me/svg/O(N%5E2)" alt="O(N^2)" />

[![](http://img.youtube.com/vi/ROalU379l3U/0.jpg)](http://www.youtube.com/watch?v=ROalU379l3U "")

*(Нажмите на заставку выше, чтобы перейти к видео на YouTube.)*


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

> См. также: [Сортировки вставками (habr.com)](https://habr.com/ru/post/415935/)


2. **Пузырьковая сортировка (bubble sort)**

Элементы сравниваются попарно и, если порядок в паре неверный, выполняется перестановка элементов в паре.

Ассимптотическая сложность: <img src="https://i.upmath.me/svg/O(N%5E2)" alt="O(N^2)" />

[![](http://img.youtube.com/vi/lyZQPjUT5B4/0.jpg)](http://www.youtube.com/watch?v=lyZQPjUT5B4 "")


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

> См. также: [Пузырьковая сортировка и все-все-все (habr.com)](https://habr.com/ru/post/204600/)

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
- Седжвик, Р., 2001. Фундаментальные алгоритмы на С++. К.: Диасофт.  - с. 299-329

Ассимптотическая сложность: 
- в среднем <img src="https://i.upmath.me/svg/O(N%5Clog_2N)" alt="O(N\log_2N)" /> ;
- в худшем случае <img src="https://i.upmath.me/svg/O(N%5E2)" alt="O(N^2)" />.

[![](http://img.youtube.com/vi/ywWBy6J5gz8/0.jpg)](http://www.youtube.com/watch?v=ywWBy6J5gz8 "")


Алгоритм требует относительно малое количество дополнительной памяти - <img src="https://i.upmath.me/svg/O(%5Clog_2N)" alt="O(\log_2N)" />. 

Не является [стабильной (устойчивой)](https://ru.wikipedia.org/wiki/%D0%A3%D1%81%D1%82%D0%BE%D0%B9%D1%87%D0%B8%D0%B2%D0%B0%D1%8F_%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0) сортировкой.

## Сортировка связных списков: сортировка слиянием (merge sort)

Сортировку слиянием очень часто характеризуют старой доброй латинской пословицей *"разделяй и влавствуй"*:

1. Сортируемый массив разбивается на две части примерно одинакового размера;
2. Каждая из получившихся частей сортируется отдельно, например — тем же самым алгоритмом;
3. Два упорядоченных массива половинного размера соединяются в один.

<img src="https://neerc.ifmo.ru/wiki/images/d/db/Merge_sort1.png" width="400" />

*Рис. 1. [Пример работы рекурсивного алгоритма сортировки слиянием.](https://neerc.ifmo.ru/wiki/index.php?title=%D0%A1%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0_%D1%81%D0%BB%D0%B8%D1%8F%D0%BD%D0%B8%D0%B5%D0%BC)*

Для более подробного объяснения см.:
- Седжвик, Р., 2001. Фундаментальные алгоритмы на С++. К.: Диасофт.  - с. 330-354
- Стивенс, Р., 2016. Алгоритмы. Разработка и применение. М.: Издательство «Э. - с. 155-157

Ассимптотическая сложность: 
- в лучшем случае <img src="https://i.upmath.me/svg/O(N%5Clog_2N)" alt="O(N\log_2N)" /> ;
- в среднем <img src="https://i.upmath.me/svg/O(N%5Clog_2N)" alt="O(N\log_2N)" /> ;
- в худшем случае <img src="https://i.upmath.me/svg/O(N%5Clog_2N)" alt="O(N\log_2N)" />.

[![](http://img.youtube.com/vi/XaqR3G_NVoo/0.jpg)](http://www.youtube.com/watch?v=XaqR3G_NVoo "")

Данный вид сортировки является стабильным.

Одним из недостатков сортировки слиянием является необходимость <img src="https://i.upmath.me/svg/O(N)" alt="O(N)" /> вспомогательной памяти для сортировки массива. Однако, в случае связных списков, где мы оперируем не самими данными, а связями между ними, данный недостаток не играет роли. 

Более того, реализации сортировки слиянием, как правило, оптимально работают со структурами, осуществляющими **последовательный** доступ к элементам, примером чего и является связный список.

## Алгоритмы сортировки, используемые в рамках CPython и NumPy

Мне кажется, еще одной интересной темой является вопрос о том, какие именно алгоритмы сортировки используются в рамках реализации других языков. В качестве примеров возьмем язык Python 3, а точнее его самую распространенную реализацию CPython, и один из самых популярных среди научного сообщества модулей данного языка - [NumPy](https://numpy.org/doc/stable/index.html).

В рамках языка Python доступны два встроенных способа сортировки:
- сортирующий на месте и возвращающий `None` метод [sort()](https://docs.python.org/3/library/stdtypes.html#list.sort), 
- и возвращающая отсортированный массив функция [sorted()](https://docs.python.org/3/howto/sorting.html).

Оба способа используют один и тот же гибридный алгоритм сортировки [Timsort](https://neerc.ifmo.ru/wiki/index.php?title=Timsort), совмещающий в себе сортировку слиянием и сортировку вставками. 

Ограничивается ли ею же и NumPy? Заходим на станицу документации метода [numpy.sort](https://numpy.org/doc/stable/reference/generated/numpy.sort.html) (версия 1.19.0) и читаем какие доступны варианты:

- `quicksort` (по умолчанию),
- `mergesort`,
- `heapsort`,
- `stable` (начиная с версии 1.15.0).

Значительно больше. Рассмотри по поряду.

 1. **quicksort**
 
 Казалось бы, уже рассмотренная нами быстрая сортировка. Однако, это не совсем так:
 
> New in version 1.12.0.
>
> quicksort has been changed to [introsort](https://neerc.ifmo.ru/wiki/index.php?title=%D0%91%D1%8B%D1%81%D1%82%D1%80%D0%B0%D1%8F_%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0#Introsort). When sorting does not make enough progress it switches to heapsort. This implementation makes quicksort O(n*log(n)) in the worst case.
 
То есть и здесь мы наблюдаем переход на гибридный вид сортировки.

2. **mergesort**

Сортировка слиянием? Опять же и да, и нет:

> New in version 1.17.0.
>
> Timsort is added for better performance on already or nearly sorted data. On random data timsort is almost identical to mergesort. It is now used for stable sort while quicksort is still the default sort if none is chosen. For timsort details, refer to [CPython listsort.txt](https://github.com/python/cpython/blob/3.7/Objects/listsort.txt). ‘mergesort’ and ‘stable’ are mapped to radix sort for integer data types. Radix sort is an O(n) sort instead of O(n log n).

То есть:
- во-первых, `mergesort` и `stable` в данном контексте синонимы,
- во-вторых, для большинства случаев подразумевается все-таки Timsort, а не чистая сортировка слиянием, 
- в-третьих, если элементы массива представляют из себя целые числа (`int`), то для таких массивов применяется наиболее быстрая [поразрядная сортировка (radix sort)](https://neerc.ifmo.ru/wiki/index.php?title=%D0%A6%D0%B8%D1%84%D1%80%D0%BE%D0%B2%D0%B0%D1%8F_%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0).

Более подробно о поразрядной сортировке можно прочитать по следующе ссылке:
- Седжвик, Р., 2001. Фундаментальные алгоритмы на С++. К.: Диасофт.  - с. 401-437

3. **heapsort**

И только здесь перед нами островок стабильности! Пирамидальная сортировка, как и заявлено в документации.

Кстати говоря, все указанные выше виды сортировок NumPy соответствуют также интерфейсу метода [sort_values()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.sort_values.html) другого распространенного среди научного сообщества (особенно в сфере науки о данных) модуля [pandas](https://pandas.pydata.org/pandas-docs/stable/index.html).

Вот мы, собственно говоря, и проследили, как простые методы сортировки усложняются, перерастают в гибридные формы и находят свое применение в том числе и в рамках современных инструментов, используемых инженерами и научным сообществом. 

### Ресурсы для визуализации алгоритмов сортировок и полезные ссылки

1. [SORTING.at](http://sorting.at/) 
2. [Sorting Algorithms Animations](https://www.toptal.com/developers/sorting-algorithms)
3. [Min Heap sort](https://www.cs.usfca.edu/~galles/visualization/Heap.html)
4. [Описание алгоритмов сортировки и сравнение их производительности](https://habr.com/ru/post/335920/)
5. [Основные виды сортировок и примеры их реализации](https://academy.yandex.ru/posts/osnovnye-vidy-sortirovok-i-primery-ikh-realizatsii)

