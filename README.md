# 在Linux端用Nonebot2整一个QQ机器人

## 首先选择一个云服务器

我选择的是腾讯云服务器：
 https://cloud.tencent.com/product/cvm

是腾讯云的2核4G的轻量云服务器。

创建一个属于自己的用户

## 然后在服务器配置PYTHON 

#### 推荐工具WINSCP和Termius

用WINSCP输入端口号远程登录到服务器查看文件

使用Termius进入编辑

#### 回到正题

这里是我的最初的PYTHON 版本!

![image](https://user-images.githubusercontent.com/99710432/157445802-1c315b94-b745-4f41-8cb0-5218ff7206a0.png)


可以看到我的PYTHON 版本不对，必须要3.7以上

### 需要重新安装PYTHON ：

```bash
cd 
ls
cd home/ubuntu
1. sudo mkdir /usr/local/python3 
2. wget https://www.python.org/ftp/python/3.9.6/Python-3.9.6.tgz 
3.tar -xzvf Python-3.9.6.tgz	
4.cd Python-3.9.6
5.sudo ./configure --prefix=/usr/local/python3 
6.sudo make&&make install
或者分开整也彳亍：
7.sudo make install
8.sudo apt install python3-pip

```

### 安装pip

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py 
python get-pip.py
pip3 install xxx
export PATH=/home/ubuntu/.local/bin


```

## 接着再配置机器人环境：

创建一个空文件夹，添加到项目中 

```bash
 pip install nb-cli
```

```bash
pip3 install pydantic loguru pygtrie httpx python-dotenv uvicorn fastapi requests selenium aiocqhttp ffmpeg aiohttp -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

安装成功后，在终端键入

```bash
 nb create
```

点击回车选择cqhttp。

安装一些需要的库

```bash
pip install nonebot2 -i https://pypi.tuna.tsinghua.edu.cn/simple/
pip install nonebot-plugin-apscheduler nonebot-adapter-cqhttp -i https://pypi.tuna. .tsinghua.edu.cn/simple/

```

删除两个文件，config.yml和go-cqhttp。

配置env文件和bot.py文件具体在官网可以看到

```bash
ENVIRONMENT=dev
CUSTOM_CONFIG=common config

```

```bash
HOST=127.0.0.1
PORT=8890
DEBUG=true

```

```bash
HOST=127.0.0.1
PORT=8890
SECRET=
ACCESS_TOKEN=

```

配置端口号，这里端口号要一致，我的是这样婶儿的：

```bash
HOST=127.0.0.1
PORT=53245
SUPERUSERS=["2411849197"]
NICKNAME=["stupid","bot"]
COMMAND_START=["/"]

```

## 运行bot.py

```bash
python3 bot.py

```

可以看到没有错误惹

## 整完之后再整go-cqhttp

在github上下载Linux的版本

解压后进入文件夹，最后一个文件里面是一个名为go-cqhttp的文件。把这个go-cqhttp文件上传和我们项目的文件夹放到一起。

### 配置运行go-cqhttp

#### 运行

```bash
./go-cqhttp
```

#### 对go-cqhttp进行权限赋予。

```bash
chmod 777 ./go-cqhttp

```

（我写反了不要在意这些细节）

一定要选择3反向websocket通信



#### 修改配置文件config.yml

```bash
vim config.yml

```

修改一下config.yml

把universal后面的改成 ws://127.0.0.1:8890/cqhttp/ws，这里的8890改成自己的端口号就行了!

#### 再次运行go-cqhttp

输入

```bash
./go-cqhttp
```

连上自己机器人的QQ号,机器人成功可以跟他简单的指令

Success!

![](https://s2.loli.net/2022/02/15/oOTVUS4WzdiyMKH.png)

## 启动不挂断

### 查看后台进程

```bash
ps -ef
```

### 搜索进程

```bash
ps -ef|grep go-cqhttp
```

### 不挂断启动

```bash
nohup ./go-cqhttp &
```

（一般不想结束进程吧）


## 接着可以编写插件

在原来的目录下创建新文件夹

(nonebot2有多种插件加载方式,官网上可以看到很多)

创建py文件

编辑想要的插件

```bash
from nonebot.adapters.cqhttp import Bot, Event
from nonebot.plugin import on_message

```

定义回复的内容,可以丰富一点比如

```bash
reply_dic = {
    '您好': '好' ,
    '晚安'    : '做个好梦',
    '哈哈'    :'哈哈哈哈'
}

```

### 设置一下优先级

```bash
reply = on_message(priority=100)
```

判断获取的QQ信息是否在上面定义的内容里，如果在则机器人可以发送字典对应的内容。

发送函数： await reply.finish(reply_msg)

然后就可以跟机器人说指定的内容,他就可以回复惹.

![](https://s2.loli.net/2022/02/15/MK53XLCPGSb76Nj.jpg)

## Ps心得:

/步骤说的比较简略,目的是想整理整体思路

/制作中间也遇到了很多坎坷,根本没有很容易,比如我已经做到快结束时才发现PYTHON 的级别不够,当时也不知道,这一个小问题就搞了一天.整体顺序都是乱的 

/遇到问题主要是上网查和问别人

/说实话也没想到能整多高级

/小白做到这里感觉很开心惹!
