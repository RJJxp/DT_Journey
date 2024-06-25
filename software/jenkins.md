# Jenkins

## 安装
可以参考 [CSDN教程](https://blog.csdn.net/wohu1104/article/details/130712512), 先安装java
```bash
sudo apt update
# 安装java11
sudo apt install openjdk-11-jdk
# 验证java版本
java -version
```
开始安装Jenkins, 有两种方法, 一种下载jenkins可执行文件到本地, 通过java运行, 可参考文档 [Jenkins官网链接](https://www.jenkins.io/zh/doc/pipeline/tour/getting-started/); 此种方法运行, 需要启动后, 才可以访问Jenkins页面

另一种安装到系统, 可以设置系统自启动, 步骤如下:
```bash
# 添加 Jenkins 仓库密钥
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
# 添加 Jenkins 仓库, 将 Jenkins 的 Debian 包仓库地址添加到系统的软件源列表中:
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
# 更新软件包索引
sudo apt update
# 安装Jenkins
sudo apt install jenkins
# 启动Jenkins服务
sudo systemctl start jenkins
# 确保 Jenkins 服务在系统启动时自动运行 
sudo systemctl enable jenkins
```

