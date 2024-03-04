# makeAnki
通过markdown固定格式批量制作anki卡片  

前提：
需要问题作为一级标题，且问题只能一行；答案不使用标题（不管多少行都可以，也支持代码等格式，图片会自动转成anki可展示的img样式）。
如果存在只有问题没有答案的情况，会自动跳过。

导入原理：
以 "# " 为切割标识，得到各问题块，再取第一行为问题，剩下的为答案；并通过正则匹配图片，将网络图片进行上传，再转换成html的img标签。
之后通过ankiconnect的api，新增牌组和卡片。

使用方式：
先在anki启用插件AnkiConnect (2055492159 )，打开anki。
将网页部署到nginx，并对AnkiConnect的接口地址进行转发，以便处理跨域问题
```
		# 部署的网页路由
		location /makeanki {
			root 	D:\a;
			index 	index.html;
		}

		# AnkiConnect 的接口地址
		location /anki {
			proxy_pass http://localhost:8765;
		}
```
部署完成后，打开地址（eg：http://localhost/makeanki/），选择文件，输入牌组名称和卡片类型点击解析即可。