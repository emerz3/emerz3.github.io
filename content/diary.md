---
title: Daily Notes
date: 2020-09-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["first"]
author: "aucker & emerz3"
# author: ["Me", "You"] # multiple authors
ShowToc: true
TocSide: left
draft: false
description: "My daily Notes"
---


## 04 Oct
### Ubuntu删除无用的ppa（之前安装的ppa现在因为proxy缘故下载不下来，需要删除）

1. 直接在/etc/apt/sources.list.d/文件夹中删除对应的文件即可。
2. 可以安装ppa-purge

### GitHub回到指定的commit
```shell
git clone $URL
cd $PROJECT_NAME
git reset --hard $SHA1
```
返回最近提交：`git pull`
### 更新了oh-my-zsh的主题
默认主题由于过于简洁导致时长分不清当前工作目录；因此更换了一个新的`eastwood`主题，这是和`westwood`相互照应吗🤔， 效果不错。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21736221/1664872878730-966d747d-e9ac-4100-b2cf-be347999fa0a.png#clientId=ud7e51f78-a6ff-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=155&id=u6a4600fe&margin=%5Bobject%20Object%5D&name=image.png&originHeight=162&originWidth=730&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27704&status=done&style=none&taskId=uc05efcbf-cdaf-4f1b-aeff-1c6fce3dde6&title=&width=697#crop=0&crop=0&crop=1&crop=1&id=IWRGL&originHeight=162&originWidth=730&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
## 05 Oct
### Check the version and codename in `WSL`
```shell
[~]$ lsb_release -dc
Description:    Ubuntu 20.04.5 LTS
Codename:       focal
```

### Bug: Protocol "https:" not supported. Expected "http:" [#2003](https://github.com/npm/cli/issues/2003)
In node, when the `npm`version is lower than `7.0.6`, you will get the error.
you can either upgrade the `npm`version or **unset** you proxy settings.

### Install `NVIM`
Trying to install nvim, and use the config from the [github](https://github.com/aucker/nvim-config/blob/master/docs/nvim_setup_linux.sh), there are some problems:

1. mind the `python`, `path`variable, you need to change it.
2. install the correct `node`version, refer to the bug above. 16 is better.
3. this sh use all from souce, so you might have problem when you install from the package manager ( like apt).
4. the shell script needs to be update, some are not so good (it use zsh as default, but it don't config the zsh).

Currently I will still use the `vim` with some default configs, when I have time, I will migrate to `nvim`.
In case I forget, the relevant folders are `~/package`, `~/tools`.

### Show the size of a directory
```shell
[main][~/archive/CS/data_structure/hashtable]$ du -sh *
4.0K    Cargo.lock
4.0K    Cargo.toml
4.0K    README.md
12K     hashtable.cpp
12K     src
20K     unsafe.gif
```

## 08 Oct
### Ubuntu apt [problem](https://askubuntu.com/questions/1096930/sudo-apt-update-error-release-file-is-not-yet-valid)
When updating ubuntu with `sudo apt update`, got the error:
```shell
[~]$ sudo apt update
Hit:1 https://mirrors.ustc.edu.cn/ubuntu focal InRelease
Get:2 https://mirrors.ustc.edu.cn/ubuntu focal-security InRelease [114 kB]
Get:3 https://mirrors.ustc.edu.cn/ubuntu focal-updates InRelease [114 kB]
Get:4 https://mirrors.ustc.edu.cn/ubuntu focal-backports InRelease [108 kB]
Reading package lists... Done
E: Release file for https://mirrors.ustc.edu.cn/ubuntu/dists/focal-security/InRelease is not valid yet (invalid for another 14h 9min 28s). Updates for this repository will not be applied.
E: Release file for https://mirrors.ustc.edu.cn/ubuntu/dists/focal-updates/InRelease is not valid yet (invalid for another 15h 3min 44s). Updates for this repository will not be applied.
E: Release file for https://mirrors.ustc.edu.cn/ubuntu/dists/focal-backports/InRelease is not valid yet (invalid for another 14h 11min 52s). Updates for this repository will not be applied.
```
Run:
```shell
sudo hwclock --hctosys
```
This command gets the latest time from Windows machine's RTC and sets the system time to that.

## 09 Oct
### Git SSH
I want to add another github account in my terminal, I refer stackoverflow for some ways. The [following](https://stackoverflow.com/questions/40069344/remote-rejected-master-master-permission-denied#:~:text=2FA%20in%20github-,Solution%20was%20next%3A,-Generate%20new%20SSH) is a work way:

1. Generate new ssh `id_rsa`key ([Link1](https://stackoverflow.com/questions/32910928/ssh-keygen-no-such-file-or-directory#:~:text=The%20command%20could%20not%20save%20your%20key.) & [Link2](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key))
2. Add the SSH key to ssh-agent [Link](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)
3. Add a new SSH key to your account [Link](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account#adding-a-new-ssh-key-to-your-account)
4. Change remote for repository from `https`to `git`
```shell
ssh-keygen -t rsa -b 4096 -C "mail@host.com" -f id_rsa_user  # step 1: should at ~/.ssh/

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_user  # step 2: add ssh key to ssh-agent

git remote set-url origin git@github.com:username/repository.git #step 4: change remote for the repo
```

This is also a work [Link](https://stackoverflow.com/questions/4220416/can-i-specify-multiple-users-for-myself-in-gitconfig#:~:text=This%20solution%20takes%20the%20form%20of%20a%20single%20git%20alias.). I haven't give it a try.

## 10 Oct
### yt-dlp
Download the youtube video, the `youtube-dl`is so slow, even with proxy.
Use the command:
```shell
yt-dlp -f b --proxy http://$host_ip:7890 $LINK
```
Speed is alright.

## 14 Oct
### Make microsoft software available even when using proxy:
Use the clash, then in `UWP Lookback`, mark the software you want to use.
![image.png](https://cdn.nlark.com/yuque/0/2022/png/21736221/1665735767707-8b778284-4117-4ca1-9c6b-f3923199ac4c.png#clientId=ucd4feb0f-6c94-4&crop=0&crop=0&crop=1&crop=1&errorMessage=unknown%20error&from=paste&height=361&id=udef999bf&name=image.png&originHeight=361&originWidth=754&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36585&status=error&style=none&taskId=u4cc87779-1e71-4a6c-93e1-555ab923aa3&title=&width=754)
## 15 Oct
### Make `python`excute `Python 3`
Install `python-is-python3` with 
```shell
sudo apt install python3
```
Then the `python2`in your computer will also be useless, then just 
```shell
sudo apt autoremove python2 
```
and every thing works fine.

### Change the hostname of Ubuntu
The default config for standard Ubuntu is at `/usr/hostname`and `/usr/hosts`, just replace them with the name you want.
But in WSL2, you should change the `wsl.conf`which is under `/usr/wsl.conf`with
```shell
[network]
hostname = focal
generateHosts = false
```
Then restart, everything works fine.

## 18 Oct
### Picture the code
`silicon`is a tool that can make png file with code input.
```shell
silicon main.rs -o main.png
```
Use with `cargo install silicon`, in Ubuntu, perhaps need install some libs to make it work,
use `apt search xxx`to search the package.

## 20 Oct
### SQL standard
I am working miniob to add some basic SQL syntex, I find in most database, like `mysql`, `postgres`, `mariadb`, the SQL query `select id, * from t;`will cause the SQL error.

But in `spark-sql`, I can even write this query: `select id, *, id, *, id from t;`, this is ridiculous, I need to remove the `COMMA STAR attr_list`in `attr_list`cause it is not the standard SQL in most databases.

### Docker in WSL
I install `docker`in my WSL2, I can test something on wsl mysql easily.
My conf is 
```shell
[boot]
command="service docker start"
```
But since wsl supports `systemd`on Sep, after I set
```shell
[boot]
systemd=true
command="service docker start"
```
I can't start my docker service, I tried `sudo dockerd --debug`, `sudo systemctl start docker`from stackoverflow, but none of them works. 
Then I tried remove `systemd=true`, docker works again. But I still don't know why.

## 22 Oct
### Type Conversion in C++

1. Implicit Type Conversion
```cpp
int num_int = 99;
double num_double;
num_double = num_int;

// Or
int num_int;
dourble num_double = 9.99;
num_int = num_double;
```
Implicit type conversion may cause data lossing when convert the larger type to smaller type.
like  from Long double to double.

2. Explict Type Conversion
- C style type conversion
```cpp
(data_type)expression
int num_int = 26;
double num_double;
num_double = (double)num_int;
```

- Function-style casting
```cpp
data_type(expression)
int num_int = 23;
double num_double;
num_double = double(num_int);
```

- Type conversion Operators
   - static_cast
   - dynamic_cast
   - const_cast
   - reinterpret_cast
## 25 Oct
The `winget`is built-in program for windows to install softwares.
The `scoop`is another package manager like `brew`in Mac, `apt`in Debian.

The text to ASCII, use the `figlet`command (a fun fact: there is also a software named `toilet` that can also generate ASCII characters.

The `feh`use the X window to display image, which is very lightweight, 
besides, use the `explorer.exe xxx.png`can also open the image file. Just add an alias with 
`alias winfile="explorer.exe"`. This is very useful.

## 01 Nov
### `SELECT *`will make the query slow

1. 效率低的原因
   1. 不需要的列会增加数据传输时间和网络开销
      1. 需要解析更多的对象、字段、权限、属性等相关内容，在SQL语句复杂，硬解析较多的情况下，会对数据库造成沉重的负担
      2. 增大网络开销；*有时会带上如log、IconMD5之类的无用且大文本字段，数据传输size会集合增长。如果DB和应用程序不在同一台机器，这种开销非常明显。
      3. 即使MySQL服务器和客户端是在同一台机器上，使用的协议还是tcp，通信也是需要额外的时间。
   2. 对于无用的大字段，如varchar、blob、text，会增加io操作
      1. 准确来说，长度超过728字节的时候，会先把超出的数据序列化到另外一个地方，因此读取这条记录会增加一次io操作。
   3. 失去MySQL优化器“覆盖索引”策略优化的可能性
      1. 基于MySQL优化器的“覆盖索引”策略又是速度极快，效率极高，业界极为推荐的查询优化方式。

## 18 Nov
### `perf` tool in WSL2

Normally, there is no perf tools in WSL2 linux, (Debian/Ubuntu), in order to use perf tools, you need to use [WSL kernel](https://github.com/microsoft/WSL2-Linux-Kernel) to compile it locally. The folder is `/WSL*/tools/perf`. Before compile, install the `bison` and `flex`. Then just run `make`. After compiling, there are two ways to use perf:
1. add the perf `realpath` to `PATH`, like `export PATH=$PATH:/<path of perf>` in `.zshrc` or `.bashrc` file.
2. just copy the `perf` file to `usr/local/bin`.

Way 2 is better.

Then refer this [blog](https://www.imlc.me/perf/) and flamegraph github readme file to draw flamegraph.

### Python tools

There are some tools to check the python code. Like `mypy`, `pyflakes3`, `black`. In C/C++, `cpplint` is a good choice.

## 19 Nov
### `IRC` on Debian

I am using the weechat as my IRC client, my IP is banned by libera, luckily, the `OFTC` is still available. There are some interesting channels on Debian IRC, and I even find a `#debian-zh` channel.

Here are step I use weechat(I did not use the ssl):
1. use `/help server` to check some useful command to connect, list, or delete a server.
2. use `/server add oftc irc.oftc.net` to add this server to weechat. PS. refer to [weechat guide](https://weechat.org/files/doc/stable/weechat_quickstart.en.html#add_irc_server)
3. you can alias your name like `/set irc.server.oftc.nicks "<your nicks>"`
4. some channels like `#debian-zh` requires you to register and verify your usename. refer to [msg NickServ help](#irc-nickserv).
5. use `/join #debian-zh`, here you are!

### IRC NickServ

Some channels require you to register a nickname before chatting.
`/msg NickServ help` will show how to register.
use `/msg NickServ register <passwd> <mail>` to register a nickname. Both are required to register. 
mail is only used to find the passwd when you do not remember.
After that you can use `/msg NickServ info <nick>` and `/msg NickServ status <nick>` to check your nickname.
![image](https://user-images.githubusercontent.com/41871212/202839682-d93c477b-913f-41ae-a8af-515782632010.png)

### Blog with hugo and papermod

Coder theme supports code not well, so I choose change coder to papermod.

It takes me a lot of time to understand the domain. At last, it works! I put the `CNAME` file in the static folder according to [manual](https://gohugo.io/hosting-and-deployment/hosting-on-github/#use-a-custom-domain), then every time I update my blog, `aucker.me` works well, cool.👏

Plus, I also learn a git command [refererence](https://stackoverflow.com/questions/2003505/how-do-i-delete-a-git-branch-locally-and-remotely#:~:text=2511-,The%20short%20answers,-If%20you%20want):

#### **Delete a remote branch**

```shell
git push origin --delete <branch> # Git version 1.7.0 or newer
git push origin -d <branch>   
git push origin :<branch>         # Git version older than 1.7.0
```

#### Delete a local branch

```shell
git branch --delete <branch>
git branch -d <branch>
git branch -D <branch>
```

To set a local mirror of git, refer [this](https://github.com/auckerlab/simple-db-hw-2022#setting-up-git)
