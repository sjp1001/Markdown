# 安装

***

- 下载显卡驱动
- 下载cuda
- 下载pytorch



# 常用

***

### 张量数据类型

- dim维度
- shape行列数（属性）
- size同上（方法）

### 创建方法

```python
1.直接从numpy数组生成
torch.from_numpy(np数据)

2.使用tensor+list生成
torch.tensor([1,2])
torch.FloatTensor([2,3.2])

# tensor接受列表，Tensor可以接受列表和维度（一个t小写一个T大写）
# eg.torch.Tensor(2,3)生成一个两行三列的矩阵

!!!
# 所以有数据一般用tensor
# 没数据用
Tensot(d1,d2,..) #可以设置默认数据类型，一般为FloatTensor
FloatTensor(d1,d2,d3)
DoubleTensor(d1,d2,..)
IntTensor(d1,d2,..)


3.未初始化的数据（一般作为载体）（生成的东西数很乱）
torch.empty(shape)

4.生成随机数
a = torch.rand(d1,d2)
b = torch.rand_like(a)
# b生成一个和a同形状的随机数据

5.torch.randint(small,big,[shape])
# 不会等于big

6.a = torch.randint(0,11,[2,3,4,5])
b = a[0,...]
b = a[0,:,:,:]
c = a[...,:2] #将所有维度隔行取数据
```

### 维度变换

- view和reshape一样

  

- squeeze和unsqueeze

  `a.unsqueeze(0)`在数组a的前边插入一个维度

  eg. 原a维度[3,2,3]，插入后[1,3,2,3]

  `a.unsqueeze(-1)`在数组a的后边插入一个维度

  eg. 原a维度[3,2,3]，插入后[3,2,3,1]



​		`a.squeeze()`去掉所有维度dim=1的部分

​		`a.squeeze(index)`去掉index处维度dim=1的部分，如果维度不为1则不去掉



- expand和repeat

  b原来为[1,32,1,1]

  

  `b.expand(4,32,14,14)`        可将[1,32,1,1]扩展为[4,32,14,14]

  前后的dim必须一样，扩展前维度为1，不为1不能扩展！



​		`b.repeat(1,2,3,4)`     将维度翻倍

​		得到结果[4,64,3,4]

