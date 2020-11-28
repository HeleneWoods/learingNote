MongoDB是一个由C++编写的数据库，是专门为了web应用而产生的存储的解决方案。

简单来说，它是专门给前端开发使用的数据库，它存储数据的格式就是json。

## MongoDB的基本命令

* `cls`--清屏
* `show dbs`--显示所有的库
* `use 库名`--使用某一个库
* `db`--显示当前正在操作的数据库名
* `show collections`--显示当前库中的集合
* `db.库名.find()`--列出集合中的所有数据
* `use`--创建一个新的数据库，当库中没有数据时，使用show dbs无法显示
* `db.createCollection('集合名')`--创建集合（在插入数据时，会在库里自动创建集合
* `db.集合名.insert({'id':123,txt:'安装MongoDBXX'})`--插入一条数据
* `db.集合名.remove({'id':123})`--删除一条数据，至少需要一个条件
* `db.集合名.save({'id':xxx})`--更新数据（也可使用update），至少需要一个条件；如果id相同，那么就覆盖
* `db.集合名.drop()`--删除整个集合
* `db.dropDatabase()`--删除整个数据库

## node访问MongoDB

### 固定操作

* 在node中间件里安装MongoDB--先全局安装，再局部安装
* `const MongoClient=require('mongodb').MongoClient;`--引入MongoDB模块并获得客户端对象
* `const DB_CONN_STR = 'mongodb://127.0.0.1:27017/';`--获取联机MongoDB的字符串，变量名也是固定的

### 获取数据

设置一个get接口来访问MongoDB数据库，并返回数据

```javascript
app.get('/mongodb_data',(req.res)=>{
    //连接数据库
    MongoClient.connect(DB_CONN_STR,(err,result)=>{
        let _dbo=db.db('数据库名');
        let _collection=_dbo.collection('集合名');
        _collection.find().toArray(err,result)=>{
            if(err) throw err;
            res.send(result);
             //关闭数据库连接  
            db.close();
        })
    })
})
```

### 增加数据

设置一个get接口来获取客户传过来的数据，用`insertOne()`方法来给MongoDB插入数据，这一步是增加数据

```javascript
app.get('/insert_data',(req.res)=>{
    let _msgObj=req.query;
    MongoClient.connect(DB_CONN_STR,(err,result)=>{
        let _dbo=db.db('数据库名');
        let _collection=_dbo.collection('集合名');
        _collection.insertOne(_msgOBj,(err,result)=>{
            if(err) throw err;
            console.log('插入数据成功');
            res.end();
            db.close();
        })
    })
})
```

### 删除数据

删除数据首先要在客户端获取到需要删除的数据的`_id`，通过上一步的相同的传值方法，让node接收到`_id`，但在MongoDB里`_id`不是一个字符串，而是一个对象，需要通过MongoDB内部方法生成一个id对象，再通过`remove()`方法删除数据

```javascript
app.get('/del_data',(req,res)=>{
    let _idString=req.query._id;
    //生成id对象
    const _ObjectID = require('mongodb').ObjectID;
    let _ObjId=_ObjectID.createFromHexString(_idString);
    
   MongoClient.connect(DB_CONN_STR,(err,db)=>{
       let _dbo=db.db('数据库名');
       let _collection=_dbo.collection('集合名');
       _collection.remove({'_id':_ObjectID},(err,result)=>{
           if (err) throw err;
            console.log('删除数据成功');
            res.end();
            db.close();
       })
   })
})
```

### 查找数据

将查找条件通过传值传入到node，使用`findOne()`方法查找到并返回数据。

#### 联表查询

```javascript
collection.aggregate([{
    $lookup:{
        localField:'通过当前集合的某个字段开始查找',
        from:'与字段的值相同的集合名',
        foreignField:'通过新集合的某个字段查找',
        as:'搜索出来的结果放在以这个命名的地方'
    }
}])
```

### 修改数据

主要分为两步：

1. 获得之前的内容
   * 通过将`_id`传入到node，使用`findOne()`方法查找到并返回数据，和查找数据步骤差不多，只是需要传入的值不一样。
   * 将返回的值传入到前端，将返回值全都保存下来，以便修改时一并传回到node里
2. 对内容进行修改
   * 将修改后的数据以及剩下的数据一并传入node
   * 将id转化为id对象，使用`save()`或`update()`方法对数据进行整条更新，而不是只对一个属性进行更新
   * 更新前端页面数据

## 实战

### 分页接口

前端传入需要跳过的数据数量和需要查询的数据集合名。

`collection.find().limit(2).skip(Number(_skipNum)).toArray()`

`limit()`--限定查找与参数相同数量的数据，上例是2，就查找两条数据；

`skip()`--指定跳过与参数相同数量的数据，参数必须是Number类型

`toArray()`--转换成数组类型，传入一个回调函数作为参数，处理查找到的数据结果。