##<font color="#556344"> nodeJS+express接入微信后台的详细教程<font>


####前期准备工作


```
  1:默认你又一个服务器
  2:nodejs
  3:一个注册号的公众号
```
####nodejs配置及使用
 ```
  1:node -v查看node版本
  2:npm install -g express-generator 全局安装express
  3:express projectName 创建express项目
  4:进入创建的文件夹 npm install 安装依赖
  5:用./www的方式启动项目
  访问该ip的3000端口 查看网页是否显示
 ``` 

####公众号的接入配置

#####<font color="#556344"> 打开微信公众号平台<font>

#####<font color="#556344">首页开发进入基本配置<font>

![接口配置](https://images2015.cnblogs.com/blog/687168/201511/687168-20151117134720640-1536864077.png)

URL:就是我们映射好的外网地址；

token:就是我们和微信后台约定好的令牌；

秘钥：随机生成；

加密方式：可以选择明文模式，也可以选择兼容模式
；

#####<font color="#556344"> 注意：此时我们还不能提交，因为我们还没有验证代码的编写；这是验证要求；<font>


#####<font color="#556344"> 首先我们这里用到了NPM的另一个包；输入命令<font>
'npm install srypto'
![crypto](https://images2015.cnblogs.com/blog/687168/201511/687168-20151117144959843-1386391179.png)

#####<font color="#556344"> 打开用express创建的文件，进入routers/index.js<font>

#####<font color="#556344"> 将里面代码改为：<font>
```
var express = require('express');
var crypto = require('crypto');
var router = express.Router();
  
var token = "你自己规定的token"; //此处需要你自己修改！
  
/* GET home page. */
router.get('/', function(req, res, next) {
		  
var signature = req.query.signature;
var timestamp = req.query.timestamp;
var nonce = req.query.nonce;
var echostr = req.query.echostr;

/*  加密/校验流程如下： */
//1. 将token、timestamp、nonce三个参数进行字典序排序
var array = new Array(token,timestamp,nonce);
array.sort();
var str = array.toString().replace(/,/g,"");
									  
//2. 将三个参数字符串拼接成一个字符串进行sha1加密
var sha1Code = crypto.createHash("sha1");
var code = sha1Code.update(str,'utf-8').digest("hex");
											  
//3. 开发者获得加密后的字符串可与signature对比，标识该请求来源于微信
if(code===signature){
res.send(echostr)
}else{

	res.send("error");
																		    }
});
module.exports = router;
```

#####<font color="#556344"> 代码完成可以运行等待微信验证：<font>

![gg](https://images2015.cnblogs.com/blog/687168/201511/687168-20151117145412218-1330562428.png)


#####<font color="#556344"> 填好微信配置进行提交<font>



###如果提示token验证失败，则是代码问题，查看代码哪里有错误！！
