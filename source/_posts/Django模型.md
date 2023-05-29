---
title: Django模型
date: 2021-11-25 00:08:00
tag: Django
description: Django模型与规范
cover: https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto
---

![](https://img1.baidu.com/it/u=2206900918,1696468527&fm=26&fmt=auto)

### 定义模型类 ###

**1） 数据库表名**

模型类如果未指明表名，Django默认以**小写app应用名_小写模型类名**为数据库表名。

可通过**db_table**指明数据库表名。

**2） 关于主键**

django会为表创建自动增长的主键列，每个模型只能有一个主键列，如果使用选项设置某属性为主键列后django不会再创建自动增长的主键列。

默认创建的主键列属性为id，可以使用pk代替，pk全拼为primary key。

**3） 属性命名限制**

- 不能是python的保留关键字。

- 不允许使用连续的下划线，这是由django的查询方式决定的。

- 定义属性时需要指定字段类型，通过字段类型的参数指定选项，语法如下：

  ```
  属性=models.字段类型(选项)
  ```

**4）字段类型**

| 类型             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| AutoField        | 自动增长的IntegerField，通常不用指定，不指定时Django会自动创建属性名为id的自动增长属性 |
| BooleanField     | 布尔字段，值为True或False                                    |
| NullBooleanField | 支持Null、True、False三种值                                  |
| CharField        | 字符串，参数max_length表示最大字符个数                       |
| TextField        | 大文本字段，一般超过4000个字符时使用                         |
| IntegerField     | 整数                                                         |
| DecimalField     | 十进制浮点数， 参数max_digits表示总位数， 参数decimal_places表示小数位数 |
| FloatField       | 浮点数                                                       |
| DateField        | 日期， 参数auto_now表示每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为False； 参数auto_now_add表示当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为False; 参数auto_now_add和auto_now是相互排斥的，组合将会发生错误 |
| TimeField        | 时间，参数同DateField                                        |
| DateTimeField    | 日期时间，参数同DateField                                    |
| FileField        | 上传文件字段                                                 |
| ImageField       | 继承于FileField，对上传的内容进行校验，确保是有效的图片      |

**5） 选项**

| 选项        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| null        | 如果为True，表示允许为空，默认值是False                      |
| blank       | 如果为True，则该字段允许为空白，默认值是False                |
| db_column   | 字段的名称，如果未指定，则使用属性的名称                     |
| db_index    | 若值为True, 则在表中会为此字段创建索引，默认值是False        |
| default     | 默认                                                         |
| primary_key | 若为True，则该字段会成为模型的主键字段，默认值是False，一般作为AutoField的选项使用 |
| unique      | 如果为True, 这个字段在表中必须有唯一值，默认值是False        |

**null是数据库范畴的概念，blank是表单验证范畴的**

**6） 外键**

在设置外键时，需要通过**on_delete**选项指明主表删除数据时，对于外键引用表数据如何处理，在django.db.models中包含了可选常量：

- **CASCADE**级联，删除主表数据时连通一起删除外键表中数据
- **PROTECT**保护，通过抛出**ProtectedError**异常，来阻止删除主表中被外键应用的数据
- **SET_NULL**设置为NULL，仅在该字段null=True允许为null时可用
- **SET_DEFAULT**设置为默认值，仅在该字段设置了默认值时可用
- **SET()**设置为特定值或者调用特定方法
- **DO_NOTHING**不做任何操作，如果数据库前置指明级联性，此选项会抛出**IntegrityError**异常

##### 例如 #####

```python
class BookInfo(models.Model):
    # 创建字段，字段类型...
    name = models.CharField(max_length=20, verbose_name='名称')
    pub_date = models.DateField(verbose_name='发布日期',null=True)
    readcount = models.IntegerField(default=0, verbose_name='阅读量')
    commentcount = models.IntegerField(default=0, verbose_name='评论量')
    is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')

    class Meta:
        db_table = 'bookinfo'  # 指明数据库表名
        verbose_name = '图书'  # 在admin站点中显示的名称

    def __str__(self):
        """定义每个数据对象的显示信息"""
        return self.name
```



### shell工具 ###

Django的manage工具提供了**shell**命令，帮助我们配置好当前工程的运行环境（如连接好数据库等），以便可以直接在终端中执行测试python语句。

**通过如下命令进入shell**

```python
python manage.py shell
```

**导入模型类，以便后续使用**

- 模型类被定义在"应用/models.py"文件中。
- 模型类必须继承自Model类，位于包django.db.models中。

```python
# 例如:
from book.models import BookInfo,PeopleInfo
```

### 查看MySQL数据库日志(结果集特性使用) ###

1. 查看mysql数据库日志可以查看对数据库的操作记录。 mysql日志文件默认没有产生，需要做如下配置：

   ```django
   sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

   

2. 把68，69行前面的#去除，然后保存并使用如下命令重启mysql服务。

   ```django
   sudo service mysql restart
   ```

   

3. 使用如下命令打开mysql日志文件。

   

   ```py
   tail -f /var/log/mysql/mysql.log  # 可以实时查看数据库的日志内容
   # 如提示需要sudo权限，执行
   # sudo
   ```

   

### **数据的增删改** ###

> 增:`book = BookInfo() book.save()` 和`BookInfo.objects.create()`
>
> 删:`book.delete()` 和`BookInfo.objects.get().delete()`
>
> 改:`book.name='xxx' book.save()` 和 `BookInfo.objects.get().update(name=xxx)`

------

1. **增加**

   增加数据有`save` 与 `create` 两种方法

   ```python
   #  save
   # 通过创建模型类对象，执行对象的save()方法保存到数据库中。
   >>> book = BookInfo(
   >>>          name='python入门',
   >>>         pub_date='2010-1-1'
   >>> 	)
   >>> book.save()
   book = <BookInfo: python入门>
   
   # create
   # 通过模型类.objects.create()保存
   >>> bookInfo.objects.create(name='Mysql必知必会',book=book)
   <BookInfo: Mysql必知必会>
   ```

   

2. **修改**

   修改更新有 `save` 与 `update` 两种方法

   ```python
   # save
   # 修改模型类对象的属性，然后执行save()方法
   >>> books = bookInfo.objects.get(name='海子的诗')
   >>> books.name = '时间简史'
   >>> books.save()
   <bookInfo: 时间简史>
   
   # update
   # 使用模型类.objects.filter().update()，会返回受影响的行数
   >>> bookinfo.objects.filter(name='时间简史').update(name='空间相对论')
   1
   ```

   

3. **删除**

   删除 `delete` 有两种方法

   

   ```python
   # delete  (1) 模型类对象delete
   >>> book = bookInfo.objects.get(name='算法图解')
   >>> book.delete()
   (1, {'book.bookInfo': 1})
   
   # delete (2）模型类.objects.filter().delete()
   >>> bookInfo.objects.filter(name='算法图解').delete()
   (1, {'book.BookInfo': 1, 'book.PeopleInfo': 0})
   ```

   

### 数据查询 ###

#### 基本查询 ####

**get** 查询单一结果，如果不存在会抛出 **模型类.DoesNotExist** 异常。

语法：`模型类.objects.get(条件)`

**all** 查询多个结果。

语法：`模型类.objects.all()`

**count ** 查询结果数量。

语法：`模型类.objects.count()`

```python
# get (true)
>>> bookInfo.objects.get(id = 1) 
<bookInfo: 海子的诗>

# get (false)
>>> bookInfo.objects.get(id =xxxx)
File "<console>", line 1, in <module>
File "/home/python/.virtualenvs/py3_django_1.11/lib/python3.5/site-
.......
book.models.DoesNotExist: BookInfo matching query does not exist
        
# all
>>> bookInfo.objects.all()
<QuerySet [<BookInfo: 海子的诗>, <BookInfo: 算法图解>, <BookInfo: 梵·高>]>

# count
>>> bookInfo.objects.count()
3
```

#### 过滤查询 ####

**filter**过滤出多个结果

**exclude**排除掉符合条件剩下的结果

**get**过滤单一结果

对于过滤条件的使用，上述三个方法相同，故仅以**filter**进行讲解

过滤条件的表达语法如下：

```python
属性名称__比较运算符=值

# 属性名称和比较运算符间使用两个下划线，所以属性名不能包括多个下划线
```

```python
查询编号为1的图书
查询书名包含'湖'的图书
查询书名以'诗'结尾的图书
查询书名为空的图书
查询编号为1或3或5的图书
查询编号大于3的图书
查询1980年发表的图书
查询1990年1月1日后发表的图书
```

**1）相等**

**exact：表示判等。**

例：查询编号为1的图书。

```python
BookInfo.objects.filter(id__exact=1)
# 可简写为：
BookInfo.objects.filter(id=1)
```

**2）模糊查询**

**contains：是否包含。**

> 说明：如果要包含%无需转义，直接写即可。

例：查询书名包含'传'的图书。

```python
>>> BookInfo.objects.filter(name__contains='诗')
<QuerySet [<BookInfo: 海子的诗>]>
```

**startswith、endswith：以指定值开头或结尾。**

例：查询书名以'部'结尾的图书

```python
>>> BookInfo.objects.filter(name__endswith='史')
<QuerySet [<BookInfo: 时间简史>]>
```

> 以上运算符都区分大小写，在这些运算符前加上i表示不区分大小写，如iexact、icontains、istartswith、iendswith.

**3） 空查询**

**isnull：是否为null。**

例：查询书名为空的图书。

```python
>>> BookInfo.objects.filter(name__isnull=True)
<QuerySet []>
```

**4） 范围查询**

**in：是否包含在范围内。**

例：查询编号为1或3的图书

```python
>>> BookInfo.objects.filter(id__in=[1,3])
<QuerySet [<BookInfo: 海子的诗>, <BookInfo: 梵·高>]>
```

**5）比较查询**

- **gt**大于 (greater then)
- **gte**大于等于 (greater then equal)
- **lt**小于 (less then)
- **lte**小于等于 (less then equal)

例：查询编号大于2的图书

```python
>>> BookInfo.objects.filter(id__gt=2)
<QuerySet [<BookInfo: 梵·高>]>
```

**不等于的运算符，使用exclude()过滤器。**

例：查询编号不等于3的图书

```python
>>> BookInfo.objects.filter(id__gt=3)
<QuerySet [<BookInfo: >]>
```

**6）日期查询**

**year、month、day、week_day、hour、minute、second：对日期时间类型的属性进行运算。**

例：查询2009年发表的图书。

```python
>>> BookInfo.objects.filter(pub_date__year=2009)
<QuerySet [<BookInfo: 海子的诗>]>
```

例：查询1990年1月1日后发表的图书。

#### F和Q对象 ####

##### 1.F对象 #####

之前的查询都是对象的属性与常量值比较，两个属性怎么比较呢？ 

答：使用F对象，被定义在django.db.models中。

语法： `F(属性名)`

例：查询阅读量大于等于评论量的图书。

```python
>>> from django.db.models import F
>>> BookInfo.objects.filter(readcount__gt=F('commentcount'))
<QuerySet [<BookInfo: 海子的诗>, <BookInfo: 时间简史>]>
```

可以在F对象上使用算数运算。

例：查询阅读量大于2倍评论量的图书。

```python
>>> BookInfo.objects.filter(readcount__gt=F('commentcount')*2)
<QuerySet [<BookInfo: 时间简史>]>
```

##### 2.Q对象 #####

**多个过滤器逐个调用表示逻辑与关系，同sql语句中where部分的and关键字。**

例：查询阅读量大于20，并且编号小于3的图书。

```python
>>> BookInfo.objects.filter(readcount__gt=20,id__lt=3)
<QuerySet [<BookInfo: 梵·高>]>

# or

>>> BookInfo.objects.filter(readcount__gt=20).filter(id__lt=3)
<QuerySet [<BookInfo: 梵·高>]>
```

**如果需要实现逻辑或or的查询，需要使用Q()对象结合|运算符**，Q对象被义在django.db.models中。

语法：`Q(属性名__运算符=值)`

例：查询阅读量大于20的图书，改写为Q对象如下。

```python
BookInfo.objects.filter(Q(readcount__gt=20))
```

Q对象可以使用&、|连接，&表示逻辑与，|表示逻辑或。

例：查询阅读量大于20，或编号小于3的图书，只能使用Q对象实现

```python
>>> BookInfo.objects.filter(Q(readcount__gt=20)|Q(id__lt=3))
<QuerySet [<BookInfo: 海子的诗>, <BookInfo: 时间简史>, <BookInfo: 梵·高>]>
```

Q对象前可以使用~操作符，表示非not。

例：查询编号不等于3的图书。

```python
>>> BookInfo.objects.filter(~Q(id=3))
<QuerySet [<BookInfo: 海子的诗>, <BookInfo: 时间简史>]>
```

#### 聚合函数和排序函数 ####

##### 聚合函数 #####

- 使用aggregate()过滤器调用聚合函数。
- `aggregate`不会返回一个`QuerySet`对象，而是返回一个字典。
- 聚合函数不能单独执行，需放在可以执行聚合函数的`aggregate`与`annotate`方法中去执行。
- 聚合函数包括：**Avg**平均，**Count**数量，**Max**最大，**Min**最小，**Sum**求和，被定义在django.db.models中。

```python
BookInfo.objects.aggregate(my_avg=AVG('price'))
```

注意aggregate的返回值是一个字典类型，格式如下：

```python
  {'属性名__聚合类小写':值}

  如:{'readcount__sum': 126}
```

使用count时一般不使用aggregate()过滤器。

例：查询图书总数。

```python
BookInfo.objects.count()
```

注意count函数的返回值是一个数字。

##### 排序 #####

使用**order_by**对结果进行排序

```python
# 默认升序
>>> BookInfo.objects.all().order_by('readcount')

# 降序
>>> BookInfo.objects.all().order_by('-readcount')
```

#### 级联查询 ####

##### 1.关联查询 #####

```python
查询书籍为1的所有人物信息
查询人物为1的书籍信息
```

**由一到多的访问语法**：

一对应的模型类对象.多对应的模型类名小写_set 例：

```python
>>> book = BookInfo.objects.get(id=1)
>>> book.peopleinfo_set.all()
```

**由多到一的访问语法**:

多对应的模型类对象.多对应的模型类中的关系类属性名 例：

```python
>>> person = PeopleInfo.objects.get(id=1)
>>> person.book
<BookInfo: 海子的诗>
```

访问一对应的模型类关联对象的id语法:

多对应的模型类对象.关联类属性_id

例：

```python
>>> person = PeopleInfo.objects.get(id=1)
>>> person.book_id
1
```

#### 2.关联过滤查询 ####

**由多模型类条件查询一模型类数据**:

语法：`关联模型类名小写__属性名__条件运算符=值`

> **注意：如果没有"__运算符"部分，表示等于。**

```
查询图书，要求图书人物为"梵高"
查询图书，要求图书中人物的描述包含"绘画"
```

例：查询图书，要求图书人物为"梵高"

```py
>>> book = BookInfo.objects.filter(peopleinfo__name='梵高')
>>> book
<QuerySet [<BookInfo: 梵·高>]>
```

查询图书，要求图书中人物的描述包含"绘画"

```py
>>> book = BookInfo.objects.filter(peopleinfo__description__contains='绘画')
>>> book
<QuerySet [<BookInfo: 梵·高>]>
```

**由一模型类条件查询多模型类数据**:

语法如下：`一模型类关联属性名__一模型类属性名__条件运算符=值`

> **注意：如果没有"__运算符"部分，表示等于。**

```
查询书名为“时间简史”的所有人物
查询图书阅读量大于30的所有人物
```

例：查询书名为“时间简史”的所有人物。

```py
>>> people = PeopleInfo.objects.filter(book__name='时间简史')
```

查询图书阅读量大于30的所有人物

```py
>>> people = PeopleInfo.objects.filter(book__readcount__gt=30)
```

#### 查询集QuerySet ####

##### 1 概念 #####

Django的ORM中存在查询集的概念。

查询集，也称查询结果集、QuerySet，表示从数据库中获取的对象集合。

当调用如下过滤器方法时，Django会返回查询集（而不是简单的列表）：

- all()：返回所有数据。
- filter()：返回满足条件的数据。
- exclude()：返回满足条件之外的数据。
- order_by()：对结果进行排序。

对查询集可以再次调用过滤器进行过滤，如

```python
>>> books = BookInfo.objects.filter(readcount__gt=30).order_by('pub_date')
>>> books
<QuerySet [<BookInfo: 海子的诗>, <BookInfo: 时间简史>]>
```

也就意味着查询集可以含有零个、一个或多个过滤器。过滤器基于所给的参数限制查询的结果。

**从SQL的角度讲，查询集与select语句等价，过滤器像where、limit、order by子句。**

**判断某一个查询集中是否有数据**：

- exists()：判断查询集中是否有数据，如果有则返回True，没有则返回False。

##### 2 两大特性 #####

###### 2.1惰性执行 ######

创建查询集不会访问数据库，直到调用数据时，才会访问数据库，调用数据的情况包括迭代、序列化、与if合用

例如，当执行如下语句时，并未进行数据库查询，只是创建了一个查询集books

```python
books = BookInfo.objects.all()
```

继续执行遍历迭代操作后，才真正的进行了数据库的查询

```python
for book in books:
    print(book.name)
```

##### 2.2缓存 #####

使用同一个查询集，第一次使用时会发生数据库的查询，然后Django会把结果缓存下来，再次使用这个查询集时会使用缓存的数据，减少了数据库的查询次数。

**情况一**：如下是两个查询集，无法重用缓存，每次查询都会与数据库进行一次交互，增加了数据库的负载。

```python
from book.models import BookInfo

 [book.id for book in BookInfo.objects.all()]

 [book.id for book in BookInfo.objects.all()]
```



**情况二**：经过存储后，可以重用查询集，第二次使用缓存中的数据。

```python
books=BookInfo.objects.all()

[book.id for book in books]

[book.id for book in books]![img](../../assets/sql_cache.png)
```

##### 3 限制查询集 #####

可以对查询集进行取下标或切片操作，等同于sql中的limit和offset子句。

> 注意：不支持负数索引。

**对查询集进行切片后返回一个新的查询集，不会立即执行查询。**

如果获取一个对象，直接使用[0]，等同于[0:1].get()，但是如果没有数据，[0]引发IndexError异常，[0:1].get()如果没有数据引发DoesNotExist异常。

示例：获取第1、2项，运行查看。

```python
>>> books = BookInfo.objects.all()[0:2]
>>> books
<QuerySet [<BookInfo: 海子的诗>, <BookInfo: 时间简史>]>
```

##### 4.分页 #####

Django提供了一个新的类来帮助你管理分页数据，这个类存放在`django/core/paginator.py`.它可以接收列表、元组或其它可迭代的对象。

```python
#查询数据
books = BookInfo.objects.all()
#导入分页类
from django.core.paginator import Paginator
#创建分页实例
paginator=Paginator(books,2)
#获取指定页码的数据
page_books = paginator.page(1)
#获取分页数据
total_page=paginator.num_pages
```