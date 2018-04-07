# centos7搭建ss并使用锐速优化网络
1. `yum install m2crypto python-setuptools`

2. `easy_install pip`

3. `pip install shadowsocks`

4. `vi  /etc/shadowsocks.json`

5. ```javascript 
    {
    "server":"0.0.0.0",
    "server_port":443,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"123456",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
    }
    ```

6. `yum install firewalld`

7. `systemctl start firewalld`

8. `firewall-cmd --permanent --zone=public --add-port=443/tcp`这里的443需要跟第五步设置的端口一样

9. `firewall-cmd --reload`

10. `ssserver -c /etc/shadowsocks.json -d start`

  到此ss已经安装完成，以下是使用锐速优化网络
     不支持Openvz
11. `wget -N --no-check-certificate https://github.com/91yun/serverspeeder/raw/master/serverspeeder.sh && bash serverspeeder.sh`
12. 如果报错：Serverspeeder is not supported on this kernel! View all supported systems and kernels here,表示内核不支持，需要执行
     1. `rpm -ivh http://soft.91yun.org/ISO/Linux/CentOS/kernel/kernel-3.10.0-229.1.2.el7.x86_64.rpm --force`
       2.`rpm -qa | grep kernel`如果有kernel-x.x.xx.x86_64则表示内核更换成功
       3.`reboot`重启
       4.重启后执行第10，11步
13. 如果报错：The name of network interface is not eth0, please retry after changing the name.表示网卡名不是eth0，需要执行
   1.`ip a`查看网卡，如果是eth0需要`yum install net-tools`
   2.重新执行第10，11步