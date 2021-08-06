##有问题反馈
在使用中有任何问题，欢迎反馈给我，可以用以下联系方式跟我交流

* TG群: [@platypus](https://t.me/jiulan_platypus)

# jd_v4

###CentOS 安装 docker
##centos8.2以下的如果部署不成功，先重置服务器，再升级一下内核就可以了
#升级内核命令
```
sudo yum update
```
##安装依赖

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo

sudo sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```
##安装

```
sudo yum makecache fast
```

## 安装过docker 忽略--------------------------↓↓↓↓↓↓↓↓↓↓↓↓↓↓-------------------------
```
sudo yum install docker-ce
```
## 启动并加入开机启动
```
sudo systemctl start docker

sudo systemctl enable docker
```
##换源
#腾讯云用腾讯云的
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://mirror.ccs.tencentyun.com"]
}
EOF
```
# 阿里云服务器 
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://ryb0h6jn.mirror.aliyuncs.com"]
}
EOF
```
# 重启docker
```
sudo service docker restart
```
# 卸载Docker
```
sudo yum remove docker docker-common docker-selinux docker-engine
```
## 安装过docker 忽略--------------------------↑↑↑↑↑↑↑↑↑↑↑↑↑↑-------------------------
## Docker 部署京东脚本
# e大v4部署

```
sudo docker pull  nevinee/jd:v4
```
# nevinee/jd:v4 拉取不到可以替换备用镜像 
#TAG test 为nevinee/jd:v4 导出 jiulan/jd:test
#TAG v4重新打包 添加内置库 jiulan/jd:v4

```
docker run -dit \
-v /jd/config:/jd/config \
-v /jd/log:/jd/log \
-v /jd/scripts:/jd/scripts \
-v /jd/own:/jd/own \
-p 5678:5678 \
-e ENABLE_HANGUP=true \
-e ENABLE_WEB_PANEL=true \
-e ENABLE_WEB_TTYD=true \
--name jd \
--hostname jd \
--restart always \
nevinee/jd:v4
```
## 多容器配置 - 安装过shuye等占用jd容器名或者调整目录使用-使用lxk库（已清库）

```
docker run -dit \
-v /你想保存的目录/config:/jd/config \
-v /你想保存的目录/log:/jd/log \
-v /你想保存的目录/scripts:/jd/scripts \
-v /jd/own:/jd/own \
-p 5679:5678 \
-e ENABLE_HANGUP=true \
-e ENABLE_WEB_PANEL=true \
-e ENABLE_WEB_TTYD=true \
--name 容器名 \
--hostname 容器名 \
--restart always \
nevinee/jd:v4

```
## 多容器配置 - 更换默认库

```
docker run -dit \
-v /jd/config:/jd/config \
-v /jd/log:/jd/log \
-v /jd/scripts:/jd/scripts \
-v /jd/own:/jd/own \
-p 5679:5678 \
-e ENABLE_HANGUP=true \
-e ENABLE_WEB_PANEL=true \
-e ENABLE_WEB_TTYD=true \
-e REPO_URL=gitee \
-e JD_SCRIPTS_URL=https://gitee.com/jiulan0229/lxk_scripts.git \
-e JD_SCRIPTS_BRANCH="master" \
--name 容器名 \
--hostname 容器名 \
--restart always \
nevinee/jd:v4

```
## 使用jiulan改动版v4  （去除ck限制、添加jiulan内置脚本库）

```
docker run -dit \
-v /jd/config:/jd/config \
-v /jd/log:/jd/log \
-v /jd/scripts:/jd/scripts \
-v /jd/own:/jd/own \
-p 5679:5678 \
-e ENABLE_HANGUP=true \
-e ENABLE_WEB_PANEL=true \
-e ENABLE_WEB_TTYD=true \
--name 容器名 \
--hostname 容器名 \
--restart always \
jiulan/jd:v4

```


## v4更新脚本命令
```
docker exec -it jd1 bash jup
``` 


## 安装v4面板
-- 开启DIY每次重启会重启面板
#先进入容器
```
docker exec -it jd1 bash
```

# 拉取面板文件
```
wget -q https://ghproxy.com/https://raw.githubusercontent.com/jiulan/jd_v4/main/v4mb.sh -O v4mb.sh && chmod +x v4mb.sh && ./v4mb.sh
```
# 重启手动运行面板
. 先进入容器

```
cd panel

npm i

pm2 start server.js
```

#  -------------------------说明--------------------
#  特殊需要才添加     容器内调用

+  替换server.js 添加update_cookie 函数、解决无法扫码获取ck、屏蔽shell接口
```
wget -q https://ghproxy.com/https://raw.githubusercontent.com/jiulan/jd_v4/main/server.js -O /jd/panel/server.js
```

+  屏蔽拉取lxk库
```
wget -q https://ghproxy.com/https://raw.githubusercontent.com/jiulan/jd_v4/main/jup.sh -O /jd/jup.sh
```

+  V4 运行ck限制调整

   [食用方法](./update_ck_number.md)

+  获取京东cookie：
   [支持扫码、直接登录](http://jd.lianggenming.link)

# 页面访问

-p 宿主机端口:容器内端口

-p A:A 内外同端口

-p B:A 异端口

# 联系方式
[call](https://t.me/jiulan0111)

