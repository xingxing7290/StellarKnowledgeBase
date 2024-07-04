# 软石科技




## SS-SW062
> [SS-SW062](/SS-SW602/docs)

## SD628
> [SD628](/SD628/) 

## DEMO
> <a href="/demo/">Docsify Demo 测试用例</a>



# Docsify_memory
基于docsify的个人知识仓库


# 官方网站
https://docsify.js.org 

# Linux云服务器部署
## node 环境部署 （华为云：Ubuntu 20.0）

在 root 目录下创建 node 文件夹
    mkdir  node
    cd node
    
下载 解压

` wget https://nodejs.org/dist/v12.16.3/node-v12.16.3-linux-x64.tar.xz `
   
` tar xf node-v12.16.3-linux-x64.tar.xz `

可以查看当前目录下的文件，执行：ls （命令）

解压成功后可以选择删除压缩包：
` rm -rf node-v14.17.4-linux-x64.tar.xz ` 
其中：-f 会提醒是否删除 ；-rf 会强制删除，不会提醒。（使用 rf，因为有些人不知道如何操作等待回车的对话线）
创建目录
` mkdir /usr/local/lib/node `
    
如果目录已经存在，则无需创建，也可以根据自己的喜好设置目录名称

移动目录并重命名

 ` mv node-v12.16.3-linux-x64 /usr/local/lib/node/nodejs `

设置环境变量(注意：这一步需要管理员权限或者对该文件的写入权限。)
执行：

   ` sudo vim /etc/profile `

输入 i 即可对文件进行编辑。
在文件底部添加环境变量：

` export NODEJS_HOME=/usr/local/lib/node/nodejs `
`  export PATH=$NODEJS_HOME/bin:$PATH `

执行命令（下方清单命令为保存退出）：

点击esc 输入:wq
保存并退出。

刷新修改

`  source /etc/profile `

安装完成，查看版本号
node版本号：

`  node -v `

npm版本号：

 `  npm -v `



## 安装docsify 


推荐全局安装 docsify-cli 工具，可以方便地创建及在本地预览生成的文档。

`npm i docsify-cli -g`

如果想在项目的 ./docs 目录里写文档，直接通过 init 初始化项目。

` docsify init ./docs `

通过运行 docsify serve 启动一个本地服务器，可以方便地实时预览效果。默认访问地址 http://localhost:3000 。

` docsify serve docs `

或者指定端口 

` docsify serve docs  -p 3001`

可以参考指令说明

https://github.com/docsifyjs/docsify-cli






参考博客

https://blog.csdn.net/liyou123456789/article/details/124504727
























