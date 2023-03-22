# 使用 psycopg2 的 Python 模块连接到 PostgreSQL 数据库服务器

> 原文：<https://towardsdatascience.com/connect-to-postgresql-database-server-using-psycopg2-with-an-elegant-configuration-file-dba6fc885989?source=collection_archive---------6----------------------->

![](img/5859e0304ae6c588768fcc3c40834f56.png)

Jan Antonin Kolar 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## [动手教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 开发一个整洁的脚本和目录来本地或远程连接到 PostgreSQL

## 目录(仅适用于 web)

```
1 [What’s it all about?](#6107)
2 [The psycopg module to connect a PostgreSQL](#64a2)
3 [How to connect with PostgreSQL](#2ac2)
4 [Create, Read, Update, and Delete (CRUD) using psycopg2](#23f6)
  • [Create a table](#e1a3)
  • [Insert data to a table](#be50)
  • [Retrieve a table](#b149)
  • [Update a table](#5d20)
  • [Delete rows or a table](#fd24)
5 [Conclusion](#194d)
6 [References](#0c68)
```

## 这是怎么回事？

ostgreSQL 是一个关系数据库管理系统，它是开源的，也有很多功能。在 Python 中，我们有几个模块可以连接到 PostgreSQL，比如`**SQLAlchemy**`、`**pg8000**`、`**py-postgresql**`等。然而，`**psycopg2**`成为最受欢迎的一个。所以在本教程中，我们将讨论如何使用`**psycopg2**` 连接到 PostgreSQL。

## 连接 PostgreSQL 的 psycopg 模块

`**psycopg2**`是 Python 开发人员通常用来连接 Python 的 PostgreSQL 连接器。它是本教程的核心模块，所以请确保我们已经在机器上安装了它。在终端上使用以下命令安装`**psycopg2**`模块。

```
**$**pip3 install psycopg2
```

## 如何连接 PostgreSQL

要连接到 PostgreSQL，我们必须在 PostgreSQL 上创建一个代表我们的数据库的文件。我们将把数据库配置文件保存到`**<filename>.ini**`。 ***如果我们与公众分享我们的主要 Python 脚本*** ，它会保证我们数据库配置的安全。在这种情况下，我们将它命名为`**database.ini**` 。请输入您的数据库配置，如*主机名*，*数据库名*，*用户名*，*密码*，*端口号*等至`**database.ini**` 如下。

```
[postgresql]
host = localhost
database = customer
user = postgres
password = admindb
port = 5432
```

关于数据库配置的信息。

*   `**host**` —是运行 PostgreSQL 的服务器名称或 IP 地址
*   `**database**` —是我们要连接的数据库名称
*   `**user**` —是 PostgreSQL 的用户名称
*   `**password**` —是连接 PostgreSQL 所需的密钥
*   `**port**` —监听 TCP/IP 连接时将使用的端口号。默认端口号是 5432

为了解析来自`**database.ini**`文件的信息，我们用如下脚本创建了`**config.py**`文件。它还创建了一个返回项目根文件夹的函数。

> 注意:`**section = ‘postgresql’**`的设置取决于`**database.ini**`文件的文件头。我们以后可以修改它。

```
# Import libraries
from configparser import ConfigParser
from pathlib import Path
def get_project_root() -> Path:
  """Returns project root folder."""
  return Path(__file__).parents[1]
def config(config_db):
  section = 'postgresql'
  config_file_path = 'config/' + config_db
  if(len(config_file_path) > 0 and len(section) > 0):
    # Create an instance of ConfigParser class
    config_parser = ConfigParser()
    # Read the configuration file
    config_parser.read(config_file_path)
    # If the configuration file contains the provided section name
    if(config_parser.has_section(section)):
      # Read the options of the section
      config_params = config_parser.items(section)
      # Convert the list object to a python dictionary object
      # Define an empty dictionary
      db_conn_dict = {}
      # Loop in the list
      for config_param in config_params:
        # Get options key and value
        key = config_param[0]
        value = config_param[1]
        # Add the key value pair in the dictionary object
        db_conn_dict[key] = value
      # Get connection object use above dictionary object
      return db_conn_dict
```

当我们的作品需要定期读取数据库上的表格时，我们可以创建一个文件名为`**db_conn.py**`的函数，如下。它将加载符合我们输入的表(`**config_db**`和`**query**`)。

```
# Import libraries
import pandas as pd
import psycopg2
from config.config import config
# Take in a PostgreSQL table and outputs a pandas dataframe
def load_db_table(config_db, query):
    params = config(config_db)
    engine = psycopg2.connect(**params)
    data = pd.read_sql(query, con = engine)
    return data
```

为了编译我们的脚本，我们设置我们的目录来定位`**database.ini**`、`**config.py**`和`**db_conn.py**`。使用以下设置。

![](img/45496af4b822f53d5fd7157c1c0a03ee.png)

运行脚本的推荐目录(图片由作者提供)

等等！有`**main.py**`文件，是什么？好吧，那么，它必须是一个 Python 脚本来测试我们通过 Python 的 PostgreSQL 连接。所以，这里是`**main.py**`。

```
# Import libraries
from src.data.db_conn import load_db_table
from config.config import get_project_root
# Project root
PROJECT_ROOT = get_project_root()
# Read database - PostgreSQL
df = load_db_table(config_db = 'database.ini', query = 'SELECT * FROM tablename LIMIT 5')
print(df)
```

有用！现在，我们将自定义并创建其他 PostgreSQL 命令，以便通过 Python 创建、读取、更新或删除。

## 使用 psycopg2 创建、读取、更新和删除(CRUD)

## 创建表格

通过`**psycopg2**`创建一个表使用了`**CREATE TABLE**`语句，但是是在 Python 脚本中。之前，我们保存了数据库配置文件。当我们想连接 PostgreSQL 时，只需导入`**config**`函数。

```
# Import libraries
import pandas as pd
import psycopg2
from config.config import config
# Connect to PostgreSQL
params = config(config_db = 'database.ini')
engine = psycopg2.connect(**params)
print('Python connected to PostgreSQL!')
# Create table
cur = con.cursor()
cur.execute("""
CREATE TABLE customer(
customer_id INT PRIMARY KEY NOT NULL,
name CHAR(50) NOT NULL,
address CHAR(100),
email CHAR(50),
phone_number CHAR(20));
""")
print('Table created in PostgreSQL')
# Close the connection
con.commit()
con.close()
```

## 将数据插入表格

创建一个新表`**customer**`后，我们将通过 SQL 语句`**INSERT INTO**`向表中插入一些值。这里有一个例子。

```
# Import libraries
import pandas as pd
import psycopg2
from config.config import config
# Connect to PostgreSQL
params = config(config_db = 'database.ini')
engine = psycopg2.connect(**params)
print('Python connected to PostgreSQL!')
# Insert values to the table
cur = con.cursor()
cur.execute("""
INSERT INTO customer (customer_id,name,address,email,phone_number)
VALUES (12345,'Audhi','Indonesia','myemail@gmail.com','+621234567');
""")
cur.execute("""
INSERT INTO customer (customer_id,name,address,email,phone_number)
VALUES (56789,'Aprilliant','Japan','email@gmail.com','+6213579246');
""")
print('Values inserted to PostgreSQL')
# Close the connection
con.commit()
con.close()
```

## 取回桌子

我们通过`**SELECT**` SQL 语句检索我们的`**customer**`表。在这种情况下，我们将检索`**customer**`表的前五行。

> **注意:**要从 PostgreSQL 中检索数据，请确保我们选择了正确的表名和列。不要使用`*****`语句，因为它会减慢我们的进度

```
# Import libraries
import pandas as pd
import psycopg2
from config.config import config
# Connect to PostgreSQL
params = config(config_db = 'database.ini')
engine = psycopg2.connect(**params)
print('Python connected to PostgreSQL!')
# Read the table
cur = con.cursor()
cur.execute("""
SELECT customer_id,name,email FROM customer LIMIT 5;
""")
print('Read table in PostgreSQL')
# Close the connection
con.commit()
con.close()
```

## 更新表格

当在数据库上工作时，定期更新我们的数据库是正常的，使用`**UPDATE**`语句很容易执行。假设我们将更新拥有`**customer_id = 12345**`的客户的地址。查看我们的表格，它将通过`**psycopg2**`自动更新。

```
# Import libraries
import pandas as pd
import psycopg2
from config.config import config
# Connect to PostgreSQL
params = config(config_db = 'database.ini')
engine = psycopg2.connect(**params)
print('Python connected to PostgreSQL!')
# Insert values to the table
cur = con.cursor()
cur.execute("""
UPDATE customer SET address = 'Japan' WHERE customer_id = 12345;
""")
print('Values updated in PostgreSQL')
# Close the connection
con.commit()
con.close()
```

## 删除行或表格

最后， **D** 用于使用`**DELETE**`语句删除(行或表)。在这种情况下，我们希望从一个`**customer**`表中删除拥有`**customer_id = 12345**`的客户信息。

```
# Import libraries
import pandas as pd
import psycopg2
from config.config import config
# Connect to PostgreSQL
params = config(config_db = 'database.ini')
engine = psycopg2.connect(**params)
print('Python connected to PostgreSQL!')
# Delete rows from the table
cur = con.cursor()
cur.execute("""
DELETE FROM customer WHERE customer_id = 12345;
""")
print('Values deleted from PostgreSQL')
# Close the connection
con.commit()
con.close()
```

## 结论

当处理大量代码时，我们不仅需要创建一个特定的脚本，还需要创建一个整洁干净的目录。有时，我们的脚本包含秘密信息，如数据库配置等。所以，我们在主脚本之外创建了`**database.ini**`。对于`**psycopg2**`模块，它有许多函数可以与 PostgreSQL 本地或远程交互，这使得我们可以更容易地开发自动化，而不是通过 PgAdmin 手动进行 CRUD 操作。

## 参考

[1] Python-Dev。[*psycopg 2 2 . 8 . 6*【https://pypi.org/project/psycopg2/】【2020】](https://pypi.org/project/psycopg2/)。