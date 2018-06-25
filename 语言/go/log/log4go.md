log.go

```
package main
import (
    "fmt"
    "log"
)

package main

import (
    l4g "github.com/alecthomas/log4go"
)

func main() {
    l4g.AddFilter("stdout", l4g.DEBUG, l4g.NewConsoleLogWriter())             //输出到控制台,级别为DEBUG
    l4g.AddFilter("file", l4g.DEBUG, l4g.NewFileLogWriter("test.log", false)) //输出到文件,级别为DEBUG,文件名为test.log,每次追加该原文件
    //l4g.LoadConfiguration("log.xml")//使用加载配置文件,类似与java的log4j.propertites
    l4g.Debug("the time is now :%s -- %s", "213", "sad")
    defer l4g.Close()//注:如果不是一直运行的程序,请加上这句话,否则主线程结束后,也不会输出和log到日志文件
}
```

log.xml

```
<logging>
  <filter enabled="true">
    <tag>stdout</tag>
    <type>console</type>
    <!-- level is (:?FINEST|FINE|DEBUG|TRACE|INFO|WARNING|ERROR) -->
    <level>DEBUG</level>
  </filter>
  <filter enabled="true">
    <tag>file</tag>
    <type>file</type>
    <level>FINEST</level>
    <property name="filename">test.log</property>
    <!--
       %T - Time (15:04:05 MST)
       %t - Time (15:04)
       %D - Date (2006/01/02)
       %d - Date (01/02/06)
       %L - Level (FNST, FINE, DEBG, TRAC, WARN, EROR, CRIT)
       %S - Source
       %M - Message
       It ignores unknown format strings (and removes them)
       Recommended: "[%D %T] [%L] (%S) %M"
    -->
    <property name="format">[%D %T] [%L] (%S) %M</property>
    <property name="rotate">false</property> <!-- true enables log rotation, otherwise append -->
    <property name="maxsize">0M</property> <!-- \d+[KMG]? Suffixes are in terms of 2**10 -->
    <property name="maxlines">0K</property> <!-- \d+[KMG]? Suffixes are in terms of thousands -->
    <property name="daily">true</property> <!-- Automatically rotates when a log message is written after midnight -->
  </filter>
  <filter enabled="true">
    <tag>xmllog</tag>
    <type>xml</type>
    <level>TRACE</level>
    <property name="filename">trace.xml</property>
    <property name="rotate">true</property> <!-- true enables log rotation, otherwise append -->
    <property name="maxsize">100M</property> <!-- \d+[KMG]? Suffixes are in terms of 2**10 -->
    <property name="maxrecords">6K</property> <!-- \d+[KMG]? Suffixes are in terms of thousands -->
    <property name="daily">false</property> <!-- Automatically rotates when a log message is written after midnight -->
  </filter>
  <filter enabled="false"><!-- enabled=false means this logger won't actually be created -->
    <tag>donotopen</tag>
    <type>socket</type>
    <level>FINEST</level>
    <property name="endpoint">192.168.0.73:12124</property> <!-- recommend UDP broadcast -->
    <property name="protocol">udp</property> <!-- tcp or udp -->
  </filter>
</logging>
```



