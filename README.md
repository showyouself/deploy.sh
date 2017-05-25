# deploy.sh
通用发布脚本：适用于使用git、svn等版本控制工具进行管理的类似php、python等语言开发的项目

脚本执行流程：
* 1、检出指定tag->修改文件->压缩
* 2、根据ENV区分发布环境(run|test),不同的环境即不同的服务器(ip,account)
* 3、再根据BUSINESS区分不同路径(考虑到同一个服务器下可能存在多个网站)
* 4、发送至目标服务器->解压->覆盖

代码回滚：本脚本执行的是覆盖操作，不存在文件冲突，发上一次tag就行。

## 一、方法及解释
```
usage               提示
set_remote_server   设置远程服务器地址
set_remote_path     设置远程服务器文件路径
chekc_par           检查发布必须变量
do_deploy           执行发布操作
last_check          发布操作前请用户检查发布参数
post_depoly         发送文件至远程服务器，即之后的相关操作
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
TAG                 发布的tag名称，例如：v0.1
BUSINESS            被发布的服务器，通过此项确定ip和登录用户
TOOL                TOOL，使用的是git还是svn
目标服务器参数：
REMOTE_IP           目标服务ip地址
REMOTE_ACCOUNT      目标服务账户
REMOTE_PATH         目标服务器文件路径
HTTP_SERVER_ACCOUNT nginx、apache用户账户，设置相应的文件权限
```
## 三、使用方法
### 1 . 在脚本中配置必要的参数
* TAGS_PATH 、TOOL：这两个参数用于确定使用svn还是git,以及版本库服务器的地址
* REMOTE_IP、REMOTE_PATH、HTTP_SERVER_ACCOUNT：在set_remote_server和set_remote_path中根据不同的运行环境、不同的域名去配置对应的服务器ip、文件路径、登录账户。

### 2 . 命令行输入方法
示例：./deploy.sh -e test -v v0.1 -b torrent

参数解释：
* -p 为 TAGS_PATH
* -e 为 ENV
* -b 为 BUSINESS
* -v 为 TAG
* -t 为 TOOL

所有变量可以在脚本中先定义。
命令行输入的参数值，将会强制替换脚本中的定义的参数值。
### 3 . 发布
发布过程会打印一次本次发布的所有参数，确认后使用[Y|y]完成发布

### 4 . 小提示
* 建议使用root用户执行发布脚本，并且配置 [2台linux ssh免密登录](http://zengbingo.com/p/252.html)
* 在modify_deploy中，修改一些文件的替换，比如config.run文件替换config文件
* post_depoly中在return 0前可以加入自己适用的shell指令
