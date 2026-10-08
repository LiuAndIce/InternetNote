Mysql

# **执行一条sql语句发生了什么？**

## 连接器

## 查询缓存

## 解析sql

![image-20251112171145463](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112171145463.png)

## 执行 SQL



预处理

*

### 优化器

![image-20251112170821810](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112170821810.png)

### 执行器





## 事务

### 脏读

  事务B读取了事务A没有提交的数据，当事务A发生回滚，那么事务B就是发生了脏读

![image-20260309200549198](C:/Users/Mr.liu/AppData/Roaming/Typora/typora-user-images/image-20260309200549198.png)

### 不可重复读

两次读取的结果不一样 

![image-20251112175941279](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112175941279.png)

![image-20251112175902104](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112175902104.png)

不可重复读是担心读取的数据是未提交的，如果事务回滚，则出现数据不一致 但是这个在数据一致性要求不高需要实时获取最新数据的应用场景

### 幻读

幻读是指在一个事务内，多次执行同一查询条件的操作时，结果集中的记录数量发生了变化

![image-20251112180931395](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112180931395.png)

**更改数据不可重复读 增删行幻读**

![image-20251114135739243](C:/Users/Mr.liu/AppData/Roaming/Typora/typora-user-images/image-20251114135739243.png)





## 事务隔离级别 

隔离水平由高到低排序



### 串行

对于「串行化」隔离级别的事务来说，通过加读写锁的方式来避免并行访问；

### 可重复读

那可重复读这个事务隔离界别 得到是数据更老一些   这个可重复读指的是“**即使重复读取得到数据也是一样的**”

![image-20251112202700495](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112202700495.png)

![image-20251112202150034](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112202150034.png)

#### 可重复读执行过程

![image-20251112210356987](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112210356987.png)

![image-20251112210323541](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112210323541.png)

![image-20251112211332235](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112211332235.png)

trx-id min-id 和max-id把空间分为三部分  中间是活跃且没有提交的事务id 前面是以及提交的事务id  后面是下一个还没有开始的事务id

注重理解事务A修改行数据后形成的版本链 和 事务A提交前后为什么事务b读取的还是100



![image-20251112210653565](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112210653565.png)







### 读已提交

启动的事务A进行的修改 操作自己的readView是实时更新的，但是外部事务B对数据的更新不会同步到事务A中

### ![image-20251112201702426](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112201702426.png)

「读提交」隔离级别是在「每个语句执行前」都会重新生成一个 Read View，而「可重复读」隔离级别是「启动事务时」生成一个 Read View，然后整个事务期间都在用这个 ReadView.

那可重复读的readView 还会记录这条语句的sql带来的改变吗？

![image-20251112203907139](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112203907139.png)

### 读未提交

### 幻读避免

mysqlInnodb默认采用可重复读的隔离界别 **可应对大多数幻读**   **但**当事务中同时存在快照读和当前读时（如先查后改再查），就可能出现幻读

![image-20251112212918694](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112212918694.png)

![image-20251112212723683](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112212723683.png)



**除了select查询语句是快照读 其他都是当前读 当前读 读取的是实时已提交的数据** 



![image-20251112213337253](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112213337253.png)

![image-20251112213700724](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112213700724.png)

![image-20251112213635640](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112213635640.png)



## 索引

### innodb 行格式 

  reduant 过时 compact最基础的  dynamic 和 compassed是在compact上改进的

![image-20251112224259780](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112224259780.png)

![image-20251112224038915](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112224038915.png)

### 聚簇索引 为什么叶子节点存了数据行？





![image-20251112224010847](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112224010847.png)

### 索引失效

#### like模糊匹配

#### 对索引或者查询条件使用函数

#### 对索引或者查询条件进行表达式计算

#### 联合索引最左匹配

![image-20251112231043195](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112231043195.png)



```mysql
-- 无法触发联合索引（跳过了最左字段 `name`）
SELECT * FROM `user` WHERE `age` = 25;

-- 无法充分利用联合索引（跳过了 `age`，仅能用到 `name` 部分）
SELECT * FROM `user` WHERE `name` = '张三' AND `gender` = '男';

-- 触发联合索引（用到全部三个字段）最理想型查询效率最高
SELECT * FROM `user` WHERE `name` = '张三' AND `age` = 25 AND `gender` = '男';
```



#### where 子句中的or

![image-20251112230801342](C:\Users\Mr.liu\AppData\Roaming\Typora\typora-user-images\image-20251112230801342.png)





# 三大日志

原子性，持久性，一致性，隔离性

![image-20251113105724791](C:/Users/Mr.liu/AppData/Roaming/Typora/typora-user-images/image-20251113105724791.png)