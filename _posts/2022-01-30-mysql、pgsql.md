## mysql、pgsql常用命令
### mysql
+ mysql终端命令  
```shell
#查看表的结构
desc test;
#查看表的创表语句
show create table test;
#查看表的索引
show index from test;
#查看sql语句的进程
show processlist;
#杀掉某些进程
kill id;
#dump备份数据库
mysqldump -u root -p dbname > xxx.db
#还原某数据库
mysql dbname -u usr -p < xxx.db
#不登陆到终端执行sql语句
mysql -u usr -p'passwd' -se "select * from test;"
#跨库重命名某表，相当于mv
rename table dbname.test to dbname1.test;
#导入以'|'分割的文本数据到表中
load data infile '1.txt' into table test fields terminated by '|';
```
+ mysql的sql语句  
```sql
#update join case when语句
update test inner join test1 on test.id = tets1.id and test.typ = test1.typ set test.num = \
case when test.opt = 'sub' then test.num - test1.num when test.opt = 'add' then test.num + test1.num end;
#复制表，创表语句
create table test1 like test;
create table test1 select * from test;
#删除表的内容，保留结构
truncate table test;
#创建唯一索引
create unique index tx on test(id);
#删除索引
drop index tx on test;
#增加索引
alter table test add index xxx(id, num);
#查询表名和记录数
select table_name, table_rows from information_schema.tables where table_schema='test';
#更改表的引擎
alter table test engine = InnoDB;
#如果存在test数据库，则删除数据库
drop databases if exists test;
#如果不存在test数据库，则创建
create database if not exists test;
```

### postgres
+ pgsql终端命令  
```shell
#登陆数据库
psql -U usr -d dbname
#查看有哪些角色
\du
#切换数据库
\c dbname
#列举数据库
\l
#列举表
\dt
#查看表结构
\d tbname
#打开执行时间
\timing
#退出
\q
#不登陆终端执行sql语句
psql -U usr -d dbname -c "select * from test;"
#导入以'|'分割的文本数据到表中
copy test from '1.txt' with delimiter '|';
```
+ pgsql的sql语句  
```sql
#创建用户
create role usr login superuser createdb;
#update join case when语句
update test as a set num = case when b.opt = 'sub' then a.num - b.num when b.opt = 'add' \
then a.num + b.num end from test1 as b where a.id = b.id and a.typ = b.typ;
```