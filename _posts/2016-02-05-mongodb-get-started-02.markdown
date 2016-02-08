---
layout: post
title: "mongodb 入门 -- shell 基本操作"
date: 2016-02-04 08:00:16 +800
categories: mongodb
---

## 基本操作

### 创建

    > item = {
        "name": "MoYu xxxx",
        "brand": "MoYu",
        "color": "white",
        "price": "$32.00"
    }

编辑一个文档，使用 insert 方法将其插入到 products 集合中去

    db.products.insert(item);

看是是否插入成功

    > db.products.findOne()
    {
        "_id": ObjectId("56b36b5de80119c7791914c6"),
        "name": "MoYu xxxx",
        "brand": "MoYu",
        "color": "white",
        "price": "$32.00"
    }
    
### 批量插入

批量插入使用 batchInsert, 用法与 insert 类似，接受一个文档数组作为参数

    > db.foo.batchInsert([{"id": 1}, {"id":2}, {"id": 3}])

一次插入多个文档效率会高很多

### 读取

find 和 findOne 方法用于查询集合中的文档，findOne 只查看一条，find 一次最多查看20条

可以传入一个查询文档作为限制条件

    > db.products.find({"brand": "MoYu"})
    
这条命令会查询出所有 魔域品牌的魔方

### 更新

update 最少接受两个参数，待更新的文档，新的文档

update 常规更新是用新的文档文档完全替换掉旧的文档，适用于大规模迁移的情况

另一种是使用修改器对文档进行局部更新

#### $set 修改器

$set 用来指定一个字段的值，如果这个字段不存在就创建它。
    
    > db.products.findOne()
    {
        "_id" : ObjectId("56b43587e48157aa97a9ed79"),
        "id" : "1",
        "brand" : "ShengShou",
        "itemNo" : "7080A-1",
        "itemName" : "ShengShou 2x2x2 with matte stick",
        "color" : "black,white"
    }

下面为其添加一个包装信息

    > db.products.update({"id": "1"}, {"$set": {"package": "color box"}})

之后文档就有 package 信息了

    > db.products.findOne()
    {
        "_id" : ObjectId("56b43587e48157aa97a9ed79"),
        "id" : "1",
        "brand" : "ShengShou",
        "itemNo" : "7080A-1",
        "itemName" : "ShengShou 2x2x2 with matte stick",
        "color" : "black,white",
        "package": "color box"
    }
    
修改 package 信息

    > db.products.update({"id": "1"}, {"$set": {"package": "plastic box"}})
    
删除 package 信息

    > db.product.update({"id": "1"}, {"$unset": {"package": 1}})
    
#### $inc 修改器

$inc 用来增加已有键的值，如果该键不存在就创建一个

    > db.products.update({"id": 1}, {"$inc": {"storage": 50}})
    
为产品添加一个库存信息，下次库存增加100个

    > db.products.update({"id": 1}, {"$inc": {"storage": 100}})
    
    > db.products.getOne()
    {
        "_id" : ObjectId("56b43587e48157aa97a9ed79"),
        "id" : "1",
        "brand" : "ShengShou",
        "itemNo" : "7080A-1",
        "itemName" : "ShengShou 2x2x2 with matte stick",
        "color" : "black,white",
        "package": "color box",
        "storage": 150
    }

$inc 修改的键的值必须为数值类型，否则会出现错误

### 数组修改器

$push 会向已有的数组末尾添加一个元素，要是没有就创建一个新的数组

    > db.foo.update({"id": 1}, {"$push": {"contacts": {"email": "walle@gmail.com"}}})

向联系方式数组中添加了一条邮箱信息
    
也可以 添加复杂嵌套的文档

配合 $each 子操作符来进行复杂的数组操作

    > db.foo.update({"id": 1}, {"$push": {"$each": {"names": ["eva", "walle", "diascubes"]}}})

    > db.foo.findOne()
    {
        "id": 1,
        "names": ["eva", "walle", "diascubes"]
    }
    
还有 $slice 子修改器来防止数组超出最大长度， $sort 对数组中的对象进行排序， 具体查阅相关文档

#### 将数组作为数据集使用

$ne 配合 $push 使用，避免插入重复的值

    > db.papers.update({"author": {"$ne": "Richie"}}, {"$push": {"author": "Richie"}})

如果 Richie 不在作者列表中，就添加进去

$addToSet 

    > db.papers.update({"title": "111"}, {"$addToSet": {"author": {"$each":["hah, heh"]}}})    

用 $addToSet 插入多条数据到 author 列表中， 列表中已经存在的不会重复插入

#### 删除元素

$pop 从数组的两端删除一个元素 

    > {"$pop": {"key": 1}}

从数组尾部删除一个元素

    > {"$pop": {"key": -1}}
    
从数组头部删除一个元素

$pull 基于特定条件删除

    > db.foo.insert({"names": ["walle", "eva", "tom"]})
    > db.foo.update({}，{"$pull": {”names“: "walle"}})

walle 会从 names 数组中被移除掉

### 删除

使用 remove 可将文档从数据库中永久删除， 如果没有使用参数，会删除集合内的所有文档，但不会删除集合本身

    > db.products.remove({"name": "MoYu xxxx"})
    
这条数据就被永久的删除了

如果要删除整个集合，直接使用 drop

    > db.products.drop()
    
### upsert（update的第三个参数）

如果没有找到符合更新条件的文档，就会以这个条件和更新文档为基础创建一个新的文档，否则，正常更新。

比如，记录网站访问次数，通过url查询，如果存在就增加访问次数，否则就新建一个文档

    > db.site.update({'url': '/blog'}, {'$inc': {'pageviews': 1}}, true);
    
### $setOnInsert，只在创建时赋值，之后不再更新

比如创建时间这个字段，只需要创建的时候赋值，后面无需对其进行修改，就可以使用$setOnInsert

    > db.users.update({}, {"$setOnInsert": {"createTime": new Date()}}, true)
    
### save函数

save是一个shell函数，如果文档不存在就会创建，否则就更新该文档，它只有一个参数，就是文档。如果该文档有_id键就会执行upsert，否则就会调用insert。

    > var x = db.foo.findOne()
    > x.num = 42
    > db.foo.save(x)
    
### 更新多个文档

update默认只更新第一个匹配的文档，要更新所有匹配到的文档，可以将update的第四个参数设置为true，只能使用 $set 修改器

    > db.users.update({'birthday': '10/13/1988'}, {$set: {'gift': 'Happy birthday'}}, false, true)

要想知道更新了多少个文档，可以运行 getLastError 命令 （返回最后一次操作的相关信息）

    > db.runCommand({getLastError: 1})
    {
        "err": 1,
        "updatedExisting": true,
        "n": 5,
        "ok": true
    }
    
