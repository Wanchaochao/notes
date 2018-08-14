## 允许任意IP地址的客户端连接mysql

```
确认3306端口开放,并且服务器可以外网访问
```

#### 方法一： 将root用户的权限改成任何主机都能访问：

* use mysql;
* select host,user,password from user;
* update user set host='%' where host='localhost';
* flush privileges;

#### 直接通过mysql命令，创建数据库，用户信息，以及客户端主机访问权限设置。

* create user username identified by "password"; 
* grant all privileges on *.*to'username'@'%'identified by 'password' with grant option;
* flush privileges;


#### 重点要注意mysql中是否有匿名用户

```
如: 
user  | host | password
      | %    | 

这样的用户
```