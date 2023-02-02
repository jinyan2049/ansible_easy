# ansible_easy #
very easy to use ansible
ansible-easy
ansible简易版，是我日常工作中经常使用到的批量执行命令和上传文件，参考了ansible工作模式，根据自己的情况定制了一个ansible-easy

# 安装python #

yum -y install epel-release

yum -y install wget zlib-devel bzip2-devel openssl-devel openssl-static ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel lzma gcc



yum -y groupinstall "Development tools"

cd /usr/local/src/ && wget -c https://mirrors.huaweicloud.com/python/3.9.13/Python-3.9.13.tgz


tar xvf Python-3.9.13.tgz


mv Python-3.9.13 /usr/local/python-3.9 && cd /usr/local/python-3.9/


./configure --prefix=/usr/local/sbin/python-3.9\
--enable-shared \
--enable-optimizations

make && make install

echo "/usr/local/sbin/python-3.9/lib/" >> /etc/ld.so.conf

ldconfig


rm -rf /usr/bin/python

ln -sv /usr/local/sbin/python-3.9/bin/python3 /usr/bin/python


sed -i '1s#usr/bin/python#usr/bin/python2.7#' /usr/bin/yum

sed -i '1s#usr/bin/python#usr/bin/python2.7#' /usr/libexec/urlgrabber-ext-down


rm -rf /usr/bin/pip

ln -s /usr/local/sbin/python-3.9/bin/pip3 /usr/bin/pip

pip install --upgrade pip -i https://pypi.tuna.tsinghua.edu.cn/simple

ln  -s  /usr/local/sbin/python-3.9/bin/pyinstaller /usr/bin/


# 依赖的第三方模块库 #
shell> pip install multiprocessing

shell> pip install paramiko

shell> pip install argparse

shell> pip install tqdm

使用介绍：

# python ansible-easy.py --help #
usage: ansible-easy.py [-h] [-c] [-p ] inventory

ansible-easy简易版（默认按照CPU核数并发执行）

positional arguments:

inventory host.txt主机ip文件

optional arguments:

-h, --help show this help message and exit

-c , --cmd 输入执行的命令，多个命令用;分号分割

-p , --mput sftp上传目录文件 [本地路径] [远程路径]

# 1）host.txt文件格式如下： #
ip:ssh端口,用户名,密码（端口不能省，必须加上）

shell> cat host.txt

192.168.137.131:22,jerry,123456

如果想设置连续多个ip机器列表，加一个横杠，如下：

192.168.137.131-150:22,jerry,123456

# 2）支持上传文件和目录 #
shell> python ansible-easy.py host.txt -p '/root/soft' '/tmp/soft/'

将本地/root/soft文件夹上传至远程主机/tmp/soft/目录下

# 3) 执行远程主机Linux命令 #
shell> python ansible-easy.py host.txt -c 'df -hT;date'

批量创建用户修改密码
shell> ./ansible-easy host.txt -c 'useradd jerry;echo "123456" | passwd --stdin jerry

shell> echo "jerry ALL=(ALL)NOPASSWD: ALL" >> /etc/sudoers'
