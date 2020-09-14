# 1. tuple

## 1.1 namedtuple

元组的子类

**特点**

* 可以像普通对象调用一样来处理类似字典功能的容器
* 占用内存少

<hr>

```Python
def namedtuple(typename, field_names, rename=False, defaults=None, module=None)
	return namedtuple
```

* typename: str, 对namedtuple进行命名
* field_names: str/iterable, nametuple可以使用的属性. str需要用`, `分开
* rename: bool, 如果`filed_names`与系统关键字, 或者出现重复, 是否进行重命名处理, False表示直接抛出异常. True表示利用索引进行重命名`_1`, `_2`...
* defaults: iterable, 对`filed_names`设置缺省值. 注意, 此值是从右开始赋予的.
* module: str, 设置`__module__`属性

### 1.1.1 通用方法支持

* getattr: `getattr(t, "x", 1)`

* 切片索引取值: `t[1]`

* 继承

  由于`namedtuple`也是对象, 所以可以利用继承特性对父类进行拓展

* 设置`__doc__`属性

  ```Python
  from collections import *
  
  Test = namedtuple("Test", ["x", "y", "z"], defaults=[1, 2])
  Test.x.__doc__ = "x description"
  help(Test.x)
  ```

  输出

  ```python
  Help on property:
  
      x description
  ```

### 1.1.2 方法

#### \_make

类方法, 将可迭代对象强转为`namedtuple`中的value

```Python
@classmethod
def _make(cls, iterable):
    return namedtuple
```

* iterable: 可迭代对象

**示例**

   ```python
Test = namedtuple("Test", ["x", "y", "z"], defaults=[1, 2])
t = Test(5)
print(t)
a = [7, 8, 9]
print(t._make(a))
   ```

**输出**

```bash
Test(x=5, y=1, z=2)
Test(x=7, y=8, z=9)
```

#### \_asdict

* < Python3.8: 将`namedtuple`转化为`OrderedDict`, 即有序字典
* \>=Python3.8: 返回`dict`

```Python
@classmethod
def _asdict(cls, iterable):
    return OrderedDict/Dict
```

**示例**

```Python
from collections import *

Test = namedtuple("Test", ["x", "y", "z"], defaults=[1, 2])
t = Test(5)
print(t)
print(t._asdict())
```

**输出**

```bash
Test(x=5, y=1, z=2)
OrderedDict([('x', 5), ('y', 1), ('z', 2)])
```

#### \_replace

修改并生成一个新的`namedtuple`

```Python
def _replace(**kwargs):
```

* kwargs: 所要修改的值, 以关键字参数传入

**示例**

```python
Test = namedtuple("Test", ["x", "y", "z"], defaults=[1, 2])
t = Test(5)
print(t)
a = {"x": 5, "y": 7}
print(t._replace(**a))
```

**输出**

```bash
Test(x=5, y=1, z=2)
Test(x=5, y=7, z=2)
```



### 1.1.3 属性

#### \_fields

输出`namedtuple`配置的属性

* 示例

  ```python
  from collections import *
  
  Test = namedtuple("Test", ["x", "y", "z"], defaults=[1, 2])
  t = Test(5)
  print(t._fields)
  ```

* 输出

  ```bash
  ('x', 'y', 'z')
  ```

#### \_field_defaults

输出`namedtuple`的缺省键值对, 以`dict`表示

* 示例

  ```python
  Account = namedtuple('Account', ['type', 'balance'], defaults=[0])
  print(Account._field_defaults)
  ```

* 输出

  ```bash
  {'balance': 0}
  ```

  



# 2. list

## 2.1 deque

## 2.2 UserList

## 2.3 UserString



# 3. dict

## 3.1 ChainMap

## 3.2 Counter

## 3.3 OrderedDict

有序字典, `dict`的子类. 由于Python3.7以上版本, `dict`做了变动, 支持了排序功能

| 功能                         | `dict`              | `OrderedDict`                  |
| ---------------------------- | ------------------- | ------------------------------ |
| 排序算法                     | 支持, 但不擅长      | 擅长                           |
| 映射/存储/迭代/空间效率/更新 | 擅长                | 支持, 但不擅长                 |
| 等号校验数据是否相等         | 只比较数据          | 比较数据和顺序                 |
| `popitem()`                  | `FIFO`弹出末尾元素  | 可以指定`FIFO`开始或者末尾弹栈 |
| `move_to_end()`              | 不支持              | 将指定元素移动到末尾           |
| `__reversed__()`             | Python3.8之前不支持 | 支持翻转字典                   |



<hr>

```Python
def __init__([items]):
    return OrderedDict
```

* 传入参数与`dict`相同

支持`dict`中的全部功能, 并拓展了部分功能

### 3.3.1 拓展`dict`功能

#### popitem

依照`FIFO`, 指定左边或者右边弹栈

```Python
def popitem(self, last=True):
    return (key, value)
```

* last: bool.  True表示右边出栈

实例

```Python
from collections import *

od = OrderedDict(x=2, y=3, z=4)
print(od.popitem(False))
print(od.popitem(True))
```

输出

```Python
('x', 2)
('z', 4)
```

#### move_to_end

将某个键值移动到末尾

```python
def move_to_end(self, key, last)
```

* key: key, 指定需要移动的键

实例

```Python
od = OrderedDict(x=2, y=3, z=4)
od.move_to_end("y", last=True)
print(od)
```

输出

```python
OrderedDict([('x', 2), ('z', 4), ('y', 3)])
```

#### reversed

利用`__reversed__()`翻转字典, 返回值为`iter`

实例

```Python
from collections import *

od = OrderedDict(x=2, y=3, z=4)
print(list(reversed(od)))
```

输出

```bash
['z', 'y', 'x']
```



## 3.4 defaultdict

## 3.5 UserDict



