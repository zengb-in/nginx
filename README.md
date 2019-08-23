# shell
脚本
部署nginx网站加密模板
#!/bin/bash
path='/usr/local/nginx/conf'
o=$(date)
echo '
-----------------------------------start install nginx-----------------------------------
'
read -p '请输入路径: ' r
cd $r
        tar -xf /nginx-1.12.2.tar.gz
cd nginx-1.12.2
echo '
开始安装依赖包...
'
yum -y install gcc pcre-devel openssl-devel > /dev/null
a=$(rpm -qa gcc)
        if [ -z $a ]
then
                echo $a'安装失败'
fi
b=$(rpm -qa pcre-devel)
        if [ -z $b ]
then
                echo $b'安装失败'
fi
c=$(rpm -qa openssl-devel)
        if [ -z $c ]
then
                echo $c'安装失败'
fi
echo '
正在进行编译安装,请稍等...
'
./configure --with-http_ssl_module > /dev/null
make > /dev/null
make install > /dev/null
echo '
安装mariadb数据库...
'
yum -y install mariadb mariadb-server mariadb-devel > /dev/null
d=$(rpm -qa mariadb)
        if [ -z $d ]
then
                echo $d'安装失败'
fi
e=$(rpm -qa mariadb-server)
        if [ -z $e ]
then
                echo $e'安装失败'
fi
f=$(rpm -qa mariadb-devel)
        if [ -z $f ]
then
                echo $f'安装失败'
fi
echo '
安装php...
'
yum -y install php php-mysql php-fpm >/dev/null
g=$(rpm -qa php)
        if [ -z $g ]
then
                echo $g'安装失败'
fi
h=$(rpm -qa php-fpm)
        if [ -z $h ]
then
                echo $h'安装失败'
fi
H=$(rpm -qa php-mysql)
        if [ -z $H ]
then
                echo $H'安装失败'
fi
sed -i '65,71s/#//' $path/nginx.conf
sed -i '/SCRIPT_FILENAME/d' $path/nginx.conf
sed -i 's/fastcgi_params/fastcgi.conf/' $path/nginx.conf
systemctl stop httpd
/usr/local/nginx/sbin/nginx
read -p '查看端口: ' port
ss -untlp | grep $port > /dev/null
        if [ $port -eq 80 ]
        then
                echo '服务已开启'
        else
                echo '服务未开启'
        fi
systemctl start mariadb
systemctl status mariadb > /dev/null
                if [ $? -eq 0 ]
        then
                        echo -e '
\033[34mmariadb.service \033[0m
Active: active \033[32m (running)\033[0m since' $o
        else
                        echo -e ' 
\033[34mmariadb.service \033[0m
Active: inactive \033[31m (dead) \033[0m '
        fi
systemctl start php-fpm
systemctl status php-fpm > /dev/null
if [ $? -eq 0 ]
then
                        echo -e '
\033[34mphp.service \033[0m
Active: active \033[32m (running)\033[0m since' $o
        else
                        echo -e ' 
\033[34mphp.service \033[0m
Active: inactive \033[31m (dead) \033[0m '
        fi
~                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
~                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
~                      
