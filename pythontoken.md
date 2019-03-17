#<font color="533455">微信公众技术平台开发者配置中python版本问题

###<font color="553333">开发者文档中选择的是python作为开发语言<font>
###<font color="51111">不过是python2(然而我这个年代python马上不会维护pip已经无法使用)<font>

###<font color="41111">跟着文档走的我遇到的问题在这里记录<font>

####<font color="31111">这里是它给的开发配置文件代码<font>
```
# -*- coding: utf-8 -*-
# filename: handle.py

import hashlib
import web

class Handle(object):
	    def GET(self):
			try:
				data = web.input()
				
				if len(data) == 0:
					return "hello, this is handle view"
				signature = data.signature
				timestamp = data.timestamp
				nonce = data.nonce
				echostr = data.echostr
				token = "xxxx" #请按照公众平台官网\基本配置中信息填写

				list = [token, timestamp, nonce]
				list.sort()
	            sha1 = hashlib.sha1()
	            map(sha1.update, list)
	            hashcode = sha1.hexdigest()
	            print "handle/GET func: hashcode, signature: ", hashcode, signature
				if hashcode == signature:
					return echostr
				else:
					return ""
			except Exception, Argument:
				return Argument
```



####<font color="21111">到处按print除了基本的2-3版本之间的区别其他我都不知道<font>

####<font color="21111">你会发现 hashcode不可能等于 signature因为 hashcode一直都是da39a3ee5e6b4b0d3255bfef95601890afd80709<font>
####<font color="21111">最终定位到map(sha1.update,list)<font>

####<font color="21111">以我的知识只是定位到了编码和版本的问题，，，具体的问题我还是不太清楚留坑。。。。。。。<font>


####[我查找的两个链接1](http://group.jobbole.com/25411/)

####[我查找的两个链接2](https://www.2cto.com/kf/201801/711368.html)


####<font color="31111">找到问题并找到答案的代码（包含过程。。。）<font>
```
import hashlib
import web

class  Handle(object):
		def GET(self):
			try:
				data = web.input()
				
				if len(data) == 0:
					return "hello, this is handle view"
				signature = data.signature
				print(signature)
				timestamp = data.timestamp
				print(timestamp)
				nonce = data.nonce
				print(nonce)
				echostr = data.echostr
				print(echostr)
				token = "mytoken"

				list = [token,timestamp,nonce]
				print(list)
				list.sort()
				print(list)
				list2 = ''.join(list)
				sha1 = hashlib.sha1()
				print(sha1)
				sha1.update(list2.encode('utf-8'))
				print(sha1)
				hashcode = sha1.hexdigest()
				print(hashcode)
				print(signature)
				print ("handle/GET func: hashcode, signature: ", hashcode, signature)
				if hashcode == signature:
					return echostr
				else:
					return ""
			except (Exception, Argument):
				return Argument
```	
