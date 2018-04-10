# 服务器配置MongoDB

## 安装MogoDB

和 [MongoDB基础操作](https://github.com/vagrantgrapefruit/My-Word/blob/master/Blog%20for%20me/MongoDB/MongoDB.md)中步骤相同。

## 远程访问配置

在mongod.cfg文件中添加

    bind_ip = 0.0.0.0//允许访问的IP，0.0.0.0代表所有

在mongoshell中设置用户名和密码，以及用户对应的角色

默认角色类别
![权限类别](https://github.com/vagrantgrapefruit/My-Word/blob/master/contant/MongoDB角色.png)

### [**用户管理方法**](https://docs.mongodb.com/manual/reference/method/js-user-management/)

### [**角色管理方法**](https://docs.mongodb.com/manual/reference/method/js-role-management/)

- 示例
```
use admin
db.createUser({user:'user',
                pwd:'pwd',
                roles:[{ role: "userAdminAnyDatabase", db: "admin" } ]
                })
use test
db.grantRoleToUser('user',['readWrite','dbAdmin','userAdmin'])

```

## 远程连接字符串

mongodb://[< user>:<PWD>]@IPaddress:portal" 
- 示例
   mongodb://sa:123456@172.0.0.1:27017"  