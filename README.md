# deploy.sh
通用发布脚本：适用于使用git、svn等版本控制工具进行管理的类似php、python等语言开发的项目

## 一、方法及解释
```
usage               提示
set_remote_server   设置远程服务器地址
set_remote_path     设置远程服务器文件地址，及发布地址
chekc_par           检查基本变量是否存在
do_deploy           执行发布操作
last_check          最后请求用户进行检测发布内容
post_depoly         发送文件至远程服务器，即之后的相关操作
modify_deploy       用户自定义修改
loop_process        用于打印进程条
```

## 二、参数及解释
```
脚本参数：
NOW_PATH            当前脚本的路径
本地参数：
TAGS_PATH           1、svn、git服务器地址
ENV                 发布环境，例如：run，test，dev
TAG                 发布的tag名称
BUSINESS            被发布的服务器，通过此项确定ip即登录用户
TOOL                TOOL，使用的是git还是svn
目标服务器参数：
REMOTE_IP           目标服务ip地址
REMOTE_ACCOUNT      目标服务账户
REMOTE_PATH         目标服务器地址(发布地址)
HTTP_SERVER_ACCOUNT nginx用户账户
```
## 三、使用方法
### 1 . 在脚本中配置必要的参数
* 建议使用root用户使用发布脚本
* TAGS_PATH 、TOOL：这两个参数用于确定使用svn还是git,以及确定版本库的路径
* REMOTE_IP、REMOTE_PATH、HTTP_SERVER_ACCOUNT：在set_remote_server和set_remote_path中配置不同的运行环境，不同的域名使用什么服务器，什么文件路径。
* 在modify_deploy中，修改一些文件的替换，比如config.run文件替换config文件
* post_depoly中在return 0前可以加入自己适用的shell指令

### 2 . 命令行输入方法
示例：./deploy.sh -e test -v 20170504-1658-export-finance-for-admin -b torrent

参数解释：
* -p 为 TAGS_PATH
* -e 为 ENV
* -b 为 BUSINESS
* -v 为 TAG
* -t 为 TOOL

所有变量可以在脚本中先定义。
命令行输入的参数值，将会强制替换脚本中的定义的参数值。
### 3 . 发布
发布过程会打印一次本次发布的所有参数，确认后使用[Y|y]继续发布
