toml

pip.toml

```
[dev-packages]
pytest = "*"
sphinx = "*"

[packages]
click = "*"
crayons = "*"
toml = "*"
"delegator.py" = ">=0.0.6"
requests = ">=2.4.0"
requirements-parser = "*"
parse = "*"
pipfile = "==0.0.1"
click-completion = "*"
"backports.shutil-get-terminal-size" = "*"
pew = ">=0.1.26"
blindspin = "*"

[requires]
python_version = "2.7"
```

python读取

```
In [5]: toml.load(open('Pipfile'))
Out[5]: 
{u'dev-packages': {u'pytest': u'*', u'sphinx': u'*'},
 u'packages': {u'backports.shutil-get-terminal-size': u'*',
  u'blindspin': u'*',
  u'click': u'*',
  u'click-completion': u'*',
  u'crayons': u'*',
  u'delegator.py': u'>=0.0.6',
  u'parse': u'*',
  u'pew': u'>=0.1.26',
  u'pipfile': u'==0.0.1',
  u'requests': u'>=2.4.0',
  u'requirements-parser': u'*',
  u'toml': u'*'},
 u'requires': {u'python_version': u'2.7'}}

In [7]: toml.dumps(data)
Out[7]: u'[dev-packages]\npytest = "*"\nsphinx = "*"\n[packages]\ncrayons = "*"\nrequirements-parser = "*"\n"backports.shutil-get-terminal-size" = "*"\n"delegator.py" = ">=0.0.6"\nblindspin = "*"\npew = ">=0.1.26"\nparse = "*"\ntoml = "*"\npipfile = "==0.0.1"\nrequests = ">=2.4.0"\nclick-completion = "*"\nclick = "*"\n[requires]\npython_version = "2.7"\n'
```

例子

```
title = "TOML 例子"

[owner]
name = "Tom Preston-Werner"
organization = "GitHub"
bio = "GitHub Cofounder & CEO\nLikes tater tots and beer."
dob = 1979-05-27T07:32:00Z # 日期时间是一等公民。为什么不呢？

[database]
server = "192.168.1.1"
ports = [ 8001, 8001, 8002 ]
connection_max = 5000
enabled = true

[servers]

  # 你可以依照你的意愿缩进。使用空格或Tab。TOML不会在意。
  [servers.alpha]
  ip = "10.0.0.1"
  dc = "eqdc10"

  [servers.beta]
  ip = "10.0.0.2"
  dc = "eqdc10"

[clients]
data = [ ["gamma", "delta"], [1, 2] ]

# 在数组里换行没有关系。
hosts = [
  "alpha",
  "omega"
]
```

环境配置

https://www.keakon.net/2016/10/22/Python%E9%A1%B9%E7%9B%AE%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86

