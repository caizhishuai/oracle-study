# Oracle 更改密码
~~~ SQL
运行cmd命令行
录入 sqlplus /nolog  无用户名登录
conn /as sysdba  连接到数据本地数据
alter user system identified by password;   修改System 密码  为password 
~~~

# 查询当前登录的用户 库
~~~ SQL
show user
show parameter instance_name
切换库
sqlplus c1/sa@testdb
~~~

解锁方法:
alter user system account unlock
~~~
# Oracle 默认用户名密码
~~~ SQL
sys/change_on_install       SYSDBA 或 SYSOPER        不能以 NORMAL 登录，可作为默认的系统管理员
system/manager              SYSDBA 或 NORMAL         不能以 SYSOPER 登录，可作为默认的系统管理员
sysman/oem_temp             sysman                   为 oms 的用户名
scott/tiger                 NORMAL                   普通用户
aqadm /aqadm                SYSDBA 或 NORMAL         高级队列管理员
Dbsnmp/dbsnmp           	SYSDBA 或 NORMAL         复制管理员
~~~
# Oracle 导入导出DMP备份文件
~~~ SQL
* cmd下登录sqlplus
* 创建表空间 tablespace_name.dbf 
* create tablespace  tablespace_name  datafile 'D:\work\app\admin\orcl\dpdump\tablespace_name.dbf' size 500m
reuse autoextend on next 10m maxsize unlimited extent management local autoallocate permanent online; 指定表空间初始大小为500M，并且指定表空间满后每次增加的大小为10M。
* 创建用户
create user +用户名+ identified by +密码+ default tablespace +表空间名;     用户、密码指定表空间
* 给用户授权
grant connect,resource,dba to user_name; 
-- 给用户user_name 授权， connect和resource是两个系统内置的角色，和dba是并列的关系。
-- DBA：拥有全部特权，是系统最高权限，只有DBA才可以创建数据库结构。
-- RESOURCE:拥有Resource权限的用户只可以创建实体，不可以创建数据库结构。
-- CONNECT：拥有Connect权限的用户只可以登录Oracle，不可以创建实体，不可以创建数据库结构。
* 导入数据库文件
imp user_name/pwd@orcl file=d:\zxcc.dmp   log=xx.log;
-- 将备份文件xx.DMP还原到user_name用户下，并创建名为xx的日志文件xx.log
* Oracle导出备份文件
exp user_name/pwd@orcl  file=d:\zxcc.dmp;
-- 导出用户user_name下的所有对象，指定导出的备份文件名称为xx.dmp。导出的备份文件默认的存放位置为oracle安装目录下的dpdump文件夹中。



将数据库zxcc中kf用户与cc用户的表导出
exp kf/zx@zxcc file=d:\zxcc_ur.dmp owner=(kf,cc)

将数据库zxcc中的表kf_operator、kf_role导出
exp kf/zx@zxcc file= d:\zxcc_tb.dmp tables=(kf_operator,kf_role)


~~~
# 查询表所在表空间
~~~ SQL
select username,default_tablespace from dba_users  where username='用户名';
select username,default_tablespace from dba_users order by username;
~~~

# Oracle 常用操作语句
~~~ SQL
create table test02(
id int not null,
name varchar(8) not null,
gender varchar2(2) not null,
age int not null,
address varchar2(20) default '' not null,
regdata date
);

修改表
alter table test01 add constraint s_id primary key;
alter table test01 add constraint CK_INFOS_GENDER check(gender=’男’ or gender=’女’)
alter table test01 add constraint CK_INFOS_AGE(age>=0 and age<=50)
alter table 表名 modify 字段名 default 默认值; //更改字段类型
alter table 表名 add 列名 字段类型; //增加字段类型
alter table 表名 drop column 字段名; //删除字段名
alter table 表名 rename column 列名 to 列名 //修改字段名
rename 表名 to 表名 //修改表名

删除表
truncate table 表名 //删除表中的所有数据，速度比delete快很多,截断表
delete from table 条件//
drop table 表名 //删除表

插入语句
insert into 表名(值1,值2) values(值1,值2);

修改语句
update 表名 set 字段=值 [修改条件]

查询语句
带条件的查询
where

模糊查询
like % _

范围查询
in

对查询结果进行排序
oeder by desc||asc
case…when
select username,case username when ‘aaa’ then ‘计算机部门’ when ‘bbb’ then ‘市场部门’ else ‘其他部门’ end as 部门 from users;
select username,case username=’aaa’ then ‘计算机部门’ when username=’bbb’ then ‘市场部门’ else ‘其他部门’ as 部门 from users;

运算符和表达式
算数运算符和比较运算符
distinct 去除多余的行
column 可以为字段设置别名 比如 column column_name heading new_name
decode 函数的使用 类似于case…when
select username,decode(username,’aaa’,’计算机部门’,’bbb’,’市场部门’,’其他’) as 部门 from users;

复制表
create table 表名 as 一个查询结果 //复制查询结果
insert into 表名 值 一个查询结果 //添加时查询

查看表信息
desc test01;

创建表空间

永久表空间
create tablespace test1_tablespace datafile ‘testfile.dbf’ size 10m;

临时表空间
create temporary tablespace temptest1_tablespace tempfile ‘tempfile.dbf’ size 10m;
desc dba_data_files;
select file_name from dba_data_files where tablespace_name=’TEST1_TABLESPACE’;

约束
非空约束 |主键约束|外键约束|唯一约束|检查约束
not null |primary key|unique|check|
联合主键 constraint pk_id_username primary key(id,username);
查看数据字典 desc user_constraint
修改表时重命名 rename constraint a to b;
修改表删除约束
禁用约束 disable constraint 约束名字;
删除约束 drop constraint 约束名字; drop primary key;直接删除主键
外键约束
create table typeinfo(typeid varchar2(20) primary key, typename varchar2(20));
create table userinfo_f( id varchar2(10) primary key,username varchar2(20),typeid_new varchar2(10) references typeinfo(typeid));
insert into typeinfo values(1,1);

创建表时设置外键约束
constraint 名字 foregin
create table userinfo_f2 (id varchar2(20) primary key,username varchar2(20),typeid_new varchar2(10),constraint fk_typeid_new foreign key(typeid_new) references typeinfo(typeid));
create table userinfo_f3 (id varchar2(20) primary key,username varchar2(20),typeid_new varchar2(10),constraint fk_typeid_new1 foreign key(typeid_new) references typeinfo(typeid) on delete cascade);

外键约束包含
删除外键约束
禁用约束 disable constraint 约束名字;
删除约束 drop constraint 约束名字;
唯一约束 与主键区别 唯一约束可以有多个，只能有一个null
create table userinfo_u( id varchar2(20) primary key,username varchar2(20) unique,userpwd varchar2(20));

创建表时添加约束
constraint 约束名字 unique(列名);

修改表时添加唯一约束 add constraint 约束名字 unique(列名);
检查约束

create table userinfo_c( id varchar2(20) primary key,username varchar2(20), salary number(5,0) check(salary>50));
constraint ck_salary check(salary>50);

/* 获取表：*/
select table_name from user_tables; //当前用户的表
select table_name from all_tables; //所有用户的表
select table_name from dba_tables; //包括系统表
select table_name from dba_tables where owner=’zfxfzb’
/*
~~~

# 死锁处理
~~~ SQL
select s.username,l.object_id, l.session_id,s.serial#, s.lockwait,s.status,s.machine,s.program from v$session s,v$locked_object l where s.sid = l.session_id;
select sql_text from v$sql where hash_value in   (select sql_hash_value from v$session where sid in  (select session_id from v$locked_object));
alter system kill session 'session_id,serial#'; 
alter system kill session '301,16405'; 
~~~