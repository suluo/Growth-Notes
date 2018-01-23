[Linux 个人目录下安装python](http://blog.csdn.net/dream_angel_z/article/details/51338546)

python

```
wget https://www.python.org/ftp/python/2.7.14/Python-2.7.14.tgz
tar -xzf Python-2.7.14.tgz
cd Python-2.7.14
mkdir -p /home/suluo/masi/.local 
./configure --prefix="/home/suluo/masi/.local"
make
make install
```

setuptools

```
wget --no-check-certificate https://pypi.python.org/packages/e9/c3/5986db56819bd88e1a250cad2a97249211686b1b7b5d95f9ab64d403a2cb/setuptools-38.2.5.zip#md5=abfd02fba07b381c3a9682a32d765cc6
unzip setuptools-38.2.5   
## tar -xzvf setuptools-2.0.tar.gz
cd setuptools-38.2.5
/home/suluo/masi/.local/bin/python setup.py install
```

pip

```
wget --no-check-certificate https://pypi.python.org/packages/41/27/9a8d24e1b55bd8c85e4d022da2922cb206f183e2d18fee4e320c9547e751/pip-8.1.1.tar.gz#md5=6b86f11841e89c8241d689956ba99ed7
tar -xzf pip-8.1.1.tar.gz
cd pip-8.1.1
/home/suluo/masi/.local/bin/python setup.py install
```

路径

```
# 路径设置
.bash_profile
export PATH=$HOME/masi/.local/bin:$PATH
```



