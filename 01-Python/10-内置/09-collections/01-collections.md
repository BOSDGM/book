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

### 1.1.4 常用功能

* 像调用属性一样处理字典

  ```python
  Test = namedtuple("Test", "a,b,c")
  nt = Test(1, 2, 3)
  print(nt.a, nt.b, nt.c)
  ```

  输出

  ```bash
  1 2 3
  ```

  



# 2. list

## 2.1 deque

双端队列, 

## 2.2 UserList

## 2.3 UserString



# 3. dict

## 3.1 ChainMap

`dict`的子类, 合并多个字典为一个字典. 只是链接映射关系, 所以比`update`要快

```python
def __init__(self, *maps)
	return dict
```

* maps: dict, 多个字典

由于继承`dict`, 拥有全部`dict`功能, 并添加了部分功能

### 3.1.1 新增功能

#### maps

存储的链接列表, 可以被修改.

* 实例

  ```python
  a = {"a": 1, "b": 2}
  b = {1: "c", 2: "d"}
  
  cm = ChainMap(a, b)
  print(cm.maps)
  ```

* 输出

  ```bash
  [{'a': 1, 'b': 2}, {1: 'c', 2: 'd'}]
  ```

#### new_child

在连接首部插入字典的链接, 并返回

```Python
def new_child(self, m):
    return chainmap
```

* m: mapping, 字典

实例

```Python
a = {"a": 1, "b": 2}
b = {1: "c", 2: "d"}
c = {"t": [1, 2], "f": {3, 4}}

cm = ChainMap(a, b)
print(cm.new_child(c))
```

输出

```bash
ChainMap({'t': [1, 2], 'f': {3, 4}}, {'a': 1, 'b': 2}, {1: 'c', 2: 'd'})
```

#### parents

输出第一个dict

* 实例

  ```python
  a = {"a": 1, "b": 2}
  b = {1: "c", 2: "d"}
  cm = ChainMap(a, b)
  print(cm)
  print(cm.parents)
  ```

* 输出

  ```bash
  ChainMap({'a': 1, 'b': 2}, {1: 'c', 2: 'd'})
  ChainMap({1: 'c', 2: 'd'})
  ```

### 3.1.2 常用功能

* 组合字典

  ```Python
  a = {"a": 1, "b": 2}
  b = {1: "c", 2: "d"}
  for t in ChainMap(a, b).items():
      print(t)
  ```

  输出

  ```Python
  (1, 'c')
  (2, 'd')
  ('a', 1)
  ('b', 2)
  ```

* 系统变量查找顺序

  系统变量查找顺序就是利用类似的方式在寻找的. 模拟代码

  ```python
  from collections import *
  
  import builtins
  pylookup = ChainMap(locals(), globals(), vars(builtins))
  print(pylookup)
  ```

  

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

### 3.3.1 新增功能

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

继承于`dict`, 增加了字典的缺省值功能

```Python
def __init__([default_factory=None], **kwargs)
	return defaultdict
```

* default_factory: 可调用的函数/对象/None, 第一个参数为缺省值
* **kwargs: 其他参数, 用于构建字典的key/value对

### 3.4.1 新增属性

#### \__missing__

自动赋予缺省值. 只能用切片, 不能用`.get()`, 否则不能获取到缺省值

```Python
def __missing__(self, key):
    self[key] = default_factory()
```

* key: key, 需要赋予缺省值的可以

示例

```python
class DD(defaultdict):

    def __missing__(self, key):
        value = self.default_factory()
        print("当前key值: ", key)
        self[key] = value
        return value

d = DD(int)
print("切片正常获取到缺省值: ", d[1])
print("get不能获取到缺省值: ", d.get(2))
```

输出

```bash
当前key值:  1
切片正常获取到缺省值:  0
get不能获取到缺省值:  None
```

#### default_factory

存储用来提供缺省值的函数

* 示例

  ```python
  def test1():
      return 2
  
  dd = defaultdict(test1)
  print(dd.default_factory.__name__, dd.default_factory)
  ```

  

* 输出

  ```bash
  test1 <function test1 at 0x000001FEBC66D378>
  ```

### 3.4.2 常用功能

* 统计

  ```python
  dd = defaultdict(int)
  count_str = "ashomoasoiuhoie"
  
  for s in count_str:
      dd[s] += 1
  
  print(dd)
  ```

  输出

  ```bash
  defaultdict(<class 'int'>, {'a': 2, 's': 2, 'h': 2, 'o': 4, 'm': 1, 'i': 2, 'u': 1, 'e': 1})
  ```

  





## 3.5 UserDict



