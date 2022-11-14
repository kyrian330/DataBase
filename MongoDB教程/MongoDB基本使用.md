### 一、MongoDB的安装



1.桌面新建文件夹data，data里再建文件夹db。

![2](..\MongoDB教程\img\2.png)





2.将mongodb3.4.rar解压到电脑的某个盘。

![1](..\MongoDB教程\img\1.png)



3.进入mongodb3.4的解压路径。就是 {{你的路径}}\bin，点击上方路径。

![3](..\MongoDB教程\img\3.png)



点击后输入cmd，点击enter，就可以进入当前目录的命令行，省去切盘之类的命令。

![4](..\MongoDB教程\img\4.png)



![5](..\MongoDB教程\img\5.png)



4.在当前窗口输入命令 `mongod.exe --dbpath {{你的数据路径}}`，数据路径就是第一步创建的db的路径。

![6](..\MongoDB教程\img\6.png)



成功后如下图，不要关闭这个后台，最小化窗口(点击右上角`-`号)。

![7](..\MongoDB教程\img\7.png)



5.bin目录下再次输入cmd，打开另一个后台。

![8](..\MongoDB教程\img\8.png)



进入后先输入`mongo`，启动程序；启动成功后输入两个数字相加，看是否可用。

![9](..\MongoDB教程\img\9.png)





### 二、增删查改

- 创建数据库

```mysql
use database_name
```

如果数据库不存在则会自动创建，否则会切换到指定库。



例，创建数据库，数据库名为 `mydb`。

```mysql
> use mydb
switched to db mydb
> db
mydb
>
```



- 查看所有数据库

```mysql
show dbs
```

例

```mysql
> show dbs
admin  0.000GB
local  0.000GB
>
```



我们发现刚刚创建的数据库mydb并不在其中，要显示mydb则需要向里面插入数据。

```mysql
db.mydb.insert({"name":"MongoDB教材", "author":"草莓夹心糖酱"})
```

- 结果

```mysql
> db.mydb.insert({"name":"MongoDB教材", "author":"草莓夹心糖酱"})
WriteResult({ "nInserted" : 1 })
> show dbs
admin  0.000GB
local  0.000GB
mydb   0.000GB
>
```

MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。



##### 1.插入

MongoDB 使用 insert() 或 save() 方法向集合中插入文档。

（注：复制后在命令行窗口**点击右键**即可进行粘贴）

```mysql
db.COLLECTION_NAME.insert(document)
```



- 输入

```mysql
db.mydb.insert({"description":"尝试插入一条数据", "author":"草莓夹心糖酱", "date":"2022/11/4"})
```



- 结果

```mysql
> db.mydb.insert({"description":"尝试插入一条数据", "author":"草莓夹心糖酱", "date":"2022/11/4"})
WriteResult({ "nInserted" : 1 })
>
```



- 查询

```mysql
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB教材", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
```

------

也可以先将数据整合成变量，再执行插入操作，如下：

```mysql
doc=({title: '个人信息', 
	age: '20',
	address: '南宁师大',
	stu: '17',
	score: 99
});
```

在命令行执行后结果如下

```mysql
{
        "title" : "个人信息",
        "age" : "20",
        "address" : "南宁师大",
        "stu" : "17",
        "score" : 99
}
>
```

接着执行插入操作

```mysql
db.mydb.insert(doc)
```

- 结果

```mysql
> db.mydb.insert(doc)
WriteResult({ "nInserted" : 1 })
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB教材", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
{ "_id" : ObjectId("63650e35ca244cd6d086571b"), "title" : "个人信息", "age" : "20", "address" : "南宁师大", "stu" : "17", "score" : 99 }
>
```



##### 2.删除

remove()方法的语法如下

```mysql
db.collection.remove(
	<query>,
	{
		justOne: <boolean>,
		writeConcern: <document>
	}
)

```

- 参数说明

- **query** :（可选）删除的文档的条件。
- **justOne** : （可选）如果设为 true 或 1，则只删除一个文档。
- **writeConcern**  :（可选）抛出异常的级别。

------



例，

删除之前插入的数据

```mysql
db.mydb.remove({"title" : "个人信息", "age" : "20", "address" : "南宁师大", "stu" : "17", "score" : 99 })
```



- 结果

```mysql
> db.mydb.remove({"title" : "个人信息", "age" : "20", "address" : "南宁师大", "stu" : "17", "score" : 99 })
WriteResult({ "nRemoved" : 1 })
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB教材", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
>
```

------



插入两条相同数据：

```mysql
db.mydb.insert({"title":"测试数据", "index":"1"})

db.mydb.insert({"title":"测试数据", "index":"1"})
```



显示：

```mysql
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB教材", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
{ "_id" : ObjectId("63650fb9ca244cd6d086571c"), "title" : "测试数据", "index" : "1" }
{ "_id" : ObjectId("63650fbaca244cd6d086571d"), "title" : "测试数据", "index" : "1" }
>
```



执行删除命令：

```mysql
db.mydb.remove({"title" : "测试数据", "index" : "1"})
```



显示如下，WriteResult为2，说明一下子就删除了两条数据。

```mysql
> db.mydb.remove({"title" : "测试数据", "index" : "1"})
WriteResult({ "nRemoved" : 2 })
>
```



如果你只想删除第一条找到的记录可以设置 justOne 为 1，如下：

再次插入数据：

```mysql
db.mydb.insert({"title":"测试数据", "index":"2"})

db.mydb.insert({"title":"测试数据", "index":"2"})
```

显示：

```mysql
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB教材", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
{ "_id" : ObjectId("63651110ca244cd6d086571e"), "title" : "测试数据", "index" : "2" }
{ "_id" : ObjectId("63651110ca244cd6d086571f"), "title" : "测试数据", "index" : "2" }
>
```



删除一条数据：

```mysql
db.mydb.remove({"title" : "测试数据", "index" : "2"}, 1)
```

- 结果

```mysql
> db.mydb.remove({"title" : "测试数据", "index" : "2"}, 1)
WriteResult({ "nRemoved" : 1 })
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB教材", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
{ "_id" : ObjectId("63651187ca244cd6d0865721"), "title" : "测试数据", "index" : "2" }
>
```



##### 3.更新

###### update() 方法

update() 方法用于更新已存在的文档。语法格式如下：

```mysql
db.collection.update(
	<query>,
	<update>,
	{
		upsert: <boolean>,
		multi: <boolean>,
		writeConcern: <document>
	}
)
```

- 参数说明

- **query** : update的**查询条件**，类似sql update查询内where后面的。
- **update** : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- **upsert**  : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- **multi**  : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- **writeConcern**  :可选，抛出异常的级别。



例，

原有的数据：

```mysql
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB教材", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
{ "_id" : ObjectId("63651187ca244cd6d0865721"), "title" : "测试数据", "index" : "2" }
>
```



输入：

```mysql
db.mydb.update({"name" : "MongoDB教材", "author" : "草莓夹心糖酱"},{$set:{'name':'MongoDB'}})
```



- 结果

```mysql
> db.mydb.update({"name" : "MongoDB教材", "author" : "草莓夹心糖酱"},{$set:{'name':'MongoDB'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
>
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
{ "_id" : ObjectId("63651187ca244cd6d0865721"), "title" : "测试数据", "index" : "2" }
>
```

我们可以发现 name属性改成了MongoDB，而author属性没有改变。

------



如果要修改多条相同的文档，则需要设置 multi 参数为 true。

```mysql
db.col.update({"title":"测试数据"， "index" : "2"},{$set:{"title":"测试数据"， "index" : "6"}},{multi:true})
```



###### sava()方法

save() 方法通过传入的文档来替换已有文档。格式如下：

```mysql
db.collection.save(
	<document>,
	{
		writeConcern: <document>
	}
)
```

- 参数说明

- **document** : 文档数据。



例

替换了 _id 为 63651187ca244cd6d0865721 的文档数据：

```mysql
db.mydb.save({
             "_id": ObjectId("63651187ca244cd6d0865721"),
             "title": "测试数据",
             "index": "3"
})
```



查询：

```mysql
> db.mydb.find()
{ "_id" : ObjectId("636509091d3c25588c5b748f"), "name" : "MongoDB", "author" : "草莓夹心糖酱" }
{ "_id" : ObjectId("63650b34d325c96a65a37189"), "description" : "尝试插入一条数据", "author" : "草莓夹心糖酱", "date" : "2022/11/4" }
{ "_id" : ObjectId("63651187ca244cd6d0865721"), "title" : "测试数据", "index" : "3" }
```





##### 4.查询

find() 方法以非结构化的方式来显示所有文档。

```mysql
db.COLLECTION_NAME.find()
```



使用pretty() 方法，使得数据更加直观。

```mysql
db.col.find().pretty()
```



例

```mysql
> db.mydb.find().pretty()
{
        "_id" : ObjectId("636509091d3c25588c5b748f"),
        "name" : "MongoDB",
        "author" : "草莓夹心糖酱"
}
{
        "_id" : ObjectId("63650b34d325c96a65a37189"),
        "description" : "尝试插入一条数据",
        "author" : "草莓夹心糖酱",
        "date" : "2022/11/4"
}
{
        "_id" : ObjectId("63651187ca244cd6d0865721"),
        "title" : "测试数据",
        "index" : "3"
}
>
```
