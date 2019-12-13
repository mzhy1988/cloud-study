## 一、新建系统用户<br>
   adduser mall
   passwd mall
## 二、创建ftp服务<br>
   ### 1、安装vsftp<br>
    yum -y install vsftpd
   ### 2、修改配置文件
    #vim /etc/vsftpd/vsftpd.conf
    anonymous_enable=NO //设定不允许匿名访问
    chroot_list_enable=YES //使用户不能离开主目录
    anon_upload_enable=YES
    anon_mkdir_write_enable=YES
  ### 3、设置vsftpd开机启动
    systemctl enable vsftpd.service
  ### 4、权限
    ftpusers和user_list没有任何关系
    ftpusers，是个黑名单，文件总是生效
    user_list则取决于userlist_enable和userlist_deny两项配置。
    userlist_enable和userlist_deny两个选项联合起来针对的是：
       本地全体用户（除去ftpusers中的用户） 和出现在user_list文件中的用户以及不在在user_list文件中的用户这三类
    用户集合进行的设置。
    当且仅当userlist_enable=YES时：userlist_deny项的配置才有效，user_list文件才会被使用；
    当其为NO时，无论userlist_deny项为何值都是无效的，本地全体用户（除去ftpusers中的用户）都可以登入FTP
    当userlist_enable=YES时，userlist_deny=YES时：user_list是一个黑名单，即：所有出现在名单中的用户都会被拒绝登入；
    当userlist_enable=YES时，userlist_deny=NO时：user_list是一个白名单，即：只有出现在名单中的用户才会被准许登入
    (user_list之外的用户都被拒绝登入)；另外需要特别提醒的是：使用白名单后，匿名用户将无法登入！
    除非显式在user_list中加入一行：anonymous
 ## 三、docker 安装redis
   ### 1、在用户目录下创建redis文件夹，redis.conf 和 data 目录
   ### redis.conf
   dir  /home/mall/redis/data
   logfile  /home/mall/redis/data/redis.log
   protected-mode no
   #注释掉，可以远程访问
   #bind 127.0.0.1
   #开启AOF
   appendonly yes
   开启redis 持久化
 ### 拉取镜像
 docker pull redis:3.2
 ### 创建容器
 docker run -d  -p 6379:6379  -v  /home/mall/redis/redis.conf:/etc/redis/redis.conf -v  /home/mall/redis/data:/data   --name mall-redis  redis:3.2 redis-server   --appendonly yes 
 ### 链接redis进行测试
 docker exec -ti mall-redis redis-cli -h localhost -p 6379 
