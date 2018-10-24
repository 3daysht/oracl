# 实验2：用户管理 - 掌握管理角色、权根、用户的能力，并在用户之间共享对象。

## 用户名：本地角色wyview，用户wy

## 实验分析：
- 在pdborcl插接式数据中创建一个新的本地角色wyview，该角色包含connect和resource角色，同时也包含CREATE VIEW权限，这样任何拥有wyview的用户就同时拥有这三种权限。
- 创建角色之后，再创建用户wy，给用户分配表空间，设置限额为50M，授予wyview角色。
- 最后测试：用新用户wy连接数据库、创建表，插入数据，创建视图，查询表和视图的数据。

## 实验过程


- 第1步：以system登录到pdborcl，创建角色wyview和用户wy，并授权和分配空间：

```sql
$ sqlplus system/123@pdborcl
SQL> CREATE ROLE wyview;
Role created.
SQL> GRANT connect,resource,CREATE VIEW TO wyview;
Grant succeeded.
SQL> CREATE USER wy IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
User created.
SQL> ALTER USER wy QUOTA 50M ON users;
User altered.
SQL> GRANT wyview TO wy;
Grant succeeded.
SQL> exit
```

- 第2步：新用户wy连接到pdborcl，创建表mytable和视图myview，插入数据，最后将myview的SELECT对象权限授予hr用户。

```sql
$ sqlplus wy/123@pdborcl
SQL> show user;
USER is "WY"
SQL> CREATE TABLE mytable (id number,name varchar(50));
Table created.
SQL> INSERT INTO mytable(id,name)VALUES(1,'zhang');
1 row created.
SQL> INSERT INTO mytable(id,name)VALUES (2,'wang');
1 row created.
SQL> CREATE VIEW myview AS SELECT name FROM mytable;
View created.
SQL> SELECT * FROM myview;
NAME
--------------------------------------------------
zhang
wang
SQL> GRANT SELECT ON myview TO hr;
Grant succeeded.
SQL>exit
```

- 第3步：用户hr连接到pdborcl，查询wy授予它的视图myview

```sql
$ sqlplus hr/123@pdborcl
SQL> SELECT * FROM wy.myview;
NAME
--------------------------------------------------
zhang
wang
SQL> exit
```

- 第4步：用户wy将视图mview权限赋予给xiaoqingyu用户，并在xiangqingyu用户上查看
```sql
$ sqlplus wy/123@pdborcl
SQL> GRANT SELECT ON myview TO xiaoqingyu;
SQL> exit
$ sqlplus xiaoqingyu/123@pdborcl
SQL> SELECT * FROM wy.myview;
NAME
--------------------------------------------------
zhang
wang
SQL> exit
```
