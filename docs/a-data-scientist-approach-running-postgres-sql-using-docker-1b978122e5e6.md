# 数据科学家的方法:使用 Docker 运行 Postgres SQL

> 原文：<https://towardsdatascience.com/a-data-scientist-approach-running-postgres-sql-using-docker-1b978122e5e6?source=collection_archive---------17----------------------->

## 面向数据科学家的 Docker 应用程序

## 面向数据科学家的 Postgres docker 容器

![](img/2a1c3d20a89590f9f06ba98a185fbf06.png)

在这篇简短的教程中，我解释了设置在 Docker 中运行的 PostgreSQL 本地实例并使用 python 与数据库交互的步骤。已采取以下步骤来设置流程。

1.为 PostgreSQL
2 设置 docker。用 Pgadmin4
3 连接 Postgres。使用 Python 与 Postgresql 交互

> 我用过 Ubuntu 18.04 和 Python 3.7

## Docker 中的 Postgres

Postgres 是一个流行的开源数据库，被从网络应用到数据科学项目的数据库应用广泛使用。

> Postgress 是现代数据相关应用的主干

有许多数据库框架可供数据科学家选择。在为您的目标应用程序选择最佳框架时，需要考虑的一些因素可能是

*   您的应用程序需要关系数据库吗？
*   数据库和表计数的大小是多少？
*   数据库将驻留在哪里？
*   开源很重要吗？
*   您的前端有与 DB 交互的工具吗？

因为这里的目标受众是热爱开源的数据科学家，所以使用 Python 的 Postgres 是事实上的选择。

在过去的几年中，Docker 在开发人员和数据科学家中变得非常流行。Docker 帮助数据科学家共享开发环境，以便在任何机器上重现结果。此外，扩大生产规模是码头工人使用的额外好处。

我用的是由【bitnami】([https://bitnami.com/](https://bitnami.com/))和`docker-compose`开发维护的 Postgres docker。下面是我在 docker 中运行 Postgress 的内容。

```
version: '2'services:
  postgresql12:
    image: 'bitnami/postgresql:12'
    ports:
      - '5432:5432'
    volumes:
      - 'postgresql_data_12:/bitnami/postgresql12'
    environment:
      - POSTGRESQL_USERNAME=postgres
      - POSTGRESQL_PASSWORD=pwd0123456789
      - POSTGRESQL_DATABASE=my_database
      - ALLOW_EMPTY_PASSWORD=yesvolumes:
  postgresql_data_12:
    driver: local
```

当我们在 python 中连接这个实例时，这些信息非常有用。按照以下步骤运行`docker-compose`。

1.  如果之前没有安装 Docker([为 Ubuntu 安装 Docker](https://docs.docker.com/engine/install/ubuntu/))。
2.  如果之前没有安装`docker-compose`([安装 Docker-Compose for Ubuntu](https://docs.docker.com/compose/install/) )
3.  从`docker-compose.yml`所在的目录运行`docker-compose up`。

按照以上步骤操作后，可以去 [http://localhost:5432/](http://localhost:5434/) 。

对于通过 Python 连接到 Postgres，我们使用`docker-compose.yml.`中定义的信息

## 连接到 Postgres

我们已经通过 docker 成功剥离了 Postgress 服务器，可以通过 post 5432 的 localhost 访问。连接到 Postgres 服务器的一些最常见的方法是

1.  [使用 PgAdmin4](https://medium.com/better-programming/connect-from-local-machine-to-postgresql-docker-container-f785f00461a7)
2.  [使用 psql 客户端工具](https://medium.com/better-programming/connect-from-local-machine-to-postgresql-docker-container-f785f00461a7)
3.  使用 Python

由于我们的重点是使用 Python 连接 Postgres 服务器，所以我们不打算讨论 1 和 2。

## 使用 Python 与 Postgres 交互

在本节中，我们将学习如何使用 python 与本地运行的 Postgres 进行交互。

> code snip pts Credits:[https://auth 0 . com/blog/sqlalchemy-ORM-tutorial-for-python-developers/](https://auth0.com/blog/sqlalchemy-orm-tutorial-for-python-developers/)

按照上面的链接，一行一行地详细描述下面的代码。

```
# To install sqlalchemy
! pip install sqlalchemy
```

安装完上面的包后，让我们连接到 Postgres 引擎。

```
```python
# Import objects
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmakerfrom datetime import date# Settings as shown in docker-compose.yml
engine = create_engine('postgresql://postgres:pwd0123456789@localhost:5432/my_database')
Session = sessionmaker(bind=engine)Base = declarative_base()from sqlalchemy import Column, String, Integer, Date, Table, ForeignKey, Boolean
from sqlalchemy.orm import relationship, backref
# from base import Base
```
```

创建一些表格

```
movies_actors_association = Table(
    'movies_actors', Base.metadata,
    Column('movie_id', Integer, ForeignKey('movies.id')),
    Column('actor_id', Integer, ForeignKey('actors.id'))
)class Movie(Base):
    __tablename__ M= 'movies'id = Column(Integer, primary_key=True)
    title = Column(String)
    release_date = Column(Date)
    actors = relationship("Actor", secondary=movies_actors_association)def __init__(self, title, release_date):
        self.title = title
        self.release_date = release_dateclass Actor(Base):
    __tablename__ = 'actors'id = Column(Integer, primary_key=True)
    name = Column(String)
    birthday = Column(Date)def __init__(self, name, birthday):
        self.name = name
        self.birthday = birthdayclass Stuntman(Base):
    __tablename__ = 'stuntmen'id = Column(Integer, primary_key=True)
    name = Column(String)
    active = Column(Boolean)
    actor_id = Column(Integer, ForeignKey('actors.id'))
    actor = relationship("Actor", backref=backref("stuntman", uselist=False))def __init__(self, name, active, actor):
        self.name = name
        self.active = active
        self.actor = actorclass ContactDetails(Base):
    __tablename__ = 'contact_details'id = Column(Integer, primary_key=True)
    phone_number = Column(String)
    address = Column(String)
    actor_id = Column(Integer, ForeignKey('actors.id'))
    actor = relationship("Actor", backref="contact_details")def __init__(self, phone_number, address, actor):
        self.phone_number = phone_number
        self.address = address
        self.actor = actor
```

按照下面的代码块将数据插入到使用上面的代码片段创建的数据库中。

```
from datetime import date# 2 - generate database schema
Base.metadata.create_all(engine)# 3 - create a new session
session = Session()# 4 - create movies
bourne_identity = Movie("The Bourne Identity", date(2002, 10, 11))
furious_7 = Movie("Furious 7", date(2015, 4, 2))
pain_and_gain = Movie("Pain & Gain", date(2013, 8, 23))# 5 - creates actors
matt_damon = Actor("Matt Damon", date(1970, 10, 8))
dwayne_johnson = Actor("Dwayne Johnson", date(1972, 5, 2))
mark_wahlberg = Actor("Mark Wahlberg", date(1971, 6, 5))# 6 - add actors to movies
bourne_identity.actors = [matt_damon]
furious_7.actors = [dwayne_johnson]
pain_and_gain.actors = [dwayne_johnson, mark_wahlberg]# 7 - add contact details to actors
matt_contact = ContactDetails("415 555 2671", "Burbank, CA", matt_damon)
dwayne_contact = ContactDetails("423 555 5623", "Glendale, CA", dwayne_johnson)
dwayne_contact_2 = ContactDetails("421 444 2323", "West Hollywood, CA", dwayne_johnson)
mark_contact = ContactDetails("421 333 9428", "Glendale, CA", mark_wahlberg)# 8 - create stuntmen
matt_stuntman = Stuntman("John Doe", True, matt_damon)
dwayne_stuntman = Stuntman("John Roe", True, dwayne_johnson)
mark_stuntman = Stuntman("Richard Roe", True, mark_wahlberg)# 9 - persists data
session.add(bourne_identity)
session.add(furious_7)
session.add(pain_and_gain)session.add(matt_contact)
session.add(dwayne_contact)
session.add(dwayne_contact_2)
session.add(mark_contact)session.add(matt_stuntman)
session.add(dwayne_stuntman)
session.add(mark_stuntman)# 10 - commit and close session
session.commit()
session.close()
```

下面是从 Postgres 服务器提取/读取数据的 python 代码片段。如果失去连接，请重新连接。

```
movies = session.query(Movie) \
    .filter(Movie.release_date > date(2015, 1, 1)) \
    .all()print('### Recent movies:')
for movie in movies:
    print(f'{movie.title} was released after 2015')
print('')# 6 - movies that Dwayne Johnson participated
the_rock_movies = session.query(Movie) \
    .join(Actor, Movie.actors) \
    .filter(Actor.name == 'Dwayne Johnson') \
    .all()print('### Dwayne Johnson movies:')
for movie in the_rock_movies:
    print(f'The Rock starred in {movie.title}')
print('')# 7 - get actors that have house in Glendale
glendale_stars = session.query(Actor) \
    .join(ContactDetails) \
    .filter(ContactDetails.address.ilike('%glendale%')) \
    .all()print('### Actors that live in Glendale:')
for actor in glendale_stars:
    print(f'{actor.name} has a house in Glendale')
print('')
```

## 结论

在这个简短的教程中，我们学习了如何使用 docker 设置一个本地 Postgres 服务器，并使用 python 与之交互。

由于 Postgres 运行在 docker 中，我们可以将这个 docker 部署到任何云上，只需做很少的更改。我们必须用主机的 IP 地址替换本地主机，这样一切都可以正常工作了。