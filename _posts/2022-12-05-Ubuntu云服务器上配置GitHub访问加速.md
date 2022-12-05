---
tags: Clash Ubuntu Tips
---

# 写在前面

想把nvim上的配置迁移到云服务器上, 但是直接`git clone`实在太慢了, 于是就通过clash走代理来完成. 参考[^1].

配置走了很多弯路, 主要是对端口的不熟悉, 把外部端口当成代理流量走的端口了(后来发现其实可以是一致的), 导致一直连不上网... 虽然之前也配置过clash, 但是真正把这个配置玩明白还是走了一些弯路.



# 配置git

首先安装:`sudo apt Install git`, 或者用LinuxBrew安装`brew install git`, 然后开始配置:

```bash
git config --global user.name "test"
git config --global user.email xxx@qq.com
```

然后还需要在GitHub导入公钥, 公钥的生成需要通过`ssh-keygen`, 目录在`~/,ssh/id_rsa.pub`, 里面的内容放在GitHub.com你的主页settings的`ssh和gpg`部分, `new ssh`里面. 

然后配置一下`~/.ssh/config`, 加上:

```bash
Host·github
Hostname·github.com
User·git
IdentityFile·~/.ssh/id_rsa.pub
```

完成之后, 可以验证一下:`ssh -T git@github.com`, 提示:

```bash
Hi Apocaly-pse! You've successfully authenticated, but GitHub does not provide shell access.
```

# clash配置

之前浅浅谈到过, 通过`pm2`的方式管理界面, 还是不错的, 这次用了web界面, 需要开启端口`addtcp 9090`, `reufw`, 然后在阿里云管理界面防火墙开一下相应端口. 

下载`clash-dashboard`, 如下:

```bash
mkdir ~/clash
cd ~/clash
git clone https://github.com/Dreamacro/clash-dashboard.git
cd clash-dashboard
```

然后配置`vi ~/.config/clash/config.yaml`:(所以后面启动的时候就可以直接用`./clash`了, 因为使用默认的配置文件夹)

```yaml
mixed-port:·7890
allow-lan:·true
bind-address:·"*"
mode:·rule
log-level:·info
secret:·"1234"
external-controller:·"0.0.0.0:9090"
external-ui:·/home/test/clash/clash-dashboard
```

主要修改的就是这么几项, secret是之后进入web界面时候的秘钥, 端口就是9090, 然后ip是公网IP.

>   内网代理是7890, 外部管理界面的端口是9090. 

然后加一个启动脚本:

```bash
vi start_clash.sh
#输入: `./clash`
chmod +x start_clash.sh
```

开启clash, 通过`pm2 start start_clash.sh`, 进入`http://<公网IP>:9090/ui`, 就可以看到管理界面了, 输入上面填写的信息, 就能进入管理界面了. 这里只能选择结点,不能更改配置. 



# 再次配置git

```bash
git config --global http.proxy 'http://127.0.0.1:7890'
```

这样就能愉快走代理加速GitHub了.

# ref

[^1]:[Linux安装Clash - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/396272999);