mongo.sh

```
start() {
    /home/mongod/mongodb/bin/mongod -f /home/mongod/mongodb/mongo.conf
}

stop() {
  /home/mongod/mongodb/bin/mongod -f /home/mongod/mongodb/mongo.conf --shutdown
}

case "$1" in
  start)
    start
    ;;
  stop)
    stop
    ;;
  restart)
    stop
    start
    ;;
  *)
 echo $"Usage: $0 {start|stop|restart}"
 exit 1
esac

```



