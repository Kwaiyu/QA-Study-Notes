## 非关系数据库

NoSQL(NoSQL = Not Only SQL )，意即"不仅仅是SQL"。包括MongoDB、Cassandra、Dynamo等等，它们都不是关系数据库。NoSQL用于超大规模数据的存储。（例如谷歌或Facebook每天为他们的用户收集万亿比特的数据）。这些类型的数据存储不需要固定的模式，无需多余操作就可以横向扩展。

## 关系数据库

随着应用程序的功能越来越复杂，数据量越来越大，如何管理这些数据就成了大问题：

- 读写文件并解析出数据需要大量重复代码；
- 从成千上万的数据中快速查询出指定数据需要复杂的逻辑。

如果每个应用程序都各自写自己的读写数据的代码，一方面效率低，容易出错，另一方面，每个应用程序访问数据的接口都不相同，数据难以复用。

所以数据库作为一种专门管理数据的软件就出现了。应用程序不需要自己管理数据，而是通过数据库软件提供的接口来读写数据。至于数据本身如何存储到文件，那是数据库软件的事情，应用程序自己并不关心，简化了数据读写。

### 数据模型

数据库按照数据结构来组织、存储和管理数据，实际上，数据库一共有三种模型：

- 层次模型
- 网状模型
- 关系模型

层次模型就是以“上下级”的层次关系来组织数据的一种方式，层次模型的数据结构看起来就像一颗树：

```ascii
            ┌─────┐
            │     │
            └─────┘
               │
       ┌───────┴───────┐
       │               │
    ┌─────┐         ┌─────┐
    │     │         │     │
    └─────┘         └─────┘
       │               │
   ┌───┴───┐       ┌───┴───┐
   │       │       │       │
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│     │ │     │ │     │ │     │
└─────┘ └─────┘ └─────┘ └─────┘
```

网状模型把每个数据节点和其他很多节点都连接起来，它的数据结构看起来就像很多城市之间的路网：

```ascii
     ┌─────┐      ┌─────┐
   ┌─│     │──────│     │──┐
   │ └─────┘      └─────┘  │
   │    │            │     │
   │    └──────┬─────┘     │
   │           │           │
┌─────┐     ┌─────┐     ┌─────┐
│     │─────│     │─────│     │
└─────┘     └─────┘     └─────┘
   │           │           │
   │     ┌─────┴─────┐     │
   │     │           │     │
   │  ┌─────┐     ┌─────┐  │
   └──│     │─────│     │──┘
      └─────┘     └─────┘
```

关系模型把数据看作是一个二维表格，任何数据都可以通过行号+列号来唯一确定，它的数据模型看起来就是一个Excel表：

```ascii
┌─────┬─────┬─────┬─────┬─────┐
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
```

随着时间的推移和市场竞争，最终基于关系模型的关系数据库获得了绝对市场份额。以学校班级为例，一个班级的学生就可以用一个表格存起来，并且定义如下：

| ID   | 姓名 | 班级ID | 性别 | 年龄 |
| :--- | :--- | -----: | ---: | ---: |
| 1    | 小明 |    201 |    M |    9 |
| 2    | 小红 |    202 |    F |    8 |
| 3    | 小军 |    202 |    M |    8 |
| 4    | 小白 |    201 |    F |    9 |

其中班级ID对应着另一个班级表：

| ID   | 名称       | 班主任 |
| :--- | :--------- | :----- |
| 201  | 二年级一班 | 王老师 |
| 202  | 二年级二班 | 李老师 |

通过给定一个班级名称，可以查到一条班级记录，根据班级ID，又可以查到多条学生记录，这样，二维表之间就通过ID映射建立了“一对多”关系。

### 数据类型

对于一个关系表，除了定义每一列的名称外，还需要定义每一列的数据类型。关系数据库支持的标准数据类型包括数值、字符串、时间等：

| 名称         | 类型           | 说明                                                         |
| :----------- | :------------- | :----------------------------------------------------------- |
| INT          | 整型           | 4字节整数类型，范围约+/-21亿                                 |
| BIGINT       | 长整型         | 8字节整数类型，范围约+/-922亿亿                              |
| REAL         | 浮点型         | 4字节浮点数，范围约+/-1038                                   |
| DOUBLE       | 浮点型         | 8字节浮点数，范围约+/-10308                                  |
| DECIMAL(M,N) | 高精度小数     | 由用户指定精度的小数，例如，DECIMAL(20,10)表示一共20位，其中小数10位，通常用于财务计算 |
| CHAR(N)      | 定长字符串     | 存储指定长度的字符串，例如，CHAR(100)总是存储100个字符的字符串 |
| VARCHAR(N)   | 变长字符串     | 存储可变长度的字符串，例如，VARCHAR(100)可以存储0~100个字符的字符串 |
| BOOLEAN      | 布尔类型       | 存储True或者False                                            |
| DATE         | 日期类型       | 存储日期，例如，2018-06-22                                   |
| TIME         | 时间类型       | 存储时间，例如，12:20:59                                     |
| DATETIME     | 日期和时间类型 | 存储日期+时间，例如，2018-06-22 12:20:59                     |

上面的表中列举了最常用的数据类型。很多数据类型还有别名，例如，`REAL`又可以写成`FLOAT(24)`。还有一些不常用的数据类型，例如，`TINYINT`（范围在0~255）。各数据库厂商还会支持特定的数据类型，例如`JSON`。

选择数据类型的时候，要根据业务规则选择合适的类型。通常来说，`BIGINT`能满足整数存储的需求，`VARCHAR(N)`能满足字符串存储的需求，这两种类型是使用最广泛的。

**主流关系数据库**

- 商用数据库，例如：[Oracle](https://www.oracle.com/)，[SQL Server](https://www.microsoft.com/sql-server/)，[DB2](https://www.ibm.com/db2/)等；
- 开源数据库，例如：[MySQL](https://www.mysql.com/)，[PostgreSQL](https://www.postgresql.org/)等；
- 桌面数据库，以微软[Access](https://products.office.com/access)为代表，适合桌面应用程序使用；
- 嵌入式数据库，以[Sqlite](https://sqlite.org/)为代表，适合手机应用和桌面程序。

### SQL

SQL（Structured Query Language）是结构化查询语言的缩写，用来访问和操作数据库系统。SQL语句既可以查询数据库中的数据，也可以添加、更新和删除数据库中的数据，还可以对数据库进行管理和维护操作。不同的数据库，都支持SQL，这样，我们通过学习SQL这一种语言，就可以操作各种不同的数据库。

虽然SQL已经被ANSI组织定义为标准，不幸地是，各个不同的数据库对标准的SQL支持不太一致。并且，大部分数据库都在标准的SQL上做了扩展。也就是说，如果只使用标准SQL，理论上所有数据库都可以支持，但如果使用某个特定数据库的扩展SQL，换一个数据库就不能执行了。例如，Oracle把自己扩展的SQL称为`PL/SQL`，Microsoft把自己扩展的SQL称为`T-SQL`。

现实情况是，如果我们只使用标准SQL的核心功能，那么所有数据库通常都可以执行。不常用的SQL功能，不同的数据库支持的程度都不一样。而各个数据库支持的各自扩展的功能，通常我们把它们称之为“方言”。

总的来说，SQL语言定义了这么几种操作数据库的能力：

**DDL：Data Definition Language**

DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。通常，DDL由数据库管理员执行。

**DML：Data Manipulation Language**

DML为用户提供添加、删除、更新数据的能力，这些是应用程序对数据库的日常操作。

**DQL：Data Query Language**

DQL允许用户查询数据，这也是通常最频繁的数据库日常操作。

### 语法特点

SQL语言关键字不区分大小写，但是针对不同的数据库对于表名和列名，有的数据库区分大小写，有的数据库不区分大小写。同一个数据库有的在Linux上区分大小写，有的在Windows上不区分大小写。约定SQL关键字总是大写以示突出，表名和列名均使用小写。

## 关系模型

关系数据库是建立在关系模型上的。而关系模型本质上就是若干个存储数据的二维表，可以把它们看作很多Excel表。

表的每一行称为记录（Record），记录是一个逻辑意义上的数据。

表的每一列称为字段（Column），同一个表的每一行记录都拥有相同的若干字段。

字段定义了数据类型（整型、浮点型、字符串、日期等），以及是否允许为`NULL`。注意`NULL`表示字段数据不存在。一个整型字段如果为`NULL`不表示它的值为`0`，同样的，一个字符串型字段为`NULL`也不表示它的值为空串`''`。

> [!NOTE]
>
> 通常字段应该避免允许为NULL。不允许为NULL可以简化查询条件，加快查询速度，也利于应用程序读取数据后无需判断是否为NULL。

关系数据库的表和表之间需要建立“一对多”，“多对一”和“一对一”的关系，这样才能够按照应用程序的逻辑来组织和存储数据。

如一个班级表：

| ID   | 名称       | 班主任 |
| :--- | :--------- | :----- |
| 201  | 二年级一班 | 王老师 |
| 202  | 二年级二班 | 李老师 |

每一行对应着一个班级，而一个班级对应着多个学生，所以班级表和学生表的关系就是“一对多”：

| ID   | 姓名 | 班级ID | 性别 | 年龄 |
| :--- | :--- | -----: | ---: | ---: |
| 1    | 小明 |    201 |    M |    9 |
| 2    | 小红 |    202 |    F |    8 |
| 3    | 小军 |    202 |    M |    8 |
| 4    | 小白 |    201 |    F |    9 |

反过来，如果我们先在学生表中定位了一行记录，例如`ID=1`的小明，要确定他的班级，只需要根据他的“班级ID”对应的值`201`找到班级表中`ID=201`的记录，即二年级一班。所以，学生表和班级表是“多对一”的关系。

如果我们把班级表分拆得细一点，例如，单独创建一个教师表：

| ID   | 名称   | 年龄 |
| :--- | :----- | :--- |
| A1   | 王老师 | 26   |
| A2   | 张老师 | 39   |
| A3   | 李老师 | 32   |
| A4   | 赵老师 | 27   |

班级表只存储教师ID：

| ID   | 名称       | 班主任ID |
| :--- | :--------- | :------- |
| 201  | 二年级一班 | A1       |
| 202  | 二年级二班 | A3       |

这样，一个班级总是对应一个教师，班级表和教师表就是“一对一”关系。

在关系数据库中，关系是通过主键和外键来维护的。

### 主键

> [!NOTE]
>
> 主键是关系表中记录的唯一标识。主键的选取非常重要：主键不要带有业务含义，而应该使用BIGINT自增或者GUID类型。主键也不应该允许`NULL`。
>
> 可以使用多个列作为联合主键，但联合主键并不常用。

在关系数据库中，一张表中的每一行数据被称为一条记录。一条记录就是由多个字段组成的。例如，`students`表的两行记录：

| id   | class_id | name | gender | score |
| :--- | :------- | :--- | :----- | :---- |
| 1    | 1        | 小明 | M      | 90    |
| 2    | 1        | 小红 | F      | 95    |

每一条记录都包含若干定义好的字段。同一个表的所有记录都有相同的字段定义，对于关系表两条记录不能重复。不能重复不是指两条记录不完全相同，而是指能够通过某个字段唯一区分出不同的记录，这个字段被称为*主键*。记录一旦插入到表中，主键最好不要再修改，因为主键是用来唯一定位记录的，修改了主键，会造成一系列的影响。

由于主键的作用十分重要，如何选取主键会对业务开发产生重要影响。所以选取主键的一个基本原则是：不使用任何业务相关的字段作为主键。因此身份证号、手机号、邮箱地址这些看上去可以唯一的字段，均不可用作主键。

作为主键最好是完全业务无关的字段，我们一般把这个字段命名为`id`。常见的可作为`id`字段的类型有：

1. 自增整数类型：数据库会在插入数据时自动为每一条记录分配一个自增整数，这样我们就完全不用担心主键重复，也不用自己预先生成主键；
2. 全局唯一GUID类型：使用一种全局唯一的字符串作为主键，类似`8f55d96b-8acc-4636-8cb8-76bf8abc2f57`。GUID算法通过网卡MAC地址、时间戳和随机数保证任意计算机在任意时间生成的字符串都是不同的，大部分编程语言都内置了GUID算法，可以自己预算出主键。

对于大部分应用来说，通常自增类型的主键就能满足需求。我们在`students`表中定义的主键也是`BIGINT NOT NULL AUTO_INCREMENT`类型。

> [!WARNING]
>
> 如果使用INT自增类型，那么当一张表的记录数超过2147483647（约21亿）时会达到上限而出错。使用BIGINT自增类型则可以最多约922亿亿条记录。

#### 联合主键

关系数据库实际上还允许通过多个字段唯一标识记录，即两个或更多的字段都设置为主键，这种主键被称为联合主键。

对于联合主键，允许一列有重复，只要不是所有主键列都重复即可：

| id_num | id_type | other columns... |
| :----- | :------ | :--------------- |
| 1      | A       | ...              |
| 2      | A       | ...              |
| 2      | B       | ...              |

如果我们把上述表的`id_num`和`id_type`这两列作为联合主键，那么上面的3条记录都是允许的，因为没有两列主键组合起来是相同的。

没有必要的情况下，我们尽量不使用联合主键，因为它给关系表带来了复杂度的上升。

### 外键

> [!NOTE]
>
> 关系数据库通过外键可以实现一对多、多对多和一对一的关系。外键既可以通过数据库来约束，也可以不设置约束，仅依靠应用程序的逻辑来保证。

当我们用主键唯一标识记录时，我们就可以在`students`表中确定任意一个学生的记录：

| id   | name | other columns... |
| :--- | :--- | :--------------- |
| 1    | 小明 | ...              |
| 2    | 小红 | ...              |

我们还可以在`classes`表中确定任意一个班级记录：

| id   | name | other columns... |
| :--- | :--- | :--------------- |
| 1    | 一班 | ...              |
| 2    | 二班 | ...              |

但是我们如何确定`students`表的一条记录，例如，`id=1`的小明，属于哪个班级呢？

由于一个班级可以有多个学生，在关系模型中，这两个表的关系可以称为“一对多”，即一个`classes`的记录可以对应多个`students`表的记录。

为了表达这种一对多的关系，我们需要在`students`表中加入一列`class_id`，让它的值与`classes`表的某条记录相对应：

| id   | class_id | name | other columns... |
| :--- | :------- | :--- | :--------------- |
| 1    | 1        | 小明 | ...              |
| 2    | 1        | 小红 | ...              |
| 5    | 2        | 小白 | ...              |

这样，我们就可以根据`class_id`这个列直接定位出一个`students`表的记录应该对应到`classes`的哪条记录。

例如：

- 小明的`class_id`是`1`，因此，对应的`classes`表的记录是`id=1`的一班；
- 小红的`class_id`是`1`，因此，对应的`classes`表的记录是`id=1`的一班；
- 小白的`class_id`是`2`，因此，对应的`classes`表的记录是`id=2`的二班。

在`students`表中，通过`class_id`的字段，可以把数据与另一张表关联起来，这种列称为`外键`。

外键并不是通过列名实现的，而是通过定义外键约束实现的：

```sql
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);
```

其中，外键约束的名称`fk_class_id`可以任意，`FOREIGN KEY (class_id)`指定了`class_id`作为外键，`REFERENCES classes (id)`指定了这个外键将关联到`classes`表的`id`列（即`classes`表的主键）。

通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果`classes`表不存在`id=99`的记录，`students`表就无法插入`class_id=99`的记录。

由于外键约束会降低数据库的性能，大部分互联网应用程序为了追求速度，并不设置外键约束，而是仅靠应用程序自身来保证逻辑的正确性。这种情况下，`class_id`仅仅是一个普通的列，只是它起到了外键的作用而已。

要删除一个外键约束，也是通过`ALTER TABLE`实现的：

```sql
ALTER TABLE students
DROP FOREIGN KEY fk_class_id;
```

注意：删除外键约束并没有删除外键这一列。删除列是通过`DROP COLUMN ...`实现的。

#### 多对多

通过一个表的外键关联到另一个表，我们可以定义出一对多关系。有些时候，还需要定义“多对多”关系。例如，一个老师可以对应多个班级，一个班级也可以对应多个老师，因此，班级表和老师表存在多对多关系。

多对多关系实际上是通过两个一对多关系实现的，即通过一个中间表，关联两个一对多关系，就形成了多对多关系：

`teachers`表：

| id   | name   |
| :--- | :----- |
| 1    | 张老师 |
| 2    | 王老师 |
| 3    | 李老师 |
| 4    | 赵老师 |

`classes`表：

| id   | name |
| :--- | :--- |
| 1    | 一班 |
| 2    | 二班 |

中间表`teacher_class`关联两个一对多关系：

| id   | teacher_id | class_id |
| :--- | :--------- | :------- |
| 1    | 1          | 1        |
| 2    | 1          | 2        |
| 3    | 2          | 1        |
| 4    | 2          | 2        |
| 5    | 3          | 1        |
| 6    | 4          | 2        |

通过中间表`teacher_class`可知`teachers`到`classes`的关系：

- `id=1`的张老师对应`id=1,2`的一班和二班；
- `id=2`的王老师对应`id=1,2`的一班和二班；
- `id=3`的李老师对应`id=1`的一班；
- `id=4`的赵老师对应`id=2`的二班。

同理可知`classes`到`teachers`的关系：

- `id=1`的一班对应`id=1,2,3`的张老师、王老师和李老师；
- `id=2`的二班对应`id=1,2,4`的张老师、王老师和赵老师；

因此，通过中间表，我们就定义了一个“多对多”关系。

#### 一对一

一对一关系是指，一个表的记录对应到另一个表的唯一一个记录。

例如，`students`表的每个学生可以有自己的联系方式，如果把联系方式存入另一个表`contacts`，我们就可以得到一个“一对一”关系：

| id   | student_id | mobile      |
| :--- | :--------- | :---------- |
| 1    | 1          | 135xxxx6300 |
| 2    | 2          | 138xxxx2209 |
| 3    | 5          | 139xxxx8086 |

有细心的童鞋会问，既然是一对一关系，那为啥不给`students`表增加一个`mobile`列，这样就能合二为一了？

如果业务允许，完全可以把两个表合为一个表。但是，有些时候，如果某个学生没有手机号，那么，`contacts`表就不存在对应的记录。实际上，一对一关系准确地说，是`contacts`表一对一对应`students`表。

还有一些应用会把一个大表拆成两个一对一的表，目的是把经常读取和不经常读取的字段分开，以获得更高的性能。例如，把一个大的用户表分拆为用户基本信息表`user_info`和用户详细信息表`user_profiles`，大部分时候，只需要查询`user_info`表，并不需要查询`user_profiles`表，这样就提高了查询速度。

### 索引

> [!NOTE]
>
> 通过对数据库表创建索引，可以提高查询速度。
>
> 通过创建唯一索引，可以保证某一列的值具有唯一性。
>
> 数据库索引对于用户和应用程序来说都是透明的。

在关系数据库中，如果有上万甚至上亿条记录，在查找记录的时候，想要获得非常快的速度，就需要使用索引。

索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。

例如，对于`students`表：

| id   | class_id | name | gender | score |
| :--- | :------- | :--- | :----- | :---- |
| 1    | 1        | 小明 | M      | 90    |
| 2    | 1        | 小红 | F      | 95    |
| 3    | 1        | 小军 | M      | 88    |

如果要经常根据`score`列进行查询，就可以对`score`列创建索引：

```sql
ALTER TABLE students
ADD INDEX idx_score (score);
```

使用`ADD INDEX idx_score (score)`就创建了一个名称为`idx_score`，使用列`score`的索引。索引名称是任意的，索引如果有多列，可以在括号里依次写上，例如：

```sql
ALTER TABLE students
ADD INDEX idx_name_score (name, score);
```

索引的效率取决于索引列的值是否散列，即该列的值如果越互不相同，那么索引效率越高。反过来，如果记录的列存在大量相同的值，例如`gender`列，大约一半的记录值是`M`，另一半是`F`，因此，对该列创建索引就没有意义。

可以对一张表创建多个索引。索引的优点是提高了查询效率，缺点是在插入、更新和删除记录时，需要同时修改索引，因此，索引越多，插入、更新和删除记录的速度就越慢。

对于主键，关系数据库会自动对其创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一。

#### 唯一索引

在设计关系数据表的时候，看上去唯一的列，例如身份证号、邮箱地址等，因为他们具有业务含义，因此不宜作为主键。

但是，这些列根据业务要求，又具有唯一性约束：即不能出现两条记录存储了同一个身份证号。这个时候，就可以给该列添加一个唯一索引。例如，我们假设`students`表的`name`不能重复：

```sql
ALTER TABLE students
ADD UNIQUE INDEX uni_name (name);
```

通过`UNIQUE`关键字我们就添加了一个唯一索引。

也可以只对某一列添加一个唯一约束而不创建唯一索引：

```sql
ALTER TABLE students
ADD CONSTRAINT uni_name UNIQUE (name);
```

这种情况下，`name`列没有索引，但仍然具有唯一性保证。

无论是否创建索引，对于用户和应用程序来说，使用关系数据库不会有任何区别。这里的意思是说，当我们在数据库中查询时，如果有相应的索引可用，数据库系统就会自动使用索引来提高查询效率，如果没有索引，查询也能正常执行，只是速度会变慢。因此，索引可以在使用数据库的过程中逐步优化。

## 查询数据

**准备数据AlaSQL内存数据库**

为了便于讲解和练习，我们先准备好了一个`students`表和一个`classes`表，它们的结构和数据如下，`students`表存储了学生信息：

| id   | class_id | name | gender | score |
| :--- | :------- | :--- | :----- | :---- |
| 1    | 1        | 小明 | M      | 90    |
| 2    | 1        | 小红 | F      | 95    |
| 3    | 1        | 小军 | M      | 88    |
| 4    | 1        | 小米 | F      | 73    |
| 5    | 2        | 小白 | F      | 81    |
| 6    | 2        | 小兵 | M      | 55    |
| 7    | 2        | 小林 | M      | 85    |
| 8    | 3        | 小新 | F      | 91    |
| 9    | 3        | 小王 | M      | 89    |
| 10   | 3        | 小丽 | F      | 85    |

`classes`表存储了班级信息：

| id   | name |
| :--- | :--- |
| 1    | 一班 |
| 2    | 二班 |
| 3    | 三班 |
| 4    | 四班 |

请注意，和`MySQL`的持久化存储不同的是，由于我们使用的是[AlaSQL](http://alasql.org/)内存数据库，两张表的数据在页面加载时导入，并且只存在于浏览器的内存中，因此，刷新页面后，数据会重置为上述初始值。

**准备数据MySQL**

```sql
-- 如果test数据库不存在，就创建test数据库：
CREATE DATABASE IF NOT EXISTS test;

-- 切换到test数据库
USE test;

-- 删除classes表和students表（如果存在）：
DROP TABLE IF EXISTS classes;
DROP TABLE IF EXISTS students;

-- 创建classes表：
CREATE TABLE classes (
    id BIGINT NOT NULL AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 创建students表：
CREATE TABLE students (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    gender VARCHAR(1) NOT NULL,
    score INT NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- 插入classes记录：
INSERT INTO classes(id, name) VALUES (1, '一班');
INSERT INTO classes(id, name) VALUES (2, '二班');
INSERT INTO classes(id, name) VALUES (3, '三班');
INSERT INTO classes(id, name) VALUES (4, '四班');

-- 插入students记录：
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'M', 90);
INSERT INTO students (id, class_id, name, gender, score) VALUES (2, 1, '小红', 'F', 95);
INSERT INTO students (id, class_id, name, gender, score) VALUES (3, 1, '小军', 'M', 88);
INSERT INTO students (id, class_id, name, gender, score) VALUES (4, 1, '小米', 'F', 73);
INSERT INTO students (id, class_id, name, gender, score) VALUES (5, 2, '小白', 'F', 81);
INSERT INTO students (id, class_id, name, gender, score) VALUES (6, 2, '小兵', 'M', 55);
INSERT INTO students (id, class_id, name, gender, score) VALUES (7, 2, '小林', 'M', 85);
INSERT INTO students (id, class_id, name, gender, score) VALUES (8, 3, '小新', 'F', 91);
INSERT INTO students (id, class_id, name, gender, score) VALUES (9, 3, '小王', 'M', 89);
INSERT INTO students (id, class_id, name, gender, score) VALUES (10, 3, '小丽', 'F', 85);

-- OK:
SELECT 'ok' as 'result:';
```

```
$ mysql -u root -p < init-test-data.sql
```

就可以自动创建`test`数据库，并且在`test`数据库下创建`students`表和`classes`表，以及必要的初始化数据。

和内存数据库不同的是，对MySQL数据库做的所有修改，都会保存下来。如果你希望恢复到初始状态，可以再次运行该脚本。

### 基本查询

查询数据库表的数据：

```sql
SELECT * FROM <表名>
# 假设表名是students，要查询students表的所有行:
SELECT * FROM students;
# 要查询classes表的所有行
SELECT * FROM classes;
```

`SELECT`语句其实并不要求一定要有`FROM`子句。我们来试试下面的`SELECT`语句：

```sql
SELECT 100+200;
```

上述查询会直接计算出表达式的结果。虽然`SELECT`可以用作计算，但它并不是SQL的强项。但是，不带`FROM`子句的`SELECT`语句有一个有用的用途，就是用来判断当前到数据库的连接是否有效。许多检测工具会执行一条`SELECT 1;`来测试数据库连接。

### 条件查询

通过`WHERE`条件查询，可以筛选出符合指定条件的记录，而不是整个表的所有记录。例如，查询分数在80分以上的学生记录。在一张表有数百万记录的情况下，获取所有记录不仅费时，还费内存和网络带宽。SELECT语句可以通过`WHERE`条件来设定查询条件，查询结果是满足查询条件的记录。例如，要指定条件“分数在80分或以上的学生”，写成`WHERE`条件就是`SELECT * FROM students WHERE score >= 80`。其中，`WHERE`关键字后面的`score >= 80`就是条件。`score`是列名，该列存储了学生的成绩，因此，`score >= 80`就筛选出了指定条件的记录。

```sql
SELECT * FROM <表名> WHERE <条件表达式>
```

条件表达式可以用`<条件1> AND <条件2>`表达满足条件1并且满足条件2。例如，符合条件“分数在80分或以上”，并且还符合条件“男生”，把这两个条件写出来：

- 条件1：根据score列的数据判断：`score >= 80`；
- 条件2：根据gender列的数据判断：`gender = 'M'`，注意`gender`列存储的是字符串，需要用单引号括起来。

```sql
SELECT * FROM students WHERE score >= 80 AND gender = 'M';
```

第二种条件是`<条件1> OR <条件2>`，表示满足条件1或者满足条件2。例如，把上述`AND`查询的两个条件改为`OR`，查询结果就是“分数在80分或以上”或者“男生”，满足任意之一的条件即选出该记录：

```sql
SELECT * FROM students WHERE score >= 80 OR gender = 'M';
```

第三种条件是`NOT <条件>`，表示“不符合该条件”的记录。例如，写一个“不是2班的学生”这个条件，可以先写出“是2班的学生”：`class_id = 2`，再加上`NOT`：`NOT class_id = 2`：

```sql
SELECT * FROM students WHERE NOT class_id = 2;
```

上述`NOT`条件`NOT class_id = 2`其实等价于`class_id <> 2`，因此，`NOT`查询不是很常用。

要组合三个或者更多的条件，就需要用小括号`()`表示如何进行条件运算。例如，编写一个复杂的条件：分数在80以下或者90以上，并且是男生：

```sql
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';
```

如果不加括号，条件运算按照`NOT`、`AND`、`OR`的优先级进行，即`NOT`优先级最高，其次是`AND`，最后是`OR`。加上括号可以改变优先级。

### 投影查询

使用`SELECT *`表示查询表的所有列，使用`SELECT 列1, 列2, 列3`则可以仅返回指定列，这种操作称为投影。`SELECT`语句可以对结果集的列进行重命名。

如果我们只希望返回某些列的数据，而不是所有列的数据，我们可以用`SELECT 列1, 列2, 列3 FROM ...`，让结果集仅包含指定列。这种操作称为投影查询。

例如，从`students`表中返回`id`、`score`和`name`这三列：

```sql
SELECT id, score, name FROM students;
```

使用`SELECT 列1, 列2, 列3 FROM ...`时，还可以给每一列起个别名，这样，结果集的列名就可以与原表的列名不同。它的语法是`SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...`。

例如，以下`SELECT`语句将列名`score`重命名为`points`，而`id`和`name`列名保持不变：

```sql
SELECT id, score points, name FROM students;
```

投影查询同样可以接`WHERE`条件，实现复杂的查询：

```sql
SELECT id, score points, name FROM students WHERE gender = 'M';
```

### 排序

使用`ORDER BY`可以对结果集进行排序；

可以对多列进行升序、倒序排序。

查询结果集通常是按照`id`排序的，也就是根据主键排序。这也是大部分数据库的做法。可以加上`ORDER BY`子句。例如按照成绩从低到高进行排序：

```sql
SELECT id, name, gender, score FROM students ORDER BY score;
```

如果要反过来，按照成绩从高到底排序，我们可以加上`DESC`表示“倒序”：

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC;
```

如果`score`列有相同的数据，要进一步排序，可以继续添加列名。例如，使用`ORDER BY score DESC, gender`表示先按`score`列倒序，如果有相同分数的，再按`gender`列排序：

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
```

默认的排序规则是`ASC`：“升序”，即从小到大。`ASC`可以省略，即`ORDER BY score ASC`和`ORDER BY score`效果一样。

如果有`WHERE`子句，那么`ORDER BY`子句要放到`WHERE`子句后面。例如，查询一班的学生成绩，并按照倒序排序：

```sql
SELECT id, name, gender, score
FROM students
WHERE class_id = 1
ORDER BY score DESC;
```

### 分页查询

使用`LIMIT <M> OFFSET <N>`可以对结果集进行分页，每次查询返回结果集的一部分；分页查询需要先确定每页的数量和当前页数，然后确定`LIMIT`和`OFFSET`的值。

分页实际上就是从结果集中“截取”出第M~N条记录。这个查询可以通过`LIMIT <M> OFFSET <N>`子句实现。我们先把所有学生按照成绩从高到低进行排序：

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC;
```

把结果集分页，每页3条记录。要获取第1页的记录，可以使用`LIMIT 3 OFFSET 0`：

```sql
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;
```

上述查询`LIMIT 3 OFFSET 0`表示，对结果集从0号记录开始，最多取3条。注意SQL记录集的索引从0开始。如果要查询第2页，那么我们只需要“跳过”头3条记录，也就是对结果集从3号记录开始查询，把`OFFSET`设定为3。查询第3页的时候，`OFFSET`应该设定为6，查询第4页的时候`OFFSET`应该设定为9。

```sql
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 3;
```

分页查询的关键在于，首先要确定每页需要显示的结果数量`pageSize`（这里是3），然后根据当前页的索引`pageIndex`（从1开始），确定`LIMIT`和`OFFSET`应该设定的值：

- `LIMIT`总是设定为`pageSize`；
- `OFFSET`计算公式为`pageSize * (pageIndex - 1)`

> [!TIP]
>
> `OFFSET`是可选的，如果只写`LIMIT 15`，那么相当于`LIMIT 15 OFFSET 0`。
>
> 在MySQL中，`LIMIT 15 OFFSET 30`还可以简写成`LIMIT 30, 15`。
>
> 使用`LIMIT <M> OFFSET <N>`分页时，随着`N`越来越大，查询效率也会越来越低。

### 聚合查询

使用SQL提供的聚合查询，我们可以方便地计算总数、合计值、平均值、最大值和最小值；聚合查询也可以添加`WHERE`条件。

对于统计总数、平均数这类计算，SQL提供了专门的聚合函数，使用聚合函数进行查询，就是聚合查询，它可以快速获得结果。仍然以查询`students`表一共有多少条记录为例，我们可以使用SQL内置的`COUNT()`函数查询：

```sql
SELECT COUNT(*) FROM students;
```

`COUNT(*)`表示查询所有列的行数，要注意聚合的计算结果虽然是一个数字，但查询的结果仍然是一个二维表，只是这个二维表只有一行一列，并且列名是`COUNT(*)`。

通常，使用聚合查询时，我们应该给列名设置一个别名，便于处理结果：

```sql
SELECT COUNT(*) num FROM students;
```

`COUNT(*)`和`COUNT(id)`实际上是一样的效果。另外注意，聚合查询同样可以使用`WHERE`条件，因此我们可以方便地统计出有多少男生、多少女生、多少80分以上的学生等：

```sql
SELECT COUNT(*) boys FROM students WHERE gender = 'M';
```

除了`COUNT()`函数外，SQL还提供了如下聚合函数：

| 函数 | 说明                                   |
| :--- | :------------------------------------- |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值                     |
| MIN  | 计算某一列的最小值                     |

注意，`MAX()`和`MIN()`函数并不限于数值类型。如果是字符类型，`MAX()`和`MIN()`会返回排序最后和排序最前的字符。

要统计男生的平均成绩，我们用下面的聚合查询：

```sql
SELECT AVG(score) average FROM students WHERE gender = 'M';
```

要特别注意：如果聚合查询的`WHERE`条件没有匹配到任何行，`COUNT()`会返回0，而`SUM()`、`AVG()`、`MAX()`和`MIN()`会返回`NULL`：

```sql
SELECT AVG(score) average FROM students WHERE gender = 'X';
```

每页3条记录，通过聚合查询获得总页数：

```sql
SELECT CEILING(COUNT(*) / 3) FROM students;
```

#### 分组

如果我们要统计一班的学生数量，我们知道，可以用`SELECT COUNT(*) num FROM students WHERE class_id = 1;`。如果要继续统计二班、三班的学生数量，难道必须不断修改`WHERE`条件来执行`SELECT`语句吗？

对于聚合查询，SQL还提供了“分组聚合”的功能。我们观察下面的聚合查询：

```sql
SELECT COUNT(*) num FROM students GROUP BY class_id;
```

`GROUP BY`子句指定了按`class_id`分组，因此执行该`SELECT`语句时，会把`class_id`相同的列先分组再分别计算，因此得到了3行结果。

但是这3行结果分别是哪三个班级的，不好看出来，所以我们可以把`class_id`列也放入结果集中：

```sql
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;
```

这下结果集就可以一目了然地看出各个班级的学生人数。我们再试试把`name`放入结果集：

```sql
SELECT name, class_id, COUNT(*) num FROM students GROUP BY class_id;
```

执行这条查询我们会得到一个语法错误，因为在任意一个分组中，只有`class_id`都相同，`name`是不同的，SQL引擎不能把多个`name`的值放入一行记录中。因此，聚合查询的列中，只能放入分组的列。

> [!WARNING]
>
> AlaSQL并没有严格执行SQL标准，上述SQL在浏览器可以正常执行，但是在MySQL、Oracle等环境下将报错，请自行在MySQL中测试。

也可以使用多个列进行分组。例如，我们想统计各班的男生和女生人数：

```sql
SELECT class_id, gender, COUNT(*) num FROM students GROUP BY class_id, gender;
```

### 多表查询

使用多表查询可以获取M x N行记录，查询的结果集可能非常巨大。

查询多张表的语法是：`SELECT * FROM <表1> <表2>`。

```sql
SELECT * FROM students, classes;
```

查询的结果也是一个二维表，它是`students`表和`classes`表的“乘积”，结果集的列数是`students`表和`classes`表的列数之和，行数是`students`表和`classes`表的行数之积，这种多表查询又称笛卡尔查询。

上述查询结果集有两列`id`和两列`name`不好区分，可以利用投影查询设置列的别名来给两个表同名列各自起别名：

```sql
SELECT
    students.id sid,
    students.name,
    students.gender,
    students.score,
    classes.id cid,
    classes.name cname
FROM students, classes;
```

多表查询时，要使用`表名.列名`这样的方式来引用列和设置别名，这样就避免了结果集的列名重复问题。但是用`表名.列名`这种方式列举两个表的所有列实在是很麻烦，FROM`子句给表设置别名的语法是`FROM <表名1> <别名1>, <表名2> <别名2>`。这样我们用别名`s`和`c`分别表示`students`表和`classes`表。：

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;
```

多表查询也是可以添加`WHERE`条件的：

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c
WHERE s.gender = 'M' AND c.id = 1;
```

### 连接查询

JOIN查询需要先确定主表，然后把另一个表的数据“附加”到结果集上；

INNER JOIN是最常用的一种JOIN查询，它的语法是`SELECT ... FROM <表1> INNER JOIN <表2> ON <条件...>`；

JOIN查询仍然可以使用`WHERE`条件和`ORDER BY`排序。

连接查询是另一种类型的多表查询。连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。

例如，我们想要选出`students`表的所有学生信息，可以用一条简单的SELECT语句完成：

```sql
SELECT s.id, s.name, s.class_id, s.gender, s.score FROM students s;
```

假设我们希望结果集同时包含所在班级的名称，上面的结果集只有`class_id`列，缺少对应班级的`name`列。而存放班级名称的`name`列存储在`classes`表中，只有根据`students`表的`class_id`，找到`classes`表对应的行，再取出`name`列，就可以获得班级名称。先使用最常用的一种内连接INNER JOIN来实现：

```sql
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id;
```

注意INNER JOIN查询的写法是：

1. 先确定主表，仍然使用`FROM <表1>`的语法；
2. 再确定需要连接的表，使用`INNER JOIN <表2>`的语法；
3. 然后确定连接条件，使用`ON <条件...>`，这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
4. 可选：加上`WHERE`子句、`ORDER BY`等子句。

把内连接查询改成外连接查询：

```sql
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
RIGHT OUTER JOIN classes c
ON s.class_id = c.id;
```

执行上述RIGHT OUTER JOIN可以看到，和INNER JOIN相比，RIGHT OUTER JOIN多了一行，多出来的一行是“四班”，但是，学生相关的列如`name`、`gender`、`score`都为`NULL`。

这也容易理解，因为根据`ON`条件`s.class_id = c.id`，`classes`表的id=4的行正是“四班”，但是，`students`表中并不存在class_id=4的行。

有RIGHT OUTER JOIN，就有LEFT OUTER JOIN，以及FULL OUTER JOIN。它们的区别是：

INNER JOIN只返回同时存在于两张表的行数据，由于`students`表的`class_id`包含1，2，3，`classes`表的`id`包含1，2，3，4，所以，INNER JOIN根据条件`s.class_id = c.id`返回的结果集仅包含1，2，3。

RIGHT OUTER JOIN返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以`NULL`填充剩下的字段。

LEFT OUTER JOIN则返回左表都存在的行。如果我们给students表增加一行，并添加class_id=5，由于classes表并不存在id=5的行，所以，LEFT OUTER JOIN的结果会增加一行，对应的`class_name`是`NULL`：![]()

```sql
-- 先增加一列class_id=5:
INSERT INTO students (class_id, name, gender, score) values (5, '新生', 'M', 88);
-- 使用LEFT OUTER JOIN
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
LEFT OUTER JOIN classes c
ON s.class_id = c.id;
```

最后，我们使用FULL OUTER JOIN，它会把两张表的所有记录全部选择出来，并且，自动把对方不存在的列填充为NULL：

```sql
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
FULL OUTER JOIN classes c
ON s.class_id = c.id;
```

假设查询语句是：

```sql
SELECT ... FROM tableA ??? JOIN tableB ON tableA.column1 = tableB.column2;
```

我们把tableA看作左表，把tableB看成右表，那么INNER JOIN是选出两张表都存在的记录：

![](https://cdn.jsdelivr.net/gh/Kwaiyu/SQA-Study-Notes@master/docs/_media/inner-join.png)

LEFT OUTER JOIN是选出左表存在的记录：

![](https://cdn.jsdelivr.net/gh/Kwaiyu/SQA-Study-Notes@master/docs/_media/left-outer-join.png)

RIGHT OUTER JOIN是选出右表存在的记录：

![](https://cdn.jsdelivr.net/gh/Kwaiyu/SQA-Study-Notes@master/docs/_media/right-outer-join.png)

FULL OUTER JOIN则是选出左右表都存在的记录：

![](https://cdn.jsdelivr.net/gh/Kwaiyu/SQA-Study-Notes@master/docs/_media/full-outer-join.png)

## 修改数据

关系数据库的基本操作就是增删改查，即CRUD：Create、Retrieve、Update、Delete。其中，对于查询已经详细讲述了`SELECT`语句的详细用法。

而对于增、删、改，对应的SQL语句分别是：

- INSERT：插入新记录；
- UPDATE：更新已有记录；
- DELETE：删除已有记录。

### INSERT

使用`INSERT`，我们就可以一次向一个表中插入一条或多条记录。

`INSERT`语句的基本语法是：

```sql
INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);
```

我们向`students`表插入一条新记录，先列举出需要插入的字段名称，然后在`VALUES`子句中依次写出对应字段的值：

```sql
INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛', 'M', 80);
-- 查询并观察结果:
SELECT * FROM students;
```

注意并没有列出`id`字段和值，这是因为`id`字段是一个自增主键，它的值可以由数据库自己推算出来。如果一个字段有默认值，那么在`INSERT`语句中也可以不出现。字段顺序不必和数据库表的字段顺序一致，但值的顺序必须和字段顺序一致。可以写`INSERT INTO students (score, gender, name, class_id) ...`，但是对应的`VALUES`就得变成`(80, 'M', '大牛', 2)`。

还可以一次性添加多条记录，只需要在`VALUES`子句中指定多个记录值，每个记录是由`(...)`包含的一组值：

```sql
INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);
SELECT * FROM students;
```

### UPDATE

使用`UPDATE`，我们就可以一次更新表中的一条或多条记录。

`UPDATE`语句的基本语法是：

```sql
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
```

更新`students`表`id=1`的记录的`name`和`score`这两个字段，先写出`UPDATE students SET name='大牛', score=66`，然后在`WHERE`子句中写出需要更新的行的筛选条件`id=1`：

```sql
UPDATE students SET name='大牛', score=66 WHERE id=1;
-- 查询并观察结果:
SELECT * FROM students WHERE id=1;
```

注意到`UPDATE`语句的`WHERE`条件和`SELECT`语句的`WHERE`条件其实是一样的，因此完全可以一次更新多条记录：

```sql
UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
-- 查询并观察结果:
SELECT * FROM students;
```

在`UPDATE`语句中，更新字段时可以使用表达式。例如，把所有80分以下的同学的成绩加10分：

```sql
UPDATE students SET score=score+10 WHERE score<80;
-- 查询并观察结果:
SELECT * FROM students;
```

如果`WHERE`条件没有匹配到任何记录，`UPDATE`语句不会报错，也不会有任何记录被更新。当`UPDATE`语句没有`WHERE`条件时整个表的所有记录都会被更新。所以在执行`UPDATE`语句时要非常小心，最好先用`SELECT`语句来测试`WHERE`条件是否筛选出了期望的记录集，然后再用`UPDATE`更新。

在使用MySQL这类真正的关系数据库时，`UPDATE`语句会返回更新的行数以及`WHERE`条件匹配的行数。

```sql
mysql> UPDATE students SET name='大宝' WHERE id=1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

### DELETE

使用`DELETE`，我们就可以一次删除表中的一条或多条记录。

`DELETE`语句的基本语法是：

```sql
DELETE FROM <表名> WHERE ...;
```

例如，我们想删除`students`表中`id=1`的记录，就需要这么写：

```sql
DELETE FROM students WHERE id=1;
-- 查询并观察结果:
SELECT * FROM students;
```

`DELETE`语句也可以一次删除多条记录：

```
DELETE FROM students WHERE id>=5 AND id<=7;
-- 查询并观察结果:
SELECT * FROM students;
```

如果`WHERE`条件没有匹配到任何记录，`DELETE`语句不会报错，也不会有任何记录被删除。

和`UPDATE`类似，不带`WHERE`条件的`DELETE`语句会删除整个表的数据：

```
DELETE FROM students;
```

这时，整个表的所有记录都会被删除。所以在执行`DELETE`语句时也要非常小心，最好先用`SELECT`语句来测试`WHERE`条件是否筛选出了期望的记录集，然后再用`DELETE`删除。

在使用MySQL这类真正的关系数据库时，`DELETE`语句也会返回删除的行数以及`WHERE`条件匹配的行数。

```
mysql> DELETE FROM students WHERE id=1;
Query OK, 1 row affected (0.01 sec)
```

## MySQL

命令行程序`mysql`实际上是MySQL客户端，真正的MySQL服务器程序是`mysqld`，在后台运行。

> [!NOTE]
>
> MySQL Client的可执行程序是mysql，MySQL Server的可执行程序是mysqld。

在MySQL Client中输入的SQL语句通过TCP连接发送到MySQL Server。默认端口号是3306，即如果发送到本机MySQL Server，地址就是`127.0.0.1:3306`。

也可以只安装MySQL Client，然后连接到远程MySQL Server。假设远程MySQL Server的IP地址是`10.0.1.99`，那么就使用`-h`指定IP或域名：

```
mysql -h 10.0.1.99 -u root -p
```

### 管理MySQL

要管理MySQL，可以使用可视化图形界面[MySQL Workbench](https://dev.mysql.com/downloads/workbench/)

MySQL Workbench可以用可视化的方式查询、创建和修改数据库表，但是归根到底是一个图形客户端，它对MySQL的操作仍然是发送SQL语句并执行。因此本质上，MySQL Workbench和MySQL Client命令行都是客户端，和MySQL交互唯一的接口就是SQL。因此MySQL提供了大量的SQL语句用于管理。虽然可以使用MySQL Workbench图形界面来直接管理MySQL，但是很多时候，通过SSH远程连接时，只能使用SQL命令。

#### 数据库

要列出所有数据库，使用命令：

```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| shici              |
| sys                |
| test               |
| school             |
+--------------------+
```

其中，`information_schema`、`mysql`、`performance_schema`和`sys`是系统库，不要去改动它们。其他的是用户创建的数据库。

创建一个新数据库，使用命令：

```
mysql> CREATE DATABASE test;
Query OK, 1 row affected (0.01 sec)
```

要删除一个数据库，使用命令：

```
mysql> DROP DATABASE test;
Query OK, 0 rows affected (0.01 sec)
```

对一个数据库进行操作时，要首先将其切换为当前数据库：

```
mysql> USE test;
Database changed
```

#### 表

列出当前数据库的所有表，使用命令：

```
mysql> SHOW TABLES;
+---------------------+
| Tables_in_test      |
+---------------------+
| classes             |
| statistics          |
| students            |
| students_of_class1  |
+---------------------+
```

查看一个表的结构，使用命令：

```
mysql> DESC students;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | bigint(20)   | NO   | PRI | NULL    | auto_increment |
| class_id | bigint(20)   | NO   |     | NULL    |                |
| name     | varchar(100) | NO   |     | NULL    |                |
| gender   | varchar(1)   | NO   |     | NULL    |                |
| score    | int(11)      | NO   |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```

还可以使用以下命令查看创建表的SQL语句：

```
mysql> SHOW CREATE TABLE students;
+----------+-------------------------------------------------------+
| students | CREATE TABLE `students` (                             |
|          |   `id` bigint(20) NOT NULL AUTO_INCREMENT,            |
|          |   `class_id` bigint(20) NOT NULL,                     |
|          |   `name` varchar(100) NOT NULL,                       |
|          |   `gender` varchar(1) NOT NULL,                       |
|          |   `score` int(11) NOT NULL,                           |
|          |   PRIMARY KEY (`id`)                                  |
|          | ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 |
+----------+-------------------------------------------------------+
1 row in set (0.00 sec)
```

创建表使用`CREATE TABLE`语句，而删除表使用`DROP TABLE`语句：

```
mysql> DROP TABLE students;
Query OK, 0 rows affected (0.01 sec)
```

修改表就比较复杂。如果要给`students`表新增一列`birth`，使用：

```
ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
```

要修改`birth`列，例如把列名改为`birthday`，类型改为`VARCHAR(20)`：

```
ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
```

要删除列，使用：

```
ALTER TABLE students DROP COLUMN birthday;
```

使用`EXIT`命令退出MySQL：

```
mysql> EXIT
Bye
```

`EXIT`仅仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行。

### 实用SQL语句

#### 插入或替换

如果我们希望INSERT插入一条新记录，但如果记录已经存在就先删除原记录，再插入新记录。此时可以使用`REPLACE`语句，这样就不必先查询再决定是否先删除再插入：

```sql
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

若`id=1`的记录不存在，`REPLACE`语句将插入新记录，否则当前`id=1`的记录将被删除，然后再插入新记录。

#### 插入或更新

如果我们希望INSERT插入一条新记录，但如果记录已经存在就更新该记录，此时可以使用`INSERT INTO ... ON DUPLICATE KEY UPDATE ...`语句：

```sql
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
```

若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则当前`id=1`的记录将被更新，更新的字段由`UPDATE`指定。

#### 插入或忽略

如果我们希望INSERT插入一条新记录，但如果记录已经存在就直接忽略，此时可以使用`INSERT IGNORE INTO ...`语句：

```sql
INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则不执行任何操作。

#### 快照

如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合`CREATE TABLE`和`SELECT`：

```sql
-- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
```

新创建的表结构和`SELECT`使用的表结构完全一致。

#### 写入查询结果集

如果查询结果集需要写入到表中，可以结合`INSERT`和`SELECT`，将`SELECT`语句的结果集直接插入到指定表中。

例如，创建一个统计成绩的表`statistics`，记录各班的平均成绩：

```sql
CREATE TABLE statistics (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    average DOUBLE NOT NULL,
    PRIMARY KEY (id)
);
```

然后，我们就可以用一条语句写入各班的平均成绩：

```sql
INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
```

确保`INSERT`语句的列和`SELECT`语句的列能一一对应，就可以在`statistics`表中直接保存查询的结果：

```sql
> SELECT * FROM statistics;
+----+----------+--------------+
| id | class_id | average      |
+----+----------+--------------+
|  1 |        1 |         86.5 |
|  2 |        2 | 73.666666666 |
|  3 |        3 | 88.333333333 |
+----+----------+--------------+
3 rows in set (0.00 sec)
```

#### 强制实用指定索引

在查询的时候数据库系统会自动分析查询语句，并选择一个最合适的索引。但是很多时候，数据库系统的查询优化器并不一定总是能使用最优索引。如果我们知道如何选择索引，可以使用`FORCE INDEX`强制查询使用指定的索引。例如：

```sql
> SELECT * FROM students FORCE INDEX (idx_class_id) WHERE class_id = 1 ORDER BY id DESC;
```

指定索引的前提是索引`idx_class_id`必须存在。

## 事物

数据库事务具有ACID特性，用来保证多条SQL的全部执行。

在执行SQL语句的时候，某些业务要求，一系列操作必须全部执行，而不能仅执行一部分。例如，一个转账操作：

```
-- 从id=1的账户给id=2的账户转账100元
-- 第一步：将id=1的A账户余额减去100
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- 第二步：将id=2的B账户余额加上100
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
```

这两条SQL语句必须全部执行，或者，由于某些原因，如果第一条语句成功，第二条语句失败，就必须全部撤销。

这种把多条语句作为一个整体进行操作的功能，被称为数据库*事务*。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

可见，数据库事务具有ACID这4个特性：

- A：Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
- C：Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
- I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
- D：Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。

对于单条SQL语句，数据库系统自动将其作为一个事务执行，这种事务被称为*隐式事务*。

要手动把多条SQL语句作为一个事务执行，使用`BEGIN`开启一个事务，使用`COMMIT`提交一个事务，这种事务被称为*显式事务*，例如，把上述的转账操作作为一个显式事务：

```
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

很显然多条SQL语句要想作为一个事务执行，就必须使用显式事务。

`COMMIT`是指提交事务，即试图把事务内的所有SQL所做的修改永久保存。如果`COMMIT`语句执行失败了，整个事务也会失败。

有些时候，我们希望主动让事务失败，这时，可以用`ROLLBACK`回滚事务，整个事务会失败：

```
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
ROLLBACK;
```

数据库事务是由数据库系统保证的，我们只需要根据业务逻辑使用它就可以。

**隔离级别**

对于两个并发执行的事务，如果涉及到操作同一条记录的时候，可能会发生问题。因为并发操作会带来数据的不一致性，包括脏读、不可重复读、幻读等。数据库系统提供了隔离级别来让我们有针对性地选择事务的隔离级别，避免数据不一致的问题。

SQL标准定义了4种隔离级别，分别对应可能出现的数据不一致的情况：

| Isolation Level  | 脏读（Dirty Read） | 不可重复读（Non Repeatable Read） | 幻读（Phantom Read） |
| :--------------- | :----------------- | :-------------------------------- | :------------------- |
| Read Uncommitted | Yes                | Yes                               | Yes                  |
| Read Committed   | -                  | Yes                               | Yes                  |
| Repeatable Read  | -                  | -                                 | Yes                  |
| Serializable     | -                  | -                                 | -                    |

后面会依次介绍4种隔离级别的数据一致性问题。

### Read Uncommitted

Read Uncommitted是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）。

首先，我们准备好`students`表的数据，该表仅一行记录：

```
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

然后分别开启两个MySQL客户端连接，按顺序依次执行事务A和事务B：

| 时刻 | 事务A                                             | 事务B                                             |
| :--- | :------------------------------------------------ | :------------------------------------------------ |
| 1    | SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; | SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED; |
| 2    | BEGIN;                                            | BEGIN;                                            |
| 3    | UPDATE students SET name = 'Bob' WHERE id = 1;    |                                                   |
| 4    |                                                   | SELECT * FROM students WHERE id = 1;              |
| 5    | ROLLBACK;                                         |                                                   |
| 6    |                                                   | SELECT * FROM students WHERE id = 1;              |
| 7    |                                                   | COMMIT;                                           |

当事务A执行完第3步时，它更新了`id=1`的记录，但并未提交，而事务B在第4步读取到的数据就是未提交的数据。

随后，事务A在第5步进行了回滚，事务B再次读取`id=1`的记录，发现和上一次读取到的数据不一致，这就是脏读。

可见，在Read Uncommitted隔离级别下，一个事务可能读取到另一个事务更新但未提交的数据，这个数据有可能是脏数据。

### Read Committed

在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。

不可重复读是指，在一个事务内多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么在第一个事务中两次读取的数据就可能不一致。

仍然先准备好`students`表的数据：

```
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

然后，分别开启两个MySQL客户端连接，按顺序依次执行事务A和事务B：

| 时刻 | 事务A                                           | 事务B                                           |
| :--- | :---------------------------------------------- | :---------------------------------------------- |
| 1    | SET TRANSACTION ISOLATION LEVEL READ COMMITTED; | SET TRANSACTION ISOLATION LEVEL READ COMMITTED; |
| 2    | BEGIN;                                          | BEGIN;                                          |
| 3    |                                                 | SELECT * FROM students WHERE id = 1;            |
| 4    | UPDATE students SET name = 'Bob' WHERE id = 1;  |                                                 |
| 5    | COMMIT;                                         |                                                 |
| 6    |                                                 | SELECT * FROM students WHERE id = 1;            |
| 7    |                                                 | COMMIT;                                         |

当事务B第一次执行第3步的查询时，得到的结果是`Alice`，随后，由于事务A在第4步更新了这条记录并提交，所以，事务B在第6步再次执行同样的查询时，得到的结果就变成了`Bob`，因此，在Read Committed隔离级别下，事务不可重复读同一条记录，因为很可能读到的结果不一致。

### Repeatable Read

在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

幻读是指，在一个事务中，第一次查询某条记录发现没有，但是当试图更新这条不存在的记录时，竟然能成功，并且再次读取同一条记录，它就神奇地出现了。

我们仍然先准备好`students`表的数据：

```
mysql> select * from students;
+----+-------+
| id | name  |
+----+-------+
|  1 | Alice |
+----+-------+
1 row in set (0.00 sec)
```

然后，分别开启两个MySQL客户端连接，按顺序依次执行事务A和事务B：

| 时刻 | 事务A                                               | 事务B                                             |
| :--- | :-------------------------------------------------- | :------------------------------------------------ |
| 1    | SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;    | SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  |
| 2    | BEGIN;                                              | BEGIN;                                            |
| 3    |                                                     | SELECT * FROM students WHERE id = 99;             |
| 4    | INSERT INTO students (id, name) VALUES (99, 'Bob'); |                                                   |
| 5    | COMMIT;                                             |                                                   |
| 6    |                                                     | SELECT * FROM students WHERE id = 99;             |
| 7    |                                                     | UPDATE students SET name = 'Alice' WHERE id = 99; |
| 8    |                                                     | SELECT * FROM students WHERE id = 99;             |
| 9    |                                                     | COMMIT;                                           |

事务B在第3步第一次读取`id=99`的记录时，读到的记录为空，说明不存在`id=99`的记录。随后，事务A在第4步插入了一条`id=99`的记录并提交。事务B在第6步再次读取`id=99`的记录时，读到的记录仍然为空，但是，事务B在第7步试图更新这条不存在的记录时，竟然成功了，并且，事务B在第8步再次读取`id=99`的记录时，记录出现了。

可见，幻读就是没有读到的记录以为不存在，但其实是可以更新成功的，并且，更新成功后再次读取就出现了。

### Serializable

Serializable是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。

虽然Serializable隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用Serializable隔离级别。

**默认隔离级别**

如果没有指定隔离级别，数据库就会使用默认的隔离级别。在MySQL中，如果使用InnoDB，默认的隔离级别是Repeatable Read。

## 开发集成

# 触发器

# MySQL触发器简介

在MySQL中，触发器是一组SQL语句，当对相关联的表上的数据进行更改时，会自动调用该语句。 触发器可以被定义为在[INSERT](http://www.yiibai.com/mysql/insert-statement.html)，[UPDATE](http://www.yiibai.com/mysql/update-data.html)或[DELETE](http://www.yiibai.com/mysql/delete-statement.html)语句更改数据之前或之后调用。在*MySQL5.7.2*版本之前，每个表最多可以定义六个触发器。

- `BEFORE INSERT` - 在数据插入表之前被激活触发器。
- `AFTER INSERT` - 在将数据插入表之后激活触发器。
- `BEFORE UPDATE` - 在表中的数据更新之前激活触发器。
- `AFTER UPDATE` - 在表中的数据更新之后激活触发器。
- `BEFORE DELETE` - 在从表中删除数据之前激活触发器。
- `AFTER DELETE` - 从表中删除数据之后激活触发器。

但是，从*MySQL 5.7.2+*版本开始，可以[为相同的触发事件和动作时间定义多个触发器](http://www.yiibai.com/mysql/triggers-create-multiple-triggers-for-the-same-trigger-event-and-action-time.html)。

当使用不使用`INSERT`，`DELETE`或`UPDATE`语句更改表中数据的语句时，不会调用与表关联的触发器。 例如，[TRUNCATE](http://www.yiibai.com/mysql/truncate-table.html)语句删除表的所有数据，但不调用与该表相关联的触发器。

有些语句使用了后台的`INSERT`语句，如[REPLACE语句](http://www.yiibai.com/mysql/replace.html)或[LOAD DATA语句](http://www.yiibai.com/mysql/import-csv-file-mysql-table.html)。如果使用这些语句，则调用与表关联的相应触发器。

必须要为与表相关联的每个触发器使用唯一的名称。可以为不同的表定义相同的触发器名称，这是一个很好的做法。

应该使用以下命名约定命名触发器：

```sql
(BEFORE | AFTER)_tableName_(INSERT| UPDATE | DELETE)
SQL
```

例如，`before_order_update`是更新`orders`表中的行数据之前调用的触发器。

以下命名约定与上述一样。

```sql
tablename_(BEFORE | AFTER)_(INSERT| UPDATE | DELETE)
SQL
```

例如，`order_before_update`与上述`before_order_update`触发器相同。

### MySQL触发存储

MySQL在数据目录中存储触发器，例如：`/data/yiibaidb/`,并使用名为`tablename.TRG`和`triggername.TRN`的文件：

- `tablename.TRG`文件将触发器映射到相应的表。
- `triggername.TRN`文件包含触发器定义。

可以通过将触发器文件复制到备份文件夹来备份MySQL触发器。也可以[使用mysqldump工具备份触发器](http://www.yiibai.com/mysql/how-to-backup-database-using-mysqldump.html)。

### MySQL触发限制

MySQL触发器覆盖标准SQL中定义的所有功能。 但是，在应用程序中使用它们之前，您应该知道一些限制。

**MySQL触发器不能：**

- 使用在`SHOW`，`LOAD DATA`，`LOAD TABLE`，[BACKUP DATABASE](http://www.mysqltutorial.org/mysql/how-to-backup-database-using-mysqldump.aspx)，`RESTORE`，`FLUSH`和`RETURN`语句之上。
- 使用隐式或明确提交或回滚的语句，如`COMMIT`，`ROLLBACK`，`START TRANSACTION`，[LOCK/UNLOCK TABLES](http://www.mysqltutorial.org/mysql-table-locking.html)，`ALTER`，`CREATE`，`DROP`，[RENAME](http://www.mysqltutorial.org/mysql-rename-table.html)等。
- 使用[准备语句](http://www.mysqltutorial.org/mysql-prepared-statement.aspx)，如`PREPARE`，`EXECUTE`等
- 使用动态SQL语句。

从*MySQL 5.1.4*版本开始，触发器可以调用[存储过程](http://www.mysqltutorial.org/mysql-stored-procedure-tutorial.aspx)或存储函数，在这之前的版本是有所限制的。

## 触发器创建

### MySQL触发语法

为了创建一个新的触发器，可以使用`CREATE TRIGGER`语句。 下面说明了`CREATE TRIGGER`语句的语法：

```sql
CREATE TRIGGER trigger_name trigger_time trigger_event
 ON table_name
 FOR EACH ROW
 BEGIN
 ...
 END;
SQL
```

下面，我们来更详细的检查上面的语法。

- 将触发器名称放在`CREATE TRIGGER`语句之后。触发器名称应遵循命名约定`[trigger time]_[table name]_[trigger event]`，例如before_employees_update。
- 触发激活时间可以在之前或之后。必须指定定义触发器的激活时间。如果要在更改之前处理操作，则使用`BEFORE`关键字，如果在更改后需要处理操作，则使用`AFTER`关键字。
- 触发事件可以是`INSERT`，`UPDATE`或`DELETE`。此事件导致触发器被调用。 触发器只能由一个事件调用。要定义由多个事件调用的触发器，必须定义多个触发器，每个事件一个触发器。
- 触发器必须与特定表关联。没有表触发器将不存在，所以必须在`ON`关键字之后指定表名。
- 将SQL语句放在`BEGIN`和`END`块之间。这是定义触发器逻辑的位置。

### MySQL触发器示例

下面我们将在MySQL中创建触发器来记录`employees`表中行数据的更改情况。

```sql
mysql> DESC employees;
+----------------+--------------+------+-----+---------+-------+
| Field          | Type         | Null | Key | Default | Extra |
+----------------+--------------+------+-----+---------+-------+
| employeeNumber | int(11)      | NO   | PRI | NULL    |       |
| lastName       | varchar(50)  | NO   |     | NULL    |       |
| firstName      | varchar(50)  | NO   |     | NULL    |       |
| extension      | varchar(10)  | NO   |     | NULL    |       |
| email          | varchar(100) | NO   |     | NULL    |       |
| officeCode     | varchar(10)  | NO   | MUL | NULL    |       |
| reportsTo      | int(11)      | YES  | MUL | NULL    |       |
| jobTitle       | varchar(50)  | NO   |     | NULL    |       |
+----------------+--------------+------+-----+---------+-------+
8 rows in set
SQL
```

首先，创建一个名为`employees audit`的新表，用来保存`employees`表中数据的更改。 以下语句创建`employee_audit`表。

```sql
USE yiibaidb;
CREATE TABLE employees_audit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    employeeNumber INT NOT NULL,
    lastname VARCHAR(50) NOT NULL,
    changedat DATETIME DEFAULT NULL,
    action VARCHAR(50) DEFAULT NULL
);
SQL
```

接下来，创建一个`BEFORE UPDATE`触发器，该触发器在对`employees`表中的行记录更改之前被调用。

```sql
DELIMITER $$
CREATE TRIGGER before_employee_update 
    BEFORE UPDATE ON employees
    FOR EACH ROW 
BEGIN
    INSERT INTO employees_audit
    SET action = 'update',
     employeeNumber = OLD.employeeNumber,
        lastname = OLD.lastname,
        changedat = NOW(); 
END$$
DELIMITER ;
SQL
```

在触发器的主体中，使用`OLD`关键字来访问受触发器影响的行的`employeeNumber`和`lastname`列。

请注意，在为[INSERT](http://www.yiibai.com/mysql/insert-statement.html)定义的触发器中，可以仅使用`NEW`关键字。不能使用`OLD`关键字。但是，在为`DELETE`定义的触发器中，没有新行，因此您只能使用`OLD`关键字。在[UPDATE](http://www.yiibai.com/mysql/update-data.html)触发器中，`OLD`是指更新前的行，而`NEW`是更新后的行。

然后，要查看当前数据库中的所有触发器，请使用`SHOW TRIGGERS`语句，如下所示：

```sql
SHOW TRIGGERS;
SQL
```

执行上面查询语句，得到以下结果 - 

```sql
mysql> SHOW TRIGGERS;
+------------------------+--------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------+------------------------+-----------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
| Trigger                | Event  | Table     | Statement                                                                                                                                                             | Timing | Created                | sql_mode                                                                          | Definer        | character_set_client | collation_connection | Database Collation |
+------------------------+--------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------+------------------------+-----------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
| before_employee_update | UPDATE | employees | BEGIN
    INSERT INTO employees_audit
    SET action = 'update',
     employeeNumber = OLD.employeeNumber,
        lastname = OLD.lastname,
        changedat = NOW();
END | BEFORE | 2017-08-02 22:06:36.40 | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION | root@localhost | utf8                 | utf8_general_ci      | utf8_general_ci    |
+------------------------+--------+-----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------+------------------------+-----------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
1 row in set
SQL
```

之后，更新`employees`表以检查触发器是否被调用。

```sql
UPDATE employees 
SET 
    lastName = 'Maxsu'
WHERE
    employeeNumber = 1056;
SQL
```

最后，要检查触发器是否被`UPDATE`语句调用，可以使用以下查询来查询`employees_audit`表：

```sql
SELECT * FROM employees_audit;
SQL
```

以下是查询的输出：

```sql
mysql> SELECT * FROM employees_audit;
+----+----------------+----------+---------------------+--------+
| id | employeeNumber | lastname | changedat           | action |
+----+----------------+----------+---------------------+--------+
|  1 |           1056 | Hill     | 2017-08-02 22:15:51 | update |
+----+----------------+----------+---------------------+--------+
1 row in set
SQL
```

如上面输出结果所示，触发器被真正调用，并在`employees_audit`表中插入一个新行。

在本教程中，您已经学会了如何在MySQL中创建一个触发器。我们还向您展示了如何开发触发器来审计员工(`employees`)表的更改。

## 创建多个触发器

本教程与*MySQL5.7.2+*版本相关。 如果您有一个较旧版本的MySQL，本教程中的语句将无法正常工作。

在*MySQL5.7.2+*版本之前，您只能为表中的事件创建一个触发器，例如，只能为`BEFORE UPDATE`或`AFTER UPDATE`事件创建一个触发器。 *MySQL 5.7.2+*版本解决了这样限制，并允许您为表中的相同事件和动作时间创建多个触发器。当事件发生时，触发器将依次激活。

参考[创建第一个触发器](http://www.yiibai.com/create-the-first-trigger-in-mysql.html)中的语法。如果表中有相同事件有多个触发器，MySQL将按照创建的顺序调用触发器。要更改触发器的顺序，需要在`FOR EACH ROW`子句之后指定`FOLLOWS`或`PRECEDES`。如下说明 - 

- `FOLLOWS`选项允许新触发器在现有触发器之后激活。
- `PRECEDES`选项允许新触发器在现有触发器之前激活。

以下是使用显式顺序创建新的附加触发器的语法：

```sql
DELIMITER $$
CREATE TRIGGER  trigger_name
[BEFORE|AFTER] [INSERT|UPDATE|DELETE] ON table_name
FOR EACH ROW [FOLLOWS|PRECEDES] existing_trigger_name
BEGIN
…
END$$
DELIMITER ;
SQL
```

### MySQL多重触发器示例

我们来看如何一个在表中的同一个事件和动作上，创建多个触发器的例子。

下面将使用[示例数据库(yiibaidb)](http://www.yiibai.com/mysql/sample-database.html)中的`products`表进行演示。假设，每当更改产品的价格(`MSRP`列)时，要将旧的价格记录在一个名为`price_logs`的表中。

首先，使用[CREATE TABLE](http://www.yiibai.com/mysql/create-table.html)语句创建一个新的`price_logs`表，如下所示：

```sql
USE yiibaidb;
CREATE TABLE price_logs (
  id INT(11) NOT NULL AUTO_INCREMENT,
  product_code VARCHAR(15) NOT NULL,
  price DOUBLE NOT NULL,
  updated_at TIMESTAMP NOT NULL DEFAULT 
             CURRENT_TIMESTAMP 
             ON UPDATE CURRENT_TIMESTAMP,

  PRIMARY KEY (id),

  KEY product_code (product_code),

  CONSTRAINT price_logs_ibfk_1 FOREIGN KEY (product_code) 
  REFERENCES products (productCode) 
  ON DELETE CASCADE 
  ON UPDATE CASCADE
);
SQL
```

*其次*，当表的`BEFORE UPDATE`事件发生时，创建一个新的触发器。触发器名称为`before_products_update`，具体实现如下所示：

```sql
DELIMITER $$

CREATE TRIGGER before_products_update 
   BEFORE UPDATE ON products 
   FOR EACH ROW 
BEGIN
     INSERT INTO price_logs(product_code,price)
     VALUES(old.productCode,old.msrp);
END$$

DELIMITER ;
SQL
```

*第三*，我们更改产品的价格，并使用以下[UPDATE](http://www.yiibai.com/mysql/update-data.html)语句，最后查询`price_logs`表：

```sql
UPDATE products
SET msrp = 95.1
WHERE productCode = 'S10_1678';
-- 查询结果价格记录
SELECT * FROM price_logs;
SQL
```

上面查询语句执行后，得到以下结果 - 

```sql
+----+--------------+-------+---------------------+
| id | product_code | price | updated_at          |
+----+--------------+-------+---------------------+
|  1 | S10_1678     |  95.7 | 2017-08-03 02:46:42 |
+----+--------------+-------+---------------------+
1 row in set
SQL
```

可以看到结果中，它按我们预期那样工作了。

假设不仅要看到旧的价格，改变的时候，还要记录是谁修改了它。 我们可以向`price_logs`表添加其他列。 但是，为了实现多个触发器的演示，我们将创建一个新表来存储进行更改的用户的数据。这个新表的名称为`user_change_logs`，结构如下：

```sql
USE yiibaidb;
CREATE TABLE user_change_logs (
  id int(11) NOT NULL AUTO_INCREMENT,
  product_code varchar(15) DEFAULT NULL,
  updated_at timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP 
  ON UPDATE CURRENT_TIMESTAMP,

  updated_by varchar(30) NOT NULL,

  PRIMARY KEY (id),

  KEY product_code (product_code),

  CONSTRAINT user_change_logs_ibfk_1 FOREIGN KEY (product_code) 
  REFERENCES products (productCode) 
  ON DELETE CASCADE ON UPDATE CASCADE
);
SQL
```

现在，我们创建一个在`products`表上的`BEFORE UPDATE`事件上激活的第二个触发器。 此触发器将更改的用户信息更新到`user_change_logs`表。 它在`before_products_update`触发后被激活。

```sql
DELIMITER $$
CREATE TRIGGER before_products_update_2 
   BEFORE UPDATE ON products 
   FOR EACH ROW FOLLOWS before_products_update
BEGIN
   INSERT INTO user_change_logs(product_code,updated_by)
   VALUES(old.productCode,user());
END$$

DELIMITER ;
SQL
```

下面我们来做一个快速测试。

*首先*，使用[UPDATE语句](http://www.yiibai.com/mysql/update-data.html)更新指定产品的价格，如下：

```sql
UPDATE products
SET msrp = 95.3
WHERE productCode = 'S10_1678';
SQL
```

*其次*，分别从`price_logs`和`user_change_logs`表查询数据：

```sql
SELECT * FROM price_logs;
SQL
```

上面查询语句执行后，得到以下结果 - 

```sql
mysql> SELECT * FROM price_logs;
+----+--------------+-------+---------------------+
| id | product_code | price | updated_at          |
+----+--------------+-------+---------------------+
|  1 | S10_1678     |  95.7 | 2017-08-03 02:46:42 |
|  2 | S10_1678     |  95.1 | 2017-08-03 02:47:21 |
+----+--------------+-------+---------------------+
2 rows in set
SQL
SELECT * FROM user_change_logs;
SQL
```

上面查询语句执行后，得到以下结果 - 

```sql
mysql> SELECT * FROM user_change_logs;
+----+--------------+---------------------+----------------+
| id | product_code | updated_at          | updated_by     |
+----+--------------+---------------------+----------------+
|  1 | S10_1678     | 2017-08-03 02:47:21 | root@localhost |
+----+--------------+---------------------+----------------+
1 row in set
SQL
```

如上所见，两个触发器按照预期的顺序激活执行相关操作了。

### 触发器顺序

如果使用`SHOW TRIGGERS`语句，则不会在表中看到触发激活同一事件和操作的顺序。

```sql
SHOW TRIGGERS FROM yiibaidb;
SQL
```

要查找此信息，需要如下查询`information_schema`数据库的`triggers`表中的`action_order`列，如下查询语句 - 

```sql
SELECT 
    trigger_name, action_order
FROM
    information_schema.triggers
WHERE
    trigger_schema = 'yiibaidb'
ORDER BY event_object_table , 
         action_timing , 
         event_manipulation;
SQL
```

上面查询语句执行后，得到以下结果 - 

```sql
mysql> SELECT 
    trigger_name, action_order
FROM
    information_schema.triggers
WHERE
    trigger_schema = 'yiibaidb'
ORDER BY event_object_table , 
         action_timing , 
         event_manipulation;
+--------------------------+--------------+
| trigger_name             | action_order |
+--------------------------+--------------+
| before_employee_update   |            1 |
| before_products_update   |            1 |
| before_products_update_2 |            2 |
+--------------------------+--------------+
3 rows in set
SQL
```

## 触发器管理

在本教程中，您将学习如何管理触发器，包括在MySQL数据库中显示，修改和删除触发器。

[创建触发器](http://www.yiibai.com/mysql/create-the-first-trigger-in-mysql.html)后，可以在包含触发器定义文件的数据文件夹中显示其定义。触发器作为纯文本文件存储在以下数据库文件夹中：

```sql
/data_folder/database_name/table_name.trg
SQL
```

也可通过查询`information_schema`数据库中的`triggers`表来显示触发器，如下所示：

```sql
SELECT 
    *
FROM
    information_schema.triggers
WHERE
    trigger_schema = 'database_name'
        AND trigger_name = 'trigger_name';
SQL
```

该语句允许您查看触发器的内容及其元数据，例如：关联表名和定义器，这是创建触发器的[MySQL用户](http://www.yiibai.com/mysql/create-user.html)的名称。

如果要检索指定数据库中的所有触发器，则需要使用以下[SELECT语句](http://www.yiibai.com/mysql/select-statement-query-data.html)从`information_schema`数据库中的`triggers`表查询数据：

```sql
SELECT 
    *
FROM
    information_schema.triggers
WHERE
    trigger_schema = 'database_name';
SQL
```

要查找与特定表相关联的所有触发器，请使用以下查询：

```sql
SELECT 
    *
FROM
    information_schema.triggers
WHERE
    trigger_schema = 'database_name'
        AND event_object_table = 'table_name';
SQL
```

例如，以下查询语句与`yiibaidb`数据库中的`employees`表相关联的所有触发器。

```sql
SELECT * FROM information_schema.triggers
WHERE trigger_schema = 'yiibaidb'
        AND event_object_table = 'employees';
SQL
```

执行上面查询，得到以下结果 - 

```sql
mysql> SELECT * FROM information_schema.triggers
WHERE trigger_schema = 'yiibaidb'
        AND event_object_table = 'employees';
+-----------------+----------------+------------------------+--------------------+----------------------+---------------------+--------------------+--------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+---------------+----------------------------+----------------------------+--------------------------+--------------------------+------------------------+-----------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
| TRIGGER_CATALOG | TRIGGER_SCHEMA | TRIGGER_NAME           | EVENT_MANIPULATION | EVENT_OBJECT_CATALOG | EVENT_OBJECT_SCHEMA | EVENT_OBJECT_TABLE | ACTION_ORDER | ACTION_CONDITION | ACTION_STATEMENT                                                                                                                                                      | ACTION_ORIENTATION | ACTION_TIMING | ACTION_REFERENCE_OLD_TABLE | ACTION_REFERENCE_NEW_TABLE | ACTION_REFERENCE_OLD_ROW | ACTION_REFERENCE_NEW_ROW | CREATED                | SQL_MODE                                                                          | DEFINER        | CHARACTER_SET_CLIENT | COLLATION_CONNECTION | DATABASE_COLLATION |
+-----------------+----------------+------------------------+--------------------+----------------------+---------------------+--------------------+--------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+---------------+----------------------------+----------------------------+--------------------------+--------------------------+------------------------+-----------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
| def             | yiibaidb       | before_employee_update | UPDATE             | def                  | yiibaidb            | employees          |            1 | NULL             | BEGIN
    INSERT INTO employees_audit
    SET action = 'update',
     employeeNumber = OLD.employeeNumber,
        lastname = OLD.lastname,
        changedat = NOW();
END | ROW                | BEFORE        | NULL                       | NULL                       | OLD                      | NEW                      | 2017-08-02 22:06:36.40 | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION | root@localhost | utf8                 | utf8_general_ci      | utf8_general_ci    |
+-----------------+----------------+------------------------+--------------------+----------------------+---------------------+--------------------+--------------+------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------+---------------+----------------------------+----------------------------+--------------------------+--------------------------+------------------------+-----------------------------------------------------------------------------------+----------------+----------------------+----------------------+--------------------+
1 row in set
SQL
```

### MySQL SHOW TRIGGERS语句

在特定数据库中显示触发器的另一种方法是使用`SHOW TRIGGERS`语句，如下所示：

```sql
SHOW TRIGGERS [FROM|IN] database_name
[LIKE expr | WHERE expr];
SQL
```

例如，如果要查看当前数据库中的所有触发器，可以使用`SHOW TRIGGERS`语句，如下所示：

```sql
SHOW TRIGGERS;
SQL
```

要获取特定数据库中的所有触发器，请在`SHOW TRIGGERS`语句中指定数据库名称，比如要查询数据库：`yiibaidb`下的所有触发器，如下所示：

```sql
SHOW TRIGGERS FROM yiibaidb;
SQL
```

上面语句返回`yiibaidb`数据库中的所有触发器。

要获取与特定表相关联的所有触发器，可以使用`SHOW TRIGGERS`语句中的`WHERE`子句。 以下语句返回与`employees`表相关联的所有触发器：

```sql
SHOW TRIGGERS FROM yiibaidb
WHERE `table` = 'employees';
SQL
```

请注意，我们使用反引号包装`table`列，因为`table`是MySQL中的保留关键字。

当执行`SHOW TRIGGERS`语句时，MySQL返回以下列 -

- `Trigger`：存储触发器的名称，例如`before_employee_update`触发器。
- `Event`：指定事件，例如，调用触发器的`INSERT`，`UPDATE`或`DELETE`。
- `Table`：指定触发器与例如相关联的表,如`employees`表。
- `Statement`：存储调用触发器时要执行的语句或复合语句。
- `Timing`：接受两个值：`BEFORE`和`AFTER`，它指定触发器的激活时间。
- `Created`：在创建触发器时记录创建的时间。
- `sql_mode`：指定触发器执行时的SQL模式。
- `Definer`：记录创建触发器的帐户。

> 请注意，要执行`SHOW TRIGGERS`语句，您必须具有`SUPER`权限。

### 删除触发器

要删除现有的触发器，请使用`DROP TRIGGER`语句，如下所示：

```sql
DROP TRIGGER table_name.trigger_name;
SQL
```

例如，如果要删除与`employees`表相关联的`before_employees_update`触发器，则可以执行以下语句：

```sql
DROP TRIGGER employees.before_employees_update;
SQL
```

要修改触发器，必须首先删除它并使用新的代码重新创建。在MySQL中没有类似:`ALTER TRIGGER`语句，因此，您不能像修改其他数据库对象，如[表](http://www.yiibai.com/mysql/database-table-maintenance-statements.html)，[视图](http://www.yiibai.com/mysql/views-tutorial.html)和[存储过程](http://www.yiibai.com/mysql/stored-procedure-tutorial.html)那样修改触发器。

## 计划任务事件

在本教程中，您将了解MySQL事件调度程序以及如何创建MySQL事件以自动执行数据库任务。

MySQL事件是基于预定义的时间表运行的任务，因此有时它被称为预定事件。MySQL事件也被称为“时间触发”，因为它是由时间触发的，而不是像[触发器](http://www.yiibai.com/mysql/triggers.html)一样更新表来触发的。MySQL事件类似于UNIX中的cron作业或Windows中的任务调度程序。

您可以在许多情况下使用MySQL事件，例如优化数据库表，清理日志，归档数据或在非高峰时间生成复杂的报告。

### MySQL事件调度器配置

MySQL使用一个名为事件调度线程的特殊线程来执行所有调度的事件。可以通过执行以下命令来查看事件调度程序线程的状态：

```sql
SHOW PROCESSLIST;
SQL
```

执行上面查询语句，得到以下结果 - 

```shell
mysql> SHOW PROCESSLIST;
+----+------+-----------------+----------+---------+------+----------+------------------+
| Id | User | Host            | db       | Command | Time | State    | Info             |
+----+------+-----------------+----------+---------+------+----------+------------------+
|  2 | root | localhost:50405 | NULL     | Sleep   | 1966 |          | NULL             |
|  3 | root | localhost:50406 | yiibaidb | Sleep   | 1964 |          | NULL             |
|  4 | root | localhost:50407 | yiibaidb | Query   |    0 | starting | SHOW PROCESSLIST |
+----+------+-----------------+----------+---------+------+----------+------------------+
3 rows in set
Shell
```

默认情况下，事件调度程序线程未启用。 要启用和启动事件调度程序线程，需要执行以下命令：

```sql
SET GLOBAL event_scheduler = ON;
SQL
```

现在看到事件调度器线程的状态，再次执行`SHOW PROCESSLIST`命令，结果如下所示 - 

```sql
mysql> SHOW PROCESSLIST;
+----+-----------------+-----------------+----------+---------+------+------------------------+------------------+
| Id | User            | Host            | db       | Command | Time | State                  | Info             |
+----+-----------------+-----------------+----------+---------+------+------------------------+------------------+
|  2 | root            | localhost:50405 | NULL     | Sleep   | 1986 |                        | NULL             |
|  3 | root            | localhost:50406 | yiibaidb | Sleep   | 1984 |                        | NULL             |
|  4 | root            | localhost:50407 | yiibaidb | Query   |    0 | starting               | SHOW PROCESSLIST |
|  5 | event_scheduler | localhost       | NULL     | Daemon  |    6 | Waiting on empty queue | NULL             |
+----+-----------------+-----------------+----------+---------+------+------------------------+------------------+
4 rows in set
SQL
```

要禁用并停止事件调度程序线程，可通过执行`SET GLOBAL`命令将`event_scheduler`其值设置为`OFF`：

```sql
SET GLOBAL event_scheduler = OFF;
SQL
```

### 创建新的MySQL事件

创建事件与创建其他数据库对象(如存储过程或触发器)类似。事件是一个包含SQL语句的命名对象。

[存储过程](http://www.yiibai.com/mysql/stored-procedure.html)仅在直接调用时执行; [触发器](http://www.yiibai.com/mysql/triggers.html)则与一个表相关联的事件(例如[插入](http://www.yiibai.com/mysql/insert-statement.html)，[更新](http://www.yiibai.com/mysql/update-data.html)或[删除](http://www.yiibai.com/mysql/delete-statement.html))事件发生时，可以在一次或更多的规则间隔执行事件时执行触发。

要创建和计划新事件，请使用`CREATE EVENT`语句，如下所示：

```sql
CREATE EVENT [IF NOT EXIST]  event_name
ON SCHEDULE schedule
DO
event_body
SQL
```

下面让我们更详细地解释语法中的一些参数 -

- *首先*，在`CREATE EVENT`子句之后指定事件名称。事件名称在数据库模式中必须是唯一的。
- *其次*，在`ON SCHEDULE`子句后面加上一个表。如果事件是一次性事件，则使用语法：`AT timestamp [+ INTERVAL]`，如果事件是循环事件，则使用`EVERY`子句：`EVERY interval STARTS timestamp [+INTERVAL] ENDS timestamp [+INTERVAL]`
- *第三*，将`DO`语句放在`DO`关键字之后。请注意，可以在事件主体内调用存储过程。 如果您有复合SQL语句，可以将它们放在`BEGIN END`块中。

我们来看几个创建事件的例子来了解上面的语法。

*首先*，创建并计划将一个消息插入到`messages`表中的一次性事件，请执行以下步骤：

```sql
USE testdb;
CREATE TABLE IF NOT EXISTS messages (
    id INT PRIMARY KEY AUTO_INCREMENT,
    message VARCHAR(255) NOT NULL,
    created_at DATETIME NOT NULL
);
SQL
```

*其次*，使用`CREATE EVENT`语句创建一个事件：

```sql
CREATE EVENT IF NOT EXISTS test_event_01
ON SCHEDULE AT CURRENT_TIMESTAMP
DO
  INSERT INTO messages(message,created_at)
  VALUES('Test MySQL Event 1',NOW());
SQL
```

*第三*，检查`messages`表; 会看到有`1`条记录。这意味着事件在创建时被执行。

```sql
SELECT * FROM messages;
SQL
```

执行上面查询语句，得到以下结果 - 

```shell
mysql> SELECT * FROM messages;
+----+--------------------+---------------------+
| id | message            | created_at          |
+----+--------------------+---------------------+
|  1 | Test MySQL Event 1 | 2017-08-03 04:23:11 |
+----+--------------------+---------------------+
1 row in set
Shell
```

要显示数据库(`testdb`)的所有事件，请使用以下语句：

```sql
SHOW EVENTS FROM testdb;
SQL
```

执行上面查询看不到任何行返回，因为事件在到期时自动删除。 在我们的示例中，它是一次性的事件，在执行完成时就过期了。

要更改此行为，可以使用`ON COMPLETION PRESERVE`子句。以下语句创建另一个一次性事件，在其创建时间`1`分钟后执行，执行后不会被删除。

```sql
CREATE EVENT test_event_02
ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 1 MINUTE
ON COMPLETION PRESERVE
DO
   INSERT INTO messages(message,created_at)
   VALUES('Test MySQL Event 2',NOW());
SQL
```

等待`1`分钟后，查看`messages`表，添加了另一条记录：

```sql
SELECT * FROM messages;
SQL
```

执行上面查询语句，得到以下结果 - 

```shell
mysql> SELECT * FROM messages;
+----+--------------------+---------------------+
| id | message            | created_at          |
+----+--------------------+---------------------+
|  1 | Test MySQL Event 1 | 2017-08-03 04:23:11 |
|  2 | Test MySQL Event 2 | 2017-08-03 04:24:48 |
+----+--------------------+---------------------+
2 rows in set
Shell
```

如果再次执行`SHOW EVENTS`语句，看到事件是由于`ON COMPLETION PRESERVE`子句的影响：

```sql
SHOW EVENTS FROM testdb;
SQL
```

执行上面查询语句，得到以下结果 - 

```shell
mysql> SHOW EVENTS FROM testdb;
+--------+---------------+----------------+-----------+----------+---------------------+----------------+----------------+--------+------+----------+------------+----------------------+----------------------+--------------------+
| Db     | Name          | Definer        | Time zone | Type     | Execute at          | Interval value | Interval field | Starts | Ends | Status   | Originator | character_set_client | collation_connection | Database Collation |
+--------+---------------+----------------+-----------+----------+---------------------+----------------+----------------+--------+------+----------+------------+----------------------+----------------------+--------------------+
| testdb | test_event_02 | root@localhost | SYSTEM    | ONE TIME | 2017-08-03 04:24:48 | NULL           | NULL           | NULL   | NULL | DISABLED |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
+--------+---------------+----------------+-----------+----------+---------------------+----------------+----------------+--------+------+----------+------------+----------------------+----------------------+--------------------+
1 row in set
Shell
```

以下语句创建一个循环的事件，每分钟执行一次，并在其创建时间的`1`小时内过期：

```sql
CREATE EVENT test_event_03
ON SCHEDULE EVERY 1 MINUTE
STARTS CURRENT_TIMESTAMP
ENDS CURRENT_TIMESTAMP + INTERVAL 1 HOUR
DO
   INSERT INTO messages(message,created_at)
   VALUES('Test MySQL recurring Event',NOW());
SQL
```

请注意，使用`STARTS`和`ENDS`子句定义事件的有效期。等待个3，5分钟后再查看`messages`表数据，以测试验证此循环事件的执行。

```sql
SELECT * FROM messages;
SQL
```

执行上面查询语句，得到以下结果 - 

```shell
mysql> SELECT * FROM messages;
+----+----------------------------+---------------------+
| id | message                    | created_at          |
+----+----------------------------+---------------------+
|  1 | Test MySQL Event 1         | 2017-08-03 04:23:11 |
|  2 | Test MySQL Event 2         | 2017-08-03 04:24:48 |
|  3 | Test MySQL recurring Event | 2017-08-03 04:25:20 |
|  4 | Test MySQL recurring Event | 2017-08-03 04:26:20 |
|  5 | Test MySQL recurring Event | 2017-08-03 04:27:20 |
+----+----------------------------+---------------------+
5 rows in set
Shell
```

### 删除MySQL事件

要删除现有事件，请使用`DROP EVENT`语句，如下所示：

```sql
DROP EVENT [IF EXISTS] event_name;
SQL
```

例如，要删除`test_event_03`的事件，请使用以下语句：

```sql
DROP EVENT IF EXISTS test_event_03;
SQL
```

在本教程中，您已经了解了MySQL事件，如何从数据库模式创建和删除事件。 在下一个教程中，我们将向您展示如何修改事件。

## 修改事件

本教程将向您展示如何使用`ALTER EVENT`语句修改现有的[MySQL事件](http://www.yiibai.com/mysql/triggers/mysql/working-mysql-scheduled-event.html)。 在学习完本教程之后，您将了解如何修改事件和计划，如何启用或禁用事件以及如何重命名事件。

MySQL允许您更改现有事件的各种属性。要更改现有事件，请使用`ALTER EVENT`语句，如下所示：

```sql
ALTER EVENT event_name
ON SCHEDULE schedule
ON COMPLETION [NOT] PRESERVE
RENAME TO new_event_name
ENABLE | DISABLE
DO
  event_body
SQL
```

请注意，`ALTER EVENT`语句仅适用于存在的事件。如果您尝试修改不存在的事件，MySQL将会发出一条错误消息，因此在更改事件之前，应先使用`SHOW EVENTS`语句检查事件的存在。

```sql
SHOW EVENTS FROM testdb;
SQL
```

执行上面查询，得到以下结果 - 

```shell
mysql> SHOW EVENTS FROM testdb;
+--------+---------------+----------------+-----------+----------+---------------------+----------------+----------------+--------+------+----------+------------+----------------------+----------------------+--------------------+
| Db     | Name          | Definer        | Time zone | Type     | Execute at          | Interval value | Interval field | Starts | Ends | Status   | Originator | character_set_client | collation_connection | Database Collation |
+--------+---------------+----------------+-----------+----------+---------------------+----------------+----------------+--------+------+----------+------------+----------------------+----------------------+--------------------+
| testdb | test_event_02 | root@localhost | SYSTEM    | ONE TIME | 2017-08-03 04:24:48 | NULL           | NULL           | NULL   | NULL | DISABLED |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
+--------+---------------+----------------+-----------+----------+---------------------+----------------+----------------+--------+------+----------+------------+----------------------+----------------------+--------------------+
1 row in set
Shell
```

### ALTER EVENT示例

我们创建一个示例事件来演示如何使用`ALTER EVENT`语句的各种功能。

以下语句创建一个事件，每分钟将一条新记录插入到`messages`表中。

```sql
USE testdb;
CREATE EVENT test_event_04
ON SCHEDULE EVERY 1 MINUTE
DO
   INSERT INTO messages(message,created_at)
   VALUES('Test ALTER EVENT statement',NOW());
SQL
```

**改变调度时间**

要修改事件为每`2`分钟运行一次，请使用以下语句：

```sql
ALTER EVENT test_event_04
ON SCHEDULE EVERY 2 MINUTE;
SQL
```

**改变事件的主体代码逻辑**

您还可以通过指定新的逻辑来更改事件的主体代码，如下所示：

```sql
ALTER EVENT test_event_04
DO
   INSERT INTO messages(message,created_at)
   VALUES('Message from event',NOW());
-- 清空表中的数据
truncate messages;
SQL
```

上面修改完成后，可以等待`2`分钟，再次查看`messages`表：

```sql
SELECT * FROM messages;
SQL
```

执行上面查询，得到以下结果 - 

```shell
mysql> SELECT * FROM messages;
+----+--------------------+---------------------+
| id | message            | created_at          |
+----+--------------------+---------------------+
|  1 | Message from event | 2017-08-03 04:46:47 |
|  2 | Message from event | 2017-08-03 04:48:47 |
+----+--------------------+---------------------+
2 rows in set
Shell
```

**禁用事件**

要禁用某个事件,请在`ALTER EVENT`语句之后使用`DISABLE`关键字，请使用以下语句：

```sql
ALTER EVENT test_event_04
DISABLE;
SQL
```

也可以通过使用`SHOW EVENTS`语句来查看事件的状态，如下所示：

```sql
SHOW EVENTS FROM testdb;
SQL
```

执行上面查询，得到以下结果 - 

```shell
mysql> SHOW EVENTS FROM testdb;
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
| Db     | Name          | Definer        | Time zone | Type      | Execute at          | Interval value | Interval field | Starts              | Ends | Status   | Originator | character_set_client | collation_connection | Database Collation |
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
| testdb | test_event_02 | root@localhost | SYSTEM    | ONE TIME  | 2017-08-03 04:24:48 | NULL           | NULL           | NULL                | NULL | DISABLED |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
| testdb | test_event_04 | root@localhost | SYSTEM    | RECURRING | NULL                | 2              | MINUTE         | 2017-08-03 04:44:47 | NULL | DISABLED |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
2 rows in set
Shell
```

**启用事件**

要启用已禁用的事件，请在`ALTER EVENT`语句之后使用`ENABLE`关键字，如下所示：

```sql
ALTER EVENT test_event_04
ENABLE;
SQL
```

查询上面语句执行结果，得到以下结果 - 

```shell
mysql> SHOW EVENTS FROM testdb;
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
| Db     | Name          | Definer        | Time zone | Type      | Execute at          | Interval value | Interval field | Starts              | Ends | Status   | Originator | character_set_client | collation_connection | Database Collation |
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
| testdb | test_event_02 | root@localhost | SYSTEM    | ONE TIME  | 2017-08-03 04:24:48 | NULL           | NULL           | NULL                | NULL | DISABLED |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
| testdb | test_event_04 | root@localhost | SYSTEM    | RECURRING | NULL                | 2              | MINUTE         | 2017-08-03 04:44:47 | NULL | ENABLED  |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
2 rows in set
Shell
```

**重命名事件**

MySQL不提供类似`RENAME EVENT`语句。幸运的是，我们可以使用`ALTER EVENT`重命名现有事件，如下所示：

```sql
ALTER EVENT test_event_04
RENAME TO test_event_05;
SQL
```

查询上面语句执行结果，得到以下结果 - 

```shell
mysql> SHOW EVENTS FROM testdb;
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
| Db     | Name          | Definer        | Time zone | Type      | Execute at          | Interval value | Interval field | Starts              | Ends | Status   | Originator | character_set_client | collation_connection | Database Collation |
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
| testdb | test_event_02 | root@localhost | SYSTEM    | ONE TIME  | 2017-08-03 04:24:48 | NULL           | NULL           | NULL                | NULL | DISABLED |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
| testdb | test_event_05 | root@localhost | SYSTEM    | RECURRING | NULL                | 2              | MINUTE         | 2017-08-03 04:44:47 | NULL | ENABLED  |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
+--------+---------------+----------------+-----------+-----------+---------------------+----------------+----------------+---------------------+------+----------+------------+----------------------+----------------------+--------------------+
2 rows in set
Shell
```

**将事件移动到其他数据库**

可以通过使用`RENAME TO`子句将事件从一个数据库移动到另一个数据库中，如下所示：

```sql
ALTER EVENT testdb.test_event_05
RENAME TO newdb.test_event_05;
SQL
```

查询上面语句执行结果，得到以下结果 - 

```sql
mysql> SHOW EVENTS FROM newdb;
+-------+---------------+----------------+-----------+-----------+------------+----------------+----------------+---------------------+------+---------+------------+----------------------+----------------------+--------------------+
| Db    | Name          | Definer        | Time zone | Type      | Execute at | Interval value | Interval field | Starts              | Ends | Status  | Originator | character_set_client | collation_connection | Database Collation |
+-------+---------------+----------------+-----------+-----------+------------+----------------+----------------+---------------------+------+---------+------------+----------------------+----------------------+--------------------+
| newdb | test_event_05 | root@localhost | SYSTEM    | RECURRING | NULL       | 2              | MINUTE         | 2017-08-03 04:44:47 | NULL | ENABLED |          0 | utf8                 | utf8_general_ci      | utf8_general_ci    |
+-------+---------------+----------------+-----------+-----------+------------+----------------+----------------+---------------------+------+---------+------------+----------------------+----------------------+--------------------+
1 row in set
SQL
```

假设`newdb`数据库在MySQL数据库服务器中可用。

在本教程中，我们向您展示了如何使用`ALTER EVENT`语句更改MySQL事件的各种属性。

# 存储过程

## 存储过程简介

在本教程中，您将学习MySQL存储过程什么以及其相关概念，并了解MySQL存储过的优缺点。

### 存储过程的定义

存储过程是存储在数据库目录中的一段声明性SQL语句。 [触发器](http://www.yiibai.com/mysql/triggers.html)，其他存储过程以及[Java](http://www.yiibai.com/jdbc/"Java")，[Python](http://www.yiibai.com/python/)，[PHP](http://www.yiibai.com/php/)等应用程序可以调用存储过程。

![img](http://www.yiibai.com/uploads/images/201707/3107/395100831_29378.jpg)

自身的存储过程称为递归存储过程。大多数数据库管理系统支持递归存储过程。 但是，MySQL不支持它。 在MySQL中实现递归存储过程之前，您应该检查MySQL数据库的版本。

### 在MySQL中存储过程

MySQL是最受欢迎的开源RDBMS，被社区和企业广泛使用。 然而，在它发布的第一个十年期间，它不支持存储过程，[存储函数](http://www.yiibai.com/mysql/stored-function.html)，[触发器](http://www.yiibai.com/mysql/triggers.html)和[事件](http://www.yiibai.com/mysql/triggers/working-mysql-scheduled-event.html)。自从*MySQL 5.0*版本以来，这些功能被添加到MySQL数据库引擎，使其更加灵活和强大。

### MySQL存储过程的优点

- 通常存储过程有助于提高应用程序的性能。当创建，存储过程被编译之后，就存储在数据库中。 但是，MySQL实现的存储过程略有不同。 MySQL存储过程按需编译。 在编译存储过程之后，MySQL将其放入缓存中。 MySQL为每个连接维护自己的存储过程高速缓存。 如果应用程序在单个连接中多次使用存储过程，则使用编译版本，否则存储过程的工作方式类似于查询。
- 存储过程有助于减少应用程序和数据库服务器之间的流量，因为应用程序不必发送多个冗长的SQL语句，而只能发送存储过程的名称和参数。
- 存储的程序对任何应用程序都是可重用的和透明的。 存储过程将数据库接口暴露给所有应用程序，以便开发人员不必开发存储过程中已支持的功能。
- 存储的程序是安全的。 数据库管理员可以向访问数据库中存储过程的应用程序授予适当的权限，而不向基础数据库表提供任何权限。

除了这些优点之外，存储过程有其自身的缺点，在数据库中使用它们之前，您应该注意这些缺点。

### MySQL存储过程的缺点

- 如果使用大量存储过程，那么使用这些存储过程的每个连接的内存使用量将会大大增加。 此外，如果您在存储过程中过度使用大量逻辑操作，则CPU使用率也会增加，因为数据库服务器的设计不当于逻辑运算。
- 存储过程的构造使得开发具有复杂业务逻辑的存储过程变得更加困难。
- 很难调试存储过程。只有少数数据库管理系统允许您调试存储过程。不幸的是，MySQL不提供调试存储过程的功能。
- 开发和维护存储过程并不容易。开发和维护存储过程通常需要一个不是所有应用程序开发人员拥有的专业技能。这可能会导致应用程序开发和维护阶段的问题。

MySQL存储过程有自己的优点和缺点。开发应用程序时，您应该决定是否应该或不应该根据业务需求使用存储过程。

在下面的教程中，我们将向您展示如何在数据库编程任务中利用MySQL存储过程与许多实际示例。

## 存储过程入门

### 编写第一个MySQL存储过程

我们将开发一个名为`GetAllProducts()`的简单[存储过程](http://www.yiibai.com/introduction-to-sql-stored-procedures.html)来帮助您熟悉创建存储过程的语法。 `GetAllProducts()`存储过程从`products`表中选择所有产品。

启动 *mysql* 客户端工具并键入以下命令：

```sql
DELIMITER //
 CREATE PROCEDURE GetAllProducts()
   BEGIN
   SELECT *  FROM products;
   END //
DELIMITER ;
SQL
```

让我们来详细地说明上述存储过程：

- 第一个命令是`DELIMITER //`，它与存储过程语法无关。 `DELIMITER`语句将标准分隔符 - 分号(`;`)更改为：`//`。 在这种情况下，分隔符从分号(`;`)更改为双斜杠`//`。为什么我们必须更改分隔符？ 因为我们想将存储过程作为整体传递给服务器，而不是让mysql工具一次解释每个语句。 在`END`关键字之后，使用分隔符`//`来指示存储过程的结束。 最后一个命令(`DELIMITER;`)将分隔符更改回分号(`;`)。
- 使用`CREATE PROCEDURE`语句创建一个新的存储过程。在`CREATE PROCEDURE`语句之后指定存储过程的名称。在这个示例中，存储过程的名称为：`GetAllProducts`，并把括号放在存储过程的名字之后。
- `BEGIN`和`END`之间的部分称为存储过程的主体。将声明性SQL语句放在主体中以处理业务逻辑。 在这个存储过程中，我们使用一个简单的[SELECT语句](http://www.yiibai.com/mysql/select-statement-query-data.html)来查询`products`表中的数据。

在mysql客户端工具中编写存储过程非常繁琐，特别是当存储过程复杂时。 大多数用于MySQL的GUI工具允许您通过直观的界面创建新的存储过程。

例如，在*MySQL Workbench*中，您可以如下创建一个新的存储过程：

**首先**，右键单击*Stored Procedures…*并选择“*Create Stored Procedure…*”菜单项。

![img](http://www.yiibai.com/uploads/images/201707/3107/317110813_81187.png)

**接下来**，编写存储过程代码，然后单击*Apply*按钮

```sql
CREATE PROCEDURE `yiibaidb`.`GetAllProducts`()
BEGIN
    SELECT * FROM yiibaidb.products;
END
SQL
```

**然后**，您可以在MySQL将其存储在数据库中之前查看代码。如果一切都没有问题，点击*Apply*按钮。如下所示 - 

![img](http://www.yiibai.com/uploads/images/201707/3107/878110827_59307.png)

**之后**，MySQL将存储过程编译并放入数据库目录中; 单击*Fished*按钮完成。

![img](http://www.yiibai.com/uploads/images/201707/3107/187110828_59329.png)

最后，可以在`yiibaidb`数据库的例程下看到上面所创建的新存储过程。如下图所示 - 

![img](http://www.yiibai.com/uploads/images/201707/3107/341110830_49828.png)

到此，我们已经成功地创建了一个存储过程。下面我们将学习如何使用它。

### 调用存储过程

要调用存储过程，可以使用以下SQL命令：

```sql
CALL STORED_PROCEDURE_NAME();
SQL
```

使用`CALL`语句调用存储过程，例如调用`GetAllProducts()`存储过程，则使用以下语句：

```sql
CALL GetAllProducts();
SQL
```

如果您执行上述语句，将查询获得`products`表中的所有产品。如下图所示 - 

![img](http://www.yiibai.com/uploads/images/201707/3107/226110838_14953.png)

在本教程中，您已经学习了如何使用`CREATE PROCEDURE`语句编写一个简单的存储过程，并使用`CALL`语句从SQL语句中调用它。

## 存储过程的变量

在本教程中，您将了解和学习存储过程中的变量，包括如何声明和使用变量。此外，您将了解变量的作用域(范围)。

变量是一个命名数据对象，变量的值可以在存储过程执行期间更改。我们通常使用[存储过程](http://www.yiibai.com/mysql/stored-procedure.html)中的变量来保存直接/间接结果。 这些变量是存储过程的本地变量。

> 注意：变量必须先声明后，才能使用它。

### 声明变量

要在存储过程中声明一个变量，可以使用`DECLARE`语句，如下所示：

```sql
DECLARE variable_name datatype(size) DEFAULT default_value;
SQL
```

下面来更详细地解释上面的语句：

- *首先*，在`DECLARE`关键字后面要指定变量名。变量名必须遵循MySQL表列名称的命名规则。
- *其次*，指定变量的数据类型及其大小。变量可以有任何[MySQL数据类型](http://www.yiibai.com/mysql/data-types.html)，如`INT`，`VARCHAR`，`DATETIME`等。
- **第三**，当声明一个变量时，它的初始值为`NULL`。但是可以使用`DEFAULT`关键字为变量分配默认值。

例如，可以声明一个名为`total_sale`的变量，数据类型为`INT`，默认值为`0`，如下所示：

```sql
DECLARE total_sale INT DEFAULT 0;
SQL
```

MySQL允许您使用单个`DECLARE`语句声明共享相同数据类型的两个或多个变量，如下所示：

```sql
DECLARE x, y INT DEFAULT 0;
SQL
```

我们声明了两个整数变量`x`和`y`，并将其默认值设置为`0`。

### 分配变量值

当声明了一个变量后，就可以开始使用它了。要为变量分配一个值，可以使用`SET`语句，例如：

```sql
DECLARE total_count INT DEFAULT 0;
SET total_count = 10;
SQL
```

上面语句中，分配`total_count`变量的值为`10`。

除了`SET`语句之外，还可以使用`SELECT INTO`语句将查询的结果分配给一个变量。 请参阅以下示例：

```sql
DECLARE total_products INT DEFAULT 0

SELECT COUNT(*) INTO total_products
FROM products
SQL
```

在上面的例子中：

- *首先*，声明一个名为`total_products`的变量，并将其值初始化为`0`。
- *然后*，使用`SELECT INTO`语句来分配值给`total_products`变量，从[示例数据库(yiibaidb)](http://www.yiibai.com/mysql/sample-database.html)中的`products`表中选择的产品数量。

### 变量范围(作用域)

一个变量有自己的范围(作用域)，它用来定义它的生命周期。 如果在存储过程中声明一个变量，那么当达到存储过程的`END`语句时，它将超出范围，因此在其它代码块中无法访问。

如果您在`BEGIN END`块内声明一个变量，那么如果达到`END`，它将超出范围。 可以在不同的作用域中声明具有相同名称的两个或多个变量，因为变量仅在自己的作用域中有效。 但是，在不同范围内声明具有相同名称的变量不是很好的编程习惯。

以`@`符号开头的变量是会话变量。直到会话结束前它可用和可访问。

在本教程中，我们向您展示了如何在存储过程中声明变量，并讨论了变量的范围(作用域)。

## 存储过程参数

在本教程中，您将学习如何编写具有参数的MySQL[存储过程](http://www.yiibai.com/mysql/stored-procedure.html)。还将通过几个存储过程示例来了解不同类型的参数。

### MySQL存储过程参数简介

在现实应用中，开发的存储过程几乎都需要参数。这些参数使存储过程更加灵活和有用。 在MySQL中，参数有三种模式：`IN`，`OUT`或`INOUT`。

- `IN` - 是默认模式。在存储过程中定义`IN`参数时，调用程序必须将参数传递给存储过程。 另外，`IN`参数的值被保护。这意味着即使在存储过程中更改了`IN`参数的值，在存储过程结束后仍保留其原始值。换句话说，存储过程只使用`IN`参数的副本。
- `OUT` - 可以在存储过程中更改`OUT`参数的值，并将其更改后新值传递回调用程序。请注意，存储过程在启动时无法访问`OUT`参数的初始值。
- `INOUT` - `INOUT`参数是`IN`和`OUT`参数的组合。这意味着调用程序可以传递参数，并且存储过程可以修改`INOUT`参数并将新值传递回调用程序。

在存储过程中定义参数的语法如下：

```sql
MODE param_name param_type(param_size)
SQL
```

上面语法说明如下 - 

- 根据存储过程中参数的目的，`MODE`可以是`IN`，`OUT`或`INOUT`。
- `param_name`是参数的名称。参数的名称必须遵循MySQL中列名的命名规则。
- 在参数名之后是它的数据类型和大小。和[变量](http://www.yiibai.com/variables-in-stored-procedures.html)一样，参数的数据类型可以是任何有效的[MySQL数据类型](http://www.yiibai.com/mysql/data-types.html)。

如果存储过程有多个参数，则每个参数由逗号(`,`)分隔。

让我们练习一些例子来更好的理解。 我们将使用[示例数据库(yiibaidb)](http://www.yiibai.com/mysql/sample-database.html)中的表进行演示。

### MySQL存储过程参数示例

**1.IN参数示例**

以下示例说明如何使用`GetOfficeByCountry`存储过程中的`IN`参数来查询选择位于特定国家/地区的办公室。

```sql
USE `yiibaidb`;
DROP procedure IF EXISTS `GetOfficeByCountry`;

DELIMITER $$
USE `yiibaidb`$$
CREATE PROCEDURE GetOfficeByCountry(IN countryName VARCHAR(255))
 BEGIN
 SELECT * 
 FROM offices
 WHERE country = countryName;
 END$$

DELIMITER ;
SQL
```

`countryName`是存储过程的IN参数。在存储过程中，我们查询位于`countryName`参数指定的国家/地区的所有办公室。

假设我们想要查询在美国(`USA`)的所有办事处，我们只需要将一个值(`USA`)传递给存储过程，如下所示：

```sql
CALL GetOfficeByCountry('USA');
SQL
```

执行上面查询语句，得到以下结果 - 

![img](http://www.yiibai.com/uploads/images/201708/0108/259140810_85981.png)

要在法国获得所有办事处，我们将`France`字符串传递给`GetOfficeByCountry`存储过程，如下所示：

```sql
CALL GetOfficeByCountry('France')
SQL
```

**2.OUT参数示例**

以下存储过程通过订单状态返回订单数量。它有两个参数：

- `orderStatus`：`IN`参数，它是要对订单计数的订单状态。
- `total`：存储指定订单状态的订单数量的`OUT`参数。

以下是`CountOrderByStatus`存储过程的源代码。

```sql
USE `yiibaidb`;
DROP procedure IF EXISTS `CountOrderByStatus`;

DELIMITER $$
CREATE PROCEDURE CountOrderByStatus(
 IN orderStatus VARCHAR(25),
 OUT total INT)
BEGIN
 SELECT count(orderNumber)
 INTO total
 FROM orders
 WHERE status = orderStatus;
END$$
DELIMITER ;
SQL
```

要获取发货订单的数量，我们调用`CountOrderByStatus`存储过程，并将订单状态传递为已发货，并传递参数(`@total`)以获取返回值。

```sql
CALL CountOrderByStatus('Shipped',@total);
SELECT @total;
SQL
```

执行上面查询语句后，得到以下结果 - 

```sql
+--------+
| @total |
+--------+
|    303 |
+--------+
1 row in set
SQL
```

要获取正在处理的订单数量，调用`CountOrderByStatus`存储过程，如下所示：

执行上面查询语句后，得到以下结果 - 

```sql
+------------------+
| total_in_process |
+------------------+
|                7 |
+------------------+
1 row in set
SQL
```

### INOUT参数示例

以下示例演示如何在存储过程中使用`INOUT`参数。如下查询语句 - 

```sql
DELIMITER $$
CREATE PROCEDURE set_counter(INOUT count INT(4),IN inc INT(4))
BEGIN
 SET count = count + inc;
END$$
DELIMITER ;
SQL
```

上面查询语句是如何运行的？

- `set_counter`存储过程接受一个`INOUT`参数(`count`)和一个`IN`参数(`inc`)。
- 在存储过程中，通过`inc`参数的值增加计数器(`count`)。

下面来看看如何调用`set_counter`存储过程：

```sql
SET @counter = 1;
CALL set_counter(@counter,1); -- 2
CALL set_counter(@counter,1); -- 3
CALL set_counter(@counter,5); -- 8
SELECT @counter; -- 8
SQL
```

在本教程中，我们向您展示了如何在存储过程中定义参数，并介绍了不同的参数模式：`IN`，`OUT`和`INOUT`。

## 存储过程返回多个值

在本教程中，您将学习如何编写/开发返回多个值的存储过程。

[MySQL存储函数](http://www.yiibai.com/mysql/stored-function.html)只返回一个值。要开发返回多个值的[存储过程](http://www.yiibai.com/mysql/stored-procedure.html)，需要使用带有`INOUT`或`OUT`参数的存储过程。

如果您不熟悉`INPUT`或`OUT`参数的用法，请查看[存储过程参数教程](http://www.yiibai.com/mysql/stored-procedures-parameters.html)的详细信息。

### 返回多个值的存储过程示例

我们来看看[示例数据库(yiibaidb)](http://www.yiibai.com/mysql/sample-database.html)中的`orders`表。

```sql
mysql> desc orders;
+----------------+-------------+------+-----+---------+-------+
| Field          | Type        | Null | Key | Default | Extra |
+----------------+-------------+------+-----+---------+-------+
| orderNumber    | int(11)     | NO   | PRI | NULL    |       |
| orderDate      | date        | NO   |     | NULL    |       |
| requiredDate   | date        | NO   |     | NULL    |       |
| shippedDate    | date        | YES  |     | NULL    |       |
| status         | varchar(15) | NO   |     | NULL    |       |
| comments       | text        | YES  |     | NULL    |       |
| customerNumber | int(11)     | NO   | MUL | NULL    |       |
+----------------+-------------+------+-----+---------+-------+
7 rows in set
SQL
```

以下存储过程接受客户编号，并返回发货(*shipped*)，取消(*canceled*)，解决(*resolved*)和争议(*disputed*)的订单总数。

```sql
DELIMITER $$

CREATE PROCEDURE get_order_by_cust(
 IN cust_no INT,
 OUT shipped INT,
 OUT canceled INT,
 OUT resolved INT,
 OUT disputed INT)
BEGIN
 -- shipped
 SELECT
            count(*) INTO shipped
        FROM
            orders
        WHERE
            customerNumber = cust_no
                AND status = 'Shipped';

 -- canceled
 SELECT
            count(*) INTO canceled
        FROM
            orders
        WHERE
            customerNumber = cust_no
                AND status = 'Canceled';

 -- resolved
 SELECT
            count(*) INTO resolved
        FROM
            orders
        WHERE
            customerNumber = cust_no
                AND status = 'Resolved';

 -- disputed
 SELECT
            count(*) INTO disputed
        FROM
            orders
        WHERE
            customerNumber = cust_no
                AND status = 'Disputed';

END
SQL
```

除`IN`参数之外，存储过程还需要`4`个额外的`OUT`参数：`shipped`, `canceled`, `resolved` 和 `disputed`。 在存储过程中，使用带有[COUNT函数](http://www.yiibai.com/mysql/count.html)的[SELECT语句](http://www.yiibai.com/mysql/select-statement-query-data.html)根据订单状态获取相应的订单总数，并将其分配给相应的参数。

要使用`get_order_by_cust`存储过程，可以传递客户编号和四个用户定义的变量来获取输出值。

执行存储过程后，使用`SELECT`语句输出变量值。

```sql
+----------+-----------+-----------+-----------+
| @shipped | @canceled | @resolved | @disputed |
+----------+-----------+-----------+-----------+
|       22 |         0 |         1 |         1 |
+----------+-----------+-----------+-----------+
1 row in set
SQL
```

### 从PHP调用返回多个值的存储过程

以下代码片段显示如何从[PHP](http://www.yiibai.com/php/)程序中调用返回多个值的存储过程。

```php
<?php
/**
 * Call stored procedure that return multiple values
 * @param $customerNumber
 */
function call_sp($customerNumber)
{
    try {
        $pdo = new PDO("mysql:host=localhost;dbname=yiibaidb", 'root', '123456');

        // execute the stored procedure
        $sql = 'CALL get_order_by_cust(:no,@shipped,@canceled,@resolved,@disputed)';
        $stmt = $pdo->prepare($sql);

        $stmt->bindParam(':no', $customerNumber, PDO::PARAM_INT);
        $stmt->execute();
        $stmt->closeCursor();

        // execute the second query to get values from OUT parameter
        $r = $pdo->query("SELECT @shipped,@canceled,@resolved,@disputed")
                  ->fetch(PDO::FETCH_ASSOC);
        if ($r) {
            printf('Shipped: %d, Canceled: %d, Resolved: %d, Disputed: %d',
                $r['@shipped'],
                $r['@canceled'],
                $r['@resolved'],
                $r['@disputed']);
        }
    } catch (PDOException $pe) {
        die("Error occurred:" . $pe->getMessage());
    }
}

call_sp(141);
PHP
```

在`@`符号之前的用户定义的变量与数据库连接相关联，因此它们可用于在调用之间进行访问。

在本教程中，我们向您展示了如何编写/开发返回多个值的存储过程以及如何从`PHP`调用它。

## if语句

在本教程中，您将学习如何使用MySQL `IF`语句来根据条件执行一个SQL代码块。

MySQL `IF`语句允许您根据表达式的某个条件或值结果来执行一组SQL语句。 要在MySQL中形成一个表达式，可以结合文字，[变量](http://www.yiibai.com/mysql/variables-in-stored-procedures.html)，运算符，甚至函数来组合。表达式可以返回`TRUE`,`FALSE`或`NULL`，这三个值之一。

> 请注意，有一个[IF函数](http://www.yiibai.com/mysql/if-function.html)与本教程中指定的`IF`语句是不同的。

### MySQL IF语句语法

下面说明了`IF`语句的语法：

```sql
IF expression THEN 
   statements;
END IF;
SQL
```

如果表达式(`expression`)计算结果为`TRUE`，那么将执行`statements`语句，否则控制流将传递到`END IF`之后的下一个语句。

以下流程图演示了`IF`语句的执行过程：

![img](http://www.yiibai.com/uploads/images/201708/0908/631190840_68526.jpg)

### MySQL IF ELSE语句

如果表达式计算结果为`FALSE`时执行语句，请使用`IF ELSE`语句，如下所示：

```sql
IF expression THEN
   statements;
ELSE
   else-statements;
END IF;
SQL
```

以下流程图说明了`IF ELSE`语句的执行过程：

![img](http://www.yiibai.com/uploads/images/201708/0908/378190849_92477.jpg)

### MySQL IF ELSEIF ELSE语句

如果要基于多个表达式有条件地执行语句，则使用`IF ELSEIF ELSE`语句如下：

```sql
IF expression THEN
   statements;
ELSEIF elseif-expression THEN
   elseif-statements;
...
ELSE
   else-statements;
END IF;
SQL
```

如果表达式(`expression`)求值为`TRUE`，则`IF`分支中的语句(`statements`)将执行；如果表达式求值为`FALSE`，则如果`elseif_expression`的计算结果为`TRUE`，MySQL将执行`elseif-expression`，否则执行`ELSE`分支中的`else-statements`语句。具体流程如下 - 

![img](http://www.yiibai.com/uploads/images/201708/0908/233080853_60470.jpg)

### MySQL IF语句示例

以下示例说明如何使用`IF ESLEIF ELSE`语句，`GetCustomerLevel()`存储过程接受客户编号和客户级别的两个参数。

*首先*，它从`customers`表中获得信用额度

然后，根据信用额度，它决定客户级别：`PLATINUM` , `GOLD` 和 `SILVER` 。

参数`p_customerlevel`存储客户的级别，并由调用程序使用。

```sql
USE yiibaidb;

DELIMITER $$

CREATE PROCEDURE GetCustomerLevel(
    in  p_customerNumber int(11), 
    out p_customerLevel  varchar(10))
BEGIN
    DECLARE creditlim double;

    SELECT creditlimit INTO creditlim
    FROM customers
    WHERE customerNumber = p_customerNumber;

    IF creditlim > 50000 THEN
 SET p_customerLevel = 'PLATINUM';
    ELSEIF (creditlim <= 50000 AND creditlim >= 10000) THEN
        SET p_customerLevel = 'GOLD';
    ELSEIF creditlim < 10000 THEN
        SET p_customerLevel = 'SILVER';
    END IF;

END$$
SQL
```

以下流程图演示了确定客户级别的逻辑 - 

![img](http://www.yiibai.com/uploads/images/201708/0908/244080857_22798.png)

在本教程中，您已经学会了如何使用MySQL `IF`语句根据条件执行一个SQL代码块。

## CASE语句

在本教程中，您将学习如何使用MySQL `CASE`语句在存储的程序中构造复杂的条件语句。

除了[IF语句](http://www.yiibai.com/mysql/if-statement.html)，MySQL提供了一个替代的条件语句`CASE`。 MySQL `CASE`语句使代码更加可读和高效。

`CASE`语句有两种形式：简单的搜索`CASE`语句。

### 简单CASE语句

我们来看一下简单`CASE`语句的语法：

```sql
CASE  case_expression
   WHEN when_expression_1 THEN commands
   WHEN when_expression_2 THEN commands
   ...
   ELSE commands
END CASE;
SQL
```

您可以使用简单`CASE`语句来检查表达式的值与一组唯一值的匹配。

`case_expression`可以是任何有效的表达式。我们将`case_expression`的值与每个`WHEN`子句中的`when_expression`进行比较，例如`when_expression_1`，`when_expression_2`等。如果`case_expression`和`when_expression_n`的值相等，则执行相应的`WHEN`分支中的命令(`commands`)。

如果`WHEN`子句中的`when_expression`与`case_expression`的值匹配，则`ELSE`子句中的命令将被执行。`ELSE`子句是可选的。 如果省略`ELSE`子句，并且找不到匹配项，MySQL将引发错误。

以下示例说明如何使用简单的`CASE`语句：

```sql
DELIMITER $$

CREATE PROCEDURE GetCustomerShipping(
 in  p_customerNumber int(11), 
 out p_shiping        varchar(50))
BEGIN
    DECLARE customerCountry varchar(50);

    SELECT country INTO customerCountry
 FROM customers
 WHERE customerNumber = p_customerNumber;

    CASE customerCountry
 WHEN  'USA' THEN
    SET p_shiping = '2-day Shipping';
 WHEN 'Canada' THEN
    SET p_shiping = '3-day Shipping';
 ELSE
    SET p_shiping = '5-day Shipping';
 END CASE;

END$$
SQL
```

上面存储过程是如何工作的？

- `GetCustomerShipping`存储过程接受客户编号作为`IN`[参数](http://www.yiibai.com/mysql/stored-procedures-parameters.html)，并根据客户所在国家返回运送时间。
- 在存储过程中，首先，我们根据输入的客户编号得到客户的国家。然后使用简单`CASE`语句来比较客户的国家来确定运送期。如果客户位于美国(`USA`)，则运送期为`2`天。 如果客户在加拿大，运送期为`3`天。 来自其他国家的客户则需要`5`天的运输时间。

以下流程图显示了确定运输时间的逻辑。

![img](http://www.yiibai.com/uploads/images/201708/0108/881150854_21504.png)

以下是上述存储过程的测试脚本：

```sql
SET @customerNo = 112;

SELECT country into @country
FROM customers
WHERE customernumber = @customerNo;

CALL GetCustomerShipping(@customerNo,@shipping);

SELECT @customerNo AS Customer,
       @country    AS Country,
       @shipping   AS Shipping;
SQL
```

执行上面代码，得到以下结果 - 

```shell
+----------+---------+----------------+
| Customer | Country | Shipping       |
+----------+---------+----------------+
|      112 | USA     | 2-day Shipping |
+----------+---------+----------------+
1 row in set
Shell
```

### 可搜索CASE语句

简单`CASE`语句仅允许您将表达式的值与一组不同的值进行匹配。 为了执行更复杂的匹配，如范围，您可以使用可搜索`CASE`语句。 可搜索`CASE`语句等同于`IF`语句，但是它的构造更加可读。

以下说明可搜索`CASE`语句的语法：

```sql
CASE
    WHEN condition_1 THEN commands
    WHEN condition_2 THEN commands
    ...
    ELSE commands
END CASE;
SQL
```

MySQL评估求值`WHEN`子句中的每个条件，直到找到一个值为`TRUE`的条件，然后执行`THEN`子句中的相应命令(`commands`)。

如果没有一个条件为`TRUE`，则执行`ELSE`子句中的命令(`commands`)。如果不指定`ELSE`子句，并且没有一个条件为`TRUE`，MySQL将发出错误消息。

MySQL不允许在`THEN`或`ELSE`子句中使用空的命令。 如果您不想处理`ELSE`子句中的逻辑，同时又要防止MySQL引发错误，则可以在`ELSE`子句中放置一个空的`BEGIN END`块。

以下示例演示如何使用可搜索`CASE`语句来根据客户的信用额度来查找客户级：`SILVER`，`GOLD`或`PLATINUM`。

```sql
DELIMITER $$

CREATE PROCEDURE GetCustomerLevel(
 in  p_customerNumber int(11), 
 out p_customerLevel  varchar(10))
BEGIN
    DECLARE creditlim double;

    SELECT creditlimit INTO creditlim
 FROM customers
 WHERE customerNumber = p_customerNumber;

    CASE  
 WHEN creditlim > 50000 THEN 
    SET p_customerLevel = 'PLATINUM';
 WHEN (creditlim <= 50000 AND creditlim >= 10000) THEN
    SET p_customerLevel = 'GOLD';
 WHEN creditlim < 10000 THEN
    SET p_customerLevel = 'SILVER';
 END CASE;

END$$
SQL
```

在上面查询语句逻辑中，如果信用额度是：

- 大于`50K`，则客户是`PLATINUM`客户。
- 小于`50K`，大于`10K`，则客户是`GOLD`客户。
- 小于`10K`，那么客户就是`SILVER`客户。

我们可以通过执行以下测试脚本来测试存储过程：

```sql
CALL GetCustomerLevel(112,@level);
SELECT @level AS 'Customer Level';
SQL
```

执行上面查询语句，得到以下结果 - 

```sql
+----------------+
| Customer Level |
+----------------+
| PLATINUM       |
+----------------+
1 row in set
SQL
```

在本教程中，我们向您展示了如何使用两种形式的MySQL `CASE`语句，包括简单`CASE`语句和可搜索`CASE`语句。

## 使用IF和CASE语句的技巧

在本教程中，我们将给您一些技巧，以便您在存储过程中在什么情况分别选择`IF`和`CASE`语句。

MySQL提供`IF`和CA`SE`语句，使您能够根据某些条件(称为流控制)执行一个SQL代码块。那么您应该使用什么语句？ 对于大多数开发人员，在`IF`和`CASE`之间进行选择只是个人偏好的问题。但是，当您决定使用`IF`或`CASE`时，应该考虑以下几点：

- 当将单个表达式与唯一值的范围进行比较时，[简单CASE语句](http://www.yiibai.com/mysql/case-statement.html)比[IF语句](http://www.yiibai.com/mysql/if-statement.html)更易读。另外，简单`CASE`语句比`IF`语句更有效率。
- 当您根据多个值检查复杂表达式时，`IF`语句更容易理解。
- 如果您选择使用`CASE`语句，则必须确保至少有一个`CASE`条件匹配。否则，需要定义一个[错误处理](http://www.yiibai.com/mysql/error-handling-in-stored-procedures.html)程序来捕获错误。`IF`语句则不需要处理错误。
- 在大多数组织(公司)中，总是有一些所谓的开发指导文件，为开发人员提供了编程风格的命名约定和指导，那么您应参考本文档并遵循开发实践。
- 在某些情况下，`IF`和`CASE`混合使用反而使您的存储过程更加可读和高效。

## 存储过程循环

在本教程中，将学习如何使用各种MySQL循环语句(包括`WHILE`，`REPEAT`和`LOOP`)来根据条件反复运行代码块。

MySQL提供循环语句，允许您根据条件重复执行一个SQL代码块。 MySQL中有三个循环语句：`WHILE`，`REPEAT`和`LOOP`。

我们将在以下部分中更详细地检查每个循环语句。

### WHILE循环

`WHILE`语句的语法如下：

```sql
WHILE expression DO
   statements
END WHILE
SQL
```

`WHILE`循环在每次迭代开始时检查表达式。 如果`expressionevaluates`为`TRUE`，MySQL将执行`WHILE`和`END WHILE`之间的语句，直到`expressionevaluates`为`FALSE`。 `WHILE`循环称为预先测试条件循环，因为它总是在执行前检查语句的表达式。

下面的流程图说明了`WHILE`循环语句：

![img](http://www.yiibai.com/uploads/images/201708/0108/535160856_72166.jpg)

以下是在存储过程中使用`WHILE`循环语句的示例：

```sql
DELIMITER $$
 DROP PROCEDURE IF EXISTS test_mysql_while_loop$$
 CREATE PROCEDURE test_mysql_while_loop()
 BEGIN
 DECLARE x  INT;
 DECLARE str  VARCHAR(255);

 SET x = 1;
 SET str =  '';

 WHILE x  <= 5 DO
 SET  str = CONCAT(str,x,',');
 SET  x = x + 1; 
 END WHILE;

 SELECT str;
 END$$
DELIMITER ;
SQL
```

在上面的`test_mysql_while_loop`存储过程中：

- 首先，重复构建`str`字符串，直到`x`[变量](http://www.yiibai.com/mysql/variables-in-stored-procedures.html)的值大于`5`。
- 然后，使用[SELECT语句](http://www.yiibai.com/mysql/select-statement-query-data.html)显示最终的字符串。

*要注意*，如果不初始化`x`变量的值，那么它默认值为`NULL`。 因此，`WHILE`循环语句中的条件始终为`TRUE`，并且您将有一个不确定的循环，这是不可预料的。

下面来测试`test_mysql_while_loopstored`调用存储过程：

```sql
CALL test_mysql_while_loop();
SQL
```

执行上面查询语句，得到以下结果 - 

```sql
mysql> CALL test_mysql_while_loop();
+------------+
| str        |
+------------+
| 1,2,3,4,5, |
+------------+
1 row in set

Query OK, 0 rows affected
SQL
```

### REPEAT循环

`REPEAT`循环语句的语法如下：

```sql
REPEAT
 statements;
UNTIL expression
END REPEAT
SQL
```

首先，MySQL执行语句，然后评估求值表达式(`expression`)。如果表达式(`expression`)的计算结果为`FALSE`，则MySQL将重复执行该语句，直到该表达式计算结果为`TRUE`。

因为`REPEAT`循环语句在执行语句后检查表达式(`expression`)，因此`REPEAT`循环语句也称为测试后循环。

下面的流程图说明了`REPEAT`循环语句的执行过程：

![img](http://www.yiibai.com/uploads/images/201708/0108/786170809_61243.jpg)

我们可以使用`REPEAT`循环语句重写`test_mysql_while_loop`存储过程，使用`WHILE`循环语句：

```sql
DELIMITER $$
 DROP PROCEDURE IF EXISTS mysql_test_repeat_loop$$
 CREATE PROCEDURE mysql_test_repeat_loop()
 BEGIN
 DECLARE x INT;
 DECLARE str VARCHAR(255);

 SET x = 1;
        SET str =  '';

 REPEAT
 SET  str = CONCAT(str,x,',');
 SET  x = x + 1; 
        UNTIL x  > 5
        END REPEAT;

        SELECT str;
 END$$
DELIMITER ;
SQL
```

要注意的是`UNTIL`表达式中没有分号(`;`)。

执行上面查询语句，得到以下结果 - 

```sql
mysql> CALL mysql_test_repeat_loop();
+------------+
| str        |
+------------+
| 1,2,3,4,5, |
+------------+
1 row in set

Query OK, 0 rows affected
SQL
```

### LOOP，LEAVE和ITERATE语句

有两个语句允许您用于控制循环：

- `LEAVE`语句用于立即退出循环，而无需等待检查条件。`LEAVE`语句的工作原理就类似[PHP](http://www.yiibai.com/php/)，`C/C++`，[Java](http://www.yiibai.com/java/)等其他语言的`break`语句一样。
- `ITERATE`语句允许您跳过剩下的整个代码并开始新的迭代。`ITERATE`语句类似于`PHP`，`C/C++`，`Java`等中的`continue`语句。

MySQL还有一个`LOOP`语句，它可以反复执行一个代码块，另外还有一个使用循环标签的灵活性。

以下是使用`LOOP`循环语句的示例。

```sql
CREATE PROCEDURE test_mysql_loop()
 BEGIN
 DECLARE x  INT;
        DECLARE str  VARCHAR(255);

 SET x = 1;
        SET str =  '';

 loop_label:  LOOP
 IF  x > 10 THEN 
 LEAVE  loop_label;
 END  IF;

 SET  x = x + 1;
 IF (x mod 2) THEN
     ITERATE  loop_label;
 ELSE
    SET  str = CONCAT(str,x,',');
 END IF;
    END LOOP;    
    SELECT str;
END;
SQL
```

- 以上存储过程仅构造具有偶数字符串的字符串，例如`2`,`4`,`6`等。
- 在`LOOP`语句之前放置一个`loop_label`循环标签。
- 如果`x`的值大于`10`，则由于`LEAVE`语句，循环被终止。
- 如果`x`的值是一个奇数，`ITERATE`语句忽略它下面的所有内容，并开始一个新的迭代。
- 如果`x`的值是偶数，则`ELSE`语句中的块将使用偶数构建字符串。

执行上面查询语句，得到以下结果 - 

```sql
mysql> CALL test_mysql_loop();
+-------------+
| str         |
+-------------+
| 2,4,6,8,10, |
+-------------+
1 row in set

Query OK, 0 rows affected
SQL
```

在本教程中，您学习了基于条件重复执行代码块的各种MySQL循环语句。

## MySQL游标

在本教程中，您将学习如何在存储过程中使用MySQL游标来遍历`SELECT`语句返回的结果集。

### MySQL游标简介

要处理[存储过程](http://www.yiibai.com/mysql/stored-procedure.html)中的结果集，请使用游标。游标允许您迭代查询返回的一组行，并相应地处理每行。

MySQL游标为只读，不可滚动和敏感。

- **只读**：无法通过光标更新基础表中的数据。
- **不可滚动**：只能按照[SELECT](http://www.yiibai.com/mysql/select-statement-query-data.html)语句确定的顺序获取行。不能以相反的顺序获取行。 此外，不能跳过行或跳转到结果集中的特定行。
- **敏感**：有两种游标：敏感游标和不敏感游标。敏感游标指向实际数据，不敏感游标使用数据的临时副本。敏感游标比一个不敏感的游标执行得更快，因为它不需要临时拷贝数据。但是，对其他连接的数据所做的任何更改都将影响由敏感游标使用的数据，因此，如果不更新敏感游标所使用的数据，则更安全。 MySQL游标是敏感的。

您可以在[存储过程](http://www.yiibai.com/mysql/stored-procedure.html)，[存储函数](http://www.yiibai.com/mysql/stored-function.html)和[触发器](http://www.yiibai.com/mysql/triggers.html)中使用MySQL游标。

### 使用MySQL游标

首先，必须使用`DECLARE`语句声明游标：

```sql
DECLARE cursor_name CURSOR FOR SELECT_statement;
SQL
```

游标声明必须在[变量](http://www.yiibai.com/mysql/variables-in-stored-procedures.html)声明之后。如果在变量声明之前声明游标，MySQL将会发出一个错误。游标必须始终与`SELECT`语句相关联。

接下来，使用`OPEN`语句打开游标。`OPEN`语句初始化游标的结果集，因此您必须在从结果集中提取行之前调用`OPEN`语句。

```sql
OPEN cursor_name;
SQL
```

然后，使用`FETCH`语句来检索光标指向的下一行，并将光标移动到结果集中的下一行。

```sql
FETCH cursor_name INTO variables list;
SQL
```

之后，可以检查是否有任何行记录可用，然后再提取它。

最后，调用`CLOSE`语句来停用光标并释放与之关联的内存，如下所示：

```sql
CLOSE cursor_name;
SQL
```

当光标不再使用时，应该关闭它。

当使用MySQL游标时，还必须声明一个`NOT FOUND`处理程序来处理当游标找不到任何行时的情况。 因为每次调用`FETCH`语句时，游标会尝试读取结果集中的下一行。 当光标到达结果集的末尾时，它将无法获得数据，并且会产生一个条件。 处理程序用于处理这种情况。

要声明一个`NOT FOUND`处理程序，参考以下语法：

```sql
DECLARE CONTINUE HANDLER FOR NOT FOUND SET finished = 1;
SQL
```

`finished`是一个变量，指示光标到达结果集的结尾。请注意，处理程序声明必须出现在存储过程中的变量和游标声明之后。

下图说明了MySQL游标如何工作。

![img](http://www.yiibai.com/uploads/images/201708/0108/764170849_82012.png)

### MySQL游标示例

为了更好地演示，我们将开发一个存储过程，来获取MySQL[示例数据库(yiibaidb)](http://www.yiibai.com/mysql/sample-database.html)中`employees`表中所有员工的电子邮件列表。

首先，声明一些变量，一个用于循环员工电子邮件的游标和一个`NOT FOUND`处理程序：

```sql
DECLARE finished INTEGER DEFAULT 0;
DECLARE email varchar(255) DEFAULT "";

-- declare cursor for employee email
DEClARE email_cursor CURSOR FOR 
 SELECT email FROM employees;

-- declare NOT FOUND handler
DECLARE CONTINUE HANDLER 
FOR NOT FOUND SET finished = 1;
SQL
```

接下来，使用`OPEN`语句打开`email_cursor`：

```sql
OPEN email_cursor;
SQL
```

然后，迭代电子邮件列表，并使用分隔符(`;`)[连接](http://www.yiibai.com/mysql/sql-concat-in-mysql.html)每个电子邮件：

```sql
get_email: LOOP
 FETCH email_cursor INTO v_email;
 IF v_finished = 1 THEN 
 LEAVE get_email;
 END IF;
 -- build email list
 SET email_list = CONCAT(v_email,";",email_list);
END LOOP get_email;
SQL
```

之后，在循环中，使用`v_finished`变量来检查列表中是否有任何电子邮件来终止循环。

最后，使用`CLOSE`语句关闭游标：

```sql
CLOSE email_cursor;
SQL
```

`build_email_list`存储过程所有代码如下：

```sql
DELIMITER $$

CREATE PROCEDURE build_email_list (INOUT email_list varchar(4000))
BEGIN

 DECLARE v_finished INTEGER DEFAULT 0;
        DECLARE v_email varchar(100) DEFAULT "";

 -- declare cursor for employee email
 DEClARE email_cursor CURSOR FOR 
 SELECT email FROM employees;

 -- declare NOT FOUND handler
 DECLARE CONTINUE HANDLER 
        FOR NOT FOUND SET v_finished = 1;

 OPEN email_cursor;

 get_email: LOOP

 FETCH email_cursor INTO v_email;

 IF v_finished = 1 THEN 
 LEAVE get_email;
 END IF;

 -- build email list
 SET email_list = CONCAT(v_email,";",email_list);

 END LOOP get_email;

 CLOSE email_cursor;

END$$

DELIMITER ;
SQL
```

可以使用以下脚本测试`build_email_list`存储过程：

```sql
SET @email_list = "";
CALL build_email_list(@email_list);
SELECT @email_list;
SQL
```

> 注：由于内容比较长，这里就不放上输出结果了。

在本教程中，我们向您展示了如何使用MySQL游标来迭代结果集并相应地处理每一行。

## MySQL列出存储过程

在本教程中，我们将向您展示如何列出MySQL数据库中的所有存储过程，并显示存储过程源代码的一些非常有用的语句。

MySQL为提供了一些有用的语句，可以更有效地管理存储过程。这些语句包括列出存储过程并显示存储过程的源代码。

### 显示存储过程字符

要显示存储过程的字符，请使用`SHOW PROCEDURE STATUS`语句如下：

```sql
SHOW PROCEDURE STATUS [LIKE 'pattern' | WHERE expr];
SQL
```

`SHOW PROCEDURE STATUS`语句是对SQL标准的MySQL扩展。此语句提供存储过程的字符，包括数据库，存储过程名称，类型，创建者等。

可以使用[LIKE](http://www.yiibai.com/mysql/sql-like-mysql.html)或[WHERE](http://www.yiibai.com/mysql/where.html)子句根据各种标准过滤出存储过程。

要列出您有权访问的数据库的所有存储过程，请使用`SHOW PROCEDURE STATUS`语句，如下所示：

```sql
SHOW PROCEDURE STATUS;
SQL
```

如果要在特定数据库中显示存储过程，可以在`SHOW PROCEDURE STATUS`语句中使用`WHERE`子句：

```sql
SHOW PROCEDURE STATUS WHERE db = 'yiibaidb';
SQL
```

如果要显示具有特定模式的存储过程，例如，名称包含`product`字符，则可以使用`LIKE`操作符，如以下命令：

```sql
SHOW PROCEDURE STATUS WHERE name LIKE '%product%'
SQL
```

### 显示存储过程的源代码

要显示特定存储过程的源代码，请使用`SHOW CREATE PROCEDURE`语句如下：

```sql
SHOW CREATE PROCEDURE stored_procedure_name
SQL
```

在`SHOW CREATE PROCEDURE`关键字之后指定存储过程的名称。例如，要显示`GetAllProducts`存储过程的代码，请使用以下语句：

```sql
SHOW CREATE PROCEDURE GetAllProducts;
SQL
```

在本教程中，您已经学习了一些有用的语句，包括`SHOW PROCEDURE STATUS`和`SHOW CREATE PROCEDURE`语句，用于列出数据库中的存储过程并获取存储过程的源代码。

## MySQL存储过程错误处理

本教程将向您展示如何使用MySQL处理程序来处理在存储过程中遇到的异常或错误。

当存储过程中发生错误时，重要的是适当处理它，例如：继续或退出当前代码块的执行，并发出有意义的错误消息。

MySQL提供了一种简单的方法来定义处理从一般条件(如警告或异常)到特定条件(例如特定错误代码)的处理程序。

### 声明处理程序

要声明一个处理程序，您可以使用`DECLARE HANDLER`语句如下：

```sql
DECLARE action HANDLER FOR condition_value statement;
SQL
```

如果条件的值与`condition_value`匹配，则MySQL将执行`statement`，并根据该操作继续或退出当前的代码块。

操作(`action`)接受以下值之一：

- `CONTINUE`：继续执行封闭代码块(`BEGIN ... END`)。
- `EXIT`：处理程序声明封闭代码块的执行终止。

`condition_value`指定一个特定条件或一类激活处理程序的条件。`condition_value`接受以下值之一：

- 一个MySQL错误代码。
- 标准`SQLSTATE`值或者它可以是`SQLWARNING`，`NOTFOUND`或`SQLEXCEPTION`条件，这是`SQLSTATE`值类的简写。`NOTFOUND`条件用于游标或`SELECT INTO variable_list`语句。
- 与MySQL错误代码或`SQLSTATE`值相关联的命名条件。

该语句可以是一个简单的语句或由`BEGIN`和`END`关键字包围的复合语句。

### MySQL错误处理示例

我们来看几个声明处理程序的例子。

以下处理程序意味着如果发生错误，则将`has_error`变量的值设置为`1`并继续执行。

```sql
DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET has_error = 1;
SQL
```

以下是另一个处理程序，如果发生错误，回滚上一个操作，发出错误消息，并退出当前代码块。 如果在存储过程的`BEGIN END`块中声明它，则会立即终止存储过程。

```sql
DECLARE EXIT HANDLER FOR SQLEXCEPTION
BEGIN
ROLLBACK;
SELECT 'An error has occurred, operation rollbacked and the stored procedure was terminated';
END;
SQL
```

以下处理程序如果没有更多的行要提取，在[光标](http://www.yiibai.com/mysql/cursor.html)或[SELECT INTO](http://www.yiibai.com/mysql/select-statement-query-data.html)语句的情况下，将`no_row_found`变量的值设置为`1`并继续执行。

```sql
DECLARE CONTINUE HANDLER FOR NOT FOUND SET no_row_found = 1;
SQL
```

以下处理程序如果发生重复的键错误，则会发出MySQL错误`1062`。 它发出错误消息并继续执行。

```sql
DECLARE CONTINUE HANDLER FOR 1062
SELECT 'Error, duplicate key occurred';
SQL
```

### 存储过程中的MySQL处理程序示例

首先，为了更好地演示，我们[创建](http://www.yiibai.com/mysql/create-table.html)一个名为`article_tags`的新表：

```sql
USE testdb;
CREATE TABLE article_tags(
    article_id INT,
    tag_id     INT,
    PRIMARY KEY(article_id,tag_id)
);
SQL
```

`article_tags`表存储文章和标签之间的关系。每篇文章可能有很多标签，反之亦然。 为了简单起见，我们不会在`article_tags`表中创建文章(`article`)表和标签(`tags`)表以及[外键](http://www.yiibai.com/mysql/foreign-key.html)。

接下来，创建一个[存储过程](http://www.yiibai.com/mysql/getting-started-with-mysql-stored-procedures.html)，将文章的`id`和标签的`id`插入到`article_tags`表中：

```sql
USE testdb;
DELIMITER $$

CREATE PROCEDURE insert_article_tags(IN article_id INT, IN tag_id INT)
BEGIN

 DECLARE CONTINUE HANDLER FOR 1062
 SELECT CONCAT('duplicate keys (',article_id,',',tag_id,') found') AS msg;

 -- insert a new record into article_tags
 INSERT INTO article_tags(article_id,tag_id)
 VALUES(article_id,tag_id);

 -- return tag count for the article
 SELECT COUNT(*) FROM article_tags;
END$$
DELIMITER ;
SQL
```

然后，通过调用`insert_article_tags`存储过程，为文章ID为`1`添加标签ID：`1`,`2`和`3`，如下所示：

```sql
CALL insert_article_tags(1,1);
CALL insert_article_tags(1,2);
CALL insert_article_tags(1,3);
SQL
```

之后，尝试插入一个重复的键来检查处理程序是否真的被调用。

```sql
CALL insert_article_tags(1,3);
SQL
```

执行上面查询语句，得到以下结果 - 

```sql
mysql> CALL insert_article_tags(1,3);
+----------------------------+
| msg                        |
+----------------------------+
| duplicate keys (1,3) found |
+----------------------------+
1 row in set

+----------+
| COUNT(*) |
+----------+
|        3 |
+----------+
1 row in set

Query OK, 0 rows affected
SQL
```

执行后会收到一条错误消息。 但是，由于我们将处理程序声明为`CONTINUE`处理程序，所以存储过程继续执行。因此，最后获得了文章的标签计数值为：`3`。

![img](http://www.yiibai.com/uploads/images/201708/0108/473190852_78515.png)

如果将处理程序声明中的`CONTINUE`更改为`EXIT`，那么将只会收到一条错误消息。如下查询语句 - 

```sql
DELIMITER $$

CREATE PROCEDURE insert_article_tags_exit(IN article_id INT, IN tag_id INT)
BEGIN

 DECLARE EXIT HANDLER FOR SQLEXCEPTION 
 SELECT 'SQLException invoked';

 DECLARE EXIT HANDLER FOR 1062 
        SELECT 'MySQL error code 1062 invoked';

 DECLARE EXIT HANDLER FOR SQLSTATE '23000'
 SELECT 'SQLSTATE 23000 invoked';

 -- insert a new record into article_tags
 INSERT INTO article_tags(article_id,tag_id)
   VALUES(article_id,tag_id);

 -- return tag count for the article
 SELECT COUNT(*) FROM article_tags;
END $$
DELIMITER ;
SQL
```

执行上面查询语句，得到以下结果 - 

```sql
mysql> CALL insert_article_tags_exit(1,3);
+-------------------------------+
| MySQL error code 1062 invoked |
+-------------------------------+
| MySQL error code 1062 invoked |
+-------------------------------+
1 row in set

Query OK, 0 rows affected
SQL
```

![img](http://www.yiibai.com/uploads/images/201708/0108/874190855_40181.jpg)

### MySQL处理程序优先级

如果使用多个处理程序来处理错误，MySQL将调用最特定的处理程序来处理错误。

错误总是映射到一个MySQL错误代码，因为在MySQL中它是最具体的。 `SQLSTATE`可以映射到许多MySQL错误代码，因此它不太具体。 `SQLEXCPETION`或`SQLWARNING`是`SQLSTATES`类型值的缩写，因此它是最通用的。

假设在`insert_article_tags_3`存储过程中声明三个处理程序，如下所示：

```sql
DELIMITER $$

CREATE PROCEDURE insert_article_tags_3(IN article_id INT, IN tag_id INT)
BEGIN

 DECLARE EXIT HANDLER FOR 1062 SELECT 'Duplicate keys error encountered';
 DECLARE EXIT HANDLER FOR SQLEXCEPTION SELECT 'SQLException encountered';
 DECLARE EXIT HANDLER FOR SQLSTATE '23000' SELECT 'SQLSTATE 23000';

 -- insert a new record into article_tags
 INSERT INTO article_tags(article_id,tag_id)
 VALUES(article_id,tag_id);

 -- return tag count for the article
 SELECT COUNT(*) FROM article_tags;
END $$
DELIMITER ;
SQL
```

我们尝试通过调用存储过程将重复的键插入到`article_tags`表中：

```sql
CALL insert_article_tags_3(1,3);
SQL
```

如下，可以看到MySQL错误代码处理程序被调用。

```sql
mysql> CALL insert_article_tags_3(1,3);
+----------------------------------+
| Duplicate keys error encountered |
+----------------------------------+
| Duplicate keys error encountered |
+----------------------------------+
1 row in set

Query OK, 0 rows affected
SQL
```

### 使用命名错误条件

从错误处理程序声明开始，如下 - 

```sql
DECLARE EXIT HANDLER FOR 1051 SELECT 'Please create table abc first';
SELECT * FROM abc;
SQL
```

`1051`号是什么意思？ 想象一下，你有一个大的存储过程代码使用了好多类似这样的数字; 这将成为维护代码的噩梦。

幸运的是，MySQL为我们提供了声明一个命名错误条件的`DECLARE CONDITION`语句，它与条件相关联。

`DECLARE CONDITION`语句的语法如下：

```sql
DECLARE condition_name CONDITION FOR condition_value;
SQL
```

`condition_value`可以是MySQL错误代码，例如：`1015`或`SQLSTATE`值。 `condition_value`由`condition_name`表示。

声明后，可以参考`condition_name`，而不是参考`condition_value`。

所以可以重写上面的代码如下：

```sql
DECLARE table_not_found CONDITION for 1051;
DECLARE EXIT HANDLER FOR  table_not_found SELECT 'Please create table abc first';
SELECT * FROM abc;
SQL
```

这段代码比以前的代码显然更可读。

请注意，条件声明必须出现在处理程序或游标声明之前。

## MySQL signal和esignal语句

在本教程中，您将学习如何使用`SIGNAL`和`RESIGNAL`语句来引发存储过程中的错误条件。

### MySQL SIGNAL语句

使用`SIGNAL`语句在存储的程序(例如存储过程，[存储函数](http://www.yiibai.com/mysql/stored-function.html)，[触发器](http://www.yiibai.com/mysql/triggers.html)或[事件](http://www.yiibai.com/mysql/triggers-modifying-mysql-events.html))中向调用者返回错误或警告条件。 `SIGNAL`语句提供了对返回值(如值和消息`SQLSTATE`)的信息的控制。

以下说明`SIGNAL`语句的语法：

```sql
SIGNAL SQLSTATE | condition_name;
SET condition_information_item_name_1 = value_1,
    condition_information_item_name_1 = value_2, etc;
SQL
```

`SIGNAL`关键字是由`DECLARE CONDITION`语句声明的`SQLSTATE`值或条件名称。 请注意，`SIGNAL`语句必须始终指定使用`SQLSTATE`值定义的`SQLSTATE`值或命名条件。

要向调用者提供信息，请使用`SET`子句。如果要使用值返回多个条件信息项名称，则需要用逗号分隔每个名称/值对。

`condition_information_item_name`可以是`MESSAGE_TEXT`，`MYSQL_ERRORNO`，`CURSOR_NAME`等。

以下存储过程将订单行项目添加到现有销售订单中。 如果订单号码不存在，它会发出错误消息。

```sql
DELIMITER $$

CREATE PROCEDURE AddOrderItem(in orderNo int,
 in productCode varchar(45),
 in qty int,in price double, in lineNo int )

BEGIN
 DECLARE C INT;

 SELECT COUNT(orderNumber) INTO C
 FROM orders 
 WHERE orderNumber = orderNo;

 -- check if orderNumber exists
 IF(C != 1) THEN 
 SIGNAL SQLSTATE '45000'
 SET MESSAGE_TEXT = 'Order No not found in orders table';
 END IF;
 -- more code below
 -- ...
END $$
DELIMITER ;
SQL
```

*首先*，它使用传递给存储过程的输入订单号对订单进行计数。
*第二步*，如果订单数不是`1`，它会引发*SQLSTATE 45000*的错误以及`orders`表中不存在订单号的错误消息。

> 请注意，`45000`是一个通用`SQLSTATE`值，用于说明未处理的用户定义异常。

如果调用存储过程`AddOrderItem()`，但是传递不存在的订单号，那么将收到一条错误消息。

```sql
CALL AddOrderItem(10,'S10_1678',1,95.7,1);
SQL
```

执行上面代码，得到以下结果 - 

```sql
mysql> CALL AddOrderItem(10,'S10_1678',1,95.7,1);
1644 - Order No not found in orders table
mysql>
SQL
```

### MySQL RESIGNAL语句

除了`SIGNAL`语句，MySQL还提供了用于引发警告或错误条件的`RESIGNAL`语句。

`RESIGNAL`语句在功能和语法方面与`SIGNAL`语句相似，只是：

- 必须在错误或警告处理程序中使用`RESIGNAL`语句，否则您将收到一条错误消息，指出“`RESIGNAL when handler is not active`”。 请注意，您可以在存储过程中的任何位置使用`SIGNAL`语句。
- 可以省略`RESIGNAL`语句的所有属性，甚至可以省略`SQLSTATE`值。

如果单独使用`RESIGNAL`语句，则所有属性与传递给条件处理程序的属性相同。

以下存储过程在将发送给调用者之前更改错误消息。

```sql
DELIMITER $$

CREATE PROCEDURE Divide(IN numerator INT, IN denominator INT, OUT result double)
BEGIN
 DECLARE division_by_zero CONDITION FOR SQLSTATE '22012';

 DECLARE CONTINUE HANDLER FOR division_by_zero 
 RESIGNAL SET MESSAGE_TEXT = 'Division by zero / Denominator cannot be zero';
 -- 
 IF denominator = 0 THEN
 SIGNAL division_by_zero;
 ELSE
 SET result := numerator / denominator;
 END IF;
END $$
DELIMITER ;
SQL
```

下面我们来调用`Divide()`存储过程。

```sql
CALL Divide(10,0,@result);
SQL
```

执行上面语句，得到以下结果 - 

```sql
mysql> CALL Divide(10,0,@result);
1644 - Division by zero / Denominator cannot be zero
SQL
```

在本教程中，我们向您展示了如何使用`SIGNAL`和`RESIGNAL`语句引发存储程序中的错误条件。

## MySQL存储函数

在本教程中，您将学习如何使用`CREATE FUNCTION`语句创建存储的函数。

存储的函数是返回单个值的特殊类型的存储程序。您使用存储的函数来封装在SQL语句或存储的程序中可重用的常用公式或业务规则。

与[存储过程](http://www.yiibai.com/mysql/stored-procedure.html)不同，您可以在SQL语句中使用存储的函数，也可以在表达式中使用。 这有助于提高程序代码的可读性和可维护性。

### MySQL存储函数语法

以下说明了创建新存储函数的最简单语法：

```sql
CREATE FUNCTION function_name(param1,param2,…)
    RETURNS datatype
   [NOT] DETERMINISTIC
 statements
SQL
```

*首先*，在`CREATE FUNCTION`子句之后指定存储函数的名称。
*其次*，列出括号内存储函数的所有[参数](http://www.yiibai.com/mysql/stored-procedures-parameters.html)。 默认情况下，所有参数均为`IN`参数。不能为参数指定`IN`，`OUT`或`INOUT`修饰符。
*第三*，必须在`RETURNS`语句中指定返回值的数据类型。它可以是任何有效的[MySQL数据类型](http://www.yiibai.com/mysql/data-types.html)。
*第四*，对于相同的输入参数，如果存储的函数返回相同的结果，这样则被认为是确定性的，否则存储的函数不是确定性的。必须决定一个存储函数是否是确定性的。 如果您声明不正确，则存储的函数可能会产生意想不到的结果，或者不使用可用的优化，从而降低性能。
*第五*，将代码写入存储函数的主体中。 它可以是单个语句或复合语句。 在主体部分中，必须至少指定一个`RETURN`语句。`RETURN`语句用于返回一个值给调用者。 每当到达`RETURN`语句时，存储的函数的执行将立即终止。

### MySQL存储函数示例

我们来看一下使用存储函数的例子，这里将使用[示例数据库(yiibaidb)](http://www.yiibai.com/mysql/sample-database.html)中的`customers`表进行演示。

以下示例是根据信用额度返回客户级别的功能。 我们使用[IF语句](http://www.yiibai.com/mysql/if-statement.html)来确定信用额度。

```sql
DELIMITER $$

CREATE FUNCTION CustomerLevel(p_creditLimit double) RETURNS VARCHAR(10)
    DETERMINISTIC
BEGIN
    DECLARE lvl varchar(10);

    IF p_creditLimit > 50000 THEN
 SET lvl = 'PLATINUM';
    ELSEIF (p_creditLimit <= 50000 AND p_creditLimit >= 10000) THEN
        SET lvl = 'GOLD';
    ELSEIF p_creditLimit < 10000 THEN
        SET lvl = 'SILVER';
    END IF;

 RETURN (lvl);
END $$
DELIMITER ;
SQL
```

现在，我们在[SELECT语句](http://www.yiibai.com/mysql/select-statement-query-data.html)中调用`CustomerLevel()`存储函数，如下所示：

```sql
SELECT 
    customerName, CustomerLevel(creditLimit)
FROM
    customers
ORDER BY customerName;
SQL
```

执行上面查询语句，得到以下结果 - 

```shell
+------------------------------------+----------------------------+
| customerName                       | CustomerLevel(creditLimit) |
+------------------------------------+----------------------------+
| Alpha Cognac                       | PLATINUM                   |
| American Souvenirs Inc             | SILVER                     |
| Amica Models & Co.                 | PLATINUM                   |
| ANG Resellers                      | SILVER                     |
| Anna's Decorations, Ltd            | PLATINUM                   |
| Anton Designs, Ltd.                | SILVER                     |
| Asian Shopping Network, Co         | SILVER                     |
| Asian Treasures, Inc.              | SILVER                     |
| Atelier graphique                  | GOLD                       |
| Australian Collectables, Ltd       | PLATINUM                   |
| Australian Collectors, Co.         | PLATINUM                   |
|************** 此处省略了一大波数据 *********************************|
| Vitachrome Inc.                    | PLATINUM                   |
| Volvo Model Replicas, Co           | PLATINUM                   |
| Warburg Exchange                   | SILVER                     |
| West Coast Collectables Co.        | PLATINUM                   |
+------------------------------------+----------------------------+
122 rows in set
Shell
```

下面，来重写在[MySQL IF语句教程](http://www.yiibai.com/mysql/if-statement.html)中开发的`GetCustomerLevel()`存储过程，如下所示：

```sql
DELIMITER $$

CREATE PROCEDURE GetCustomerLevel(
    IN  p_customerNumber INT(11),
    OUT p_customerLevel  varchar(10)
)
BEGIN
    DECLARE creditlim DOUBLE;

    SELECT creditlimit INTO creditlim
    FROM customers
    WHERE customerNumber = p_customerNumber;

    SELECT CUSTOMERLEVEL(creditlim) 
    INTO p_customerLevel;
END $$
DELIMITER ;
SQL
```

如您所见，`GetCustomerLevel()`存储过程在使用`CustomerLevel()`存储函数时可读性更高。

请注意，存储函数仅返回单个值。 如果没有包含`INTO`子句的`SELECT`语句，则将会收到错误。

另外，如果存储的函数包含SQL语句，则不应在其他SQL语句中使用它; 否则，存储的函数将减慢查询的速度。

**纠正/补充内容：**

> RETURNS子句指示函数返回值的类型为{STRINGINTEGERREALDECIMAL}  