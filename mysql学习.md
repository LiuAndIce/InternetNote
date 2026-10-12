Mysql

# **执行一条sql语句发生了什么？**

## 连接器

## 查询缓存

## 解析sql

![image-20251112171145463](assets/images/image-20251112171145463-d73e6edcf776.png)

## 执行 SQL



预处理

*

### 优化器

![image-20251112170821810](assets/images/image-20251112170821810-2da4819678ce.png)

### 执行器





## 事务

### 脏读

  事务B读取了事务A没有提交的数据，当事务A发生回滚，那么事务B就是发生了脏读

![image-20260309200549198](assets/images/image-20260309200549198-aa74a2f6a62a.png)

### 不可重复读

两次读取的结果不一样 

![image-20251112175941279](assets/images/image-20251112175941279-b44256d994fa.png)

![image-20251112175902104](assets/images/image-20251112175902104-c02d20ce3e94.png)

不可重复读是担心读取的数据是未提交的，如果事务回滚，则出现数据不一致 但是这个在数据一致性要求不高需要实时获取最新数据的应用场景

### 幻读

幻读是指在一个事务内，多次执行同一查询条件的操作时，结果集中的记录数量发生了变化

![image-20251112180931395](assets/images/image-20251112180931395-6d6c824602e7.png)

**更改数据不可重复读 增删行幻读**

![image-20251114135739243](assets/images/image-20251114135739243-a10278bea0ab.png)





## 事务隔离级别 

隔离水平由高到低排序



### 串行

对于「串行化」隔离级别的事务来说，通过加读写锁的方式来避免并行访问；

### 可重复读

那可重复读这个事务隔离界别 得到是数据更老一些   这个可重复读指的是“**即使重复读取得到数据也是一样的**”

![image-20251112202700495](assets/images/image-20251112202700495-c4b55a2b3b20.png)

![image-20251112202150034](assets/images/image-20251112202150034-ccca9d7fe3f3.png)

#### 可重复读执行过程

![image-20251112210356987](assets/images/image-20251112210356987-38dc0d698c9e.png)

![image-20251112210323541](assets/images/image-20251112210323541-05b240aecd9b.png)

![image-20251112211332235](assets/images/image-20251112211332235-60d131004b13.png)

trx-id min-id 和max-id把空间分为三部分  中间是活跃且没有提交的事务id 前面是以及提交的事务id  后面是下一个还没有开始的事务id

注重理解事务A修改行数据后形成的版本链 和 事务A提交前后为什么事务b读取的还是100



![image-20251112210653565](assets/images/image-20251112210653565-31b52b9da50a.png)







### 读已提交

启动的事务A进行的修改 操作自己的readView是实时更新的，但是外部事务B对数据的更新不会同步到事务A中

### ![image-20251112201702426](assets/images/image-20251112201702426-581890e45a8c.png)

「读提交」隔离级别是在「每个语句执行前」都会重新生成一个 Read View，而「可重复读」隔离级别是「启动事务时」生成一个 Read View，然后整个事务期间都在用这个 ReadView.

那可重复读的readView 还会记录这条语句的sql带来的改变吗？

![image-20251112203907139](assets/images/image-20251112203907139-9284f08080fa.png)

### 读未提交

### 幻读避免

mysqlInnodb默认采用可重复读的隔离界别 **可应对大多数幻读**   **但**当事务中同时存在快照读和当前读时（如先查后改再查），就可能出现幻读

![image-20251112212918694](assets/images/image-20251112212918694-36c06c926fe3.png)

![image-20251112212723683](assets/images/image-20251112212723683-cd32d549bf3f.png)



**除了select查询语句是快照读 其他都是当前读 当前读 读取的是实时已提交的数据** 



![image-20251112213337253](assets/images/image-20251112213337253-acf40bea5cca.png)

![image-20251112213700724](assets/images/image-20251112213700724-8e854e69ace1.png)

![image-20251112213635640](assets/images/image-20251112213635640-b7156235cf25.png)



## 索引

### innodb 行格式 

  reduant 过时 compact最基础的  dynamic 和 compassed是在compact上改进的

![image-20251112224259780](assets/images/image-20251112224259780-ae2461052b1d.png)

![image-20251112224038915](assets/images/image-20251112224038915-655ed64775b9.png)

### 聚簇索引 为什么叶子节点存了数据行？





![image-20251112224010847](assets/images/image-20251112224010847-6308c140e4b1.png)

### 索引失效

#### like模糊匹配

#### 对索引或者查询条件使用函数

#### 对索引或者查询条件进行表达式计算

#### 联合索引最左匹配

![image-20251112231043195](assets/images/image-20251112231043195-a282e4393847.png)



```mysql
-- 无法触发联合索引（跳过了最左字段 `name`）
SELECT * FROM `user` WHERE `age` = 25;

-- 无法充分利用联合索引（跳过了 `age`，仅能用到 `name` 部分）
SELECT * FROM `user` WHERE `name` = '张三' AND `gender` = '男';

-- 触发联合索引（用到全部三个字段）最理想型查询效率最高
SELECT * FROM `user` WHERE `name` = '张三' AND `age` = 25 AND `gender` = '男';
```



#### where 子句中的or

![image-20251112230801342](assets/images/image-20251112230801342-4b3b407b738d.png)





# 三大日志

原子性，持久性，一致性，隔离性

![image-20251113105724791](assets/images/image-20251113105724791-5ccafb859909.png)