+++
author = "Aucker AN"
title = "HEXO Blog"
date = "2020-04-19T20:16:10+08:00"
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
+++

## Build a HEXO blog!!

* ### install node(nvm)
```shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
nvm install node
```
* ### change the `npm` mirrors
    * temporary use
    `npm --registry https://registry.npm.taobao.org install express`
    * set in the config
    `npm config set registry https://registry.npm.taobao.org`
    * use `cnpm`
    `npm install -g cnpm --registry=https://registry.npm.taobao.org`
* ### install HEXO
`sudo npm install -g hexo`
* ### create Blog folder
```shell
mkdir [filename]
cd [filename]
hexo init [filename]
npm install
hexo s
```
* ### associate with Git
    * create a new repo named with `$username.github.io`
    * back to the directory of the blog, edit the `_config.yaml` file
    ```yaml
    deploy:
    type: 'git'
    repository: https://github.com/XXX/XXX.github.io.git
    branch: master
    ```
    * generate the static HTML
    `hexo generate` or `hexo g`
    * config the HTML, and deploy the local blog to Github pages
    `npm install hexo-deployer-git --save`
    `hexo deploy` or `hexo d`
* ### Deploy blog
    * go the blog directory, execute `hexo new "newblog"`, a new file named newblog.md will apear in the directory [filename]/source/_posts, edit it with vim or typora or...
    * after editing, push it to github page:
    ```shell
    hexo generate
    hexo deploy
    ```
* ### change themes
defalut theme is `landscape`, you can choose anyone you like. like **Ada**

go to the directory of blog, execute:
```
git clone https://github.com/litten/hexo-theme-yilia themes/yilia
```
then, change the `_config.yaml`, change the `landscape` to `yilia`,after that:
```shell
hexo clean      //清除缓存文件（db.json）和静态文件（public）
hexo g          //生成缓存和静态文件
hexo s          //重新部署到服务器
```