树莓派安装pip3

---

1.首先安装setuptools

    cd /usr/local/src/
    wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-19.6.tar.gz
    tar -zxvf setuptools-19.6.tar.gz
    cd setuptools-19.6/
    python3 setup.py build
    python3 setup.py install

\#如果安装不成功，可以再试一次，如果是权限问题可以使用sudo命令来安装

2.安装pip3

    cd /usr/local/src/
    wget --no-check-certificate https://pypi.python.org/packages/source/p/pip/pip-8.0.2.tar.gz
    tar -zxvf pip-8.0.2.tar.gz
    cd pip-8.0.2/
    python3 setup.py build
    python3 setup.py install

\#如果安装不成功，可以再试一次，如果是权限问题可以使用sudo命令来安装

3.根据需要选择升级

    pip install --upgrade pip

4.安装python3虚拟环境测试PIP是否安装成功

    sudo pip install virtualenv


补充：

解决ModuleNotFoundError: No module named 'distutils.util'模块报错

    apt-get install python3-distutils

---

作者：竹门日叔

链接：https://www.jianshu.com/p/d4c1a08b993f

来源：简书

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。