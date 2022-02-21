# 红岩网校寒假考核前端单人项目：网易云音乐
## 1.运行项目的方法
    本项目所用后端接口来自http://redrock.udday.cn:2022/，需要先安装
    安装后在文件夹NeteaseCloudMusicApi下打开cmd,输入命令 node app.js 启动后端服务。
    即可用Live Server插件进入index.html。
## 2.项目功能介绍
### 2.1 主页
        登录：
            点击头像或未登录文字可进入登录界面，可通过手机号和密码登录（仅支持点击登录按钮登录，不支持回车）。登录后可展示用户昵称和头像，同时侧边栏会显示该用户创建的歌单和收藏的歌单。
        搜索框：
            输入搜索关键词后敲击回车跳转至搜索界面（仅支持单曲搜索）。输入框中显示推荐搜索词，若敲击回车后输入框内无内容，将以推荐搜索词进行搜索。
        热搜榜：
            搜索输入框获得焦点时显示热搜榜。展示20个热门搜索推荐，每个推荐会根据后台数据展示搜索词、搜索数量、小图标、热搜评语。点击热搜词条可以进行该词条的搜索。
        左侧边栏：
            仿网易云桌面客户端布局。“创建的歌单”和“收藏的歌单”将在用户登录后分别显示该用户创建的歌单和收藏的歌单。点击灰色表头可以展开或收起歌单列表。点击歌单方块可以进入歌单详情页。
        轮播图：
            仿网易云桌面客户端卡片轮播。支持自动播放、手动切换、底部分页切换。
        推荐歌单：
            展示10个推荐歌单，含封面、标题、播放数信息。点击歌单封面或歌单标题可进入歌单详情页。
        歌单广场：
            点击顶部红色导航栏下方的“歌单”可进入歌单广场（点击“个性推荐”可返回）。 展示的歌单含封面、标题、播放数信息。默认歌单类型为“全部”，可点击界面右上方的歌单类型文字可动态切换歌单广场类型，按钮左侧的方块显示当前界面类型。点击歌单封面或歌单标题可进入歌单详情页。
        底部播放板块：
            动态展示当前播放歌曲的封面、名字、歌手。支持上一首下一首、进度拖拽、播放与暂停。点击右侧喇叭按钮可调节音量，点击右侧列表按钮可查看当前播放的歌单（该功能不完善）。
### 2.2 歌单详情页
        同上的功能：
            登录、搜索、左侧边栏、底部播放
        歌单信息：
            页面顶部显示当前歌单的所有信息（封面、标题、创建日期、标签、歌曲数、收藏数、播放量、简介（不全））
        播放全部按钮：
            点击之后可以替换正在播放的歌单、并从第一首开始播放。
        歌曲列表：
            展示该歌单所有歌曲的名字、歌手、专辑、时间。仿仿网易云桌面客户端样式。点击单个歌曲可进入播放页面（会替换当前播放的歌单）
        歌单评论：
           点击歌单信息下方的“评论”按钮可查看该歌单的部分评论（点击“歌单列表”可返回）。展示内容包括评论数、评论用户头像昵称、评论内容、评论时间、点赞数。评论分热评和最新评论。
### 2.3 搜索页
        页面顶部展示输入的搜索关键词。
        同上的功能：
            登录、搜索、左侧边栏、底部播放
        播放全部按钮：
            点击之后可以替换正在播放的歌单、并从第一首开始播放。
        展示该关键词搜索到的部分歌曲。
### 2.4 播放详情页
        同上的功能：
            搜索、底部播放。
        歌曲信息：
            展示歌曲名、歌手、专辑
        播放动画：
            点击底部播放按钮唱片开始转动。
        歌词：
            展示歌词与翻译。
        歌词滚动：
            滚动的同时样式改变。与歌曲播放进度同步，进度拖拽后自动滑至当前播放歌词。
        歌曲评论：
            分块展示该歌曲的热评和部分最新评论。展示内容包括评论数、评论用户头像昵称、评论内容、评论时间、点赞数。
## 3.代码说明
    本项目的CSS和JS代码看似做了分块（预想是每个界面都有独自的文件），但是由于界面间有许多通用功能（登录、搜索、底部播放），导致html的结构大多很相似，导致声明冲突、很多函数在不同JS文件里面都存在，一个HTML引用了多个JS文件、结构十分混乱（说到底还是不知道怎么分）。

    CSS :
    banner:轮播图
    logIn:登录方块
    main:主页歌单方形方块
    nav:顶部、侧边导航栏、底部播放
    nowplay:现在播放列表
    play:播放界面
    search:搜索界面
    songlist:歌单详情页

    JS基本同上
    play:几个样式的改变
    plays才是播放界面
    test：与主页有关

    JS文件里我对部分个函数的用途进行了备注。有的函数重复出现了很多次，只写了一次备注。

    接下来是项目里的几个关键点的实现原理说明

### 3.1动态插入带样式的方块（评论、歌单列表）
        预计插入的父元素.append(预计插入的、带样式的子元素.cloneNode(true))
        初始界面带一个容纳子元素的大div，和一个或两个（评论是一个、歌单是一灰一白两个）带样式的空白方块
        ┌──────────┐
          初始方块         
        └──────────┘
        插入时是克隆一个与初始方块一样的方块、并插入在最顶上
        ┌──────────┐
         初始方块 新
          初始方块                             
        └──────────┘
        插入完成后对初始方块1写入内容，第一个方块插入完成
        ┌──────────┐
           内容1
          初始方块                                
        └──────────┘
        进入第二次循环，此时克隆的是最顶上的内容1，会把内容1的样式和内容一起克隆下来，并插入在最顶上，同时内容1会被移动到最底下（这个是append的特性吗？其实我也没有彻底搞懂，只是打断点发现它是这样执行的）
        ┌──────────┐
          内容1 新 
          初始方块 
           内容1                               
        └──────────┘
        接下来在刚克隆进去的 内容1 新 里写入内容2
        ┌──────────┐
           内容2 
          初始方块 
           内容1                               
        └──────────┘
        接下来就是重复进行此过程，但是最后会导致如下的状况
        ┌──────────┐
         最后一个内容 
          初始方块 
           内容1
           .....
        倒数第二个内容                               
        └──────────┘对此我的解决方法是在数据写入前先用数组方法调整内容的顺序，调为(2 3 ..... 最后一个内容 1)，这样插入后的效果如下
        ┌──────────┐
           内容1 
          初始方块 
           内容2
           内容3
           .....
         最后一个内容
        └──────────┘再删除父元素的第二个子元素即可
### 3.2歌词滚动：
      (1)把从后端拿来的歌词依句分成数组
      (2)用slice把时间部分和文字部分分开，时间部分去零处理成与当前播放时间相同的格式，之后push进不同数组，动态创建p标签，内容为歌词数组的每一项
      (3)写一个获取当前播放时间的函数，100毫秒执行一次
      (4)100毫秒执行一次if判断，如果当前播放时间大于时间数组的某个时间，说明播放到此句，获取i。由于歌词数组和时间数组顺序相同，直接歌词数组[i]样式改变，歌词盒子scroll i*90px实现滑动
      还要判断是否存在翻译，比较歌词数组和翻译数组前面的时间，如果相同插入同一个p并换行
### 3.3localStorage：
        界面之间的数据传输大量使用了localStorage，好处是下次打开浏览器的时候可以保留上一次播放的歌单和歌曲，缺点是更新数据时需要刷新界面。
## 4.常见问题说明
    通过这个项目我进一步认清了自己菜到家的技术。这个项目可以说是bug满满。
### 4.1 bug1
        歌单的顺序可能不正确。原因就是上面所写的我动态插入新节点前对数据进行了调整。这个bug需要再仔细打断点查。尚未解决。
### 4.2 bug2
        由localStorage引起的。这个bug的解决方法简单粗暴，直接刷新。常见情况有当前播放的歌曲列表不正确、登录后侧边栏不显示用户歌单等
        同时，刷新也可以解决这个项目的大多数布局bug。常见情况有播放界面歌曲标题不居中、歌单界面缩放后界面出现留白等
### 4.3 bug3
        在这个项目代码开始写之前，我先是通过PS测量了每个元素的尺寸，在纸上规划，并把需要的图标扣下来导出成png,算是项目的前期准备。
        但是正式开始写的时候我发现实际显示的元素尺寸都偏大。后来发现是我的电脑自带系统的150%缩放，我量的尺寸全部都是乘以1.5之后的。但是我已经不想再重新规划和再扣一遍图标了，所以我写了一句规定缩放比的代码进行补救。
### 4.4 bug4
        页面加载和JS代码执行时间不同这个问题一直困扰着我。所以在代码里可能会看到许多定时器。这个是为了解决上述问题的临时补救方法。
### 总结：刷新可以解决大部分bug！要是刷新了还解决不了，就是我还没写好，对不起！
## 5.其他
    作者姓名：史楚莹
    作者联系方式：709097694（QQ），QQ邮箱同
    编写过程中借鉴了很多其他人的的思想，在此列出一个对我提供了巨大帮助的文章并对作者表示感谢！
    HTML+JavaScript+CSS+JQuery实现音乐播放器
    https://blog.csdn.net/qq_46253184/article/details/117136630




    
