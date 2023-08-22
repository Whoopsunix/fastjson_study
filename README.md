# fastjson 全版本漏洞复现

By. Whoopsunix

# Why fastjson？

fastjson复现简单、调用链多，很多时候反而更像是在看其他组件的序列化链，很适合拿来做java研究

所以起了个项目记录自己复现过的POC，顺便记录pom依赖，毕竟找环境还是挺麻烦的

后续poc 环境 分析文章在 github 同步  
https://github.com/Whoopsunix/fastjson_study

# 环境

[环境](https://github.com/Whoopsunix/PPPVULNS/tree/master/components/fastjsonDemo)

# json检测

通过实战案例记录不断更新，如何区分不同的框架、dnslog探测、版本探测、利用链探测

[fastjson check](fastjsonCheck/fastjsonCheck.md)

# 全版本poc合集

[1.2.24-1.2.80 poc](recurring.md)

# bypass

绕过手段

[bypass](bypass/bypass.md)

# 其他利用

二次反序列化、原生反序列化相关在其他项目中

# 感谢以下师傅的研究

+
+ https://github.com/LeadroyaL/fastjson-blacklist fastjson黑白名单
+ https://github.com/safe6Sec/Fastjson  目前最全的poc合集
+ https://github.com/su18/hack-fastjson-1.2.80 1.2.80 POC
+ https://github.com/safe6Sec/ShiroAndFastJson 1.2.80 poc含环境
+ https://mp.weixin.qq.com/s/5mO1L5o8j_m6RYM6nO-pAA  版本区分
+ https://b1ue.cn/archives/506.html 浅蓝博客
+ https://github.com/knownsec/KCon/tree/master/2022 浅蓝kcon分享
+ https://www.yulegeyu.com/2022/11/12/Java%E5%AE%89%E5%85%A8%E6%94%BB%E9%98%B2%E4%B9%8B%E8%80%81%E7%89%88%E6%9C%ACFastjson-%E7%9A%84%E4%B8%80%E4%BA%9B%E4%B8%8D%E5%87%BA%E7%BD%91%E5%88%A9%E7%94%A8/
  雨了个雨 低版本 bcel