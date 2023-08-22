```java
JSON.parse("{\"@type\":\"org.example.User\",\"username\":\"1\"}")

&User {
	username: 1
}
```

# WAF bypass

demo

```json
{
  "@type": "org.example.User",
  "username": "1"
}
```

# 编码绕过

fastjson 对 key,value 值会自动进行 hex 解码和 unicode解码

hex

```json
{
  "\x40\x74\x79\x70\x65": "\x6f\x72\x67\x2e\x65\x78\x61\x6d\x70\x6c\x65\x2e\x55\x73\x65\x72",
  "username": "1"
}
```

unicode

```json
{
  "@type": "\u006f\u0072\u0067\u002e\u0065\u0078\u0061\u006d\u0070\u006c\u0065\u002e\u0055\u0073\u0065\u0072",
  "username": "1"
}

{
  "\u0040\u0074\u0079\u0070\u0065": "\u006f\u0072\u0067\u002e\u0065\u0078\u0061\u006d\u0070\u006c\u0065\u002e\u0055\u0073\u0065\u0072",
  "username": "1"
}
```

# 字符填充

```json
{
  "@type": "org.example.User",
  "username": "1",
  "f": "a*20000"
}
```

二次反序列化

$ref

http://www.bmth666.cn/bmth_blog/2022/04/11/Fastjson%E6%BC%8F%E6%B4%9E%E5%AD%A6%E4%B9%A0/#%E9%A2%98%E7%9B%AE%E5%A4%8D%E7%8E%B0

编码

https://blog.csdn.net/fmyyy1/article/details/121674546

绕过 WAF ，在部分中间件中，multipart 支持指定 Content-Transformer-Encoding 可以使用 Base64 或 quoted-printable （QP 编码）
来绕过 WAF

大量字符绕过 WAF

```
[11111111111111111111111111111111111,[11111111111111111111111111111111111... ,[11111111111111111111111111111111111... ,[11111111111111111111111111111111111... ,[11111111111111111111111111111111111... ,...,{'\x40\u0074\x79\u0070\x65':xjava.lang.AutoCloseable"... ]]]]]

```

各种特性

```
,new:[NaN,x'00',{,/*}*/'\x40\u0074\x79\u0070\x65':xjava.lang.AutoClosea ble"
```

