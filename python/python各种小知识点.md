- pandas读取文件中存在 " nan " 的时候（dataframe格式）：

  ```python
  # 将nan转换成0
  df = df.where(data_frame.notnull(),0)
  ```



- True类型的东西可以用 1 - True

  ```python
  flag = True
  flag = 1 - flag
  # flag变成false
  ```



- 判定列表参数是否足够：

  ```python
  # 使用all函数
  # all() 函数用于判断给定的可迭代参数 iterable 中的所有元素是否都为 TRUE
  # 元素除了是 0、空、None、False 外都算 True。
  
  a = 1
  b = 1
  c = 1
  
  flag = all([a,b,c])
  # flag = True
  ```



- 字符串传递参数

  - Python2.6 开始，新增了一种格式化字符串的函数 **str.format()**，它增强了字符串格式化的功能。

    基本语法是通过 **{}** 和 **:** 来代替以前的 **%** 。

    format 函数可以接受不限个参数，位置可以不按顺序

  ```python
  >>>"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
  'hello world'
   
  >>> "{0} {1}".format("hello", "world")  # 设置指定位置
  'hello world'
   
  >>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
  'world hello world'
  ```
  
  也可以直接设置参数

  ```python
  print("网站名：{name}, 地址 {url}".format(name="菜鸟教程", url="www.runoob.com"))  
  
  # 通过字典设置参数 
  site = {"name": "菜鸟教程", "url": "www.runoob.com"} 
  print("网站名：{name}, 地址 {url}".format(**site))  
  
  # 通过列表索引设置参数 
  my_list = ['菜鸟教程', 'www.runoob.com'] 
  print("网站名：{0[0]}, 地址 {0[1]}".format(my_list))  # "0" 是必须的
  ```

  输出结果为：
  
  ```python
  网站名：菜鸟教程, 地址 www.runoob.com
  网站名：菜鸟教程, 地址 www.runoob.com
  网站名：菜鸟教程, 地址 www.runoob.com
  ```
  
  转义数字格式化
  
  ```python
  >>> print("{:.2f}".format(3.1415926));
  3.14
  ```
  
  - 也可以直接用**%**
  
  ```python
  name = 'xiaoming'
  
  s2 = '%s 学习很认真' % name
  
  # 结果
  # xiaoming 学习很认真
  ```
  
  ```python
  name = 'xiaoming'
  # s1 = "{} {}".format("hello", "world")
  s2 = '%s %s 学习很认真' %(name,name)
  ```
  
  

- 判定一个值是不是nan，用numpy库

  ```python
  np.isnan(num)
  # num如果是nan则返回True
  ```