主要参考：phoenixnap.com/kb/openvpn-centos

差异点：
* 增加拷贝easyrsa3/pki/issued/server.crt 到 /etc/openvpn下
* 修改server.conf下的`dh dh2048.pem`为对应的pem名字，比如文章中生成为`dh.pem`
* 在client的配置中，去掉`remote-cert-eku`
* 在不使用firewalld时，可以改为iptables： 
```bash
systemctl mask firewalld
systemctl stop firewalld

vim /etc/sysconfig/selinux# 修改为SELINUX=disabled
systemctl enable iptables
systemctl start iptables
iptables -F
iptables -t nat -A POSTROUTING -s 192.168.200.024 -o eth0 -j MASQUERADE
iptables-save > /etc/sysconfig/iptablesvpn
```