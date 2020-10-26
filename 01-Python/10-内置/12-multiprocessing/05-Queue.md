<1> 创建: q = Queue(number) # 限制存放数量 
<2> 存放: q.put(item, timeout) # 放入, 不等待: put_nowait 
<3> 获取: q.get(item, timeout) # 取出, 不等待: get_nowait 
<4> 大小: q.qsize() 
<5> 判空: q.empty() 
<6> 判满: q.full()