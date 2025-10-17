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


