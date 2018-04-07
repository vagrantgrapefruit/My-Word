# MongoDB 配置与使用


>大多数情况下不区分“”和‘’

>基本结构：db>collection>doc
## MongoDB的安装

> MongoDB官方已经不支持32位系统

1. 下载MongoDB  [官方链接](https://www.mongodb.com/download-center#community)

2. 安装MongoDB（建议安装在某个盘的根目录下便于操作）

3. 创建数据文件目录（建议C:/data/db）

4. 启动MongoDB 
    >cd c:\mongoDB
    >mongod.exe --dbpath c:/data

5. 配置并使用服务来启动mongoDB
    创建日志目录
    >c:\data\log
    创建配置文件
    >C:\mongodb\mongod.cfg

    >内容：
    >>logpath=c:\data\log\mongod.log
    >>
    >>dbpath=c:\data\db
    >>
    >>logappend=true
    >>
    >>journal=true # 启用日志文件，默认启用
    >>
    >>quiet=true # 这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为 false
    >>
    >>port=27017 # 端口号 默认为 27017

6. 安装服务(将MongoDb配置为服务)
    >"C:\mongodb\bin\mongod.exe" --config "C:\mongodb\mongod.cfg" --install

    >服务启动与停止
    >
    >net start/stop mongodb


## 基本使用(MongoShell)
- 查看数据库/集合
    >show dbs/collections
- 创建/切换数据库
    >use [DBName]
- 创建集合
    >db.createCollection([name], [options])
    ![option图示](https://github.com/vagrantgrapefruit/schoolStudy/blob/master/contant/mongoDB1.png)
- 删除数据库
    >use [DBName]
    >
    >db.dropDatabase()
- 删除集合
    >use [DBName]
    >
    >db.[COLLECTION_NAME].drop()
- 插入文档(文档内容为键值对)
    >文档的数据结构和JSON基本一样。
    >所有存储在集合中的数据都是BSON格式。
    >BSON是一种类json的一种二进制形式的存储格式,简称Binary JSON。

    >db.[COLLECTION_NAME].insert(document)
    >
    >db.collection.insertOne(document):向指定集合中插入一条文档数据
    >
    >db.collection.insertMany(document):向指定集合中插入多条文档数据
    - 定义BSON类型的变量
        >变量声明前面可以加var也可以不加
        >>重复赋值会覆盖变量的内容
    ```
    document=({
        Name:'游子佳'，
        Sex:'男'，
        Age:18
    });
    ```
    - 示例
    ```
        using test
        var document=({Name:'游子佳',Sex:'男',Age:18})
        db.test.insert(document)
        db.test.find().pretty()
    ```
- 查询
    ```
    >use [DBName]
    >db.[CollectionName].find(query, projection)
    ```
    >query ：可选，使用查询操作符指定查询条件
    >
    >projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。
    ![参数说明](https://github.com/vagrantgrapefruit/schoolStudy/blob/master/contant/mongoDB4.png)
    - 示例
    ```
        var document=({Name:'小方',Sex:'男',Age:18})
        db.test.insert(document)
        document.Name='dalao'
        document.Sex='女'
        db.test.insert(document)
        db.test.find({Sex:'男'}).pretty()

    ```

- 删除
    ```
    >use [DBName]
    >db.[CollectionName].remove(<query>,
        {
            justOne: <boolean>,
            writeConcern: <document>
        })
    ```
    >query :（可选）删除的文档的条件。
    >
    >justOne : （可选）如果设为 true 或 1，则只删除一个文档。
    >
    >writeConcern :（可选）抛出异常的级别。
    ![异常级别](https://github.com/vagrantgrapefruit/schoolStudy/blob/master/contant/mongoDB5.png)
    - 示例
    ```
        db.test.remove({Name:'dalao'})
    ```
    ```删除全部
        db.test.remove({})
    ```
- 更新文档
    ### Update()
    ```
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
    ![参数说明](https://github.com/vagrantgrapefruit/schoolStudy/blob/master/contant/mongoDB2.png)
    - 示例
    ```
    db.test.update({Name:'dalao'},{$set:{Sex:'男'}})
    ```

    ### Save()
    
- 