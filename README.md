# HPCM


列表去重, 并保证顺序不乱, 各个实现方式的效率如下

```python
import time
from collections import OrderedDict

ids = [1, 4, 3, 3, 4, 2, 3, 4, 5, 6, 1] + list(range(0, 1000))

def test1():
    ids_list = list(set(ids))
    ids_list.sort(key=ids.index)
    return ids_list

def test2():
    return OrderedDict().fromkeys(ids).keys()

def test3():
    new_li = []
    for i in ids:
        if i not in new_li:
            new_li.append(i)
    return new_li

def test4():
    ids_list = set(ids)
    return sorted(ids_list, key=ids.index)

count = 1000

st1 = time.time()
for _ in range(count):
    test1()
st2 = time.time()

for _ in range(count):
    test2()
st3 = time.time()

for _ in range(count):
    test3()
st4 = time.time()
```
输出
```python
set-list-sort: 5.71999979019
orderdict: 0.84500002861
new_list: 5.75
set-sorted: 5.69500017166
```
