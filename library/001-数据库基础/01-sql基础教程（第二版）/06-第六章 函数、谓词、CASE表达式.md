## 各种各样的函数

### 函数的种类
1. 什么叫函数。
   所谓函数，就是输入某一值得到相应输出结果的功能，输入值称为参数（parameter），输出值称为返回值。
2. 函数大致可以分为以下几种。
   - 算术函数（用来进行数值计算的函数）
   - 字符串函数（用来进行字符串操作的函数）
   - 日期函数（用来进行日期操作的函数）
   - 转换函数（用来转换数据类型和值的函数）
   - 聚合函数（用来进行数据聚合的函数）

### 算术函数
- ABS——绝对值
  ```markdown
  ABS(数值)
  ```
  ```markdown
  SELECT m,
       ABS(m) AS abs_col
  FROM SampleMath;
  ```
  ABS 函数的参数为 NULL 时，结果也是 NULL。并非只有 ABS 函数如此，其实绝大多数函数对于 NULL 都返回 NULL.
- MOD——求余
  ```markdown
  MOD(被除数，除数)
  ```
  ```markdown
  SELECT n, p,
       MOD(n, p) AS mod_col
  FROM SampleMath;
  ```
  **这里有一点需要大家注意：主流的 DBMS 都支持 MOD 函数，只有SQL Server 不支持该函数。**
  SQL Server使用特殊的运算符（函数）“%”来计算余数，使用如下的专用语法可以得到与代码清单6-3相同的结果。需要使用SQL Server的读者需要特别注意。
  ```markdown
  SELECT n, p,
       n % p AS mod_col
  FROM SampleMath;
  ```
- ROUND——四舍五入
  ```markdown
  ROUND(对象数值，保留小数的位数)
  ```
  ```markdown
  SELECT m, n,
       ROUND(m, n) AS round_col
  FROM SampleMath;
  ```

### 字符串函数
在日常生活中，我们经常会像使用数字那样，对字符串进行替换、截取、简化等操作，因此 SQL 也为我们提供了很多操作字符串的功能。  
- ||——拼接
  在实际业务中，我们经常会碰到 abc + de = abcde 这样希望将字符串进行拼接的情况。在 SQL 中，可以通过由两条并列的竖线变换而成的“||”函数来实现。
  ```markdown
  SELECT str1, str2,
       str1 | | str2 AS str_concat
  FROM SampleStr;
  ```
  **进行字符串拼接时，如果其中包含 NULL，那么得到的结果也是NULL。这是因为“||”也是变了形的函数。当然，三个以上的字符串也可以进行拼接**
  ```markdown
  SELECT str1, str2, str3,
       str1 | | str2 | | str3 AS str_concat
  FROM SampleStr
  WHERE str1 = '山田';
  ```
  **这里也有一点需要大家注意，|| 函数在 SQL Server 和 MySQL 中无法使用。**   

  **SQL Server使用“+”运算符（函数）来连接字符串A。**  
  **MySQL使用CONCAT函数来完成字符串的拼接。**  
  **使用如下SQL Server/MySQL的专用语法能够得到与代码清单6-7相同的结果。**  
  **另外，在SQL Server 2012及其之后的版本中也可以使用CONCAT函数。**
  ```markdown
  **SQL Server**

  SELECT str1, str2, str3,
         str1 + str2 + str3 AS str_concat
    FROM SampleStr;

  **MySQLSQL Server 2012 及之后**

  SELECT str1, str2, str3,
         CONCAT(str1, str2, str3) AS str_concat
    FROM SampleStr;
  ```
- LENGTH——字符串长度
  ```markdown
  LENGTH(字符串)
  ```
  ```markdown
  SELECT str1,
       LENGTH(str1) AS len_str
  FROM SampleStr;
  ```
  **需要注意的是，该函数也无法在 SQL Server 中使用。**  
  **SQL Server使用LEN函数来计算字符串的长度。使用如下SQL Server的专用语法能够得到与代码清单6-8相同的结果。**
  ```markdown
  **SQL Server**

  SELECT str1,
         LEN(str1) AS len_str
    FROM SampleStr;
  ```
- LOWER——小写转换
  ```markdown
  LOWER(字符串)
  ```
  ```markdown
  SELECT str1,
       LOWER(str1) AS low_str
  FROM SampleStr
  WHERE str1 IN ('ABC', 'aBC', 'abc', '山田');
  ```
  **既然存在小写转换函数，那么肯定也有大写转换函数，UPPER 就是大写转换函数。**  
- REPLACE——字符串的替换
  ```markdown
  REPLACE(对象字符串，替换前的字符串，替换后的字符串)
  ```
  ```markdown
  SELECT str1, str2, str3,
       REPLACE(str1, str2, str3) AS rep_str
  FROM SampleStr;
  ```
- SUBSTRING——字符串的截取
  ```markdown
  SUBSTRING（对象字符串 FROM 截取的起始位置 FOR 截取的字符数）
  ```
  ```markdown
  **PostgreSQLMySQL**

  SELECT str1,
         SUBSTRING(str1 FROM 3 FOR 2) AS sub_str
    FROM SampleStr;
  ```
  **虽然上述 SUBSTRING 函数的语法是标准 SQL 承认的正式语法，但是现在只有 PostgreSQL 和 MySQL 支持该语法。**
- UPPER——大写转换
  UPPER 函数只能针对英文字母使用，它会将参数中的字符串全都转换为大写。
  ```markdown
  SELECT str1,
       UPPER(str1) AS up_str
  FROM SampleStr
  WHERE str1 IN ('ABC', 'aBC', 'abc', '山田');
  ```
### 日期函数
- CURRENT_DATE——当前日期
  ```markdown
  CURRENT_DATE
  ```
  CURRENT_DATE 函数能够返回 SQL 执行的日期，也就是该函数执行时的日期。由于没有参数，因此无需使用括号。  
  执行日期不同，CURRENT_DATE 函数的返回值也不同。  
  如果在2009 年 12 月 13 日执行该函数，会得到返回值“2009-12-13”。  
  如果在2010 年 1 月 1 日执行，就会得到返回值“2010-01-01”
  ```markdown
  SELECT CURRENT_DATE;
  ```
  **该函数无法在 SQL Server 中执行。此外，Oracle 和 DB2 中的语法略有不同。**
- CURRENT_TIME——当前时间
  ```markdown
  CURRENT_TIME
  ```
  CURRENT_TIME 函数能够取得 SQL 执行的时间，也就是该函数执行时的时间。  
  由于该函数也没有参数，因此同样无需使用括号。
  ```markdown
  **PostgreSQLMySQL**

  SELECT CURRENT_TIME;
  ```
  该函数同样无法在 SQL Server 中执行，在 Oracle 和 DB2 中的语法同样略有不同。
- CURRENT_TIMESTAMP——当前日期和时间
  CURRENT_TIMESTAMP 函数具有 CURRENT_DATE + CURRENT_TIME 的功能。使用该函数可以同时得到当前的日期和时间，当然也可以从结果中截取日期或者时间。
  ```markdown
  **SQL ServerPostgreSQLMySQL**

  SELECT CURRENT_TIMESTAMP;
  ```
  该函数可以在 SQL Server 等各个主要的 DBMS 中使用A。但是，与之前的 CURRENT_DATE 和 CURRENT_TIME 一样，在 Oracle 和 DB2中该函数的语法略有不同。
- EXTRACT——截取日期元素
  ```markdown
  EXTRACT(日期元素 FROM 日期)
  ```
  使用 EXTRACT 函数可以截取出日期数据中的一部分，例如“年” “月”，或者“小时”“秒”等。
  ```markdown
  **PostgreSQLMySQL**

  SELECT CURRENT_TIMESTAMP,
   EXTRACT(YEAR FROM CURRENT_TIMESTAMP) AS year,
   EXTRACT(MONTH FROM CURRENT_TIMESTAMP) AS month,
   EXTRACT(DAY FROM CURRENT_TIMESTAMP) AS day,
   EXTRACT(HOUR FROM CURRENT_TIMESTAMP) AS hour,
   EXTRACT(MINUTE FROM CURRENT_TIMESTAMP) AS minute,
   EXTRACT(SECOND FROM CURRENT_TIMESTAMP) AS second;
   ```
   **需要注意的是 SQL Server 也无法使用该函数。**
### 转换函数
“转换”这个词的含义非常广泛，在 SQL 中主要有两层意思：一是数据类型的转换，简称为类型转换，在英语中称为 castA ；另一层意思是值的转换。
- CAST——类型转换
  ```markdown
  CAST（转换前的值 AS 想要转换的数据类型）
  ```
  之所以需要进行类型转换，是因为可能会插入与表中数据类型不匹配的数据，或者在进行运算时由于数据类型不一致发生了错误，又或者是进行自动类型转换会造成处理速度低下。这些时候都需要事前进行数据类型转换。
  ```markdown
  **将字符串类型转换为数值类型**
  **SQL ServerPostgreSQL**

  SELECT CAST('0001' AS INTEGER) AS int_col;

  **MySQL**

  SELECT CAST('0001' AS SIGNED INTEGER) AS int_col;

  **Oracle**

  SELECT CAST('0001' AS INTEGER) AS int_col
    FROM DUAL;

  **DB2**

  SELECT CAST('0001' AS INTEGER) AS int_col
    FROM SYSIBM.SYSDUMMY1;
  ```
  ```markdown
  **将字符串类型转换为日期类型**
  **SQL ServerPostgreSQLMySQL**

  SELECT CAST('2009-12-14' AS DATE) AS date_col;
  Oracle
  SELECT CAST('2009-12-14' AS DATE) AS date_col
    FROM DUAL;

  **DB2**

  SELECT CAST('2009-12-14' AS DATE) AS date_col
    FROM SYSIBM.SYSDUMMY1;
  ```
- COALESCE——将NULL转换为其他值
  ```markdown
  COALESCE(数据1，数据2，数据3……)
  ```
  COALESCE 是 SQL 特有的函数。该函数会返回可变参数A 中左侧开始第 1 个不是 NULL 的值。参数个数是可变的，因此可以根据需要无限增加。其实转换函数的使用还是非常频繁的。在 SQL 语句中将 NULL 转换为其他值时就会用到转换函数（代码清单 6-19、代码清单 6-20）。就像之前我们学习的那样，运算或者函数中含有 NULL 时，结果全都会变为NULL。能够避免这种结果的函数就是 COALESCE。
  ```markdown
  **将NULL转换为其他值**
  **SQL ServerPostgreSQLMySQL**

  SELECT COALESCE(NULL, 1) AS col_1,
         COALESCE(NULL, 'test', NULL) AS col_2,
         COALESCE(NULL, NULL, '2009-11-01') AS col_3;

  **Oracle**

  SELECT COALESCE(NULL, 1) AS col_1,
         COALESCE(NULL, 'test', NULL) AS col_2,
         COALESCE(NULL, NULL, '2009-11-01') AS col_3
    FROM DUAL;

  **DB2**

  SELECT COALESCE(NULL, 1) AS col_1,
         COALESCE(NULL, 'test', NULL) AS col_2,
         COALESCE(NULL, NULL, '2009-11-01') AS col_3
    FROM SYSIBM.SYSDUMMY1;
  ```
  ```markdown
  **使用SampleStr表中的列作为例子**

  SELECT COALESCE(str2, 'NULL')
  FROM SampleStr;
  ```

## 谓词

### 什么是谓词
通俗来讲谓词是需要满足特定条件的函数，该条件就是返回值是真值。对通常的函数来说，返回值有可能是数字、字符串或者日期等，但是谓词的返回值全都是真值（TRUE/FALSE/UNKNOWN）。这也是谓词和函数的最大区别

### LIKE谓词——字符串的部分一致查询

## CASE表达式

### 什么是CASE表达式
CASE表达式是在区分情况时使用的，这种情况的区分在编程中通常称为（条件）分支。

### CASE表达式的语法
CASE表达式的语法分为简单CASE表达式和搜索CASE表达式两种。但是，由于搜索CASE表达式包含了简单CASE表达式的全部功能。
