# URL请求
## @截断
`https://fanyi.youdao.com@baidu.com/`实际请求的是`https://www.baidu.com/`

@前面的东西会被当作用户名，可以拿邮箱格式来理解

# springboot路径访问绕过
<font style="color:rgb(35, 38, 59);">路径结尾加	</font>`<font style="color:rgb(35, 38, 59);">;</font>`<font style="color:rgb(35, 38, 59);">	 </font>`/api/v2/api-docs;`

路径结尾加`<font style="color:rgb(35, 38, 59);">;.js</font>``/api/v2/api-docs;.js`

路径中间加 `/..;/ 或 /;/ 或 /..;a/` `<font style="color:rgb(35, 38, 59);">/..;/..;/..;/actuator/httptrace</font>`

<font style="color:rgb(35, 38, 59);">结尾加</font>`<font style="color:rgb(35, 38, 59);">/</font>`<font style="color:rgb(35, 38, 59);">	</font>`<font style="color:rgb(35, 38, 59);">/env/</font>`

<font style="color:rgb(35, 38, 59);">参考链接：</font>[<font style="color:rgb(35, 38, 59);">https://mp.weixin.qq.com/s/btYzzG2Jfrqoj5xvnr3Tgw</font>](https://mp.weixin.qq.com/s/btYzzG2Jfrqoj5xvnr3Tgw)

# powershell能访问但是浏览器等访问不了
<font style="color:rgb(35, 38, 59);">目标网站大概率是判断了User-Agent头，如果是Powershell的头就能访问到目标页面，但是浏览器头就自动跳转，可以尝试修改User-Agent头。</font>

<font style="color:rgb(35, 38, 59);">powershell的UA头：</font>`<font style="color:rgb(35, 38, 59);">Mozilla/5.0 (Windows NT; Windows NT 10.0; zh-CN) WindowsPowerShell/5.1.19041.4046</font>`

# <font style="color:rgb(35, 38, 59);">Linux</font>
linux命令，中间加\也可以正常执行，例如c\at /etc/passwd

# 内容编码降级攻击
`Accept-Encoding: gzip;q=0, deflate;q=0`  
原理：强制服务端返回未压缩明文  
防御：禁用不安全的编码方式

# 协议走私攻击向量
```http
GET / HTTP/1.1
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
```

防御：配置中间件拒绝歧义请求

# Null字节注入
在尝试使用特殊字符注入的时候，可以尝试Null字节。例如URL编码`%00`，转义字符`\0`  
在JSON请求中，不能直接使用常规的URL编码等，可以用\u序列插入Uonicode字符，即`\u0000`，表示Null字节字符。

