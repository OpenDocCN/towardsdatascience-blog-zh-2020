# 使用 JavaScript 查询关系数据库的 5 种方法

> 原文：<https://towardsdatascience.com/5-ways-to-query-your-relational-db-using-javascript-d5499711fc7d?source=collection_archive---------4----------------------->

![](img/203cd62927f51a63b8233739f5b2cbde.png)

图片在 Shutterstock.com[和 T2](http://shutterstock.com/)[的许可下使用](http://shutterstock.com/)

如果您正在开发 web 应用程序，您几乎肯定会不断地与数据库进行交互。当需要选择*方式*互动时，选择会让人不知所措。

在本文中，我们将详细探讨使用 JavaScript 与数据库交互的 5 种不同方式，并讨论每种方式的优缺点。我们将从最底层的选择——SQL 命令——开始，然后进入更高层的抽象。

为 JavaScript 应用程序选择正确的数据库库会对代码的可维护性、可伸缩性和性能产生很大的影响，所以花一些时间来找出您的选择是值得的。

# 我们的示例应用程序

我们将使用一个托管在 Heroku 上的普通 Express 应用程序作为例子。本文的所有代码都在这个 [GitHub 库](https://github.com/digitalronin/query-database-javascript.git)中。随意克隆它并跟随它。

# 先决条件

要运行示例应用程序，您的机器上需要以下软件:

*   一个类似 unix 的终端环境(Mac OSX 和 Linux 都可以。如果你使用的是 Windows，你将需要用于 Linux 的 [Windows 子系统](https://docs.microsoft.com/en-us/windows/wsl/install-win10)。
*   [git](https://git-scm.com/) (还有一个 github 账号)。
*   [npm](https://www.npmjs.com/) (版本 6 或更高版本)。
*   Heroku [命令行工具](https://devcenter.heroku.com/articles/heroku-cli)。

如果你还没有 Heroku 账户，你需要[注册一个免费账户。如果你不想注册 Heroku，你也可以在本地 Postgres 实例上运行应用程序。如果您对此感到满意，应该很容易看到您需要做哪些更改，而不是部署到 Heroku。](https://signup.heroku.com/)

一旦你安装了以上所有的东西，在终端中运行`heroku login`，你就可以开始了。

# 构建和部署 Hello World 应用程序

首先，我们将设置以下内容:

*   一个普通的 [Express](https://expressjs.com/) 应用程序，只提供一个“Hello，World”网页。
*   一个 Postgres 数据库。
*   两个表，分别代表“用户”和“评论”(一个用户有很多评论)。
*   一些样本数据(在这种情况下，通过[mockaroo.com](https://mockaroo.com/)生成)。

我已经创建了一个示例应用程序，它将为您设置所有这些(假设您已经如上所述运行了`heroku login`)。要设置它，请从命令行执行以下命令:

`git clone [https://github.com/digitalronin/query-database-javascript.git](https://github.com/digitalronin/query-database-javascript.git)`

`cd query-database-javascript make setup`

这需要几分钟才能完成。在您等待的时候，您可以查看 makefile 来查看相关的命令，这些命令执行以下操作:

*   创建一个新的 Heroku 应用程序。
*   添加 Postgres 数据库实例。
*   将应用程序部署到 Heroku。
*   在 Heroku 上运行一个命令来设置数据库表并导入 CSV 示例数据。
*   在新的浏览器窗口中打开 Heroku 应用程序的 URL。

在此过程结束时，您应该会在网页上看到“Hello，World”。

# 使用 SQL 提取数据

好了，我们都准备好了！我们已经创建了一个包含两个表和一些示例数据的数据库。但是我们还没有做任何事情。下一步是让我们的 web 应用程序能够从数据库中检索数据。

无论何时与关系数据库交互，都是通过向数据库正在监听的网络套接字发送 SQL 命令来实现的。对于本文中我们将要研究的所有库来说都是如此——在最底层，它们都向数据库发送 SQL 命令，并检索返回的任何输出。

因此，我们要考虑的与数据库交互的第一种方式就是这样做——发送 SQL 命令。为此，我们将安装 pg JavaScript 库，它允许我们向 Postgres 数据库发送 SQL 并检索结果。

要安装 pg 库，请执行以下命令:

`npm install pg`

这将获取并安装这个库，并将它添加到 package.json 和 package-lock.json 文件中。让我们提交这些更改:

`git add package.json package-lock.json git`

`commit -m "Install the pg library"`

要与我们的数据库对话，我们需要一些详细信息:

*   运行 Postgres 的机器的主机名。
*   Postgres 正在监听的网络端口。
*   我们的数据所在的数据库的名称。
*   有权访问数据的用户名和密码。

大多数数据库库会让我们建立一个连接，要么通过向库中提供一个对象，该对象包含所有这些细节的键和值，要么通过将它们组合成一个“数据库 URL”，这就是我们将要做的。

当您向 Heroku 应用程序添加数据库时，您会自动获得一个名为 DATABASE_URL 的环境变量，其中包含连接数据库所需的所有详细信息。您可以通过运行以下命令来查看 DATABASE_URL 的值:

`heroku config`

这将输出您的应用程序可以使用的所有环境变量。现在应该只有一个，所以您应该在输出中看到类似这样的内容:

数据库 _ 网址:`postgres://clqcouauvejtvw:1b079cad50f3ff9b48948f15a7fa52123bc6795b875348d668864`
`07a266c0f5b@ec2-52-73-247-67.compute-1.amazonaws.com:5432/dfb3aad8c026in`

在我们的例子中，分解如下:

结构化查询语言

```
{"hostname": "ec2-52-73-247-67.compute-1.amazonaws.com", "port": 5432, "database": "dfb3aad8c026in","username": "clqcouauvejtvw","password": "1b079cad50f3ff9b48948f15a7fa52123bc6795b875348d66886407a266c0f5b"}
```

您的 DATABASE_URL 值会有所不同，但结构是相同的。

现在我们已经安装了 pg 库，并且我们知道如何连接到我们的数据库，让我们执行我们的第一个与数据库交互的例子。我们将简单地获取用户列表，并将它们显示在我们的网页上。在 index.js 文件的顶部，我们需要 pg 库，并创建一个数据库连接对象。

Java Script 语言

```
const { Pool } = require('pg');const conn = new Pool({ connectionString: process.env.DATABASE_URL });
```

在`express()`块中，我们将修改 get 行来调用显示数据库中用户列表的方法:

`.get('/', (req, res) => listUsers(req, res))`

最后，我们将实现 listUsers 函数:

Java Script 语言

```
async function listUsers(req, res) {try {const db = await conn.connect()const result = await db.query('SELECT * FROM users');const results = { users: (result) ? result.rows : null};res.render('pages/index', results );db.release();} catch (err) {console.error(err);res.send("Error " + err);}}
```

这段代码一直等到与我们的数据库建立了连接，然后使用 query 函数发送一个 SQL 查询并检索结果。

现在，这一步可能由于许多不同的原因而失败，所以在代码中，我们进行测试以确保我们获得了一些数据，如果我们获得了一些数据，我们就将 result.rows 分配给 results 对象的关键用户。接下来，我们将结果传递给 render 函数，然后释放数据库连接。

在 views/pages/index.ejs 中，我们可以访问结果对象，因此我们可以像这样显示我们的用户数据:

超文本标记语言

```
<h1>Users</h1><ul><% users.map((user) => { %><li><%= user.id %> - <%= user.first_name %> <%= user.last_name %></li><% }); %></ul>
```

你可以在这里看到这些变化的代码[。](https://github.com/digitalronin/query-database-javascript/tree/pg) `first_name`和`last_name`是我们数据库的 users 表中两列的名称。

让我们部署这些更改，这样我们就可以在 Heroku 应用程序中看到数据:

`git add index.js views/pages/index.ejs`

`git commit -m "Display a list of users"`

`git push heroku master`

这将需要一两分钟来部署。当该命令执行完毕后，重新加载您的浏览器，您应该会在网页上看到一个用户列表。

# MySQL 示例

上面的例子是针对 Postgres 的，但是针对其他常见关系数据库的代码也是类似的。例如，如果您使用的是 [MySQL](https://www.mysql.com/) :

*   使用`npm install mysql2`而不是`npm install pg`(使用 mysql2，而不是 MySQL——MySQL 2 更快并且支持异步/等待)
*   在 index.js 中，您需要 mysql，如下所示:

`const mysql = require('mysql2/promise');`

*   listUsers 函数如下所示:

Java Script 语言

```
async function listUsers(req, res) {try {const conn = await mysql.createConnection(process.env.DATABASE_URL);const [rows, fields] = await conn.execute('SELECT * FROM users');const results = { 'users': rows };res.render('pages/index', results );await conn.end();} catch (err) {console.error(err);res.send("Error " + err);}}
```

视图/页面/索引. ejs 保持不变。

您可以在这里看到带有这些变化的示例项目[。](https://github.com/digitalronin/query-database-javascript/tree/mysql)

现在，让我们研究几个构建在这个基础之上的库，它们添加了抽象层，让您能够以更“类似 JavaScript”的方式读取和操作数据库数据。

到目前为止，我们已经看到了如何将原始 SQL 发送到数据库；像这样的陈述:

`SELECT * FROM users`

如果我们想得到某个特定用户的评论，比如说 id 为 1 的用户，我们可以使用这样的代码:

`SELECT * FROM comments WHERE user_id = 1`

以这种方式与您的数据库进行交互没有任何问题，但是可能会感觉有点麻烦，并且需要您在心理上保持“换档”。您用一种方式考虑您的 JavaScript 代码，但是当您需要考虑数据库中的数据时，您必须开始用 SQL 来考虑。

我们要考虑的其余数据库库的目的是让您将数据库中的数据处理得更像应用程序中的 JavaScript 对象和代码。“在引擎盖下”都是 SQL，但是你不需要太在意这些，除非你想。

# Knex —抽象出 SQL

我们要讨论的第一个库是 [Knex](http://knexjs.org/) 。文档页面将 Knex 描述为“查询构建器”，其目的是在原始 SQL 之上提供一个抽象层。

# 安装 Knex

Knex 需要 pg(或者 MySQL，如果您使用 MySQL 数据库的话)。我们已经安装了 pg，所以我们只需像这样添加 knex:

`npm install knex`

`git add package.json package-lock.json`

`git commit -m "Install the knex library"`

# 使用 Knex

knex 的 NPM 页面将其描述为“查询生成器”Knex 在一定程度上抽象了 SQL，但不是很远。我们仍然需要理解底层的 SQL，但是我们可以用更像 JavaScript 的语法来编写它，而不是将 SQL 字符串分割成小块。更重要的是，我们可以用一种对 JavaScript 程序员来说更舒服的方式使用组合来链接 knex 术语。

所以，当我们使用 pg 时，我们有这样的声明:

`const result = await db.query('SELECT * FROM users');`

当我们使用 knex 时，我们可以这样写:

`const result = await db.select().from('users');`

这可能看起来没有太大的区别，但是由于我们可以编写 knex 函数调用的方式，我们也可以这样做:

`const result = await db.select().from('users').limit(5).offset(8);`

这里，我们得到了 5 个用户记录，从匹配我们查询的所有可能用户记录的第 8 个位置开始。您可以在 [knex 文档](http://knexjs.org/)中看到全套可用选项。

让我们将 Express 应用程序改为使用 knex 来显示数据库中的一些记录。首先，在 index.js 中替换这两行:

Java Script 语言

```
const { Pool } = require('pg');
const conn = new Pool({ connectionString: process.env.DATABASE_URL });
```

…有了这个:

Java Script 语言

```
const db = require('knex')({
client: 'pg',
connection: process.env.DATABASE_URL
});
```

然后，将`listUsers`的实现改为:

Java Script 语言

```
async function listUsers(req, res) {
try {
const result = await db.select().from('users').limit(5).offset(5);
const results = { 'users': (result) ? result : null};res.render('pages/index', results );
} catch (err) {
console.error(err);res.send("Error " + err);
}
}
```

我们的 views/pages/index.ejs 文件可以保持和以前完全一样。

提交、推送和部署:

`git add index.js`

`git commit -m "Use knex to display user data"`

`git push heroku master`

当您刷新浏览器时，应该会在页面上看到用户记录 6 到 10。

你可以在这里看到修改后的代码[。](https://github.com/digitalronin/query-database-javascript/tree/knex)

# 对象关系映射

Knex 为我们提供了一种与数据库交互的方式，这更像 JavaScript，但是当我们需要操作数据时，仍然需要以数据库为中心进行思考。

接下来我们要讲的三个库都是建立在 knex 之上的(knex 是建立在 pg 或者 MySQL 之上的)，是“对象关系映射”或者 ORM 库的例子。顾名思义，ORM 库的目的是在关系数据库中的数据和应用程序中的 JavaScript 对象之间进行转换。这意味着，当您编写 JavaScript 代码时，不用考虑 **users** 表中的记录，您可以考虑用户对象。

# 反对

我们要看的第一个库是 [objection](https://vincit.github.io/objection.js/) ，它构建在 knex 之上:

`npm install objection`

`git add package.json package-lock.json`

`git commit -m "Install the objection library"`

为了突出 ORM 库的一些效用，我们将修改我们的应用程序来显示用户和他们的评论。Objection 构建在 knex 之上，因此在我们的 index.js 文件中，我们必须保留 knex 块，并添加更多的代码(为了简单起见，我将所有内容都放在 index.js 文件中。在真实的应用程序中，您会将代码分成单独的文件):

`const { Model } = require('objection');`

`Model.knex(db);`

这给了我们一个模型类，我们可以继承它来定义两个类 User 和 Comment。我们将首先定义注释:

Java Script 语言

```
class Comment extends Model {static get tableName() {
return 'comments';
}
}
```

我们的类需要扩展`Model`，并且必须实现一个`tableName`函数来告诉 Objection 哪个数据库表包含底层记录。

`User`类是类似的，但是我们要给我们的类添加一些行为；一个`fullName`函数，我们可以在视图模板中使用它。我们还将告诉异议`Users`拥有`Comments`(即用户拥有零个或多个评论)。用 ORM 的话来说，这通常被描述为“有许多关系”——即一个用户有许多评论。下面是它的代码:

Java Script 语言

```
class User extends Model {static get tableName() {
return 'users';
}fullName() {
return `${this.first_name} ${this.last_name}`;
}static get relationMappings() {return {
comments: {
relation: Model.HasManyRelation,
modelClass: Comment,
join: {
from: 'users.id',
to: 'comments.user_id'
}
}
};
}
}
```

我们在我们的`User`类中定义了一个`relationMappings`对象，用一个注释键和一个值告诉异议这是`Comment`类上的一个`HasManyRelation`，其中 users 表的 id 列的值与 comments 表的 user_id 列的值相匹配。

现在我们已经定义了我们的类，让我们在代码中使用它们。下面是`listUsers`的新实现:

Java Script 语言

```
async function listUsers(req, res) {try {
const users = await User.query().limit(5);for (i in users) {
const user = users[i];
user.comments = await User.relatedQuery('comments').for(user.id);
}const results = { 'users': users };res.render('pages/index', results );
} catch (err) {
console.error(err);res.send("Error " + err);
}
}
```

在这里，我们获取 5 个用户，然后对于每个用户，我们获取他们的评论，并将其分配给用户对象的 comments 属性。在 views/pages/index.ejs 中，我们可以像这样显示我们的用户及其评论:

超文本标记语言

```
<h1>Users</h1>
<ul>
<% users.map((user) => { %>
<li><%= user.id %> - <%= user.fullName() %></li>
<ul>
<% user.comments.map((comment) => { %>
<li><%= comment.body %></li>
<% }); %>
</ul>
<% }); %>
</ul>
```

你可以在这里看到修改后的代码[。](https://github.com/digitalronin/query-database-javascript/tree/objection)像往常一样，提交并推送部署:

`git add index.js views/pages/index.ejs`

`git commit -m "Show users and comments using Objection"`

`git push heroku master`

现在，当您重新加载页面时，您应该会看到用户和评论。

# “N+1 选择”问题

这段代码强调了人们在使用 ORM 库时遇到的一个常见问题，称为“N+1 选择”问题。

这是我们用来获取用户及其评论的代码块:

Java Script 语言

```
const users = await User.query().limit(5);for (i in users) {
const user = users[i];
user.comments = await User.relatedQuery('comments').for(user.id);
}
```

这是可行的，但是效率很低。首先，我们获取 5 个用户，然后对于这 5 个用户中的每一个，我们通过再次调用数据库来获取他们的评论*。因此，我们为用户打了 1 个电话，然后又打了 5 个电话来获取评论。这是 5 次呼叫加上前 1 次，即 5+1 或 N+1，其中 N == 5。因此出现了“N+1 选择”问题。*

除非数据库查询非常复杂，否则往返调用数据库所需的时间要比数据库计算和传输查询结果所需的时间长得多。因此，为了保持应用程序的速度，我们需要尽可能减少对数据库的调用。上面的代码与此完全相反。

对于这个微不足道的例子，您不会注意到任何差异，但是对于真实世界的应用程序，性能影响可能非常严重，并导致许多问题。

幸运的是，每个 ORM 库都有可以轻松避免这个问题的特性(前提是你知道它的存在)。以下是异议是如何做到的:在 index.js 中，将上面的代码块替换为:

`const users = await User.query().limit(5).withGraphFetched('comments');`

这一行代码与上面的代码块做的一样，但是以一种更加高效的数据库方式。Objection 将使用我们提供的关系信息来确定如何在单个查询中获取用户数据和评论数据，并将结果解包并缝合到我们在使用 for 循环之前构建的同一对象结构中。

你可以在这里看到修改后的代码[。](https://github.com/digitalronin/query-database-javascript/tree/n-plus-one)

# 书架

我们要看的下一个 ORM 库是[书架](https://bookshelfjs.org/)。

ORM 库之间的许多差异取决于库针对什么用例进行了优化。在 Bookshelf 的例子中，它的设计显然是为了尽可能容易地呈现数据的分页列表，这在 web 应用程序中是一个非常常见的用例。

让我们在应用程序中将异议替换为书架:

`npm uninstall objection`

`npm install bookshelf`

`git add package.jsonpackage-lock.json`

`git commit -m "Replace Objection with Bookshelf"`

在 index.js 中，替换这些行:

Java Script 语言

```
const { Model } = require('objection');
Model.knex(db);
```

…有了这个:

Java Script 语言

```
const bookshelf = require('bookshelf')(db);
```

用这些替换我们的类定义:

Java Script 语言

```
const Comment = bookshelf.model('Comment', {
tableName: 'comments'
});const User = bookshelf.model('User', {
tableName: 'users',comments() {
// by default, bookshelf infers that the foreign key is 'user_id'
return this.hasMany('Comment');
}});
```

我们的`listUsers`函数现在看起来像这样:

Java Script 语言

```
async function listUsers(req, res) {
try {
const models = await new User()
.fetchPage({
pageSize: 5,
page: 1,
withRelated: ['comments']
});users = [];models.map(m => {
const user = m.attributes;
const comments = m.related('comments');user.comments = comments.map(c => c.attributes);
users.push(user);
});const results = { 'users': users };res.render('pages/index', results );
} catch (err) {
console.error(err);
res.send("Error " + err);
}
}
```

正如您所看到的，类的定义更简洁一些，但是 Bookshelf 需要一个更详细的定义来说明如何解包我们的数据以构建用户/评论结构。还要注意数据页面的概念是如何直接构建到库的 API 中的。

views/pages/index.ejs 中的代码几乎相同(我已经从 User 类中删除了 fullName 函数):

超文本标记语言

```
<h1>Users</h1>
<ul>
<% users.map((user) => { %>
<li><%= user.id %> - <%= user.first_name %> <%= user.last_name %></li>
<ul>
<% user.comments.map((comment) => { %>
<li><%= comment.body %></li>
<% }); %>
</ul>
<% }); %>
</ul>
```

你可以在这里看到这些变化[的代码。当然，再次提交和部署。](https://github.com/digitalronin/query-database-javascript/tree/bookshelf)

`git add index.js views/pages/index.ejs`

`git commit -m "Show users and comments using Bookshelf"`

`git push heroku master`

# 序列

我们要看的最后一个库是 [Sequelize](https://sequelize.org/) 。

Sequelize 对数据的组织方式非常固执己见。如果你遵循它的惯例，你可以写更少的代码，让 Sequelize 为你做很多工作。特别是，Sequelize 有很多特性可以帮助您创建表格，默认情况下，它会按照自己的结构和命名约定来创建表格。

我们一直在使用的数据库的结构并不完全符合 Sequelize 的预期，所以我们需要添加一些额外的配置来允许 Sequelize 使用它。

# 安装序列

要删除 bookshelf 并安装 sequelize，请运行以下命令:

`npm uninstall bookshelf`

`npm install sequelize`

`git add package.json package-lock.json`

`git commit -m "Replace Bookshelf with Sequelize"`

# 使用序列

在 index.js 中，替换这些行:

Java Script 语言

```
const db = require('knex')({
client: 'pg',
connection: process.env.DATABASE_URL
});const bookshelf = require('bookshelf')(db)
```

…有了这些:

Java Script 语言

```
const { Sequelize, DataTypes } = require('sequelize');
const sequelize = new Sequelize(process.env.DATABASE_URL);
```

然后，用以下代码替换 User 和 Comment 的类定义:

Java Script 语言

```
const User = sequelize.define('User', {
first_name: { type: DataTypes.STRING },
last_name: { type: DataTypes.STRING },
email: { type: DataTypes.STRING }
},
{
tableName: 'users',
timestamps: false
}
);const Comment = sequelize.define('Comment', {
body: { type: DataTypes.STRING }
}, {
tableName: 'comments',
timestamps: false
}
);User.hasMany(Comment, { foreignKey: 'user_id' });
```

注意，我们向`sequelize.define`传递了两个对象。第一个对象定义了对象的属性，第二个对象包含一些元数据。

在这种情况下，我们告诉 Sequelize，支撑用户类的数据库表称为“users”(默认情况下，Sequelize 会推断该表称为“Users”)，而`timestamps: false`告诉 Sequelize，我们的表没有名为 createdAt 和 updatedAt 的时间戳列。

Sequelize 使得编写为您创建表的代码变得非常容易，当您向数据库写入数据时，它会添加这些时间戳列并相应地设置它们的值。[序列文档](https://sequelize.org/master/index.html)非常好，有更多关于这方面的内容。

我们传递给 hasMany 的是另一个我们必须告诉 Sequelize 我们没有遵循其惯例的地方。它期望(并将为我们创建)一个名为 UserId 的列，将评论链接到用户。

在我们的`listUsers`函数中，我们可以替换所有这些代码:

Java Script 语言

```
const models = await new User().fetchPage({
pageSize: 5,
page: 1,
withRelated: ['comments']
});users = [];models.map(m => {
const user = m.attributes;
const comments = m.related('comments');user.comments = comments.map(c => c.attributes);
users.push(user);
});
```

…只有这一行:

Java Script 语言

```
const users = await User.findAll({ include: Comment });
```

我们还必须在 views/pages/index.ejs 中做一个微小的更改。替换这一行:

`<% user.comments.map((comment) => { %>`

…与此(区别在于用户。Comments 而不是 user.comments):

`<% user.Comments.map((comment) => { %>`

你可以在这里看到修改后的代码[。](https://github.com/digitalronin/query-database-javascript/tree/sequelize)

`git add index.js views/pages/index.ejs`

`git commit -m "Show users and comments using Sequelize"`

`git push heroku master`

# 那么哪个选项是最好的呢？

现在，您有了从 JavaScript 应用程序查询关系数据库的 5 种方法。我们从通过 pg/mysql 库的原始 SQL 开始，然后看了 knex 查询构建器，然后转到三个 ORM 库；反对，书架和顺序。

那么，哪一个才是适合你应用的选择呢？

一如既往，视情况而定。使用 ORM 库，您可以做任何您使用查询构建器甚至原始 SQL 不能做的事情。因为一切都是在“幕后”使用 SQL 工作的。这并不奇怪。此外，即使您决定使用 ORM，大多数库仍然会向您提供将原始 SQL 发送到数据库的方法。所以你使用什么样的抽象层次取决于你试图解决的问题，以及你想关注什么样的代码。

如果您大量使用数据库的特性，可能使用复杂的视图或存储过程，您可能会发现使用 knex 或 raw SQL 更容易。但是，对于大多数 web 应用程序来说，ORM 库很可能会通过抽象出表结构并允许您将应用程序数据视为 JavaScript 对象来简化您的工作。

如果你已经决定使用 ORM，选择使用哪个 ORM 库并不总是一目了然的。JavaScript 库的前景是非常动态的。新的库经常被创建，旧的库就不再受欢迎了。做出选择时，请考虑以下几点:

*   浏览一下库的文档，看看是否清晰全面。然后，决定 API 的组合方式对您是否有意义。不同的库使用不同的方法，您可能会发现其中一种方法比其他方法更适合您的需求和偏好。如果您正在编写使用现有数据库的代码，或者在开发应用程序时创建数据库，这一点尤其正确。
*   看看图书馆周围的社区。这是很多人都在积极使用的东西吗？如果是这样，如果你需要的话，可能会有很多帮助和建议。一些库也有广泛的插件生态系统，可能是特定的插件让你的生活变得更容易。
*   一个相关的问题是图书馆的年龄。如果已经有一段时间了，更有可能是发现并修复了常见问题。如果这是一个相对较新的图书馆，你可能需要自己想出更多的东西(如果你喜欢玩新的闪亮的玩具和解谜，这可能是一件好事)。
*   性能更可能取决于您如何使用库，而不是库本身。但是，如果您绝对必须从应用程序中挤出最后几微秒的延迟，那么使用 SQL 或 knex 在更接近数据库的地方工作会快一点。请注意，这通常是一个很小的好处，代码可维护性的成本很可能高于基准性能的收益。

查询愉快！