seelog.go

```
// TestSeelog.go
package main

import (
    seelog "github.com/cihub/seelog"
)

func main() {
    logger, err := seelog.LoggerFromConfigAsFile("seelog.xml")

    if err != nil {
        seelog.Critical("err parsing config log file", err)
        return
    }
    seelog.ReplaceLogger(logger)

    seelog.Error("seelog error")
    seelog.Info("seelog info")
    seelog.Debug("seelog debug")
	defer seelog.Flush()
}
```

seelog.xml

```
<seelog type="asynctimer" asyncinterval="5000000" minlevel="debug" maxlevel="error">
<!---<seelog levels="trace,info,critical">-->
    <exceptions>
        <exception funcpattern="*main.test*Something*" minlevel="info"/>
        <exception filepattern="*main.go" minlevel="error"/>
    </exceptions>
    <outputs formatid="main">
        <filter levels="info">
            <console/>
        </filter>

        <splitter formatid="format1">
            <file path="log.log"/>
            <file path="log2.log"/>
        </splitter>
        <splitter formatid="format2">
            <file path="log3.log"/>
            <file path="log4.log"/>
            <conn addr="192.168.0.2:8123"/>
            <!---<network address="192.168.0.2" port="8123"/> -->
        </splitter>

        <rollingfile formatid="someformat" type="size" filename="./log/roll.log" maxsize="100" maxrolls="5" />
        <!--<rollingfile formatid="someformat" type="date" filename="./log/roll.log" datepattern="02.01.2017" maxrolls="30" />-->

        <buffered formatid="testlevels" size="10000" flushperiod="1000">
            <file path="./log/bufFileFlush.log"/>
        </buffered>

        <!--<buffered size="10000" flushperiod="1000" formatid="testlevels">
            <rollingfile type="date"/>
        </buffered>-->

        <filter levels="error">
            <file path="./log/error.log" formatid="critical"/>
            <smtp senderaddress="noreply-notification-service@none.org"
                  sendername="Automatic notification service"
                  hostname="mail.none.org"
                  hostport="587"
                  username="nns"
                  password="123"
                  formartid="criticalemail">
                <cacerdirpath path="cacdp1">
                <recipient address="john-smith@none.com"/>
     			<header name="Importance" value="high" />
     			<header name="Sensitivity" value="Company-Confidential" />
     			<header name="Auto-Submitted" value="auto-generated" /> address="hans-meier@none.com"/>
            </smtp>
            <conn net="tcp4" addr="server.address:5514" tls="true" insecureskipverify="true" />
        </filter>

    </outputs>
    <formats>
        <format id="main" format="%Date(2006 Jan 02/3:04:05.000000000 PM MST) [%Level] %Msg%n"/>
        <format id="someformat" format="%Ns [%Level] %Msg%n"/>
        <format id="testlevels" format="%Level %Lev %LEVEL %LEV %l %Msg%n"/>
        <format id="usetags" format="&lt;msg&gt;%Msg&lt;/time&gt;"/>
        <format id="format1" format="%Date/%Time [%LEV] %Msg%n"/>
        <format id="format2" format="%File %FullPath %RelFile %Msg%n"/>
        <format id="critical" format="%Time %Date %RelFile %Func %Msg"/>
        <format id="criticalemail" format="Critical error on server!\n   %Time %Date %RelFile %Func %Msg \nSent by Seelog"/>
        <format id="colored" format="%EscM(46)%Level%EscM(49) %Msg%n%EscM(0)"/>
    </formats>
</seelog>
```

seelog.example.xml

```
<seelog>
    <outputs formatid="main">   -->去找id为main的格式
        <filter levels="info,debug,critical,error">    -->定义记录格式
            <console />    -->向屏幕输出
        </filter>
        <filter levels="debug">
            <file path="debug.txt" />    -->向文件输出。可以多个共存。
        </filter>
    </outputs>
    <formats>
        <format id="main" format="%Date/%Time [%LEV] %Msg%n"/>    -->format内容，可以多个共存，只要id不相同。然后上面可以用不同的id来输出不同格式的日志。
    </formats>
</seelog>
```



