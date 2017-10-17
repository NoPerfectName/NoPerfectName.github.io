---
layout: post
title: JEECG部署到远程服务器Tomcat的步骤
categories: Java
tags: Java
author: NoPerfectName
excerpt: 对JEECG部署到远程服务器（基于centos操作系统）Tomcat的步骤
---

* content
{:toc}

### 步骤
1. 安装java  
> yum install open-1.8.0-openjdk  

2. 获取java安装路径（后面会用）  
> /usr/sbin/alternatives --config java  

3. 安装Tomcat  
> （1）访问Tomcat官网，点击下载，然后在下载选项中获取下载的地址  
（2）wget "获取的下载的地址"  
（3）将Tmcat解压到自己想安装的路径下：tar -zxvf  "tomcat当前路径" -C "想要安装的路径"   

4. 测试一下Tomcat是否安装成功  
> 进入bin目录，输入./startup.sh，然后在本地访问服务器  

5. 如果步骤四成功，可忽略，否则，需要配置8080端口
> (1)vim /etc/sysconfig/iptables  
(2)添加-A INPUT -m state NEW -m tcp -p tcp --dport 22 -j ACCEPT  
(3) 重启防火墙：service iptables restart  

6. 完成步骤五后对tomcat的安装后，接下来就可以部署项目，但是这里存在一个问题，那就是Linux的系统和重启我们每次都需要接路径并且执行命令，比较麻烦，因此我们可以设置成service的形式来实现这个功能。在/etc/init.d目录下新建一个tomcat文件  
> (1)vim /etc/init.d/tomcat  
(2)添加以下内容,并将文件的权限修改为755（chmod 755 /etc/init.d/tomcat）,之后可以输入service tomcat start和service tomcat stop试一下    

```java
#!/bin/bash
# description: Tomcat Start Stop Restart
# processname:tomcat
# chkconfig:234 20 80
JAVA_HOME="获取的路径"
export JAVA_HOME
PATH=$JAVA_HOME/bin:$PATH
export PATH
CATALINA_HOME="Tomcat的安装路径"

case $1 in
start)
sh $CATALINA_HOME/bin/startup.sh
;;
stop)
sh $CATALINA_HOME/bin/shutdown.sh
;;
restart)
sh $CATALINA_HOME/bin/shutdown.sh
sh $CATALINA_HOME/bin/startup.sh
;;
esac
exit 0
``` 

 7.设置远程访问  
>  (1)windows下通过Xshell访问服务器  
（2）linux下安装rz，sz：yun install lrzsz -y  
   (3)将打包好的jeecg项目的war包拖拽到Xshell中  
  （4）将war包放在tomcat的webapps目录下  


