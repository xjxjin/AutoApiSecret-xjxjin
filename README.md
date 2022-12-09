# AutoApiSecret-加密版

AutoApi系列：AutoApi、AutoApiSecret、AutoApiSR、AutoApiS

### 项目说明 ###

* 利用github action实现**定时自动调用api**，保持E5开发活跃。
* **免费，不需要额外设备/服务器**，部署完不用管啦。
* 加密版，隐藏应用id+机密，保护账号安全。

### 特别说明/Thanks ###

* 原教程博主-黑幕（酷安id-Paran）：https://blog.432100.xyz/index.php/archives/50/

* 普通版地址：https://github.com/wangziyingwen/AutoApi

* 加密版地址（推荐）：https://github.com/wangziyingwen/AutoApiSecret

* 模仿人为应用开发版（包含升级步骤）：https://github.com/wangziyingwen/AutoApiSR

* 超级版地址： https://github.com/wangziyingwen/AutoApiS

* **常见错误及解决办法/更新日志**：https://github.com/wangziyingwen/Autoapi-test

* 网页获取refresh_token小工具（不建议使用）：https://github.com/wangziyingwen/GetAutoApiToken

* 视频教程：（我操作很慢，自行倍速/快进）

  * 在线/下载地址：https://kino-onemanager.herokuapp.com/Video/AutoApi%E6%95%99%E7%A8%8B.mp4?preview

  * B站：https://www.bilibili.com/video/av95688306/

    ​      

   **以上推荐/不建议等只是个人意见，请自行选择版本，可同时使用**。

--------------------------------------------------------------

### 步骤 ###



第一步，首先去https://portal.azure.com/#home注册一个应用,这一步网上的教程实在是太多了,我就不详细写了,大致写一下流程
先用e5管理员账号登录网站,然后在主页找到Azure Active Directory点进去，再在左侧目录找到点击应用注册，再点上方的新注册就会跳出一个新建应用的界面，应用名字随意填写,然后选择任何组织目录(任何 Azure AD 目录 - 多租户)中的帐户，重定向url选web，填入`http://localhost:53682/`,最后点注册即可



   *** **有错误/问题请看**:    [常见错误及解决办法/更新日志](https://github.com/wangziyingwen/Autoapi-test) ***   

* 第一步，首先去https://portal.azure.com/#home注册一个应用,这一步网上的教程实在是太多了,我就不详细写了,大致写一下流程
  先用e5管理员账号登录网站,然后在主页找到Azure Active Directory点进去，再在左侧目录找到点击应用注册，再点上方的新注册就会跳出一个新建应用的界面，应用名字随意填写,然后选择任何组织目录(任何 Azure AD 目录 - 多租户)中的帐户，重定向url选web，填入`http://localhost:53682/`,最后点注册即可

* 第二步，注册好应用会跳转到应用概述界面,你会看到一个应用程序(客户端) ID,复制这个Id记录下来，后面要用到,然后点击左侧目录的API权限,依次点击`添加权限`、 `Microsoft Graph` 、`委托的权限`,然后依次搜索以下这12个权限并勾选:

  ```
  Files.Read.All Files.ReadWrite.All Sites.Read.All Sites.ReadWrite.All
  User.Read.All User.ReadWrite.All Directory.Read.Al Directory.ReadWrite.All
  Mail.Read Mail.ReadWrite MailboxSettings.Read MailboxSettings.ReadWrite
  ```

  全部勾选好后点击底部的添加权限，然后又返回到了API权限界面，这时候你一定要再点一下`代表xxx授予管理员同意`,不点这个,outlook api会无法调用

* 第三步，点击左侧证书和密码,点+新客户端密码,说明随便填，年限随便选多久都行,然后点添加,添加好后,客户端密码下面会有一个值,复制值下面的那一串代码，这是应用秘钥，后面会用到,到这一步，注册应用已经结束了

![输入图片说明](.github/bbb%E5%9B%BE%E7%89%87.png)

* 第四步，windows下载rclone获取token,[点击这里下载rclone](https://rclone.org/downloads/),随意下载到电脑的任意一个目录,下载后`不要双击rclone.exe安装!`,而是在rclone.exe同目录下,`按住shift后点鼠标右键`，选择`在此处打开cmd窗口`或`在此处打开power shell窗口`,弹出窗口后,CMD窗口就执行:

  ```
  rclone authorize "onedrive" "之前保存的应用id" "之前保存的应用秘钥"
  ```

  请自行将双引号内的替换为之前我们保存的应用id和秘钥,示例:

  ```
  rclone authorize "onedrive" "729xx16f-8x70-4xb8-8fd6-1xxx9b582b1f" "?@P@4u/fxxcxxx28:B-3i_QxxFxc6_ZO"
  ```

  如果是power shell的窗口请执行:

  ```
  .\rclone authorize "onedrive" "729xx16f-8x70-4xb8-8fd6-1xxx9b582b1f" "?@P@4u/fxxcxxx28:B-3i_QxxFxc6_ZO"
  ```

  执行后电脑浏览器会弹出一个界面,登陆自己的e5账号,然后看到浏览器显示`Success`!，说明获取token成功了。然后我们返回的cmd窗口或者power shell窗口，你会看到一大段`Paste the following into your remote machine --->`开头,`<---End paste`结尾的代码，找到`"refresh_token":"`复制后面的代码直到`","expiry"`，说白了就是复制refresh_token，不要带双引号,类似格式如下: `OAQABAAAAAABeAFzDwllzTYxxxx_qYbH8UALCVjtv_6YeHHOwXExxxxxywOKSg2Hd_GSjW1vcLzqLhDC51Sl4T2ZYfK1p64_ps3qidrodIZLkz-4f-21IfUUgQdEi-g-jIw-La9FjREuUuQnSSKgOlBAKpiwVjwPGdaO_G9yB5cLvX5zi3MZ-_ZwEVHEp-ldDGYqQiZFSnpD6G-cjQIzuN0w8lxl_9laIH0dkA1uUOKtA64qbC976OHSIaidaF4oZi_ntQIsMHWnUssYbR-2X446apxxMupLRM5oaHb8bKMTDlzk6_zUOw23y1jcb8gzyzL5IZdBVVX9UIuPrR-yuzyTd24v39OGk-I9xxhRms5vM6-vUPgxKzuIwFq_CYothdbo8ZvBuMJebl21D1UeaBerjPzxxxxxxxxxVQakxjMBHPC1ueyxR2UvRrlhHhNs8qYFBe5lzceofNWvy1QYRObT8DqCENyLa4Nb08jVTcA6Eh7oxkXtflg_xEY8ECRTWGIZ2qo4ziW70xxxxxxxvq6MCubQgOdt0qdWrc15LVV99YAl9L0KtC3ql0tMPVJBvodTNrvVqcxD-LNtnpxxx1J2tmDuc15xxxxxxTPp5MjXDhSbq8MACmRQh4dR09QqmqXps1c80pxyVkQbr8O669MQ1FMqlICTKJQ8c54_U9NWBo1rAU_zPmE841mDEFjy7dXakFkYR9IIthPNBr2nCQDdvjTitCiIwcT-NTitAd7TseSpiWg9zBsd6Q1OOcL83anZnaJ4sHy68XupeFydmjIYWZw83m96xRaw5MMHJAoyeTkwkHH9qqaSZ0mNM_PN09-tj6nUVYWf5lajMNzd_0GPfwqxxxx9LC0deo43zNTZq20f94_-HNTscKg5dJOA8jUeddxxxxLQa-ZXZV38-lxxxYL_ZDvPu5-0FP-aDTwvxxxx0F7g97o3wTrHSZw14Ra9uxniTh4gAA`

![输入图片说明](.github/aaa%E5%9B%BE%E7%89%87.png)



以上为准备工作

## 本地服务器执行

6、修改以下脚本，在脚本11行和13行的单引号内分别填入之前保存的应用id和应用秘钥，保存为1.py文件。

```
import requests as req
import json,sys,time,random
#先注册azure应用,确保应用有以下权限:
#files:	Files.Read.All、Files.ReadWrite.All、Sites.Read.All、Sites.ReadWrite.All
#user:	User.Read.All、User.ReadWrite.All、Directory.Read.All、Directory.ReadWrite.All
#mail:  Mail.Read、Mail.ReadWrite、MailboxSettings.Read、MailboxSettings.ReadWrite
#注册后一定要再点代表xxx授予管理员同意,否则outlook api无法调用

###################################################################
#在下方单引号内填入应用id                                         #
id=r''                         
#在下方单引号内填入应用秘钥                                       #
secret=r''                        
###################################################################

path=sys.path[0]+r'/1.txt'
num1 = 0

def gettoken(refresh_token):
    headers={'Content-Type':'application/x-www-form-urlencoded'
            }
    data={'grant_type': 'refresh_token',
          'refresh_token': refresh_token,
          'client_id':id,
          'client_secret':secret,
          'redirect_uri':'http://localhost:53682/'
         }
    html = req.post('https://login.microsoftonline.com/common/oauth2/v2.0/token',data=data,headers=headers)
    jsontxt = json.loads(html.text)
    refresh_token = jsontxt['refresh_token']
    access_token = jsontxt['access_token']
    with open(path, 'w+') as f:
        f.write(refresh_token)
    return access_token
def main():
    fo = open(path, "r+")
    refresh_token = fo.read()
    fo.close()
    global num1
    access_token=gettoken(refresh_token)
    headers={
    'Authorization':access_token,
    'Content-Type':'application/json'
    }
    try:
        if req.get(r'https://graph.microsoft.com/v1.0/me/drive/root',headers=headers).status_code == 200:
            num1+=1
            print("1调用成功"+str(num1)+'次')
        if req.get(r'https://graph.microsoft.com/v1.0/me/drive',headers=headers).status_code == 200:
            num1+=1
            print("2调用成功"+str(num1)+'次')
        if req.get(r'https://graph.microsoft.com/v1.0/users ',headers=headers).status_code == 200:
            num1+=1
            print('3调用成功'+str(num1)+'次')
        if req.get(r'https://graph.microsoft.com/v1.0/me/messages',headers=headers).status_code == 200:
            num1+=1
            print('4调用成功'+str(num1)+'次')    
        if req.get(r'https://graph.microsoft.com/v1.0/me/mailFolders/inbox/messageRules',headers=headers).status_code == 200:
            num1+=1
            print('5调用成功'+str(num1)+'次')    
        if req.get(r'https://graph.microsoft.com/v1.0/me/mailFolders/inbox/messageRules',headers=headers).status_code == 200:
            num1+=1
            print('6调用成功'+str(num1)+'次')
        if req.get(r'https://graph.microsoft.com/v1.0/me/drive/root/children',headers=headers).status_code == 200:
            num1+=1
            print('7调用成功'+str(num1)+'次')
        if req.get(r'https://api.powerbi.com/v1.0/myorg/apps',headers=headers).status_code == 200:
            num1+=1
            print('8调用成功'+str(num1)+'次') 
        if req.get(r'https://graph.microsoft.com/v1.0/me/mailFolders',headers=headers).status_code == 200:
            num1+=1
            print('9调用成功'+str(num1)+'次')
        if req.get(r'https://graph.microsoft.com/v1.0/me/outlook/masterCategories',headers=headers).status_code == 200:
            num1+=1
            print('10调用成功'+str(num1)+'次')
    except:
        print("pass")
        pass
while True:
    main()
    for i in range(random.randint(150,300),0,-1):
        print("\r"+str(i)+'秒后开始下一轮调用         ', end='')
        time.sleep(1)
```

保存好脚本后再在脚本同目录下创建名为1.txt的文件,将第4步获取的OAQ开头的那一大段token复制进1.txt，保存退出。

将1.py和1.txt都保存好就可以上传到服务器了,一定要保证1.py和1.txt两个文件在同目录。

7、服务器需要安装python3，并安装pip3、requests，如果是linux服务器，需要使用screen窗口来运行。

安装好后，就可以运行1.py来刷脚本啦。



以上就是通过脚本来刷office e5的教程，windows和linux服务器都可以，但是有很多人，可能并没有自己的服务器，单独购买来刷脚本很不划算，所以有高手利用github action实现定时自动调用api，保持E5开发活跃。这个后续再更新。





## github action自动执行



* 第一步，使用之前的应用id、机密、refresh_token 3样东西，以方便接下来的操作。

* 第二步，登陆/新建github账号，回到本项目页面，点击右上角fork本项目的代码到你自己的账号，然后你账号下会出现一个一模一样的项目，接下来的操作均在你的这个项目下进行。（看不到图片/图裂请科学上网）

* 获取应用id、机密、refresh_token（自己复制保存，注意区分id机密，别弄混了）

  然后在线编辑你项目里的1.txt，将整个refresh_token覆盖粘贴进去（里面是我的数据，先删掉或者覆盖掉）。（千万不要改1.py）

    > refresh_token位置如图下。复制refresh_token紧接着的双引号里的内容（红竖线框起来的），不要把双引号复制进去。复制进1.txt后，留意结尾不要留空格或者空行

  

* 第三步，依次点击上栏Setting > Secrets > Add a new secret，新建两个secret如图：CONFIG_ID、CONFIG_KEY。

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

![输入图片说明](.github/workflows/%E5%9B%BE%E7%89%87.png)



* 第四步，进入你的个人设置页面(右上角头像 Settings，不是仓库里的 Settings)，选择 Developer settings > Personal access tokens > Generate new token,

  设置名字为GITHUB_TOKEN , 然后勾选 repo , admin:repo_hook , workflow 等选项，最后点击Generate token即可。
  
  
  
* 第五步，点击右上角星星/star立马调用一次，再点击上面的Action就能看到每次的运行日志，看看运行状况

（必需点进去Test Api看下，api有没有调用到位，有没有出错。外面的Auto Api打勾只能说明运行是正常的，我们还需要确认10个api调用成功了，就像图里的一样。如果少了几个api，要么是注册应用的时候赋予api权限没弄好；要么是没登录激活onedrive，登录激活一下）

* 第六步，没出错的话，就搞定啦！！再看看下面的定时次数要不要修改，不打算改就忽略。

  **然后第二天回来确认下是否自动运行了（ation里是否多出来几个）**,是的话就不用管了，完结。

  我设定的每3小时自动运行一次，每次调用3轮（点击右上角星星/star也可以立马调用一次），你们自行斟酌修改（我也不知道保持活跃要调用多少次、多久）：

  * 定时自动启动修改地方：（在.github/workflow/AutoApiSecret.yml文件里，自行百度cron定时任务格式，最短每5分钟一次）

  * 每次轮数修改地方：（在1.py最后面）


------------------------------------------------------------

### 题外话 ###

> Api调用
> 你们可以自己去graph浏览器看一下，学着自己修改要调用什么api(最重要的是调用outlook、onedrive)
> https://developer.microsoft.com/zh-CN/graph/graph-explorer/preview

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

### 最后 ###

  教程很直白了，应该都会弄吧！



**常见错误以及答疑**

为什么报错
Traceback (most recent call last):
File "1.py", line 83, in
main()
File "1.py", line 40, in main
access_token=gettoken(refresh_token)
File "1.py", line 33, in gettoken
refresh_token = jsontxt['refresh_token']
KeyError: 'refresh_token'

答:这是因为你使用rclone获取的refresh_token有错误,大概率是你复制错了，一定要是OAQ开头的字符串,并且不带双引号!

使用ubuntu或debian也要下载rclone.exe获取token吗?
答:要，rclone的作用是获取token，微软的api是要你带着token访问才能成功的,所以无论哪个平台，都要使用rclone.exe获取token

一定要服务器吗?
答:不一定，只要能使用python3的平台都可以使用，你可以自己百度windows安装python3,或者安卓手机下载termux安装python3,具体怎么操作自行摸索

脚本要挂多久?
答:微软没有个明确的指标，所以默认越久越好，所以建议24小时挂在服务器上

screen是做什么的?
答:screen是服务器上用的，一般我们服务器执行脚本如果断开ssh脚本也会断掉，所以我们使用screen -S api开了一个新的名为api的屏幕，即使断开ssh，脚本依然在后台运行，下一次连接ssh的时候执行screen -r api即可再查看脚本执行情况，如果是windwos挂脚本的话就不需要screen