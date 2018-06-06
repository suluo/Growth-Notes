#### start.sh

```bash
echo "$(date) Server start"
BASE_DIR=$(cd `dirname $0`;pwd)
cd ${BASE_DIR}
filename=server_name.py

Running_pids=$(pgrep -f ${filename})
#running_pids=$(lsof -i:10310 | awk '{print $2}' | grep -v PID | uniq -c)
# ping -c 1 -w 1 ip:port &>/dev/null && result=0 || result=1

Cpu_nums=$(/proc/cpuinfo | grep "processor" | wc -l)
echo -n "logical CPU number in total: $Cpu_nums"
multi_nums=$[$Cpu_nums-1]
echo -n "advice Multi_process num: $multi_nums"

if [ -n "$Running_pids" ];then
    echo "Had Running_pids:$Running_pids"
else
    nohup python2.7 ${filename} >>../log/tornado.log 2>&1 &
    multi_num=$()
    nohup python2.7 ${filename} -port=port -dev=False -num=${Cpu_nums-1} >> ./log/tornado.log 2>&1 &
    #nohup python2.7 ${filename} -port=port -log_file_prefix=../log/tornado@port.log > ../log/tornado.log 2>&1 &
#    sleep 1
    running_pids=$(pgrep -f ${filename})
    [ -n "$running_pids" ] && echo "The pids:$running_pids" || echo "The server fail"
fi
```

#### stop.sh

```bash
echo "$(date) stop"
curl ip:port/stop
#[ "$#" -lt 1 ] && xtime=120 || xtime=$(($1*60))
#echo "please wait $xtime"
#sleep $xtime

filename=server_name.py
running_pids=$(pgrep -f ${filename})
while [ -n "$running_pids" ]
do
    echo "running_pids:$running_pids exist"
    echo "please wait 30"
    sleep 30
    running_pids=$(pgrep -f ${filename})
done
echo "running_pids not exist"
```

#### kstop.sh

    echo "$(date) kStop"
    BASE_DIR=$(cd `dirname $0`; pwd)
    cd ${BASE_DIR}
    filename=server_name.py
    running_pids=$(pgrep -f ${filename})
    if [ -n "$running_pids" ]
    then
        echo "$(date) kill $running_pids"
        kill $running_pids
    else
        echo "$finename server not exists"
    fi

#### state.sh

    #set x
    echo "$(date) state"
    BASE_DIR=$(cd `dirname $0`; pwd)
    cd $BASE_DIR
    filename=group_server.py

    #read -p "Input state which you want:"choice
    #case $choice in 
    # [ "$#" -gt 1 -o "$#" -lt 1 ] && echo "error";exit 0
    if [ "$#" -gt 1 -o "$#" -lt 1 ];then
        echo "please input one arg"
        exit 0
    fi

    case $1 in 
        "start")
            sh start.sh
            ;;
        "stop")
            echo "please wait 5 minite"
            sh stop.sh
            ;;
        "kstop")
            sh kstop.sh
            ;;
        "restart")
            echo "please wait 5 minites"
            sh stop.sh
            echo "stop & will start"
            sh start.sh
            ;;
        *)
            echo "please input correct args(start|stop|kstop|restart)"
            exit 0
    esac



