## 我的阿里镜像地址
地址 https://cr.console.aliyun.com/cn-shanghai/instances/repositories <br
账户 mzhy198888 <br>
密码 15250987825a

## docker 安装
安装所需要的依赖包
yum install -y yum-utils device-mapper-persistent-data lvm2
改用阿里源
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
查看仓库内可选的版本包
yum list docker-ce --showduplicates | sort -r
选则其中一个包安装
yum install docker-ce-18.06.1.ce -y


## docker 镜像加速
鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，我们可以需要配置加速器来解决，我使用的是阿里的镜像地址：

1.查看 /etc/docker下是否有daemon.json 文件,如果有,直接在文件上按下面步骤更改,如果没有,则创建此文件: touch daemon.json
2.vi daemon.json 加入内容如下并保存退出:
{"registry-mirrors": ["https://96e6e1rd.mirror.aliyuncs.com"] }
执行命令:sudo systemctl daemon-reload
执行命令sudo systemctl restart docker

## rancher 安装
1、搜索镜像
docker search rancher
2、启动
docker run -d --restart=always -p 8080:8080 rancher/server


## 私有镜像仓库harbor 安装,
参考 https://blog.csdn.net/weixin_41465338/article/details/80146218
1、安装docker
2、安装python-pip
   yum install python-pip
3、安装 docker-compose
   pip install docker-compose
4、离线安装Harbor 
   ### 下载
   wget https://storage.googleapis.com/harbor-releases/release-1.8.0/harbor-offline-installer-v1.8.0.tgz
   ### 解压
   tar xvf harbor-offline-installer-v1.8.0.tgz
   #进入解压目录修改 harbor.cfg 文件
   #如下证书使用的是腾讯云申请的 DV SSL证书（Domain Validation SSL），申请后大概一小时会通过邮件得到信息，将申请到的证书下载下来，使用Nginx包内的Crt及Key文件即可。
   设置 hostname
   设置证书地址
   ssl_cert = /data/cert/server.crt
   ssl_cert_key = /data/cert/server.key
   设置管理员账户密码 harbor_admin_password
   #安装
   ./prepare
   ./install.sh 
   harbor的启动命令 docker-compose up 
   #安装后公司harbor地址 http://192.168.20.232:8999/harbor/
 
默认密码 Harbor12345

## 向harbor推送镜像###########################
### 1、关于harbor的安全问题 
    docker 的镜像拉取和推送默认https方式，harbor的https要去域名证书
	如果没有域名证书可以尝试使用http协议，在docker的配置里面增加可信镜像仓库
	1、查看 /etc/docker下是否有daemon.json 文件,如果有,直接在文件上按下面步骤更改,如果没有,则创建此文件: touch daemon.json

    2、daemon.json 添加 {"registry-mirrors": ["https://96e6e1rd.mirror.aliyuncs.com"],"insecure-registries": ["192.168.20.232:8999"]  }
       systemctl daemon-reload
       systemctl restart docker

### 2、docker login 192.168.20.232:8999 登陆harbor //用哪个用户登录就只能push到登录的用户 (超级管理员可以push镜像到任何项目）
  
   docker tag nginx:latest 192.168.20.232:8999/test/nginx-ma:v1 其中test为harbor中创建的仓库名称，nginx-ma为镜像名称

### 3、docker push  192.168.20.232:8999/test/nginx-ma:v1 上传镜像（上传时会根据tag的名称规则上传到响应的镜像仓库地址和仓库目录）

