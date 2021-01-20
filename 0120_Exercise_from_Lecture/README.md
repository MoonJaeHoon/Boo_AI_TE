# 1. Python_Data_Structure.ipynb

> 기본적인 파이썬의 데이터 구조들을 살펴보는 시간을 가졌다.
>
> 스택, 큐, 리스트, 딕셔너리 등 다양한 자료구조를 살펴보았다.

> 
>
> 특히 collections 모듈에서 queue와 stack의  특징을 가져서 자주 쓰던 deque이 특별한 기능이 많다는 것을 알 수 있었다.
>
> - Linked List의 특성을 지니고 있다.
>   - rotate, reverse 등의 기능을 가진다는 점이 특별하였다.
> - deque은 시간 효율성 면에서 리스트보다 우월하였다.

> ```python
> # deque 활용
> from collections import deque
> import time
> 
> start_time = time.process_time()
> deque_list = deque()
> # Stack
> for i in range(10000):
>     for i in range(10000):
>         deque_list.append(i)
>         deque_list.pop()
> print(time.process_time() - start_time, "seconds")
> ```
>
> ```
> 27.78125 seconds
> ```
>
> ```python
> # list 활용
> 
> import time
> 
> start_time =time.process_time()
> my_list = list()
> # Stack
> for i in range(10000):
>     for i in range(10000):
>         my_list.append(i)
>         my_list.pop()
> print(time.process_time() - start_time, "seconds")
> ```
>
> ```
> 82.265625 seconds
> ```
>
> ```python
> # rotate 기능
> deque_list.rotate(2)
> print(deque_list)
> deque_list.rotate(2)
> print(deque_list)
> ```
>
> ```
> deque([3, 4, 10, 0, 1, 2])
> deque([1, 2, 3, 4, 10, 0])
> ```
>
> ```python
> print(deque_list)
> print(deque(reversed(deque_list)))
> ```
>
> ```
> deque([1, 2, 3, 4, 10, 0])
> deque([0, 10, 4, 3, 2, 1])
> ```









# 2. Pythonic_Code_CPU_Time.ipynb

> function Calls를 직접 보고, CPU Time을 측정해보면서 어느 것이 어떻게 효율적인 코드인지 확인해보는 시간을 가졌다.



# 3. Pythonic_Code_Memory_Usage.py



```python

from memory_profiler import profile

d1 = {i:i for i in range(10000)}
d2 = {j:j for j in range(10001,20000)}

@profile(precision=4)
def for_loop(d1, d2):
    result = {}
    
    for k in d1:
        result[k] = d1[k]
    for k in d2:
        result[k] = d2[k]
        
    return result

@profile(precision=4)
def update_method(d1,d2):
    result = {}
    result.update(d1)
    result.update(d2)
    return result

@profile(precision=4)
def dict_comprehension(d1,d2):
    result = {k:v for d in [d1,d2] for k,v in d.items()}
    return result

@profile(precision=4)
def dict_kwargs(d1,d2):
    result = {**d1,**d2}
    return result

if __name__ == "__main__":
    data1 = for_loop(d1,d2)
    data2 = update_method(d1,d2)
    data3 = dict_comprehension(d1,d2)
    data4 = dict_kwargs(d1,d2)    
```

```
Line #    Mem usage    Increment  Occurences   Line Contents
============================================================
     7  41.8633 MiB  41.8633 MiB           1   @profile(precision=4)
     8                                         def for_loop(d1, d2):
     9  41.8633 MiB   0.0000 MiB           1       result = {}
    10                                             
    11  42.2773 MiB   0.0000 MiB       10001       for k in d1:
    12  42.2773 MiB   0.4141 MiB       10000           result[k] = d1[k]
    13  42.8398 MiB   0.0000 MiB       10000       for k in d2:
    14  42.8398 MiB   0.5625 MiB        9999           result[k] = d2[k]
    15                                                 
    16  42.8398 MiB   0.0000 MiB           1       return result



Line #    Mem usage    Increment  Occurences   Line Contents
============================================================
    18  42.8594 MiB  42.8594 MiB           1   @profile(precision=4)
    19                                         def update_method(d1,d2):
    20  42.8594 MiB   0.0000 MiB           1       result = {}
    21  42.8594 MiB   0.0000 MiB           1       result.update(d1)
    22  43.4219 MiB   0.5625 MiB           1       result.update(d2)
    23  43.4219 MiB   0.0000 MiB           1       return result



Line #    Mem usage    Increment  Occurences   Line Contents
============================================================
    25  43.4219 MiB  43.4219 MiB           1   @profile(precision=4)
    26                                         def dict_comprehension(d1,d2):
    27  43.9844 MiB   0.5625 MiB       20004       result = {k:v for d in [d1,d2] for k,v in d.items()}
    28  43.9844 MiB   0.0000 MiB           1       return result



Line #    Mem usage    Increment  Occurences   Line Contents
============================================================
    30  43.9844 MiB  43.9844 MiB           1   @profile(precision=4)
    31                                         def dict_kwargs(d1,d2):
    32  44.5469 MiB   0.5625 MiB           1       result = {**d1,**d2}
    33  44.5469 MiB   0.0000 MiB           1       return result



Process finished with exit code 0

```

- Update Method와 keyword arguments가 가장 빨랐습니다.
- For loop - For문도 돌아가야 하고, local 변수를 찾았다가 global 변수도 찾는 등 많은 Occurences를 발생시킵니다.
- kwargs - 반면에, local 변수만 찾고 unpacking으로 수행하기 때문에 Occurences가 1번입니다.

