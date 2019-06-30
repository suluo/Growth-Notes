火车票余票、电影、歌词、医院等查询

```
pip install iquery
 Usage:
        iquery (-c|彩票)
        iquery (-m|电影)
        iquery -p <city>
        iquery -l song [singer]
        iquery -p <city> <hospital>
        iquery <city> <show> [<days>]
        iquery [-dgktz] <from> <to> <date>

    Arguments:
        from             出发站
        to               到达站
        date             查询日期

        city             查询城市
        show             演出的类型
        days             查询近(几)天内的演出, 若省略, 默认15

        city             城市名,加在-p后查询该城市所有莆田医院
        hospital         医院名,加在city后检查该医院是否是莆田系

    Options:
        -h, --help       显示该帮助菜单.
        -dgktz           动车,高铁,快速,特快,直达
        -m               热映电影查询
        -p               莆田系医院查询
        -l               歌词查询
        -c               彩票查询

    Show:
        演唱会 音乐会 音乐剧 歌舞剧 儿童剧 话剧
        歌剧 比赛 舞蹈 戏曲 相声 杂技 马戏 魔术
```

知乎 api

```
pip install git+git://github.com/lzjun567/zhihu-api --upgrade
from zhihu import Zhihu
zhihu = Zhihu()
zhihu.user(user_slug="xiaoxiaodouzi")

{'avatar_url_template': 'https://pic1.zhimg.com/v2-ca13758626bd7367febde704c66249ec_{size}.jpg',
     'badge': [],
     'name': '我是小号',
     'headline': '程序员',
     'gender': -1,
     'user_type': 'people',
     'is_advertiser': False,
     'avatar_url': 'https://pic1.zhimg.com/v2-ca13758626bd7367febde704c66249ec_is.jpg',
     'url': 'http://www.zhihu.com/api/v4/people/1da75b85900e00adb072e91c56fd9149', 'type': 'people',
     'url_token': 'xiaoxiaodouzi',
     'id': '1da75b85900e00adb072e91c56fd9149',
     'is_org': False}
```

ncmbot

网易云音乐数据

```
import ncmbot
#登录
bot = ncmbot.login(phone='xxx', password='yyy')
bot.content # bot.json()
#获取用户歌单
ncmbot.user_play_list(uid='36554272')
```

财经数据接口包

```
pip install tushare
import tushare as ts
#一次性获取最近一个日交易日所有股票的交易数据
ts.get_today_all()

代码，名称，涨跌幅，现价，开盘价，最高价，最低价，最日收盘价，成交量，换手率
      code    name     changepercent  trade   open   high    low  settlement
0     002738  中矿资源         10.023  19.32  19.32  19.32  19.32       17.56   
1     300410  正业科技         10.022  25.03  25.03  25.03  25.03       22.75   
2     002736  国信证券         10.013  16.37  16.37  16.37  16.37       14.88   
3     300412  迦南科技         10.010  31.54  31.54  31.54  31.54       28.67   
4     300411  金盾股份         10.007  29.68  29.68  29.68  29.68       26.98   
5     603636  南威软件         10.006  38.15  38.15  38.15  38.15       34.68   
6     002664  信质电机         10.004  30.68  29.00  30.68  28.30       27.89   
7     300367  东方网力         10.004  86.76  78.00  86.76  77.87       78.87   
8     601299  中国北车         10.000  11.44  11.44  11.44  11.29       10.40   
9     601880   大连港         10.000   5.72   5.34   5.72   5.22        5.20   
10    000856  冀东装备         10.000   8.91   8.18   8.91   8.18        8.10
```

北京实时公交

```
pip install -r requirements.txt 安装依赖
python manage.py build_cache 获取离线数据，建立本地缓存
#项目自带了一个终端中的查询工具作为例子，运行：python manage.py cli
>>> from beijing_bus import BeijingBus
>>> lines = BeijingBus.get_all_lines()
>>> lines
[<Line: 运通122(农业展览馆-华纺易城公交场站)>, <Line: 运通101(广顺南大街北口-蓝龙家园)>, ...]
>>> lines = BeijingBus.search_lines('847')
>>> lines
[<Line: 847(马甸桥西-雷庄村)>, <Line: 847(雷庄村-马甸桥西)>]
>>> line = lines[0]
>>> print line.id, line.name
541 847(马甸桥西-雷庄村)
>>> line.stations
[<Station 马甸桥西>, <Station 马甸桥东>, <Station 安华桥西>, ...]
>>> station = line.stations[0]
>>> print station.name, station.lat, station.lon
马甸桥西 39.967721 116.372921
>>> line.get_realtime_data(1) # 参数为站点的序号，从1开始
[
    {
        'id': 公交车id,
        'lat': 公交车的位置,
        'lon': 公交车位置,
        'next_station_name': 下一站的名字,
        'next_station_num': 下一站的序号,
        'next_station_distance': 离下一站的距离,
        'next_station_arriving_time': 预计到达下一站的时间,
        'station_distance': 离本站的距离,
        'station_arriving_time': 预计到达本站的时间,
    },
    ...
]
```

文章提取

```
git clone https://github.com/grangier/python-goose.git
cd python-goose
pip install -r requirements.txt
python setup.py install

>>> from goose import Goose
>>> from goose.text import StopWordsChinese
>>> url  = 'http://www.bbc.co.uk/zhongwen/simp/chinese_news/2012/12/121210_hongkong_politics.shtml'
>>> g = Goose({'stopwords_class': StopWordsChinese})
>>> article = g.extract(url=url)
>>> print article.cleaned_text[:150]
香港行政长官梁振英在各方压力下就其大宅的违章建筑（僭建）问题到立法会接受质询，并向香港民众道歉。

梁振英在星期二（12月10日）的答问大会开始之际在其演说中道歉，但强调他在违章建筑问题上没有隐瞒的意图和动机。

一些亲北京阵营议员欢迎梁振英道歉，且认为应能获得香港民众接受，但这些议员也质问梁振英有
```

伪装浏览器身份

```
pip install fake-useragent
from fake_useragent import UserAgent
ua = UserAgent()

ua.ie
# Mozilla/5.0 (Windows; U; MSIE 9.0; Windows NT 9.0; en-US);
ua.msie
# Mozilla/5.0 (compatible; MSIE 10.0; Macintosh; Intel Mac OS X 10_7_3; Trident/6.0)'
ua['Internet Explorer']
# Mozilla/5.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0; GTB7.4; InfoPath.2; SV1; .NET CLR 3.3.69573; WOW64; en-US)
ua.opera
# Opera/9.80 (X11; Linux i686; U; ru) Presto/2.8.131 Version/11.11
ua.chrome
# Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.2 (KHTML, like Gecko) Chrome/22.0.1216.0 Safari/537.2'
```

抓取发放代理

```
pip install -U getproxy
➜ ~ getproxy --help
Usage: getproxy [OPTIONS]

Options:
--in-proxy TEXT Input proxy file
--out-proxy TEXT Output proxy file
--help Show this message and exit.
```



