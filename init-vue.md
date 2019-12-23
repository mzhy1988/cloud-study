# 创建 vue 项目

## 1、第一步npm安装 <br>
   首先：先从nodejs.org中下载nodejs,安装后查看 node 和 npm的版本<br>
    node -v
    npm -v
	
## 2、使用淘宝NPM 镜像
   1、国内直接使用npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。<br>
   npm  install  -g  cnpm  --registry=https://registry.npm.taobao.org <br>
   2、windows 添加cnpm 的环境变量 <br>
   C:\Users\Administrator\AppData\Roaming\npm <br>
  这样就可以使用cnpm 命令来安装模块了：<br>

## 3、第二步项目初始化
   1.第一步：安装vue-cli <br>
   cnpm install vue-cli -g      //全局安装 vue-cli <br>
   2.查看vue-cli是否成功，不能检查vue-cli,需要检查vue <br>
   vue list 
## 4、选定路径，新建vue项目，cd目录路径
   vue init webpack  ”项目名称“ <br>
   
   现在已经创建好了，那就让项目先安装下依赖再运行一下，会出现下面的页面，操作指令是： <br>
   cnpm install <br>
   cnpm run dev <br>
   注意 这里要在sell下进行安装和运行的!!! <br>
