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