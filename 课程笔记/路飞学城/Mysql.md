# Mysql

## 基础知识

```python
1.数据库服务器
	运行数据库管理软件的计算机
2.数据库管理软件
	- 关系型数据库(有表结构)	mysql oracle...
    - 非关系型数据库(无表结构	key-value存储)	mongDB redis..
3.库
	分类的文件夹
4.表
	文件
5.记录
	事物一系列的典型特征
6.数据
	描述事物的符号
7.mysql服务端(mysqld)帮助我们管理文件和文件夹，需要下载mysql客户端或其他的模块连接到mysql服务端，然后使用mysql规定语法格式提交命令，实现文件和文件夹的管理。
```

## 库操作

```mysql
系统数据库
1.information_schema
	虚拟库，不占磁盘空间，存储一些数据库启动后的参数    用户表信息、权限信息、字符信息等
2performace_schema
	收集数据库服务器的性能参数
3.mysql
	授权库  存储系统用户的权限信息
4.test 
	自动创建的测试库
```

```mysql
1.创建数据库
	create database db_name;
2.查看数据库
	show databases;
3.使用数据库
	use db_name;
4.删除数据库
	drop database db_name;
5.修改数据库(只能修改编码)
	alter database db_name charset gbk;
```



## 表操作

```mysql
1.创建表
	create table t_name (字段1 类型(宽度) 约束条件,字段1 类型(宽度) 约束条件...)
    同一张表中字段名不鞥相同，宽度、约束条件都是可选的，字段名，类型是必选的
2.查看表结构
	desc table_name
3.修改表
	修改表名    alter table old_name rename new_name;
    增加字段    alter table table_name add 字段1 类型 约束条件
    删除字段    alter table table_name drop 字段1
    修改字段
    		alter table t1 modify 字段名  新数据类型 约束条件
    		alter table t1 change 旧字段 新字段 数据类型 约束条件
4.复制表
	结构+内容	create table t2 select * from t1;
	结构		create table t2 select * from t1 where 1=2;
5.删除表
	drop table table_name;
	
	
	
俩张表之间的关系
	1.多对一    俩张表任意一张对另一张表是多对一关系 
 	2.多对多    双向多对一就是多对多    需要用另一张表来表示俩个表的之间的关系
 	3.一对一
子查询：一个查询语句嵌套在另一个查询语句中，内层查询语句的结果可以作为外层查询语句的条件
```



## 记录操作

```mysql
1.插入数据	insert into table_name（字段1，字段2，字段3，...）values（值1，值2，值3，...）,（值1，值2，值3，...）,（值1，值2，值3，...）
2.更新数据	update table_name set 字段1='值1',字段2='值2',....;
3.删除数据
	delete from table_name
```

```mysql
4.查询
单表
	语法:select * from t1 where 条件 group by 字段 having 过滤 order by 排序 limit 限制条数
    执行流程	from->where->group by->having->select->distinct->order by->limit
    	1.from找到对应的表
    	2.where取出满足条件的记录
    	3.group by 根据字段分组，如果没有group by就整体作为一组
    	4.having 分组后过滤
    	5.select 查询字段
    	6.distinct 去重
    	7.order by 排序
    	8.limit	限制显示条数
多表查询
	外连接：
		左连	显示左表的全部记录 select * from t1 left join t2 on t1.dep_id=t2.id
		右连	显示右表的全部记录 select * from t1 right join t2 on t1.dep_id=t2.id
	内连接：只连接匹配的	select * from t1 inner join t2 on t1.dep_id = t2.id
	全连接：显示表的全部记录	左连接	union 右连接
```



## 数据类型和约束

```mysql
数据类型
1.数字类型
	整数	整型的宽度不是储存宽度，而是显示宽度
	小数	eg float(m,d)  m  指的是数字的总数，d  指的是小数点后面的位数		
2.日期类型： year 2023     date 2023-8-22      datetime 2023-08-22 20:41:07        
3.字符类型
	char
		定长：存的时候补空格，取得时候删除空格    （利于条件查询）
		简单粗暴，浪费空间，存取速度快
	varchar
		变长：字符个数+字符数据
		精准，节省空间，读取速度慢
4.枚举类型 enum 单选
5.集合类型 set 多选
创建表时 定长类型（性别）放前面，变长类型（地址）放后面


约束条件
1.not null    表示该字段不能为空
2.default    为该字段设置默认值
3.unqiue    标识该字段是唯一的
4.primary key    标识该字段是该表的主键，可以唯一的标识记录
5.auto_increment    标标识该字段的值自动增长（整数类型，且为主键）
6.unsigned  无符号
7.distinct    去重
8.foreign key   标识该字段是该表的外键 外键建立表之前的关系
先建立被关联的表，再见建立关联的表（多对一），
级联删除，级联更新  on delete cascade    on update  cascade
实现逻辑上的关联关系，不建议使用foreign key建立表之前的关系
起到一个约束作用，在被关联表中，如果删除一个记录，那么外键关联到这条记录的都会被删除
```



```mysql
where 约束
	1.比较运算符
	2.between a and b （包含俩端）
	3.成员运算符	in
	4.身份运算符 is
	5.模糊查询 like _单个字符 %多个字符			
	6.逻辑运算符 and or not
group by
	查询的字段只能是分组字段或其他字段的聚合形式，分组字段是unique没有什么意义！
```

## 锁和事务

```mysql



事务的四大特性
1.原子性 多个sql语句绑定在一起，要么都成功，要么都失败
2.一致性 事务实行前后，数据保持一致
3.隔离性 一个事务的执行和另外一个事务的执行是相互隔离的
4.持久性 事务一旦提交，就永久刷到磁盘上了
```

