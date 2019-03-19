###<font color ="879342">微信开发者文档中实现图尚往来</font>
直接贴代码

####<font color ="811111">开发者文档中的代码为2.7 我用的是3.5</font>

####<font color ="811111">贴出我的代码 只是跟据官网代码能够在3.x版本使用 对比提供意见</font>

####<font color ="819911">官网为主我的仅供参考 毕竟去空格了半天 可能哪错了</font>
```
# -*- coding: utf-8 -*-
# filename: main.py                                

import web
from handle import Handle
 

urls = (
			'/wx', 'Handle',
		)        



if __name__ == '__main__':
		app = web.application(urls, globals())
		app.run()

```



```
# -*- coding: utf-8 -*-                            
# filename: receive.py
import xml.etree.ElementTree as ET
def parse_xml(web_data):

	if len(web_data) == 0:
		return None
	xmlData = ET.fromstring(web_data)
	msg_type = xmlData.find('MsgType').text
	if msg_type == 'text':
		return TextMsg(xmlData)
	elif msg_type == 'image':
		return ImageMsg(xmlData)
class Msg(object):
	def __init__(self,xmlData):
		self.ToUserName = xmlData.find('ToUserName').text
		self.FromUserName = xmlData.find('FromUserName').text
		self.CreateTime = xmlData.find('CreateTime').text
		self.MsgType = xmlData.find('MsgType').text
		self.MsgId = xmlData.find('MsgId').text
class TextMsg(Msg):
	def __init__(self,xmlData):
		Msg.__init__(self,xmlData)
		self.Content = xmlData.find('Content').text.encode("utf-8")
class ImageMsg(Msg):
	def __init__(self,xmlData):
		Msg.__init__(self,xmlData)
		self.PicUrl = xmlData.find('PicUrl').text
		self.MediaId = xmlData.find('MediaId').text
												             
```

```
# -*- coding: utf-8 -*-                                                                     
# filename: reply.py
import time

class Msg(object):
	def __init__(self):
		pass
	def send(self):
		return success
class TextMsg(Msg):
	def __init__(self,toUserName,fromUserName,content):
		self.__dict = dict()
		self.__dict['ToUserName'] = toUserName
		self.__dict['FromUserName'] = fromUserName
		self.__dict['CreateTime'] = int(time.time())
		self.__dict['Content'] = content
	def send(self):
		XmlForm = """
		<xml>
		<ToUserName><![CDATA[{ToUserName}]]></ToUserName>
		<FromUserName><![CDATA[{FromUserName}]]></FromUserName>
		<CreateTime>{CreateTime}</CreateTime>
		<MsgType><![CDATA[text]]></MsgType>
		<Content><![CDATA[{Content}]]></Content>
		</xml>
		"""
		return XmlForm.format(**self.__dict)
class ImageMsg(Msg):
	def __init__(self,toUserName,fromUserName,mediaId):
		self.__dict = dict()
		self.__dict['ToUserName'] = toUserName
		self.__dict['FromUserName'] = fromUserName
		self.__dict['CreateTime'] = int(time.time())
		self.__dict['MediaId'] = mediaId
	def send(self):
		XmlForm = """
		<xml>
		<ToUserName><![CDATA[{ToUserName}]]></ToUserName>
		<FromUserName><![CDATA[{FromUserName}]]></FromUserName>
		<CreateTime>{CreateTime}</CreateTime>
		<MsgType><![CDATA[image]]></MsgType>
		<Image>
		<MediaId><![CDATA[{MediaId}]]></mediaId>
		</Image>
		</xml>
		"""
		return XmlForm.format(**self.__dict) 

```
