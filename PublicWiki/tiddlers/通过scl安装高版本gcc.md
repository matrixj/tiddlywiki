# 系统版本
centos-7.2

# 安装步骤

```
[ol7_software_collections]
name=Software Collection packages for Oracle Linux 7 ($basearch)
baseurl=http://yum.oracle.com/repo/OracleLinux/OL7/SoftwareCollections/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1
```
保存到 /etc/yum.repos.d/ol7_collection.repo
```bash
curl https://yum.oracle.com/RPM-GPG-KEY-oracle-ol7 -o /etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
yum install devtoolset-10-gcc devtoolset-10-gcc-c++
scl enable devtoolset-10 bash
```

```bash
bash# gcc -v
gcc version 10.2.1 20200804 (Red Hat 10.2.1-2.0.1) (GCC)
```