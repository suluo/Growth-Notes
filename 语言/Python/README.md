[Anaconda 教程命令](https://blog.csdn.net/u012151283/article/details/54145347)

```
wget https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh
sh Anaconda3-5.2.0-Linux-x86_64.sh

# 创建新环境 conda create --name <env_name> <package_names>
conda create -n python2 python=2.7 numpy pandas
# 切换环境 source activate <env_name>
source activate python2
# 退出环境
source deactivate
# 显示已创建环境
conda info --envs
conda info -e
conda env list
# 更新包
conda update --all
conda update <package_name>

# 删除环境 conda remove -n <env_name> --all
conda remove -n py36 --all
```

[生成requirement](https://my.oschina.net/zhangzhe/blog/413781)

```
#方法一
$ pip freeze > requirements.txt
#方法二
$ pip install pipreqs
$ pipreqs /path/to/projectr
# 方法三
$ pip install pigar
$ pigar
```

[Linux 个人目录下安装python](http://blog.csdn.net/dream_angel_z/article/details/51338546)

python

```
wget https://www.python.org/ftp/python/2.7.15/Python-2.7.15.tgz
wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz

tar -xzf Python-2.7.15.tgz
cd Python-2.7.15
mkdir -p /home/suluo/.local 
./configure --prefix="/home/suluo/.local"
make && make install
```

setuptools

```
wget --no-check-certificate https://pypi.python.org/packages/e9/c3/5986db56819bd88e1a250cad2a97249211686b1b7b5d95f9ab64d403a2cb/setuptools-38.2.5.zip#md5=abfd02fba07b381c3a9682a32d765cc6
unzip setuptools-38.2.5   
## tar -xzvf setuptools-2.0.tar.gz
cd setuptools-38.2.5
/home/suluo/.local/bin/python setup.py install
```

pip

```
wget --no-check-certificate https://pypi.python.org/packages/41/27/9a8d24e1b55bd8c85e4d022da2922cb206f183e2d18fee4e320c9547e751/pip-8.1.1.tar.gz#md5=6b86f11841e89c8241d689956ba99ed7
tar -xzf pip-8.1.1.tar.gz
cd pip-8.1.1
/home/suluo/.local/bin/python setup.py install
```

路径

```
# 路径设置
.bash_profile
export PATH=$HOME/.local/bin:$PATH
```

swig

[swig 无root](http://www.voidcn.com/article/p-xkijjfbj-eu.html)

```
wget http://prdownloads.sourceforge.net/swig/swig-3.0.12.tar.gz
tar -xzvf swig-3.0.12.tar.gz
cd swig-3.0.12/
#我们这里只编译Python的语言支持，可以根据你得需要加别的！
./configure --without-alllang --with-python|grep python
make && make install
```



