# 马哥带你学sql

## SQL

### SQL数据定义 DDL

SQL DDL不仅能定义表的集合，还可指定每个表的详细信息，具体包括:表的模式，列的数据类型，完整性约束，表的索引，表的授权权限信息，表使用的物理存储结构。

#### 数据类型（mysql）

##### 1. 整数类型（Integer Types）

| 数据类型              | 字节  | 范围（有符号）                        | 范围（无符号）           | 说明     |
| ----------------- | --- | ------------------------------ | ----------------- | ------ |
| **TINYINT**       | 1   | -128 ~ 127                     | 0 ~ 255           | 小整数值   |
| **SMALLINT**      | 2   | -32,768 ~ 32,767               | 0 ~ 65,535        | 较小整数值  |
| **MEDIUMINT**     | 3   | -8,388,608 ~ 8,388,607         | 0 ~ 16,777,215    | 中等整数值  |
| **INT / INTEGER** | 4   | -2,147,483,648 ~ 2,147,483,647 | 0 ~ 4,294,967,295 | 常用整数类型 |
| **BIGINT**        | 8   | -2⁶³ ~ 2⁶³-1                   | 0 ~ 2⁶⁴-1         | 大整数值   |

> 💡 可使用 `UNSIGNED` 修饰为无符号类型，例如：`INT UNSIGNED`。

---

##### 2. 浮点数与定点数（Floating-Point & Fixed-Point Types）

| 数据类型             | 字节    | 范围         | 精度        | 说明      |
| ---------------- | ----- | ---------- | --------- | ------- |
| **FLOAT(M,D)**   | 4     | 约 ±3.4E38  | 精度 7 位左右  | 单精度浮点数  |
| **DOUBLE(M,D)**  | 8     | 约 ±1.7E308 | 精度 15 位左右 | 双精度浮点数  |
| **DECIMAL(M,D)** | 取决于 M | 精度最高 65 位  | 精确小数存储    | 货币、财务常用 |

> ✅ **建议：** 对于需要高精度的金额计算，使用 `DECIMAL` 而不是 `FLOAT`。

---

##### 3、字符串类型（String Types）

| 数据类型                  | 最大长度          | 说明          |
| --------------------- | ------------- | ----------- |
| **CHAR(M)**           | 0~255 字节      | 定长字符串（空间固定） |
| **VARCHAR(M)**        | 0~65535 字节    | 变长字符串（常用）   |
| **TINYTEXT**          | 255 字节        | 短文本         |
| **TEXT**              | 65,535 字节     | 普通文本        |
| **MEDIUMTEXT**        | 16,777,215 字节 | 中等长度文本      |
| **LONGTEXT**          | 4GB           | 超长文本        |
| **ENUM('a','b',...)** | 1~2 字节        | 枚举类型        |
| **SET('a','b',...)**  | 1~8 字节        | 集合类型，可多选    |

> ⚠️ 注意：`CHAR` 会自动填充空格至指定长度，`VARCHAR` 不会。

---

##### 5、日期与时间类型（Date & Time Types）

| 数据类型          | 字节  | 格式                  | 范围                      | 说明       |
| ------------- | --- | ------------------- | ----------------------- | -------- |
| **DATE**      | 3   | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-31 | 日期       |
| **TIME**      | 3   | HH:MM:SS            | -838:59:59 ~ 838:59:59  | 时间       |
| **DATETIME**  | 8   | YYYY-MM-DD HH:MM:SS | 同上                      | 日期和时间    |
| **TIMESTAMP** | 4   | YYYY-MM-DD HH:MM:SS | 1970-01-01 ~ 2038-01-19 | 自动记录修改时间 |
| **YEAR**      | 1   | YYYY                | 1901 ~ 2155             | 年份       |

> 🕐 `TIMESTAMP` 会受时区影响，而 `DATETIME` 不受时区影响。

---

##### 6、二进制类型（Binary Types）

| 数据类型             | 最大长度          | 说明       |
| ---------------- | ------------- | -------- |
| **BINARY(M)**    | 0~255 字节      | 定长二进制字符串 |
| **VARBINARY(M)** | 0~65535 字节    | 变长二进制字符串 |
| **TINYBLOB**     | 255 字节        | 小二进制对象   |
| **BLOB**         | 65,535 字节     | 二进制大对象   |
| **MEDIUMBLOB**   | 16,777,215 字节 | 中等二进制对象  |
| **LONGBLOB**     | 4GB           | 超大二进制对象  |

> 💾 通常用于存储图片、文件、音频等二进制数据。

---

##### 7、JSON 类型（MySQL 5.7+）

| 数据类型     | 说明                       |
| -------- | ------------------------ |
| **JSON** | 用于存储 JSON 格式数据，支持函数查询与校验 |

#### 模式定义

##### 建表

建表语句

```sql
CREATE Table students(
    name VARCHAR(20),
    age INT,
    gpa DECIMAL(3,2)
);
```

如图，students表建立成功

![](/Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-17-15-48-42-image.png)

##### 删表

```sql
DROP Table students;
```

##### 改表

```sql
ALTER TABLE students ADD sex char(4);//增加性别属性

ALTER TABLE students DROP sex; //删除性别属性
```

#### 完整性约束

primary key(主键)：规范指出构成，主码的属性必须是非空且唯一的。

```sqf
CREATE Table students(
    name VARCHAR(20),
    age INT,
    gpa DECIMAL(3,2),
    PRIMARY KEY(name)
);
//设置name为主键
```

foreign key(外键)：规范指出任意元组的外键属性取值都必须对应被参照关系s中的某个元组的主键的属性值。

```sql
CREATE TABLE course (
    course_id VARCHAR(20),
    course_name VARCHAR(20),
    PRIMARY KEY(course_id)
);//建立课程表
CREATE Table students(
    name VARCHAR(20),
    age INT,
    gpa DECIMAL(3,2),
    course_id VARCHAR(20),
    PRIMARY KEY(name),
    FOREIGN KEY(course_id) REFERENCES course(course_id)//设置外键
);
```

not null(不能为空):

```sql
name VARCHAR(20) not null
```

候选键：

  标识性：一个数据表的所有记录都具有不同的候选键
  最小性：任一候选键的任何真子集都不能唯一标识一个记录（比如在成绩表中（学号,课程号）是一个候选键，单独的学号，课程号都不能决定一条记录）
  非空性：不能为空
  候选键是没有多余属性的超键
  举例：学生ID是候选码，那么含有候选码的都是超键。

```sql
unique(A,B);
//A,B组成一个候选键
```

定义完整性约束

```sql
//例如学生的绩点大于0
CREATE Table students(
    name VARCHAR(20),
    age INT,
    gpa DECIMAL(3,2),
    course_id VARCHAR(20),
    PRIMARY KEY(name),
    FOREIGN KEY(course_id) REFERENCES course(course_id),
    CHECK(gpa >= 0)
);
```

```sql
CREATE Table students(
    name VARCHAR(20),
    age INT,
    gpa DECIMAL(3,2),
    course_id VARCHAR(20),
    sex CHAR(4),
    PRIMARY KEY(name),
    FOREIGN KEY(course_id) REFERENCES course(course_id),
    CHECK(gpa >= 0),
    CHECK(sex in('男','女'))//只能是男或者女
);
```

#### 数据操纵-DML

##### 插入

```sql
INSERT INTO course VALUES
 ('C1', '计算机基础'),
 ('C2', '综合日语');
```

```sql
INSERT INTO course VALUES
('C3','');
```

##### 更新

```sql
UPDATE course SET course_name = '日语听说' WHERE course_id = 'C3'
```

##### 删除

```sql
DELETE FROM course WHERE course_id = 'C1';
//这样计算机基础就被删掉了
```

##### 查询

```sql
SELECT course_name FROM course WHERE course_id = 'C3' OR course_id = 'C2';
```

<img src="file:///Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-17-16-57-14-image.png" title="" alt="" width="344">如图数据已经被查到了

查询的实际运行顺序

![](/Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-17-17-01-08-image.png)

order by

```sql
SELECT course_name 
FROM course 
WHERE course_id = 'C3' OR course_id = 'C2'
ORDER BY course_id ;
//默认是生序的
```

```sql
SELECT course_name 
FROM course 
WHERE course_id = 'C3' OR course_id = 'C2'
ORDER BY course_id ASC;
```

<img src="file:///Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-17-17-05-32-image.png" title="" alt="" width="335">

```sql
SELECT course_name 
FROM course 
WHERE course_id = 'C3' OR course_id = 'C2'
ORDER BY course_id DESC;
```

<img src="file:///Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-17-17-05-58-image.png" title="" alt="" width="319">

### DML详解 FROM语句-JOIN运算

From 子句用于指定从哪个表中获取数据，以便后续操作能够找到所需的信息。

```sql
SELECT * FROM students,course WHERE students.course_id = course.course_id;
```

![](/Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-18-22-33-25-image.png)

```sql
SELECT * FROM students JOIN course ON students.course_id = course.course_id;
```

![](/Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-18-22-38-00-image.png)

怎么使用JOIN语句连接多个表呢

```sql
SELECT * FROM (students JOIN classes ON classes.class_id=students.class_id) JOIN course ON students.course_id = course.course_id
```

![](/Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-19-14-02-07-image.png)

**Inner join**

使用等号进行inner join以获得两表的交集，即共有的行。

```sql
select * from a INNER JOIN b on a.a = b.b;
select a.*,b.*  from a,b where a.a = b.b;

a | b
--+--
3 | 3
4 | 4
```

**Left outer join**

 left outer join 除了获得B表中符合条件的列外，还将获得A表所有的列。

```sql
select * from a LEFT OUTER JOIN b on a.a = b.b;
select a.*,b.*  from a,b where a.a = b.b(+);

a |  b  
--+-----
1 | null
2 | null
3 |    3
4 |    4
```

**Full outer join**

full outer join 得到A和B的交集,即A和B中所有的行.。如果A中的行在B中没有对应的部分,B的部分将是 null, 反之亦然。

```sql
select * from a FULL OUTER JOIN b on a.a = b.b;

 a   |  b  
-----+-----
   1 | null
   2 | null
   3 |    3
   4 |    4
null |    6
null |    5
```

Inner join 和outer join的部分由[尤慕](http://my.csdn.net/youmoo)译自[Stack Overflow](http://stackoverflow.com/questions/38549/sql-difference-between-inner-and-outer-join)，转载请保留此信息。

当使用join子句而没有outer前缀时，默认的连接类型是内连接（inner join）

**natural join**

- 自然连接仅考虑哪些在两个关系模式中都出现的属性上取值相同的元素对，每一个相同属性列仅留一个拷贝。

- 自然连接（natural join）的风险：需警惕同名但无关的属性，它们可能会被错误地等同起来。

**self join**

自连接

```sql
-- 自连接示例：查找同班同学
SELECT s1.name AS student1, s2.name AS student2, s1.class_id
FROM students s1 
JOIN students s2 ON s1.class_id = s2.class_id 
WHERE s1.name < s2.name;

-- 自连接示例：查找年龄相同的学生
SELECT s1.name AS student1, s2.name AS student2, s1.age
FROM students s1 
JOIN students s2 ON s1.age = s2.age 
WHERE s1.name != s2.name;
```

### DML详解 RESTRICT语句

![](/Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-19-14-56-52-image.png)

逻辑连词的计算优先级: not > and > or

**% _的用法**

```sql
-- LIKE操作符示例演示

-- 1. 使用 % 匹配任意子字符串
-- 查找姓名以"张"开头的学生
SELECT * FROM students WHERE name LIKE '张%';

-- 查找姓名以"三"结尾的学生
SELECT * FROM students WHERE name LIKE '%三';

-- 查找姓名中包含"四"的学生
SELECT * FROM students WHERE name LIKE '%四%';

-- 2. 使用 _ 匹配任意单个字符
-- 查找姓名为两个字符的学生（第二个字符任意）
SELECT * FROM students WHERE name LIKE '__';

-- 查找姓名为三个字符且第二个字符为"五"的学生
SELECT * FROM students WHERE name LIKE '_五_';

-- 3. 组合使用 % 和 _
-- 查找姓名第一个字符任意，第二个字符为"三"，后面可以有任意字符的学生
SELECT * FROM students WHERE name LIKE '_三%';

-- 查找课程名称中包含"计算机"的课程
SELECT * FROM course WHERE course_name LIKE '%计算机%';

-- 查找班级名称以"计算机"开头且以"班"结尾的班级
SELECT * FROM classes WHERE class_name LIKE '计算机%班';

-- 查找班级名称格式为"计算机X班"的班级（X为任意单个字符）
SELECT * FROM classes WHERE class_name LIKE '计算机_班';
```

**选定的字符可以用作转义字符。**

例如，like 'ab\\_cd%' escape '\';
escape'\' 会匹配所有以“ab_cd”开头的字符串

**集合的用法**

```sql
SELECT * FROM students WHERE class_id IN (1, 3);
```

**ANY（mysql不支持）**

```sql
-- ANY操作符示例演示
-- 查找GPA大于等于任意一个值(3.0, 3.5, 3.8)的学生
SELECT * FROM students WHERE gpa >= ANY ( VALUES (3.0), (3.5), (3.8) );

-- 查找年龄小于等于任意一个值(19, 20, 21)的学生
SELECT * FROM students WHERE age <= ANY ( VALUES (19), (20), (21) );

-- 查找班级ID等于任意一个值(1, 2)的学生
SELECT * FROM students WHERE class_id = ANY ( VALUES (1), (2) );
```

**EXISTS**

EXISTS子查询查询结果只有0或1

```sql
-- 查找有学生的班级
SELECT *
FROM classes cl
WHERE
    EXISTS (
        SELECT *
        FROM students s
        WHERE
            s.class_id = cl.class_id
    );

-- 查找没有学生的班级
SELECT *
FROM classes cl
WHERE
    NOT EXISTS (
        SELECT *
        FROM students s
        WHERE
            s.class_id = cl.class_id
    );
```

### DML详解 聚集运算 GROUP BY 和  HAVING 子句

```sql
SELECT COUNT(*) FROM students;
```

这样你就可以查询到students表有多少行数据

如果你想看某项非空的行数，可以加上DISTINCT

```sql
SELECT deptno,count(DISTINCT job) jobTypeCnt

FROM EMP e

GROUP BY DEPTNO

ORDER BY DEPTNO
```

当然你也可以为结果命名

```sql
SELECT COUNT(*) NUM FROM students;
```

同样聚合查询也支持where

```sql
SELECT COUNT(*) NUM FROM students WHERE gpa >=3.5;
```

SUM计算某一列的合计值，该列必须为数值类型

```sql
SELECT SUM(gpa) FROM students;
```

AVG计算某一列的平均值，该列必须为数值类型

```sql
SELECT AVG(gpa) FROM students;
```

MAX计算某一列的最大值

```sql
SELECT MAX(gpa) FROM students;
```

MIN计算某一列的最小值

```sql
SELECT MIN(gpa) FROM students;
```

注意，`MAX()`和`MIN()`函数并不限于数值类型。如果是字符类型，`MAX()`和`MIN()`会返回排序最后和排序最前的字符。

**分组**

```sql
SELECT students.class_id,count(*) FROM students GROUP BY students.class_id;
```

这样会将students表根据class_id分组，之后对每组进行计数

<img src="file:///Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-20-10-09-13-image.png" title="" alt="" width="418">

也可以使用多个列进行分组。

```sql
SELECT s.class_id,s.sex,COUNT(*) FROM students s GROUP BY s.class_id,s.sex;
```

`HAVING condition`：一个条件，用于筛选分组的结果。只有满足条件的分组会包含在结果集中。

```sql
SELECT s.class_id,s.sex,COUNT(*) FROM students s GROUP BY s.class_id,s.sex HAVING sex = '男';
```

<img src="file:///Users/easyimpr/Library/Application%20Support/marktext/images/2025-10-20-11-14-27-image.png" title="" alt="" width="421">

### DML详解-select子句-project运算
