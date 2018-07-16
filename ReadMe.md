# Oracle 更改密码
~~~ SQL
运行cmd命令行
录入 sqlplus /nolog  无用户名登录
conn /as sysdba  连接到数据本地数据
alter user system identified by password;   修改System 密码  为password
~~~