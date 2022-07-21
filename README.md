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

`mysql> select ename,sal * 12 as '年薪' from emp;`

别名中有中文？

`select ename,sal * 12 as 年薪 from emp; // 错误`

`select ename,sal * 12 as '年薪' from emp;`

**注意**：标准sql语句中要求字符串使用单引号括起来。虽然mysql支持双引号，尽量别用。as关键字可以省略

`mysql> select empno,ename,sal * 12 yearsal from emp;`



查询所有字段

`mysql> select * from emp;`

**实际开发不建议使用，效率太低**







## 条件查询



### 语法格式

---

select 

​      	字段,字段...

from

​      	表名

where

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





## 数据处理函数



### 单行处理函数

---



#### 特点

一个‘输入对应一个输出

与单行函数相对的是：多行处理函数。（多行函数特点：多个输入，对应一个输出）



#### 常见的单行处理函数

##### lower

转换小写

`mysql> select lower(ename) from emp;`

`mysql> select lower(ename) as ename from emp;`

##### upper 

转换大写



##### substr

取子串（substr（被截取的字符串，起始下标，截取的长度）

`mysql> select substr(ename,1,2) as sub from emp;`



###### 注意

起始下标从 1  开始



###### 示例

`mysql> select ename from emp where substr(ename,1,1) = 'A';`

查询 emp 中 名字首字母 为 A 的 人员



##### concat 

进行字符串的拼接

`mysql> select concat(ename,empno) from emp;`



###### 示例

首字母小写

`mysql> select concat(lower(substr(ename,1,1)),substr(ename,2,length(ename)-1)) from emp;`



##### length 

取长度

`mysql> select length(ename) as enamelength from emp;`



##### trim 

去空格

`mysql> select ename from emp where ename = '  KING';  

`mysql> select * from emp where ename = trim('   KING');`



##### str_to_date

将字符串转换成日期



##### date_format

格式化日期



##### format

设置千分位



##### round 

四舍五入

```mysql
mysql> select abs from emp;
ERROR 1054 (42S22): Unknown column 'abs' in 'field list'
```

这样肯定报错，因为会把 abc 当作一个字段的名字，去 emp 表中去找 abc 字段



###### 引入

`mysql> select 'csq' from emp;`

select 后面可以跟某个表的字段名（可以等同看作变量名），也可以跟字面量/字面值（数据）



`mysql> select round(12524.365,0) from emp;`

`mysql> select round(12524.365,1) from emp;` 保留一位小数

`mysql> select round(12524.365,-1) from emp;`保留十位

`mysql> select round(rand() * 100,1) from emp;` 100 以内的随机数



##### iffull

可以将 null 转换为一个具体值

ifnull 是空处理函数，专门处理空的。

在所有数据库当中，只要有 null 参与的数学运算，最终结果就是 null

`mysql> select ename ,sal + comm  as salcomm from emp;`

<img src="C:\Users\CSQ-PC\AppData\Roaming\Typora\typora-user-images\image-20220709222152642.png" alt="image-20220709222152642" style="zoom:33%;" />



###### 示例

计算每个员工的年薪

年薪 = （月薪 + 月补助）* 12 

`mysql> select ename , (sal + comm) * 12 as yearsal  from emp;`  // error

**null 只要参与运算，最终结果一定是 null ,为了避免这个现象，需要使用  ifnull 函数**.

<img src="C:\Users\CSQ-PC\AppData\Roaming\Typora\typora-user-images\image-20220709232249769.png" alt="image-20220709232249769" style="zoom:33%;" />



###### 用法

ifnull(数据，被当作哪个值)

`mysql> select ename,(sal + ifnull(comm,0)) * 12 yearsal from emp;`

##### rand

生成随机数

`mysql> select rand() from emp;`

`mysql> select round(rand() * 100,1) from emp;` 100 以内的随机数



##### case..when..then..when..then..else..end

###### 案例  

当员工的工作岗位是 MANAGER时，工资上调 10 % 。 当员工的工作岗位是 SALESMAN 时，工资上调 50 % 。（注意：不修改数据库，只是将查询结果显示为工资上调）

```mysql
mysql> select
    -> ename,
    -> job,
    -> (case job when 'MANAGER' then sal * 1.1
    -> when 'SALESMAN' then sal * 1.5
    -> else sal end)
    -> as newsal
    -> from
    -> 		emp;
```







## 分组函数



### 多行处理函数

---



#### 特点

- 输入多行，最终 输出一行
- 分组函数在使用的时候，必须先进行分组，然后才能用
- 如果不进行分组，整张表默认为一组



#### 常用函数

##### count

计数



##### sum

求和



##### avg 

求平均值

 

##### max

最大值

`select max(sal) from emp;`

##### min

最小值

`mysql> select min(sal) from emp;`



#### 注意事项

- 分组函数自动忽略 null，不需要提前对 bull  进行处理
- count (*)  和 count(字段) 有什么区别
  - count（具体字段) ： 表示统计该字段下 所有不为 null 的元素总数
  - count（*）：统计表中的总行数（只要有一行数据，count++)【因为每一行记录不可能 全为 null.数据中有一列不为 null ,则这行数据就是有效的)
- 分组函数不能直接使用在 where 子句中
- 所有的分组函数可以组合起来一起用











## 分组查询

分组查询主要涉及到两个子句，分别是 group by  和  having



### group by

---

- 取得每个工作岗位的工资合计，要求显示岗位名称和工资合计

  `select job,sum(sal) from emp group by job;`

  **如果使用了 order by,order by 必须放在 group  by 后面**

  

- 按照工作岗位和部门编码分组，取得的工资合计

  `mysql> select job,sum(sal) from emp group by deptno,job;`

  在SQL语句中若有group by 语句，那么在select语句后面只能跟**分组函数** + **参与分组的字段**。





### having

---

如果想对分组数据再进行过滤需要使用having子句



- 取得每个岗位的平均工资大于两千

  `mysql> select job,avg(sal) from emp group by job having avg(sal) > 2000;`





### 分组函数的执行顺序

---

- 根据条件查询数据
- 分组
- 采用having过滤，取得正确的数据





### select 语句总结

---

#### 完整的 select 语句

```mysql
select 字段
from 表名
where …….
group by ……..
having …….(就是为了过滤分组后的数据而存在的—不可以单独的出现)
order by ……..
```



#### 执行顺序

1.  首先执行where语句过滤原始数据
2.  执行group by进行分组
3.  执行having对分组数据进行操作
4.  执行select选出数据
5. 执行order by排序



#### 原则

能在where中过滤的数据，尽量在where中过滤，效率较高。having的过滤是专门对分组之后的数据进行过滤的。







## 连接查询



### SQL92 语法

---

也可以叫跨表查询，需要关联多个表进行查询



- 显示每个员工信息，并显示所属的部门名称

  `select ename, dname from emp, dept;`

  以上语句不正确，会输出56条语句，其实就是两张表记录的成绩，这种情况我们称为 笛卡尔积 现象，出现错误的原因是 没有指定连接条件

  **指定连接条件**

  `mysql> select e.ename,d.dname from emp e,dept d where e.deptno = d.deptno;`

   

  以上查询也称为 “内连接”，只查询相等的数据（连接条件相等的数据）

  

- 取得员工和所属的领导的姓名

  `mysql> select e.ename,m.ename from emp e,emp m where e.mgr = m.empno;`

  以上称为“自连接”，只有一张表连接，具体的查询方法，把一张表看作两张表即可，如以上示例：第一个表emp e代码了员工表，emp m代表了领导表，相当于员工表和部门表一样





### SQL99 语法

---



- （**内连接**）显示薪水大于2000的员工信息，并显示所属的部门名称

  - 采用SQL92语法：

    `select e.ename, e.sal, d.dname from emp e, dept d where e.deptno=d.deptno and  e.sal > 2000;`

  

  - 采用SQL99语法：

    `select e.ename, e.sal, d.dname from emp e join dept d on e.deptno=d.deptno where e.sal>2000;`

    `select e.ename, e.sal, d.dname from emp e inner join dept d on e.deptno=d.deptno where e.sal>2000;`在实际中一般不加inner关键字

    

  Sql92语法和sql99语法的区别：99语法可以做到表的连接和查询条件分离，特别是多个表进行连接的时候，会比sql92更清晰

  

- （**外连接**）显示员工信息，并显示所属的部门名称，如果某一个部门没有员工，那么该部门也必须显示出来

  - 左连接 

    `select e.ename, e.sal, d.dname from emp e right join dept d on e.deptno=d.deptno;`

  - 右连接

    `select e.ename, e.sal, d.dname from dept d left join emp e on e.deptno=d.deptno;`



#### 连接分类

##### 内连接

- 表1  inner join  表2  on  关联条件
- 做连接查询的时候一定要写上关联条件
- inner 可以省略

##### 外连接

- 左外连接

  表1  left  outer  join  表2  on  关联条件

  做连接查询的时候一定要写上关联条件



* 右外连接.

  outer  可以省略*右外连接

  表1  right  outer  join  表2  on  关联条件

  做连接查询的时候一定要写上关联条件

  outer  可以省略

  



##### 左外连接（左连接）和右外连接（右连接）的区别：

- 左连接以左面的表为准和右边的表比较，和左表相等的不相等都会显示出来，右表符合条件的显示,不符合条件的不显示
- 右连接恰恰相反，以上左连接和右连接也可以加入outer关键字，但一般不建议这种写法，



```mysql
select e.ename, e.sal, d.dname from emp e right outer join dept d on e.deptno=d.deptno;
select e.ename, e.sal, d.dname from dept d left outer join emp e on e.deptno=d.deptno;
```





## 子查询

子查询就是嵌套的select语句，可以理解为子查询是一张表



### 在where语句中使用子查询，也就是在where语句中加入select语句

---

- 查询员工信息，查询哪些人是管理者，要求显示出其员工编号和员工姓名

  - 首先取得管理者的编号，去除重复的

    `mysql> select distinct mgr from emp where mgr is not null;`

  - 查询员工编号包含管理者编号的

    `mysql> select empno,ename from emp where empno in(select mgr from emp where mgr is not null);`

  - 查询哪些人的薪水高于员工的平均薪水，需要显示员工编号，员工姓名，薪水

    实现思路

    1. 取得平均薪水

       `mysql> select avg(sal) from emp;`

    2. 取得薪水大于平均薪水的员工 

       `mysql> select empno,ename,sal from emp where sal > (select avg(sal) from emp);`





### 在from语句中使用子查询，可以将该子查询看做一张表

---

- 查询员工信息，查询哪些人是管理者，要求显示出其员工编号和员工姓名

  1. 首先取得管理者的编号，去除重复的

     `mysql> select distinct mgr from emp where mgr is not null;`

  2. 将以上查询作为一张表，放到from语句的后面

     ```mysql
     使用92语法：
     select e.empno, e.ename from emp e, (select distinct mgr from emp where mgr is not null) m where e.empno=m.mgr;
     使用99语法：
     select e.empno, e.ename from emp e join (select distinct mgr from emp where mgr is not null) m on e.empno=m.mgr;
     ```

     

- 查询各个部门的平均薪水所属等级，需要显示部门编号，平均薪水，等级编号实现思路

  1. 首先取得各个部门的平均薪水

     `mysql>  select deptno ,avg(sal) avg_sal from emp group by deptno;`

  2. 将部门的平均薪水作为一张表与薪水等级表建立连接，取得等级

     `select a.deptno,a.avg_sal,g.grade from (select deptno,avg(sal) avg_sal from emp group by deptno ) a join salgrade g on a.avg_sal between g.losal and hisal;`

     

- 在select语句中使用子查询

  1. 查询员工信息，并显示出员工所属的部门名称

     ```mysql
     第一种做法，将员工表和部门表连接
     select e.ename, d.dname from emp e, dept d where e.deptno=d.deptno;
     第二种做法，在select语句中再次嵌套select语句完成部分名称的查询
     select e.ename, (select d.dname from dept d where e.deptno=d.deptno) as dname from emp e;
     ```





## Union



### union可以合并集合（相加）

---











































































































































































# Java Web





# Spring 5





# Spring MVC





# MyBatis





# MyBatis Plus





# SpringBoot 2





# Spring Security





# Maven/Gradle
