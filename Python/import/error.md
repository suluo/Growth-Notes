twisted

```
wget https://twistedmatrix.com/Releases/Twisted/17.1/Twisted-17.1.0.tar.bz2
tar -jxvf Twisted-17.1.0.tar.bz2
cd Twisted-17.1.0
python setup.py install

```

mysql-python

```
wget https://jaist.dl.sourceforge.net/project/mysql-python/mysql-python-test/1.2.4b4/MySQL-python-1.2.4b4.tar.gz
# tar -zxvf MySQL-python-1.2.3.tar.gz
# cd MySQL-python-1.2.3
# whereis mysql_config
mysql_config: /usr/bin/mysql_config /usr/share/man/man1/mysql_config.1.gz
# vi site.cfg
threadsafe = False
mysql_config = /usr/bin/mysql_config
# whereis mysql
mysql: /usr/bin/mysql /usr/lib/mysql /usr/include/mysql /usr/share/mysql /usr/share/man/man1/mysql.1.gz
# export LD_LIBRARY_PATH=/usr/include/mysql
# python setup.py build
# python setup.py install
```



