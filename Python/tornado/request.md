tornado获取客户端设备信息

```
HTTPServerRequest(
protocol='http', host='192.168.1.137:8000', 
method='GET', 
uri='/', 
version='HTTP/1.1', 
remote_ip='192.168.1.137', 
headers={
			'Accept-Language': 'zh-CN,zh;q=0.8', 
			'Accept-Encoding': 'gzip, deflate, sdch', 
			'Host': '192.168.1.137:8000', 
			'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8', 
			'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/47.0.2526.73 Chrome/47.0.2526.73 Safari/537.36', 
			'Connection': 'keep-alive', 
			'Cookie': 'rm="2|1:0|10:1452658833|2:rm|8:ZW5lbg==|eada375b8d4e7e6f2977d951fca13d35d2a8b94f83d0a032c92f1922dd71deb2"', 
			'Cache-Control': 'max-age=0', 
			'Upgrade-Insecure-Requests': '1'
		}
)


header = self.request.headers
header = self.request.headers["Accept-Language"]
remote_ip = self.request.remote_ip 
```



