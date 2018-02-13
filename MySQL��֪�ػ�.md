# 第一章 了解SQL
## 什么是SQL
#### SQL（Structured Query Language）机构化查询语言，是一种专门与数据库通信的语言。
***
### 什么是数据库？
  #### 数据库是一个以某种有组织的方式存储的数据集合。
 数据库(database):保存有组织数据的容器（通常是一个文件或一组文件） 
 ***
 ### 什么是表：
 #### 某种特定类型数据的结构化清单
 ***
 ### 什么是模式：
 模式（schema）：关于数据库和表的布局及特性的信息
 
 ```
 graph TD
    A[DBMS数据库管理系统] --> B{DB数据库}
    B --> C((table1))
    B --> D((table2))
    C --> E[data]
    D --> F[data]
    
 ```
 # 第二章 MySQL简介
 # 第三章 使用MySQL
 ### MySQL广泛使用的原因：
 - reason
   - 成本 - 免费开源，还可以修改
   - 性能 - 执行快
   - 可依赖 - 众多声望高的公司使用其处理重要数
   - 简单 - 安装和使用都很简单
 
 > 数据的所有存储，检索管理和处理实际是有DBMS完成的，MySQL是一种DBMS,即一种数据库软件。目前最新的稳定版本是5.1
 ***
 > DB是DBMS创建或操作的容器，使用DBMS访问DB,DB中有一张或多张有关系的表
 ***
 > table中的数据是一种类型的数据或者一个清单
 
 ```
 graph TD
        B[基于文件共享] -->A{DBMS分类}
        C[基于客户机-服务器] -->A
        D(Microsoft Access和File Maker桌面应用) -->B
        E(MySQL Oracle Microsoft SQL Server) -->C
 ```
 # 第四章 检索数据
 ### sql语句
 > SQL关键字大写，字段小写，使代码更容易阅读和调试，不一定非要大写，都小写也行。
 - 查看mysql版本：进入后 show variables like 'version';
 - 退出：quit exit ctrl+D
 - 显示表列
    - show columns from customers;
    - desc customers;
    - describe customers;
 -  显示广泛的服务器状态：
    - show status;
 -  显示创建指定数据库的MySQL语句：
    - show create database youknowyoucan;
 -  显示创建指定表的MySQL语句：
    - show create table customers;
 -  显示授权用户的安全权限：
    - show grants;
 -  显示服务器报错信息：
    - show errors;
 -  显示服务器警告信息: 
    - show warnings;
 -  在mysql命令行显示允许的show语句：
    - help show;
 -  检索单列：
    - select prod_name from products;
 -  检索多列：
    - select prod_id, prod_name, prod_price from products;
 -  检索所有列：
    - select * from products;
 -  检索不同的行(去重)：
    - select vend_id from products;[14]
    - select distinct vend_id from products;[4]
 -  限制结果：
    - select prod_name from products limit 5,5;[5，5表从行5开始的5行]【索引从0开始】
    - select prod_id, prod_name from products limit 3,4;[从4行开始4行]
    - select prod_id, prod_name from products limit 4 offset 3;[同上mysql5才有]
 -  使用完全限定的表名或列名：
    - select products.prod_name from products;
    - select products.prod_nsme from youknowyoucan.products;

# 第五章 排序检索数据
### sql语句
> sql语句是有子句(clause)组成，排序使用order by子句
- 5.1 排序索引
    - select prod_name from products;
    - select prod_name from products order by prod_name;
- 5.2 按多个列排序
    - select prod_id, prod_price, prod_name from products order by prod_price, prod_name;[价格一样，按照名排]
- 5.3 指定排序方向(asc/desc)
    - select prod_id, prod_price, prod_name from products order by prod_price desc;
    - select prod_id, prod_price, prod_name from products order by prod_price desc, prod_name;[以价格降序，名升序]
    - select prod_price from products order by prod_price desc limit 1;[找出最贵的]
> order by 必须位于from后，limit必须位于order by之后，否则检索信息会失真

# 第六章 过滤数据
> 本章使用select的where子句指定搜索条件
### sql语句
- 6.1 使用where子句
    - select prod_name, prod_price from products where prod_price = 2.50;[相等测试]
> where子句的位置，同时使用order by和where时，order by位于where之后
- 6.2 where子句操作符
    - 6.2.1 检查单个值
      - select prod_name, prod_price from products where prod_name = 'fuses';
      - select prod_name, prod_price from products where prod_price < 10;
      - select prod_name, prod_price from products where prod_price <= 10;
    - 6.2.2 不匹配查询
      - select vend_id, prod_name from products where vend_id <> 1003;
    - 6.2.3 范围值查询 -- between操作符
      - select prod_name, prod_price from products where prod_price between 5 and 10;
    - 6.2.4 空值检查 -- null
      - select prod_name from products where prod_price is null;
      - select cust_id from customers where cust_email is null;
      
> <>与!=结果一样；当与字符串列对比时需要将条件字符串加上单引号，如果是数值列进行比较值的话，不需要加单引号。
***
> between x and y -- 是包括x和y的值。
***
> null(no value) 与字段包含0，空字符串或者仅仅包含空格不同。


# 第七章 数据过滤
> 组合where子句建立跟高级的搜索条件；not和in操作符的学习
### sql语句
- 7.1 组合where子句 -- and与or(操作符)进行连接
   - 7.1.1 and操作符
     - select prod_id, prod_price, prod_name from products where vend_id = 1003 and prod_price <= 10;
   - 7.1.2 or操作符
     - select prod_id, prod_price, prod_name from products where vend_id = 1003 or prod_price <= 10;
   - 7.1.3 计算次序
     - select prod_name, prod_price from products where vend_id = 1002 or vend_id = 1003 or prod_price >= 10;
     - select prod_name, prod_price from products where (vend_id = 1002 or vend_id = 1003) and prod_price >= 10;
- 7.2 in操作符 -- eg: id in(1,2,3)
   - select prod_name, prod_price from products where vend_id in (1002,1003) order by prod_name;  --- 等同于下面
   - select prod_name, prod_price from products where  not in (1002,1003) order by prod_name;
- 7.3 not操作符
   - select prod_name, prod_price from products where vend_id = 1002 or vend_id = 1003 order by prod_name;
    
> and where子句中使用关键字，匹配的是符合所有条件的行；or where 子句匹配的是任意一个符合条件行。
***
> and 默认计算顺序是优于 or,但and or连用都应加上括号以明确地操作符。
***
> or 也能实现in where子句实现的条件查询效果，in的优点：
- 1.使用长的合法清单，in操作符的语法更清楚直观；
- 2.in使用的次序更好管理，因其使用的操作符更是少
- 3.in执行速度比or快
- 4.in可以包含其他select子句，能够动态地建立where子句。


# 第八章 用通配符进行过滤
> 知道什么是通配符；怎样使用like操作符
- 8.1 like操作符
   - 通配符(wildcard)：
     - 用来匹配值的一部分的特殊字符
   - 搜索模式(search pattern)：
     - 由字面值、通配符或者两者组合构成的搜索条件
   - like：
     - 指示mysql后跟的搜索模式利用通配符匹配而不是直接相等匹配进行比较
   - 8.1.1 百分号(%)通配符
     - % --- 表示任意字符出现任意次数 (jet开头的)
       - select prod_id, prod_name from products where prod_name like 'jet%';
     - % --- 在任意位置使用 (不管两端是什么字符，只要包含anvil)
       - select prod_id, prod_name from products where prod_name like '%anvil%';
     - % --- 位置中间(以s开头e结尾)
       - select prod_id, prod_name from products where prod_name like 's%e';
   - 8.1.2 下划线(_)通配符
     - (_)与%用途一样但是，下划线只是匹配单个字符
       - select prod_id, prod_name from products where prod_name like '_ ton anvil';
       - select prod_id, prod_name from products where prod_name like '%ton anvil';
- 8.2 使用通配符的技巧
   - 不要过度使用通配符
   - 除非绝对有必要，不要将通配符置于搜索模式的开始处，搜索起来是最慢的。
   - 注意通配符的位置

> 从技术上，like是谓词(predicate)而不是操作符。
***
> 注意：%不能可以匹配一个或0个或任意多个
   字符，但是不能匹配null值。

# 用正则表达式进行搜素
- 9.1 正则表达式介绍
   - 正则表达式用来匹配文本的特殊的串(字符集合)
   - 所有种类的程序设计语言、文本编辑器、操作系统等都支持正则表达式。
- 9.2 使用MySQL正则表达式
   - 9.2.1 基本字符匹配
     - select prod_name from products where prod_name regexp '1000' order by prod_name;
     - select prod_name from products where prod_name regexp '.000' order by prod_name;
   - 9.2.2 进行or匹配 -- |匹配多个串
     - select prod_name from products where prod_name regexp '1000|2000' order by prod_name;
   - 9.2.3 匹配几个字符之一 --[]
     - select prod_name from products where prod_name regexp '[123 ton]' order by prod_name;【等价于下面】
     - select prod_name from products where prod_name regexp '[1|2|3 ton]' order by prod_name;【不同于下面】
     - select prod_name from products where prod_name regexp '1|2|3 ton' order by prod_name;
   - 9.2.4 匹配范围 -- [0-9]
     - select prod_name from products where prod_name regexp '[1-5] ton' order by prod_name;
   - 9.2.5 匹配特殊字符 -- 双反斜杠+.
     - select vend_name from vendors where vend_name regexp '.' order by vend_name;【匹配所有的】
     ```
     - select vend_name from vendors where vend_name regexp '\\.' order by vend_name;【匹配.】
     ```
   - 9.2.6 匹配字符类
     - 为了方便工作，可使用预定义的字符集，称为字符类(character class)。
   - 9.2.7 匹配多个实例
     - 匹配的条件中的字符出现多次的情况，由重复元字符来完成。
     - select prod_name from products where prod_name regexp '\\([0-9] sticks?\\) order by prod_name';
     - select prod_name from products where prod_name regexp '[[:digit:]]{4}' order by prod_name;   
     - select prod_name from products where prod_name regexp '[0-9][0-9][0-9][0-9]' order by prod_name;
   - 9.2.8 特殊定位符
     - 匹配特定位置的字符，由定位元字符完成
     ```
     - select prod_name from products where prod_name regexp '^[0-9\\.]' order by prod_name;
     ```
     
> ^双重作用：在集合中表取反，否则，指字符串的开始。
> 简单正则表达式测试：在不使用数据库情况下，select检测regexp返回0或者1，如 select 'hello' regexp '[0-9]';
***
|   定位元字符|   说明  |
|:------------|:--------|
|      ^      |文本的开始|
|      $      |文本的结束|
|    [[:<:]]  |词的开始  |
|    [[:>:]]  |词的结尾  |
  
***
|   重复元字符   |   说明   |
|:-----------|---------:|
|      *     |0个或多个匹配|
|      +     |1个或多个匹配，{1,}|
|      ？    |0个或1个匹配，{0,1}|
|     {n}    |指定数目的匹配|
|    {n,}    |不少于指定数目的匹配|
|    {n,m}   |匹配数目的范围m<=255|
***
|   字符类   |   说明   |
|:-----------|---------:|
|  [:alnum:] |任意字母和数字[a-z0-9A-Z]|
|  [:alpha:] |任意字符[a-zA-Z]|
|  [:blank: ]|空格和制表[\\\t]|
|  [:cntrl:] |ASCII制表字符(ASCII0-31和127)|
|  [:digit:] |任意数字[0-9]|
|  [:graph:] |同[:print:可打印任意字符,不包括空格]|
|  [:lower:  ]|任意小写字母[a-z]|
|  [:print:]  |任意可打印字符|
|  [:punct:]  |不在[:alnum:]和[:cntrl:]中 的任意字符|
|  [:space:]  |包括空格在内的任意空白符[空白元字符]|
|  [:upper:]  |任意大写字母[A-Z]|
|  [:xdigit:] |任意十六进制数字[a-fA-F0-9]|
***

    

> like 与 regexp一个重要区别：like 匹配字段，regexp可匹配字段的值，regexp也可以^$来替代like。
***
> mysql中regexp匹配不区分大小写，用binary进行区分，格式 -- regexp binary + '条件'
***
> 双反斜杠加上特殊字符的处理，称为转义(escaping),双反斜杠也用来引用元字符(特殊意义的字符)
##### 空白元字符

|  元字符  |  说明   |
|:---------|---------|
|   \\\f    |  换页   |
|   \\\n    |  换行   |
|   \\\r    |  回车   |
|   \\\t    |  制表   |
|   \\\v    | 纵向制表|
|都是双反斜杠|如果匹配\本身需要三个\|


# 第十章 创建计算字段
> 怎么创建计算字段，以及给字段起别名并引用
- 10.1 计算字段
   - 字段(field):基本上与列(column)的意思相同。
   - 客户机与服务器的格式：在sql语句内完成的许多转换和格式化工作都可以直接在客户机应用程序内完成。一般在数据库服务器上完成这些操作比客户机完成要快得多。
- 10.2 拼接字段
   - 拼接(concatenate)：将值联结到一起构成单个值
   - mysql的不同之处：多数DBMS使用+或||来实现拼接，使用Concat()函数来拼接两个列。
     - select Concat(vend_name,'(',vend_country,')') from vendors order by vend_name;
   - 删除数据右侧的多余空格，用mysql的RTrim()函数来完成。
     - select Concat(RTrim(vend_name),'(,RTrim(vend_country),)') from vendors order by vend_name;
   - 使用别名
     - select Concat(RTrim(vend_name),'(',RTrim(vend_country),')') as vend_title from vendors order by vend_name;
- 10.3 执行算数计算
   - select prod_id, quantity, item_price from orderitems where order_num = 20005;
   - select prod_id, quantity, item_price, quantity*item_price as expanded_price where item_num = 20005;
   - select 测试计算
     - select 3*2；--- 6
     - select Trim('abc'); --- abc
     - select now(); --- 当前时间
> Trim函数，RTrim()去掉串右边的空格，LTrim()去掉串左边的空格，Trim()去掉串两端的空格。
> 导出列:别名有时也称导出列(derived column)

# 第十一章 使用数据处理函数
- 11.1 函数
   - 函数没有sql的可移植性强：能运行在、在多个系统上的代码为可移植性的(portable).
- 11.2 使用函数
   - 处理文本串的文本函数
   - 在数值数据上进行算数操作的数值函数
   - 处理日期和时间值并从这些值中提取特定成分的日期和时间函数
   - 返回DBMS正使用的特殊信息的系统函数
   - 11.2.1 文本处理函数
     - select vend_name, Upper(vend_name) as vend_name_upcase from vendors oeder by vend_name;
     - select cust_name, cust_contact from customers where cust_contact = 'Y. Lie';
     - select cust_name, cust_contact from customers where Soundex(cust_contact) = Soundex('Y Lie');
   - 11.2.2 日期和时间处理函数
     - mysql使用的日期格式：yyyy-mm-dd
     - select cust_id, order_id from orders where order_date = '2005-09-01';【结果同下】
     - select cust_id, order_num from orders where Date(order_date) = '2005-09-01';【更优】
     - select cust_id, order_num from orders where Date(order_date) between '2005-09-01' and '2005-09-30';
     - select cust_id, order_num from orders where Year(order_date) = 2005 and Month(order_date) = 9;
   - 11.2.3 数值处理函数
     - 数值处理函数仅处理数值数据。


#### 常用数值处理函数
|   函数   |   说明   |
|:---------|---------:|
|   Abs()  |返回一个数的绝对值|
|   Cos()  |返回一个数的余弦|
|   Exp()  |返回一个数的指数值|
|   Mod()  |返回除操作的余数|
|   Pi()   |返回圆周率|
|   Rand() |返回一个随机数|
|   Sin()  |返回一个角度的正弦|
|   Sqrt() |返回一个数的平方根|
|   Tan()  |返回一个角度的正切|
***
#### 常用日期和时间处理函数
|   函数   |   说明   |
|:---------|---------:|
| AddDate()|增加一个日期|
| AddTime()|增加一个时间|
| CurDate()|返回当前日期|
| CurTime()|返回当前时间|
| Date()   |返回日期时间的日期部分|
|DateDiff()|计算两个日期之间的差值|
|Date_Add()|高度灵活的日期运算函数|
|Date_Format()|返回一个格式化的日期或时间串|
|  Day()   |返回一个日期的天数部分|
|  Hour()  |返回一个日期的小时部分|
|DayOfWeek()|返回一个日期对应的星期几|
|  Minute()|返回一个时间的分钟部分|
|  Month() |返回一个日期的月份部分|
|  Now()   |返回当前日期和时间|
|  Second()|返回一个时间的秒部分|
|  Time()  |返回一个日期时间的时间部分|
|  Year()  |返回一个日期时间的年份部分|
***
#### 常用文本函数说明
|   函数   |   说明   |
|:---------|---------:|
|  Left()  |返回串左边的字符|
|  Length()|返回串的长度|
|  Locate()|找出串的一个子串|
|  Lower() |将串转为小写|
|  LTrim() |去掉串左边的空格|
|  Right() |返回串右边的字符|
|  RTrim() |去掉串右边的空格|
| Soundex()|返回穿的SOUNDEX值|
|SubString()|返回字串的字符|
|  Upper() |将串转为大写|
> soundex()
是一个将任何文本串转为描述其语音表示的字母数字模式的算法。比较类似发音字符和音节，而不是比较字母。



# 第十二章 汇总数据
> 什么是sql的聚集函数？如何利用它们汇总表的数据？
- 12.1 聚集函数
   - 聚集函数(aggregate function)运行在行祖上，计算和返回单个值的函数。mysql给出了5个聚集函数。
   - 12.1.1 AVG() 函数
     - select AVG(prod_price) as avg_price from products;
     - select AVG(prod_price) as avg_price from products where vend_id = 1003;
   - 12.1.2 COUNT() 函数
     - 两种使用方式：count(*)--行的数目；count(column)--符合条件的行的数目
     - select COUNT(*) as cust_num from customers;
     - select COUNT(cust_email) as cust_num from customers;
   - 12.1.3 MAX() 函数
     - select MAX(prod_price) as max_price from products;
   - 12.1.4 MIN() 函数
     - select MIN(prod_price) as max_price from products;
   - 12.1.5 SUM() 函数
     - select SUM(quantity) as items_ordered from orderitems where order_num = 20005;
     - select SUM(item_price*quantity) as total_price from orderitems where order_num = 20005;
- 12.2 聚集不同值
   - distinct(去重) 与 all ，不指定distinct，默认参数为all
   - select AVG(DISTINCT prod_price) as avg_price from products where vend_id = 1003;
- 12.3 组合聚集函数
   - select COUNT(*) as num_items, MIN(prod_price) as price_min, MAX(prod_price) as price_max, AVG(prod_price) as price_avg from products;

> distinct 可以应用count(),不能应用count(*),distinct必须使用列名。
***
> SUM() 忽略null值列。 所有聚集函数都利用标准的算数操作符在多个列上进行计算。
***
> MAX() 一般是计算数值日期的最大值，也可计算文本最大值，如果数据按照相应的列排序，返回的是最后一行。MAX()忽略null值。
***
> COUNT() 在使用COUNT(*)时，列中null值不忽略；使用特定条件计算时，忽略列中null值。
***
> AVG() --- 只用于单个列，获得多个列的平均值，用多个AVG()
> AVG() 忽略null值
#### sql聚集函数
|   函数   |   说明   |
|:---------|---------:|
|   AVG()  |返回某列的平均值|
|  COUNT() |返回某列的行数|
|   MAX()  |返回某列的最大值|
|   MIN()  |返回某列的最小值|
|   SUM()  |返回某列之和|


# 第十三章 分组数据
> 如何分组数据，学习两个新的select子句：GROUP BY 和HAVING。
- 13.1 数据分组
   - 分组把数据分为多个逻辑组，以便对每个组进行聚集计算。
- 13.2 创建分组
   - select vend_id, COUNT(*) as num_prods from products GROUP BY vend_id;
   - select vend_id, COUNT(*) as num_prods from products GROUP BY vend_idWITH ROLLUP;
- 13.3 过滤分组
   - HACING非常类似WHERE，所有的where子句都可以用having代替。唯一区别：where过滤行，having过滤分组。
   - select cust_id, COUNT(*) as orders from orders GROUP BY cust_id HAVING COUNT(*) >= 2;
   - select vend_id, COUNT(*) as num_prods from products where prod_price >= 10 GROUP BY vend_id HAVING COUNT(*) >= 2;
   -  select vend_id, COUNT(*) as num_prods from products GROUP BY vend_id HAVING COUNT(*) >= 2;
- 13.4 分组和排序 
   - select order_num, SUM(quantity*item_price) as ordertotal from orderitems GROUP BY 
   order_num HAVING SUM(quantity*item_price) >= 50;
   - select order_num, SUM(quantity*item_price) as ordertotal from orderitems GROUP BY 
   order_num HAVING SUM(quantity*item_price) >= 50 ORDER BY ordertotal;
- 13.5 select子句顺序
***
#### select子句及其顺序
|   子句   |   说明   |   是否必须使用   |
|:---------|----------|-----------------:|
| select   |要返回的列或者表达式|  是    |
|  from    |从中检索数据的表| 仅在从表检查数据时使用|
|  where   |行级过滤  |       否         |
|  group by|分组说明  |仅在按组计算聚集时使用|
|  having  |组级过滤  |       否         |
|  order by|输出排序顺序|       否       |
|  limit   |要检索的行数|       否       |


> group by 必须在where子句之后，order by子句之前。
***
> 使用with rollup关键字，得到每个分组及每个分组级别的值，null值组也有返回。


# 第十四章 使用子查询
> 什么是子查询，以及子查询的使用
- 14.1 子查询
   - mysql4.1或更高版本支持
   - 查询(query)任何sql语句都是查询，此术语一般只select。
   - 子查询(subquery)即嵌套在其他查询中的查询。
- 14.2 利用子查询进行过滤
   - select order_num from orderitems where prod_id = 'TNT2';
   - select cust_id from orders where order_num IN (20005,20007);
   - select cust_id from orders where order_num IN (select order_num from orderitems where prod_id = 'TNT2');
   - select cust_name, cust_contact from customers where cust_id IN (10001,10004);
   - select cust_name, cust_contact from customers where cust_id IN (select cust_id from orders where order_num  IN (select order_num from orderitems where prod_id = 'TNT2'));
- 14.3 作为计算字段使用子查询
   - select cust_name, cust_state, (select COUNT(*) from orders where orders.cust_id = customers.cust_id) as orders from customers order by cust_name;
   - 相关子查询(correlated subquery)涉及外部查询的子查询
   - select cust_name, cust_state, (select COUNT(*) from orders where cust_id = cust_id) as orders from customers order by cust_name;
   
> 格式化sql 包含子查询的select子句难以阅读和调试，应把子查询分解为多行并适当进行缩进，简化子查询的使用。


# 第十五章 联结表
> 什么是联结，为什么使用联结，如何编写使用联结的select语句。
- 15.1 联结
   - sql最强大的功能之一是能在数据检索查询的执行中联结(join)表。
     - 15.1.1 关系表
       - 主键(primary key) 唯一标识
       - 外键(foreign key) 为某个表中的一列，它包含一个表的主键值，定义两个表之间的关系。
       - 可伸缩性(scale)能够适应不断增加的工作量而不失败。设计良好的数据库或应用程序称之为可伸缩性好(scale well)。
     - 15.1.2 为什么要使用联结
       - 联结是一种机制，用来在一条select语句中关联表，因此称之为联结。
- 15.2 创建联结
   - select vend_name, prod_name, prod_price from vendors, products where vendors.vend_id = products.vend_id order by vend_name, prod_name;
   - 完全限定列名：当引用的列可能出现二义性时，必须使用完全限定列名(用一个点分隔表名和列名)，否则，mysql将返回错误。
   - 15.2.1 where子句的重要性
     - 笛卡儿积(cartesian product) 由没有联结条件的表关系返回的结果为笛卡儿积。检索出的行数将是第一个表中的行数乘以第二个表中的行数。
     - select vend_name, prod_name, prod_price from vendors, products order by vend_name, prod_name;
   - 15.2.2 内部联结 
     - 目前为止用的联结称为等值联结(equijoin),它基于两个表之间的相等测试，这种联结也成为内部联结。
     - select vend_name, prod_name, prod_price from vendors 
     INNER JOIN products ON vendors.vend_id = products.vend_id;
   - 15.2.3 联结多个表
     - select prod_name, vend_name, prod_price, quantity from orderitems, products, vendors where products.vend_id = vendors.vend_id and orderitems.prod_id = products.prod_id and order_num = 20005;
     - select cust_name, cust_contact from customers where cust_id IN (select cust_id from orders where order_num IN (select order_num from orderitems where prod_id = 'TNT2'));【嵌套的子查询】
     - select cust_name, cust_contact from customers, orders, orderitems where customers.cust_id = orders.cust_id and orderitems.order_num = orders.order_num and prod_id = 'TNT2';【两个联结】
> 多做实验 --- 如上所见，执行一个给定sql操作，一般存在不止一种方法。性能可能会受操作类型、表中的数据量、是否存在索引或键以及其他条件的影响。     
***     
> 性能考虑 --- mysql在运行时关联指定每个表以处理联结。这种处理可能非常消耗资源，应慎重，不要联结不必要的表。联结的表越多，性能下降地越厉害。
***     
> 使用哪种语法 -- ANSI SQL规范首选inner join语法。尽管，where子句定义联结的确比较简单，但使用明确的联结语法能够确保不会忘记联结条件，有时这样做也能影响性能。
***
> 不要忘了where子句，应保证所有联结都有where子句，否则，mysql将返回比想要的多得多的数据。
***
> 叉联结(cross join)有时我们会听到返回称为叉联结的笛卡儿积的联结类型。


# 第十六章 创建高级联结
> 讲解另外一些联结类型/含义/使用方法，如何对被联结的表使用表别名和聚集函数。
- 16.1 使用表别名
   - select Concat(RTrim(vend_name),'(',RTrim(vend_country),')') as vend_title from vendors order by vend_name;
   - select cust_name, cust_contact from customers as c, orders as o, orderitems as oi where c.cust_id = o.cust_id and oi.order_num = o.order_num and prod_id = 'TNT2';
- 16.2 使用不同类型的联结
   - 迄今为止，我们使用的只是称为内部联结或等值联结(equijoin)的简单联结。下面介绍其他3种联结：自联结、自然联结和外部联结。
   - 16.2.1 自联结
     - 使用表别名能在单条   select语句中不止一次引用相同的表。
     - select prod_id, prod_name from products where vend_id = (select vend_id from products where prod_id = 'DTNTR');
     - select p1.prod_id, p1.prod_name from products as p1, products as p2 where p1.vend_id = p2.vend_id and p2.prod_id = 'DTNTR';
   - 16.2.2 自然联结
     - 自然联结是这样一种联结，其中你只能选择哪些唯一的列。一般是通过对表使用select(*)通配符。
     - select c.*, o.order_num, o.order_date, oi.prod_id, oi.quantity, oi.item_price from customers as c, orders as o, orderitems as oi where c.cust_id = o.cust_id and oi.order_num = o.order_num and prod_id = 'FB';  
     - 迄今为止，我们建立的每个内部联结都是自然联结。
   - 16.2.3 外部联结
     - 联结包含了那些在相关表中没有关联行的行，这个类型的联结称为外部联结。
     - select customers.cust_id, orders.order_num from customers INNER JOIN orders ON customers.cust_id = order.cust_id;
     - select customers.cust_id, orders.order_num from customers LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id;
     - select customers.cust_id, orders.order_num from customers RIGHT OUTER JOIN orders ON customers.cust_id = orders.cust_id;
- 16.3 使用带聚集函数的联结
   - select customers.cust_name, customers.cust_id, COUNT(orders.order_num) as num_ord from customers INNER JOIN orders ON customers.cust_id = orders.cust_id GROUP BY customers.cust_id;
   - select customers.cust_name, customers.cust_id, COUNT(orders.order_num) as num_ord from customers LEFT OUTER JOIN orders ON customers.cust_id = orders.cust_id GROUP BY customers.cust_id;
- 16.4 使用联结和联结条件
   - [ ] 注意使用的联结类型，一般使用内联结。
   - [ ] 保证使用正确的联结条件，否则返回不正确的数据。
   - [ ] 应该总是提供联结条件，否则会得出笛卡儿积。
   - [ ] 在一个联结中可以包含多个表，甚至对于每个联结可以采用不同类型的联结，虽然合法，但应分别测试每个联结。
     
> 没有*=操作符 --- mysql不支持简化字符*=和=*的使用，这两个操作符在其他DBMS中是很流行。
***
> 外部联结的类型 --- 做外部联结和右外部联结。它们之间的唯一差别是所关联的表的顺序不同。即左外部联结可通过颠倒from和where子句中的表的顺序为由外部联结。


# 第十七章 组合查询
> 利用union操作符将多条select语句组合成一个结果集。
- 17.1 组合查询
   - mysql允许执行多个查询(多条select语句)，并将结果作为单个查询结果集返回。这些组合查询通常称为并(union)或复合查询(compound query)。
   - 使用组合查询的情况：
     - 在单个查询中从不同的表返回类似结构的数据
     - 对单个表执行多个查询，按单个查询返回数据
- 17.2 创建组合查询
   - 17.2.1 使用union
     - 在给出的每条select语句之间放上关键字union
     - select vend_id, prod_id, prod_price from products where prod_price <= 5;
     - select vend_id, prod_id, prod_price from products where vend_id IN (1001,1002);
     - select vend_id, prod_id, prod_price from products where prod_price <= 5
     UNION
     select vend_id, prod_id, prod_price from products where vend_id IN (1001,1002);
   - 17.2.2 union规则
     - [ ] union必须由两条及两条以上的select语句组成，语句之间用union关键字分开。
     - [ ] union中的每个查询必须包含相同的列、表达式或聚集函数(不过各个列不需要以相同的次序列出)
     - [ ] 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐含的转换类型。
   - 17.2.3 包含或取消重复行
     - union的默认行为，重复的行自动取消，如果想返回所有匹配的行，可使用union all。
     - select vend_id, prod_id, prod_price from products where vend_id IN (1001,1002);
     - select vend_id, prod_id, prod_price from products where prod_price <= 5
     UNION ALL
     select vend_id, prod_id, prod_price from products where vend_id IN (1001,1002);
   - 17.2.4 对组合查询结构排序
     - select语句的输出用order by子句排序。在使用union组合查询时，只能用一个order by，出现在最后一个select语句后。mysql实际是用它来排序所有的select语句。
     - select vend_id, prod_id, prod_price from products where vend_id IN (1001,1002);
     - select vend_id, prod_id, prod_price from products where prod_price <= 5
     UNION
     select vend_id, prod_id, prod_price from products where vend_id IN (1001,1002) order by vend_id, prod_price;

> 组合不同的表 可使用union组合查询应用不同的表。


# 第十八章 全文本搜索
> 学习如何使用mysql的全文本搜索功能进行高级的数据查询和选择。
- 18.1 理解全文本搜索
   - 并非所有的引擎都支持全文本搜索。两个最常用的引擎为MyISAM和InnoDB，前者支持全文搜索，后者不支持。
   - 搜索机制(之前的like、正则)存在几个重要的限制:
     - [ ] 性能 —— 通配符和正则匹配通常匹配表中所有行(这些搜索极少使用表索引)。因此，搜索行增加，搜索可能非常耗时。
     - [ ] 明确控制 —— 使用通配符和正则很难明确控制匹配什么和不匹配什么。
     - [ ] 智能化的结果 —— 虽然基于通配符和正则表达式的搜索提供了非常灵活的搜索，但它们不能提供一种智能化的选择结果。
- 18.2 使用全文本搜索
   - 18.2.1 启用全文本搜索支持
     - 一般在创建表时启用全文本搜素。create table语句接收FULLTEXT子句，它给出被索引列的一个逗号分隔的列表。
     - 不要在导入数据时使用FULLTEXT —— 更新索引要花时间。创建一个新表，应先导入所有数据，再修改表定义FULLTEXT。
   - 18.2.2 进行全文搜索
     - Match()和Against()执行全文本搜索。
     - Match()指定被搜索的列
     - Against()指定要使用的搜索表达式
     - select note_text from productnotes where Match(note_text) Against('rabbit');
     - select note_text from productnotes where note_text like '%rabbit%';【like实现】
     - select note_text, Match(note_text) Against('rabbit') as rank from productnotes;     
   - 18.2.3 使用查询扩展
     - 查询扩展功能只用于mysql版本4.1.1或更高级的版本
     - select note_text from productnotes where Match(note_text) Against('anvils');【未使用只查询出1条】
     - select note_text from productnotes where Match(note_text) Against('anvils' WITH QUERY EXPANSION);
   - 18.2.4 布尔文本搜索
     - mysql支持全文本搜索的另外一种形式，布尔方式(boolean mode)。是个非常缓慢的操作。
     - 提供以下细节：
     - [ ] 要匹配的词
     - [ ] 要排斥的词
     - [ ] 排列提示
     - [ ] 表达式分组
     - [ ] 等
     - select note_text from productnotes Match(note_text) Against('heavy' IN BOOLEAN MODE);
     - select note_text from productnotes Match(note_text) Against('heavy -rope*' IN BOOLEAN MODE);
     - select note_text from productnotes Match(note_text) Against('+rabbit +bait' IN BOOLEAN MODE);【包含】
     - select note_text from productnotes Match(note_text) Against('rabbit bait' IN BOOLEAN MODE);【或】
     - select note_text from productnotes Match(note_text) Against('"rabbit bait"' IN BOOLEAN MODE);【短语】
     - select note_text from productnotes Match(note_text) Against('>rabbit <carrot' IN BOOLEAN MODE);【增减等级】
     - select note_text from productnotes where Match(note_text) Against('+safe +(<combination)' IN BOOLEAN MODE);
   - 18.2.5 全文本搜索的使用说明
     - [ ] 短词（<=3的字符）被忽略且从索引中排除
     - [ ] mysql自带一个内建非用词(stopword
     )列表，全文本搜索时被忽略。
     - [ ] 忽略单引号。
     - [ ] 不具有词分隔符的语言不能恰当地返回全文本检索结果。
     - [ ] 仅在MyISAM数据库引擎中支持全文本搜索。

#### 全文本布尔操作符
|   布尔操作符   |   说明   |
|:---------------|---------:|
|       +        |包含，词必须存在|
|       -        |排除，词必须存在|
|       >        |包含，而且增加等级值|
|       <        |包含，而且减少等级值|
|      ()        |把词组成子表达式|
|       ~        |取消一个词的排序值|
|       *        |词尾的通配符|
|      ""        |定义一个短语|
***    
> 全文本搜索的一个重要部分就是对结果排序，符合条件的文本出现的次序越靠前，优先级越高，越先返回。
***
> 使用完整match()说明 -- 传递给match的值必须与FULLTEXT()定义一样，，如果指定多个列，必须列出它们(次序正确)
***
> 搜索不区分大小写，除非使用binary方式。