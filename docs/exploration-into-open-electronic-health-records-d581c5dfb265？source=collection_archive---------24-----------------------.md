# 开放式电子健康档案的探索

> 原文：<https://towardsdatascience.com/exploration-into-open-electronic-health-records-d581c5dfb265?source=collection_archive---------24----------------------->

![](img/a8ec8d78af5341a041bc5b2f725e908c.png)

路易斯·梅伦德斯在 [Unsplash](https://unsplash.com/s/photos/healthcare?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 放弃

这篇文章的内容是面向学术读者的，我不作任何保证，也不承担任何责任。作为读者，您在探索或以其他方式使用此处包含的材料时，风险完全由您自己承担。您有责任**遵守您所在地区、州、省、国家/地区和/或司法管辖区的所有适用隐私法**，尤其是关于本文中提及或提及的任何及所有材料。你被警告了。

# 介绍

在这篇文章中，我将讨论一个公开的电子健康记录(EHR)数据集。作为一个乐观主义者，我希望有一天去身份化的 EHR 能被广泛使用。掌握这些信息的研究人员应该能够更好地测试他们的医疗保健应用。潜在的应用包括训练非线性编程模型，以帮助预测给定某些治疗和程序的患者结果。其他包括改善医疗机构内部和之间的互操作性和信息交换。第三个应用是各种后端的基准测试。最后但同样重要的是，大数据的使用应该有助于为运营和管理改进提供信息，包括更好的护理和服务交付模式。结果是:越来越多的人得到了最佳的医疗保健！

直截了当地说，在写这篇文章的时候，公开的电子病历还很少。我是说，真的很少。许多网站初看起来很有帮助，提供了大量的报告和各种其他研究工具。不幸的是，用于生成这些产品的大部分基础数据是高度模糊的。堪比那么多冰山的尖端！除非您是合作机构或政府机构的成员，否则获取源数据几乎是不可能的。

尽管最初遇到了一些挫折，但我并没有放弃寻找电子病历公共数据集的旅程。我的收集标准包括以下内容:

*   隐私。必须对数据进行适当的去识别，以保护患者及其家属的隐私。除了什么都不做这一显而易见的选择之外，模拟数据是帮助缓解隐私担忧的一个不错的选择。参见此[链接](http://www.emrbots.org/)了解模拟 EHR。不幸的是，根据研究人员的预期应用，模拟数据可能无法提供足够的真实世界保真度、相关性、噪声或其他标准的分数。第三种选择包括去除身份的患者记录，医疗机构或团体同意在他们通过隐私“嗅探测试”后发布记录。在这些情况下，对于研究人员来说，有这样的决定伴随他们的工作是谨慎的。这可以包括经批准的讨论记录、参考号或其他形式的可审计证据，从隐私角度证明数据的去标识性和可发布性。见以上免责声明。你最好小心行事，因为在你的国家，侵犯病人健康记录的隐私会受到严厉的惩罚。
*   容易接近，如没有任何附加条件的接近。任何提供“合作伙伴计划”和“免费试用”的网站都被排除在我的搜索之外。我的排除列表中的其他网站包括要求人们提供信息请求的网站，由底层组织决定请求是否有效。这些网站通常会在提供数据之前宣传几周到几个月的延迟，如果有的话。
*   这项研究工作的第三个标准涉及互操作性和通过一种或多种已知且受支持的格式集成到数据科学应用程序的容易程度。用更通俗的话来说:如果我能快速、轻松地将其融入 Python 生态系统，那就万事大吉了。这部分是我喜欢称之为“mungieness”的部分。
*   最后，我对包含代表性样本的数据集感兴趣。如果可能的话，它应该基于跨越数年的大量人口。它还应该具有与企业级应用程序生成的文件相当的大小，例如，中型到大型医疗记录系统(MRS)的大小。

我重申一下:这篇文章的目的是什么？以获得包含去识别的 EHR 的代表性数据集。有了这样一个数据集，我打算通过制定和测试几个以医疗保健研究主题为基础的假设来解决问题。我觉得有趣的一些主题包括:

*   一个机构能在多大程度上为患者提供及时有效的服务？
*   管理人员和其他员工是否可以使用任何指标来梳理效率，从而帮助优化护理的提供？

剧透:我不幸发现的数据集并没有帮助我回答这些问题。根据一篇关于 [ORBDA:一个用于电子健康记录服务器性能评估的 openEHR 基准数据集](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5749730/)的论文的作者，他们研究中的数据旨在用于 OpenEHR 系统的基准测试:“我们不鼓励在基准评估以外的其他情况下使用 ORBDA。”[1]尽管如此，庞大的信息量(以 GB 为单位)意味着我至少可以使用我认为非常现实的医疗保健信息技术副产品来测试我的一些分析工具集。请查看此[链接](http://www.lampada.uerj.br/en/orbda/)以访问数据集。

# 程序

这些数据可以在几个文件中下载，并有附带的 SQL 脚本来帮助研究人员将其加载到 PostgreSQL 数据库中。我的测试是在运行 Ubuntu 20.04 的笔记本电脑上进行的。

首先，您需要安装 PostgreSQL:

```
sudo apt-get install postgresql
```

我建议打开两个终端窗口，一个运行 shell 命令，另一个在登录到数据库后从 postgresql 提示符运行命令。

从这个[网站](http://www.lampada.uerj.br/en/orbda/)下载 ORBDA 数据文件和脚本，并将它们放在一个合适的文件夹中。

接下来，解压缩脚本和数据文件。

为了让 PostgreSQL 在您的本地机器上启动并运行，可以在[这里](https://itsfoss.com/install-postgresql-ubuntu/)找到一个有用的教程。使用默认用户名登录。这一步需要您的 Ubuntu 帐户的管理员密码:

```
sudo su postgres
```

在第二个终端窗口中执行上述步骤。登录后，运行:

```
psql
```

提示符应该变成类似如下的内容:

```
postgres=#
```

使用这个新提示，您现在可以输入 PostgreSQL 命令来与您的数据库进行交互。

在创建数据库之前，打开名为“orbdaCreateDB.sql”的脚本，并根据您所在的地区更改排序规则，否则会出现错误。对于我所在的地区，我必须将其从:

```
 LC_COLLATE = ‘en_US.UTF-8’
    LC_CTYPE = ‘en_US.UTF-8’
```

收件人:

```
 LC_COLLATE = ‘en_CA.UTF-8’
    LC_CTYPE = ‘en_CA.UTF-8’
```

更改后，保存脚本。切换回第一个终端窗口，运行以下脚本。

```
psql -U postgres -f orbdaCreateDB.sql
```

如果它成功完成，您现在应该有一个名为“orbda”的数据库。在 PostgreSQL 提示符的第二个终端窗口中，键入:

```
\c orbda
```

您应该会收到一条消息，说明:

```
You are now connected to database “orbda” as user “postgres”.
orbda=#
```

您在该提示符下运行的后续命令应该在 orbda 数据库上执行。

切换回第一个终端并运行表创建脚本:

```
psql -U postgres -d orbda -f orbdaCreateTables.sql
```

如果一切顺利，您可以切换到第二个终端来查看新创建的表的列表。在 postgresql 提示符下键入' \dt '以查看表列表:

```
orbda=# \dt
 List of relations
 Schema | Name | Type | Owner 
 — — — — + — — — — — — — — -+ — — — -+ — — — — — 
 public | bariatrics | table | postgres
 public | chemotherapy | table | postgres
 public | hospitalisation | table | postgres
 public | medication | table | postgres
 public | miscellaneous | table | postgres
 public | nephrology | table | postgres
 public | radiotherapy | table | postgres
(7 rows)
```

如果你已经达到了这篇文章的这一点，你就可以开始玩一些有趣的游戏了，叫做等待游戏。在此之前，请打开“importingFiles.sql”脚本，将目录更改为您在上述步骤中放置文件的位置。打开“importingFiles.sql ”,并根据需要将路径修改为文件的绝对路径。如果不这样做，在尝试导入数据时会出现错误。

更新并保存脚本后，导航到第一个终端并运行:

```
psql -U postgres -d orbda -f importingFiles.sql
```

导入脚本运行后，您应该会看到连续写入数据库的条目数量。将鼠标移到第二个终端上，在导入数据时查看表的详细信息。它们的大小应该增加。在短暂停顿之间，在 PostgreSQL 提示符下执行以下命令:

```
\dt+
```

一旦导入开始，您可能需要等待几个小时才能完成。您的里程可能会有所不同。

当我发现其中一个文件夹位置命名错误时，一个令人高兴的小后果发生了，导致我在纠正错误后不得不重新运行“导入文件”脚本。我注意到，不仅我试图追加的表变大了，许多其他表也变大了。显然，这些表的一些记录在上次导入时丢失了。第三次运行该脚本导致数据库中没有新条目。

如果一切顺利，您应该有一个 PostgreSQL 数据库，其中有几十亿字节的去标识 EHR。在 postgresql 提示符下使用以下命令检查其中一个表:

```
SELECT * FROM nephrology LIMIT 1;
```

# 结论

这是一个快速入门教程，可以通过[链接](http://www.lampada.uerj.br/en/orbda/)获得一组公开的电子健康记录。我将重申我上面的友好提醒，这些 EHR 中包含的数据不应用于研究或其他目的，因为研究人员的预期范围是关于 OpenEHR 系统的基准测试。关于将信息从 PostgreSQL 的关系数据库格式转换成 OpenEHR 的更多细节包含在他们的网站上。如果您希望继续转换，建议您遵循附加步骤。

本文到此结束，祝您在医疗保健信息技术领域的数据科学应用一切顺利！

# 参考

[1] [ORBDA: An *open* EHR 电子健康记录服务器性能评估基准数据集](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5749730/)，Douglas Teodoro，Erik Sundvall，Mario joo Junior，Patrick Ruch，Sergio Miranda Freire，2018。