# request

`request`为客户端请求信息的封装, 可以使用这个对象来提取需要的参数

# 1. ImmutableMultiDict

request中多次采用了不可变字典, 来封装客户端传递的数据

类型`ImmutableMultiDict`, 不可变字典, 不能对其修改, 否则抛出

```python
TypeError: 'ImmutableMultiDict' objects are immutable
```

由于不可变类型, 不能使用增删改操作, 所以不可用方法没有展示.

**方法:**

* **get(self, key, default=None, type=None)**

  获取一个key对应的value, 如果参数对应的值是list, 只能获取到list的第一个元素

  * key: 键
  * default: 如果没有取到值, 这使用此数据做填充
  * type: 将获取到值, 按照type转换为该类型

  ```python
  http://www.baidu.com/?key=1&key=2&key=3
      
  request.args.get("key")  # 1
  ```

  

* **getlist(self, key, type=None)**

  获取一个key对应的value, 可以获得全部参数

  * key: 键
  * type: 类型转换

  ```python
  http://www.baidu.com/?key=1&key=2&key=3
      
  request.args.getlist("key")  # ["1", "2", "3"]
  ```

* **items(self, multi=False)**

  获取key, value的二元组的可迭代对象.

  * multi: 类似get与getlist的区别, False一个元素/True一个列表

  ```python
  http://www.baidu.com/?key=1&key=2&key=3
      
  list(request.args.items(False))  # [('key', '1')]
  ```

* **keys**

* **lists**

* **listvalues**

* **to_dict**

* **update**

* **values**



# 1. 请求数据
## 1.1 url数据

`request.args`, 用于提取url中的参数.



**示例:**

```python
request.args.get("a")
request.items()
```

## 1.2 form表单数据

form表单数据包含两种:

* 文本类型
* 文件类型

### 1.2.1 文本类型

`request.form`, 用于获取普通的text, 选择标签等.

* 类型`ImmutableMultiDict`
* 

# 2. 客户端信息

