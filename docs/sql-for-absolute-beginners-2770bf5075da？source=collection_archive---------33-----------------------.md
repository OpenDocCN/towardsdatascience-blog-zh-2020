# 绝对初学者的 SQL

> 原文：<https://towardsdatascience.com/sql-for-absolute-beginners-2770bf5075da?source=collection_archive---------33----------------------->

## 利用您的禁闭时间来更新您的数据库知识

![](img/cb9681981da9bcdc5ad4bcf2ecf5676b.png)

发音是*续作*，不是松鼠！由[阿马尔·伊萨](https://unsplash.com/@ammarissa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/squirrel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

![S](img/b81da93da3b6771f6bfd3ff1b1c6a02b.png)Shakespearer 在隔离期间写了*李尔王*。牛顿在隔离期间为他的运动定律奠定了基础。人们在这个隔离区发明东西——嗯，[愚蠢的东西](https://www.scmp.com/video/china/3049347/silly-inventor-china-becomes-internet-sensation-his-creations)。

也许你不是诗人、科学大师或杰出的发明家。但至少你可以学习 SQL 来给你的简历拉皮条。或者让自己从四面墙的沉闷中转移注意力。

有一些关于 SQL 的不错的免费教程，例如[这里](https://www.w3schools.com/sql/default.asp)和[这里](https://www.tutorialrepublic.com/sql-tutorial/)。但是缺少的是一个易于理解并带有真实知识的实践指南，而不是浏览器中的文本示例。这件作品填补了空白。

当你阅读这篇文章的时候，你被鼓励去编码。一旦你学完了，你就掌握了 SQL 的基础知识！

[](/11-best-data-science-classes-if-youre-locked-home-because-of-coronavirus-ca7d2d74a454) [## 如果你因为冠状病毒而被锁在家里，11 堂最好的数据科学课

### 不用花一分钱就可以开始你的职业生涯

towardsdatascience.com](/11-best-data-science-classes-if-youre-locked-home-because-of-coronavirus-ca7d2d74a454) 

# 基础知识

从严格意义上来说，SQL 不是一种编程语言，而是一种结构化查询语言——这就是 SQL 的意思——用于与数据库进行交互。后者通常位于数据库管理系统(DBMS)中。

最流行的数据库管理系统是 MySQL。也有其他的，但是我们将在这篇文章中使用这个，因为它是免费的和可访问的。

我建议你用[全装](https://dev.mysql.com/doc/refman/8.0/en/installing.html)，即使很繁琐。如果你曾在数据科学或相关行业工作，安装东西将是你最小的障碍。此外，边做边学将比浏览器中的文本练习教给你多一百倍的东西，即使这需要更长的时间。

我将带你了解我是如何在 Mac 上安装 MySQL 的。根据您使用的系统，您的安装可能会略有不同。但是不用担心——MAC OS 和 Linux 系统非常相似。此外，MySQL 有非常好的文档记录，因此您应该能够在任何系统上安装它，而不会出现更大的问题。

![](img/13e565a3202613fa412385e12d3ea9ca.png)

还是续集，不是松鼠。在 [Unsplash](https://unsplash.com/s/photos/squirrel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Demi-Felicia Vares](https://unsplash.com/@dfv?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# 装置

从[下载页面](https://dev.mysql.com/downloads/mysql/)，您需要选择与您的系统兼容的档案。我选择了 DMG 档案的早期版本，因为我的 Mac 电脑太旧了，最新版本的 MySQL 无法在上面运行。在下载之前，该页面会要求您在 Oracle 创建一个帐户，但是您可以跳过这一步。

双击 DMG 文件，等待 10 分钟左右，你就可以开始了！但是一定要保存安装时给你的密码，否则你会后悔的…

在 Mac 上，你只需要进入系统偏好设置并激活服务器。然后，在`.bash_profile`中设置几个别名可能会很方便:

```
alias mysql=/usr/local/mysql/bin/mysql alias mysqladmin=/usr/local/mysql/bin/mysqladmin
```

完成后不要忘记输入`source bash_profile`,这样别名就会被激活。

# 测试

让我们检查一下安装是否正常！如果输入`mysql --version`，应该会得到这样的东西:

```
/usr/local/mysql/bin/mysql  Ver 14.14 Distrib 5.7.29, for macos10.14 (x86_64) using  EditLine wrapper
```

现在，您需要连接到您的 MySQL 服务器并重置密码:

```
mysql -u root -p
```

选项`-p`打开密码提示，你必须输入你在安装阶段得到的密码。还要注意，您是作为根用户连接的，因为您可能没有作为本地用户连接的权限(至少我没有)。

你应该得到这样的提示:`mysql>`。现在让我们设置一个新密码:

```
mysql> SET PASSWORD = PASSWORD('your_new_password');
mysql> FLUSH PRIVILEGES;
```

在每个命令之后，您应该会看到类似于`Query OK, 0 rows affected, 1 warning (0.00 sec)`的内容。从现在起，当您连接到服务器时，将使用新密码。

现在，让我们做最后的检查:

```
mysql> SHOW DATABASES;+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+4 rows in set (0.01 sec)
```

如果你得到了上述的东西，你就可以开始了！

![](img/e3685abc1621dd24d02f7a2f3571298a.png)

嘿！还是念 SEQUEL。Vincent van Zalinge 在 [Unsplash](https://unsplash.com/s/photos/squirrel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 放下麦克风

以下是与 SQL 共存的最重要的命令——我喜欢称之为 MIC:

*   `QUIT`或`\q`退出与服务器的连接。
*   你可以向`help;`或`\h`求助。
*   您可以使用`\c`清除缓冲区。当你写一个多行命令，但是有错误的时候，这很有帮助。

在我们开始破解一些真实的数据库之前，让我们先来探索一下基本的命令。试试这个:

```
mysql> SELECT VERSION(), CURRENT_DATE;+-----------+--------------+
| VERSION() | CURRENT_DATE |
+-----------+--------------+
| 5.7.29    | 2020-04-09   |
+-----------+--------------+1 row in set (0.00 sec)
```

如果您研究一下这个查询，您会注意到一些事情:

*   所有的查询都不区分大小写:它们可以写成`UPPERCASE`、`lowercase`或`mixedCASE`。但习惯上是全部大写。
*   除了像`QUIT`这样的少数例外，查询总是以分号`;`结尾
*   您可以将一个查询扩展到多行，如下所示…

```
mysql> SELECT_VERSION(),
    -> CURRENT_DATE();
```

*   …或者您可以将两个查询放在一行中，如下所示:

```
mysql> SHOW DATABASES; SELECT_VERSION(), CURRENT_DATE();
```

*   您可以像这样注释查询:

```
mysql> SELECT_VERSION(), CURRENT_DATE(); /* shows MySQL version and date */
```

*   SQL 的输出总是以表格形式显示。
*   此外，您将始终看到输出有多少行—即数据点—以及从服务器获取它花了多长时间。

# 创建您的第一个数据库！

让我们开始一个名为`fancydata`的新数据库。首先，您需要确保您是它的所有者，并且拥有所有权限:

```
mysql> GRANT ALL ON fancydata.* TO 'root'@'localhost';
```

您可能想要更改您的用户——以及您的主机名，这取决于您登录的方式。您应该会收到一条确认消息，表明该查询是正确的。

现在，让我们创建数据库:

```
mysql> CREATE DATABASE fancydata;
```

这创建了数据库，但是不能确保您当前正在使用这个数据库。为此，您需要:

```
mysql> USE fancydata
Database changed
```

注意我们在`USE`后面不需要分号。如果你放一个在那里，它不会伤害。如果您开始一个新的会话，您需要再次运行`USE`-命令。

现在我们可以在这个数据库中创建一个表:

```
CREATE TABLE customer (firstname VARCHAR(20), lastname VARCHAR(20), birthday DATE, sex CHAR(1));
```

`VARCHAR`后面的数字指定了该值可以包含的最大字符数。

让我们检查该表是否存在:

```
mysql> SHOW TABLES;+---------------------+
| Tables_in_fancydata |
+---------------------+
| customer            |
+---------------------+
```

完美！现在让我们检查表格的结构:

```
mysql> DESCRIBE customer;+-----------+-------------+------+-----+---------+-------+
| Field     | Type        | Null | Key | Default | Extra |
+-----------+-------------+------+-----+---------+-------+
| firstname | varchar(20) | YES  |     | NULL    |       |
| lastname  | varchar(20) | YES  |     | NULL    |       |
| birthday  | date        | YES  |     | NULL    |       | 
| sex       | char(1)     | YES  |     | NULL    |       |
+-----------+-------------+------+-----+---------+-------+
```

看起来不错！现在我们有一张空桌子。我们可能想用一些数据填充它，如下所示:

```
mysql> INSERT INTO customer VALUES ('Brian', 'Smith', '1987-05-07', 'x');
```

你已经输入了一个客户，布莱恩·史密斯，她今年 33 岁，还没有结婚。

如果你不知道一个值，你可以让它为空。例如，如果您不知道客户的生日，请:

```
INSERT INTO customer VALUES ('Luca', 'Maltoni', NULL, 'm');
```

但是逐行输入数据相当繁琐。如果您有一个包含数据的`customer-file.txt`,那么您可以使用以下命令加载它:

```
mysql> LOAD DATA LOCAL INFILE '/path/to/customer-file.txt' INTO TABLE customer;
```

该文件应该具有与表相同的结构，其中每个条目用制表符分隔。如果有空值，您需要在文件中将它们标记为`\N`。

![](img/a1a25d399aeec1f6c2c225f21c64287c.png)

好了，松鼠笑话讲够了。 [Saori Oya](https://unsplash.com/@saorio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/squirrel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 检索您的数据

您可以看到这样一个表格中的内容:

```
mysql> SELECT * from customer;+-----------+----------+------------+------+
| firstname | lastname | birthday   | sex  |
+-----------+----------+------------+------+
| Brian     | Smith    | 1987-05-07 | x    |
| Luca      | Maltoni  | NULL       | m    |
+-----------+----------+------------+------+
```

如果您发现表格中的某些数据错误或缺失，您可以删除所有内容，如下所示:

```
mysql> DELETE FROM customer;
```

或者，您可以更新单行，如下所示:

```
mysql> UPDATE customer SET birthday = '1992-08-31' WHERE firstname = 'Luca';
```

如果您需要更具体的标准，您可以使用逻辑`AND`来选择它们:

```
mysql> UPDATE customer SET birthday = '1992-08-31' WHERE firstname = 'Luca' AND sex = 'm';
```

这样，如果您有两个名为`Luca`的不同性别的顾客，您可以选择男性顾客。

或者，如果您选择多个实例，您可以使用逻辑`OR`:

```
mysql> UPDATE customer SET name = 'Luca' WHERE sex = 'x' AND sex = 'm';
```

如果您只想要名字的列表，您可以:

```
mysql> SELECT firstname FROM customer;+-----------+
| firstname |
+-----------+
| Brian     |
| Luca      |
+-----------+
```

您可以对数据进行排序，例如:

```
mysql> SELECT * FROM customer 
    -> ORDER BY sex, birthday DESC;
```

这将显示所有客户，按性别排序，最年轻的排在最前面。如果你想显示他们的年龄，你可以这样做:

```
mysql> SELECT *,
    -> TIMESTAMPDIFF(YEAR, birthday, CURDATE()) AS age
    -> FROM customer;+-----------+----------+------------+------+------+
| firstname | lastname | birthday   | sex  | age  |
+-----------+----------+------------+------+------+
| Brian     | Smith    | 1987-05-07 | x    |   32 |
| Luca      | Maltoni  | 1992-08-31 | m    |   27 |
+-----------+----------+------------+------+------+
```

现在你已经可以做基本的计算了！还有类似于`TIMESTAMPDIFF`的其他函数用于其他操作，你可以在 MySQL [文档](https://dev.mysql.com/doc/refman/8.0/en/numeric-functions.html)中找到。

# 检查质量

如果要检索已知生日的所有客户的数据，可以这样做:

```
mysql> SELECT * FROM customer WHERE birthday IS NOT NULL;
```

相反，您可以检索生日未知的所有客户:

```
mysql> SELECT * FROM customer WHERE birthday IS NULL;
```

也许您不想显示整个表，但仍然想知道有多少行。然后你做:

```
mysql> SELECT COUNT(*) FROM customer;+----------+
| COUNT(*) |
+----------+
|        2 |
+----------+
```

如果你想知道有多少人被称为`Luca`，请这样做:

```
mysql> SELECT firstname, COUNT(*) FROM customer GROUP BY firstname;
```

# 成批处理方式

以批处理模式运行 MySQL 很容易。假设我们有一个名为`batchfile`的批处理文件，那么您只需要做:

```
mysql < batchfile
```

在`batchfile`中，你基本上输入所有你通常在交互会话中使用的命令。你完了！

# 包扎

恭喜你！您已经做到了——我们已经讲述了如何安装 MySQL、构建数据库和表、用数据填充表以及检索数据。现在你可以在你的简历:D 上写上基本的 SQL 技能

你会在官方 MySQL [文档](https://dev.mysql.com/doc/refman/8.0/en/preface.html)中找到更多信息。如果你遇到问题，总会有[stack overflow](https://stackoverflow.com/questions/tagged/sql)……或者给我留言，我会根据你的反馈更新这篇文章。