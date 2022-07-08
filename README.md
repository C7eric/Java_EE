**Java EE**                                                                                                   

​																															     - Cs7eric





# MySQL







## 数据库概述



### sql、DB、DBMS 是什么

---

#### DB

DataBase 数据库，数据库实际上在硬盘上以文件的形式存在



#### DBMS

DataBase Management System（数据库管理系统，常见的有：MySQL Oracle DB2 Sybase SqlServer...）



#### SQL

结构化查询语言，是一门标准通用的语言。标准的 sql 适合于所有的数据库产品。SQL 属于高级语言，在执行时，实际上内部也会先进行编译，然后再执行 sql （sql 语句的编译由 DBMS 完成）



#### 三者关系

DBMS 负责执行 sql 语句，通过执行 sql 语句来操作 DB 当中的数据

DBMS --（执行）--> SQL -- (操作) ---> DB



### 什么是表

---

#### 表 table

是数据库的基本组成单元，所有的数据都以表格的形式组织，目的是可读性强

一个表包括行列

- 行

  被称为数据/记录（data）

- 列

  被称为字段（column）

  

学号(int)  姓名(varchar)  年龄(int)

  \------------------------------------

  110        张三                    20

  120        李四                    21



##### 每一个字段应该包括哪些属性？

- 字段名
- 数据类型
- 相关的约束





### SQL 语句分类

---

#### DQL

数据查询语言:查询语句，凡是 select  语句都是 DQL

#### DML

数据操作语言：insert 、delete、update 对b表中的数据进行 增删改

#### DDL

事务控制语言：commit 提交事务，rollback 回滚业务

#### DCL

数据控制语言：grant 授权，revoke 撤销权限





### 导入演示数据

---

#### 第一步：登录 MySQL 数据库管理系统

dos 命令窗口

mysql -uroot -p123456



#### 第二步：查看有哪些数据库 

show databases；（这个不是SQL语句，属于MySQL的命令。）



#### 第三步：创建属于我们自己的数据库

create databases bjpowernode; （这个不是SQL语句，属于MySQL的命令。）



#### 第四步：使用 bjpowernode 数据

use bjpowernode;(这个不是 SQL 语言，是MySQL的命令 )



#### 第五步：查看当前使用的数据库中有哪些表

show tables;(这个不是SQL语句，属于MySQL的命令。)



#### 第六步：初始化数据

source D:\C\Java_EE\bjpowernode.sql；



> bjpowernode.sql，这个文件以sql结尾，这样的文件被称为“sql脚本”。
>
> 什么是 **sql** 脚本呢？
>
> 当一个文件的扩展名是.sql，并且该文件中编写了大量的sql语句，我们称这样的文件为sql脚本。



  **注意**：直接使用source命令可以执行sql脚本。

  sql脚本中的数据量太大的时候，无法打开，请使用source命令完成初始化。







#### 删除数据库

drop database bjpowernode;



#### 查看表结构

mysql> desc dept ;
![image-20220708225121242](C:\Users\CSQ-PC\AppData\Roaming\Typora\typora-user-images\image-20220708225121242.png)





#### 表中的数据

mysql> select * from emp;

![image-20220708225221098](C:\Users\CSQ-PC\AppData\Roaming\Typora\typora-user-images\image-20220708225221098.png)







## 常用命令



### 查看版本

mysql> select version();



### 查看当前使用的是哪个数据库

mysql> select database();



### 结束一条语句

\c  



### 退出 MySQL

exit 



### 查看创建表的语句

mysql> show create table emp;







## 简单的查询语句（DQL）



### 语法格式

---

select  字段名1，字段名2，字段名3， ...  from 表名；



#### 提示

- 任何一条 sql 语句都以 “ ； ” 结尾
- sql 语句 不区分大小写



### 演示

查询员工年薪

`mysql> select ename,sal * 12 from emp;`

字段可以参与 数学运算



给查询结果重命名

​    `mysql> select ename,sal * 12 as '年薪' from emp;`

别名中有中文？

​    `select ename,sal * 12 as 年薪 from emp; // 错误`

​    `select ename,sal * 12 as '年薪' from emp;`

**注意**：标准sql语句中要求字符串使用单引号括起来。虽然mysql支持双引号，尽量别用。as关键字可以省略

​    `mysql> select empno,ename,sal * 12 yearsal from emp;`



查询所有字段

`mysql> select * from emp;`

**实际开发不建议使用，效率太低**







## 条件查询



### 语法格式

---

​    select 

​      	字段,字段...

​    from

​      	表名

​    where

​      	条件;



#### 执行顺序 

​	 先是 select 再是 from 再是 where



### 演示

---

#### <>!=

查询工资等于 5000 的员工

`mysql> select ename from emp where sal = 5000;`



查询SMITH的工资？

`select sal from emp where ename = 'SMITH';` // 字符串使用单引号括起来。



找出工资高于3000的员工

`select ename,sal from emp where sal > 3000;`

`select ename,sal from emp where sal >= 3000;`

`select ename,sal from emp where sal < 3000;`

`select ename,sal from emp where sal <= 3000;`

  

找出工资不等于3000的？

`select ename,sal from emp where sal <> 3000;`

`select ename,sal from emp where sal != 3000;`

  

#### between and

找出工资在1100和3000之间的员工，包括1100和3000？

`select ename,sal from emp where sal >= 1100 and sal <= 3000;`

`select ename,sal from emp where sal between 1100 and 3000;` // between...and...是闭区间 [1100 ~ 3000]

`select ename,sal from emp where sal between 3000 and 1100;` // 查询不到任何数据

 

##### 小结

- ​    between and在使用的时候必须左小右大。
- ​    between and除了可以使用在数字方面之外，还可以使用在字符串方面。
  - ` select ename from emp where ename between 'A' and 'C';` 
  - `select ename from emp where ename between 'A' and 'D'; // 左闭右开。`

   

####  is null

 找出哪些人津贴为NULL？

 在数据库当中NULL不是一个值，代表什么也没有，为空。空不是一个值，不能用等号衡量。

 必须使用 is null或者is not null

 `select ename,sal,comm from emp where comm is null;`



找出哪些人津贴不为NULL？

` select ename,sal,comm from emp where comm is not null;`



#### or

找出哪些人没有津贴？

` select ename,sal,comm from emp where comm is null or comm = 0;`



找出工作岗位是MANAGER和SALESMAN的员工？

`select ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';`



#### and和or联合起来用

找出薪资大于1000的并且部门编号是20或30部门的员工。

`mysql> select ename,sal,deptno from emp where sal > 1000 and (deptno = 20 or deptno = 30);`



##### 注意

当运算符的优先级不确定的时候加小括号。



#### in等同于or

找出工作岗位是MANAGER和SALESMAN的员工？

`select ename,job from emp where job = 'SALESMAN' or job = 'MANAGER';`

`select ename,job from emp where job in('SALESMAN', 'MANAGER');`

`select ename,job from emp where sal in(800, 5000);` // in后面的值不是区间，是具体的值。



#### not in 

不在这几个值当中。

`select ename,job from emp where sal not in(800, 5000);`

​    

#### 模糊查询like 

找出名字当中含有O的？（在模糊查询当中，必须掌握两个特殊的符号，一个是%，一个是_）

%代表任意多个字符，_代表任意1个字符。

`select ename from emp where ename like '%O%';`



找出名字中第二个字母是A的？

`select ename from emp where ename like '_A%';`



找出名字中有下划线的？

`mysql> select * from t_user;`

`select name from t_user where name like '%_%';`



找出名字中最后一个字母是T的？

`select ename from emp where ename like '%T';`





## 排序数据

 

### 语法格式

---

  select 

   	 ename,sal 

  from 

   	 emp 

  order by

  	  sal;



#### 注意

默认是升序。怎么指定升序或者降序呢？

- asc表示升序
- desc表示降序。

 ```mysql
 select ename , sal from emp order by sal; // 升序
 
 select ename , sal from emp order by sal asc; // 升序
 
 select ename , sal from emp order by sal desc; // 降序。
 ```



 

按照工资的降序排列，当工资相同的时候再按照名字的升序排列。

  ```mysql
  select ename,sal from emp order by sal desc;
  
  select ename,sal from emp order by sal desc , ename asc;
  ```



#### 细节

越靠前的字段越能起到主导作用。只有当前面的字段无法完成排序的时候，才会启用后面的字段。

 

找出工作岗位是SALESMAN的员工，并且要求按照薪资的降序排列。

```mysql
  select 

​    ename,job,sal

  from

​    emp

  where 

​    job = 'SALESMAN'

  order by

​    sal desc;
```



select 

​    字段            3

  from

​    表名            1

  where

​    条件            2

  order by

​    ....                4

  

  **order by是最后执行的。**

































































































































































































































# Java Web





# Spring 5





# Spring MVC





# MyBatis





# MyBatis Plus





# SpringBoot 2





# Spring Security





# Maven/Gradle
