# Oracle 更改密码
~~~ SQL
运行cmd命令行
录入 sqlplus /nolog  无用户名登录
conn /as sysdba  连接到数据本地数据
alter user system identified by password;   修改System 密码  为password 

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
impdp user_name/pwd@orcl dumpfile=xx.DMP   log=xx.log
-- 将备份文件xx.DMP还原到user_name用户下，并创建名为xx的日志文件xx.log
* Oracle导出备份文件
expdp user_name/pwd@orcl  dumpfile =xx.dmp ;
-- 导出用户user_name下的所有对象，指定导出的备份文件名称为xx.dmp。导出的备份文件默认的存放位置为oracle安装目录下的dpdump文件夹中。
~~~