# collections
本模块主要用于Python内置的容器数据类型的拓展. 目前支持:
* dict
  * ChainMap: 将多个dict组合为一个dict
  * Counter: 数量统计
  * OrderedDict: 可排序字典
  * defaultdict: 可设置默认值字典
  * UserDict: 精简版字典子类
* list
  * deque: 双端队列
  * UserList: 精简版list
  * UserString: 封装了list, 精简版的str子类
* tuple
  * namedtuple: 创建命名元组的工厂函数, 类似对象调用