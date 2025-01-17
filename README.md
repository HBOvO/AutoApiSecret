# AutoApiSecret-加密版
AutoApi系列：AutoApi、AutoApiSecret、AutoApiSR、AutoApiS

### 项目说明 ###
* 利用github action实现**定时自动调用api**，保持E5开发活跃。
* **免费，不需要额外设备/服务器**，部署完不用管啦。
* 加密版，隐藏应用id+机密，保护账号安全。


* **常见错误及解决办法/更新日志**：https://github.com/wangziyingwen/Autoapi-test
* 网页获取refresh_token小工具（不建议使用）：https://github.com/wangziyingwen/GetAutoApiToken
* 视频教程：（我操作很慢，自行倍速/快进）
   * 在线/下载地址：https://kino-onemanager.herokuapp.com/Video/AutoApi%E6%95%99%E7%A8%8B.mp4?preview
   * B站：https://www.bilibili.com/video/av95688306/

  

--------------------------------------------------------------

## 步骤 ##  

   >:blush: :blush: :blush: :blush: :blush: :blush: 

   >请注意！请注意！请注意！

   >*** **有错误/问题请看**:    [常见错误及解决办法/更新日志](https://github.com/wangziyingwen/Autoapi-test)

   >*** 原教程/博客好像也坏了，看[视频教程](https://www.bilibili.com/video/av95688306/)吧，懒得补充，ORZ. （或者去网上搜一下，有一堆转载，关键词：github action e5 续期）

   >*** 注册应用时用到的[azure管理页面](https://portal.azure.com/)，或者直接到[仪表板](https://aad.portal.azure.com/)，找到注册应用选项

   >***【重定向URI】填写的内容：http://localhost:53682/

   >*** rclone下载，请自行百度谷歌，官网好像是[rclone.org](https://rclone.org)

   >:anguished: :anguished: :anguished: :anguished: :anguished: :anguished:  

* 第一步，先大致浏览[原教程](https://blog.432100.xyz/index.php/archives/50/)，了解如何获取应用id、机密、refresh_token 3样东西，以方便接下来的操作。  
  To get refresh_token:  
  ```shell
  rclone authorize "onedrive" "Client_ID" "Client_secret"
  ```

* 第二步，登陆/新建github账号，回到本项目页面，点击右上角fork本项目的代码到你自己的账号，然后你账号下会出现一个一模一样的项目，接下来的操作均在你的这个项目下进行。（看不到图片/图裂请科学上网）

  ![image-20210622090731400](https://cdn.jsdelivr.net/gh/donxan/pics@master/upic/image-20210622090731400_202106220907.png)
  
  根据[原教程](https://blog.432100.xyz/index.php/archives/50/)获取应用id、机密、refresh_token（自己复制保存，注意区分id机密，别弄混了）
  
  然后在线编辑你项目里的1.txt，将整个refresh_token覆盖粘贴进去（里面是我的数据，先删掉或者覆盖掉）。
  
    > refresh_token位置如图下。复制refresh_token紧接着的双引号里的内容（红竖线框起来的），不要把双引号复制进去。复制进1.txt后，留意结尾不要留空格或者空行
  
  ![image-20210622091159040](https://cdn.jsdelivr.net/gh/donxan/pics@master/upic/image-20210622091159040_202106220911.png)第三步，依次点击上栏Setting > Secrets > Add a new secret，新建两个secret如图：CONFIG_ID、CONFIG_KEY。
  
  内容分别如下: ( 把你的应用id改成你的应用id , 你的应用机密改成你的机密，单引号不要动 )
  
  CONFIG_ID
  ```shell
  id=r'你的应用id'
  ```
  CONFIG_KEY
  ```shell
  secret=r'你的应用机密'
  ```
  最终格式应该是类似这样的：
  
  ![image-20210622091414645](https://cdn.jsdelivr.net/gh/donxan/pics@master/upic/image-20210622091414645_202106220914.png)第四步，进入你的个人设置页面(右上角头像 Settings，不是仓库里的 Settings)，选择 Developer settings > Personal access tokens > Generate new token,
  
  设置名字为GITHUB_TOKEN , 然后勾选 repo , admin:repo_hook , workflow 等选项，最后点击Generate token即可。
  
  
  
* 第五步，点击右上角星星/star立马调用一次，再点击上面的Action就能看到每次的运行日志，看看运行状况

（必需点进去Test Api看下，api有没有调用到位，有没有出错。外面的Auto Api打勾只能说明运行是正常的，我们还需要确认10个api调用成功了，就像图里的一样。如果少了几个api，要么是注册应用的时候赋予api权限没弄好；要么是没登录激活onedrive，登录激活一下）

* 第六步，没出错的话，就搞定啦！！再看看下面的定时次数要不要修改，不打算改就忽略。

  **然后第二天回来确认下是否自动运行了（ation里是否多出来几个）**,是的话就不用管了，完结。
  
  我设定的每6小时自动运行一次，每次调用3轮（点击右上角星星/star也可以立马调用一次），你们自行斟酌修改（我也不知道保持活跃要调用多少次、多久）：

  * 定时自动启动修改地方：（在.github/workflow/AutoApiSecret.yml文件里，自行百度cron定时任务格式，最短每5分钟一次）
  
  * 每次轮数修改地方：（在1.py最后面）
  
  
------------------------------------------------------------
### 题外话 ###
> Api调用
  你们可以自己去graph浏览器看一下，学着自己修改要调用什么api(最重要的是调用outlook、onedrive)
  https://developer.microsoft.com/zh-CN/graph/graph-explorer/preview

### GithubAction介绍 ###
提供的虚拟环境：

· 2-core CPU
· 7 GB RAM 内存
· 14 GB SSD 硬盘空间

使用限制：
* 每个仓库只能同时支持20个 workflow 并行。
* 每小时可以调用1000次 GitHub API 。
* 每个 job 最多可以执行6个小时。
* 免费版的用户最大支持20个 job 并发执行，macOS 最大只支持5个。
* 私有仓库每月累计使用时间为2000分钟，超过后$ 0.008/分钟，公共仓库则无限制。

（我们这里用的公共仓库，按理，你们可以设定无限循环调用，然后6小时启动一次，保证24小时全天候调用）

