---
title: hexo之在gitcafe上搭建静态博客简明教程
layout: post
date: 2015-02-13 10:46:34
tags: hexo
categories: hexo
comments: true
description: 用hexo搭建一个静态博客，并将其托管在gitcafe上的简明教程
---

##起

写这篇简明教程的博文源于一妹纸看到我的博客后突然心血来潮要弄个博客，然后我便大(bu)言(zhi)不(si)惭(huo)的应承下要写一篇博文供其参考，基于我一向的说到做到的臭毛病，况且是受美女之托，便有此文。

##承

此步乃是一些简单的准备工作，以及初期效果。*对了，一切都是基于Windows的。*
准备工作有三：

1. 安装node.js。
2. 安装git。
3. 安装markdownpad2

快捷安装入口：[三合一安装传送门，非最新版](http://pan.baidu.com/s/1i3rbfiH)

简要说明(可跳过)：由于`hexo`是基于`Node.js`的静态博客程序，所以`Node.js`是必须要安装的，对于致力于前台工程师的人来说，这个是电脑上必备的软件。`hexo`的效率也是极高的，编译上百篇文字只需要几秒。而`git`是由于身为懒人，将博客放在`gitcafe`或者`github`上，不仅免去了备案之繁琐的事情，而且还不用花钱买服务器，这样既省力又省钱的好事为嘛不做。最后就是`markdownpad2`了，由于写此博文要用到`markdown`格式的语法，所以下载个离线编辑器是不错的选择，当然有时候我也喜欢用`sublime text2`来编辑，个人喜好了。不熟悉`markdown语法`或者写长篇的博文还是建议用编辑器，因为这样可以在左边写，右边直接就出效果了，很是赞。

##转

准备工作到此结束，下面是搭建了。搭建之前先简要介绍下hexo常用的命令：

###hexo常用命令

	hexo new "postName" #新建文章
	hexo new page "pageName" #新建页面
	hexo generate #生成静态页面至public目录
	hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
	hexo deploy #将.deploy目录部署到github/gitcafe

简写为：

	hexo n == hexo new
	hexo g == hexo generate
	hexo s == hexo server
	hexo d == hexo deploy

###本地博客

打开命令行窗口，键入

	npm install -g hexo

来杯咖啡稍作等待，即可安装成功。接下来，执行

	mkdir blog && cd blog

此处`blog`便是你的博客目录，当然其他什么名字也是极好的，看心情了，此时最好将此目录备份到云盘或者其他地方，以防文件夹丢失后博客就没有了。


然后执行

	hexo init

安装依赖包

	npm install

至此，博客搭建成功！当然，仅仅是本地的了。此时执行
	
	hexo g

即可生成静态页面，然后执行

	hexo s

访问<http://localhost:4000>即可看到你的博客了。如果想让放到网上该怎么办呢？那就接着往下看咯。


###将博客搭在gitcafe上

到[gitcafe官网](https://gitcafe.com/),点击大大的`立即试用`绿色按钮，在右边框框里注册，邀请人填写`voidy`（可选），注册完成后登录gitcafe，然后创建一个新的项目，接下来需要把你的ssh公钥放到gitcafe上，具体看[git公钥设置](https://gitcafe.com/GitCafe/Help/wiki/%E5%A6%82%E4%BD%95%E5%AE%89%E8%A3%85%E5%92%8C%E8%AE%BE%E7%BD%AE-Git#wiki)
。至此git设置完毕，接下来就是将博客搭上去了。

这里我们用了gitcafe的gitcafe-pages来搭建我们的静态博客。先进入到`blog`的目录下，然后右键打开`git bash`，命令行输入

	git init

执行之后具体参照[这个](https://gitcafe.com/GitCafe/Help/wiki/Pages-%E7%9B%B8%E5%85%B3%E5%B8%AE%E5%8A%A9#wiki)来建立gitcafe-pages的分支。建立完成之后，用sublime text2打开你的博客文件夹目录，在`blog/_config.yml中最后，按照下面来写你的配置：

	deploy:
	  type: github
	  repository: git@gitcafe.com:voidy/voidy.git
	  branch: gitcafe-pages

其中只需修改一下`repository`，将其内容修改为刚刚新建的项目的仓库地址即可。至此，静态博客搭建完毕。只需执行：

	hexo d -g

即可将博客发布在gitcafe上了。进入<XXX.gitcafe.com>看到你的博客了。其中`XXX`代表你的用户名。


##合

###主题篇

接下来，开始对博客进行一番改造。毕竟博客是自己精神上的一个`家园`，当然要`装饰打造`一番了，首先嘛，自然是进行主题的选择了，主题在[这里](https://github.com/hexojs/hexo/wiki/Themes)。选择好一个主题之后，就是进行主题的安装了。在刚刚那个网站上，点击右边的链接可以看到主题的`Demo`，选则一个喜欢的主题然后点击左边的链接进入github上，然后命令行进入到你的博客目录，执行下面语句：

	$ git clone https://github.com/A-limon/pacman.git themes/pacman

其中`git clone`后面的链接为你进入的主题的链接地址，`themes/pacman`为你的保存目录，此处已pacman主题为栗子，具体的以你选择的主题为准。然后进入到`/blog/_config.yml`里面，在`85行`左右将theme改为你刚刚下载保存的主题的名字。然后执行：

	hexo d -g

进入到你的站点<XXX.gitcafe.com>即可看到你的新主题效果。

###配置清单

首先是博客的配置清单，目录为/blog/_config.yml

	# Hexo Configuration
	## Docs: http://zespia.tw/hexo/docs/configure.html
	## Source: https://github.com/tommy351/hexo/
	
	# Site 这里的配置，哪项配置反映在哪里，可以参考我的博客
	title: voidy's blog #站点名，站点左上角
	subtitle: stay hungry, stay foolish #副标题，站点左上角
	description:                      #站点描述，搜索引擎要看的
	author: voidy #在站点左下角可以看到
	email: #你的联系邮箱
	language: zh-CN #中国人嘛，用中文
	
	# URL #这项暂不配置，绑定域名后，欲创建sitemap.xml需要配置该项
	## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as 	'/child/'
	url: http://voidy.net
	root: /
	permalink: :year/:month/:day/:title/
	tag_dir: tags
	archive_dir: archives
	category_dir: categories
	
	# Writing 文章布局、写作格式的定义，不修改
	new_post_name: :title.md # File name of new posts
	default_layout: post
	auto_spacing: false # Add spaces between asian characters and western characters
	titlecase: false # Transform title into titlecase
	max_open_file: 100
	filename_case: 0
	highlight:
	  enable: true
	  backtick_code_block: true
	  line_number: true
	  tab_replace:
	
	# Category & Tag
	default_category: uncategorized
	category_map:
	tag_map:
	
	# Archives 默认值为2，这里都修改为1，相应页面就只会列出标题，而非全文
	## 2: Enable pagination
	## 1: Disable pagination
	## 0: Fully Disable
	archive: 1
	category: 1
	tag: 1
	
	# Server 不修改
	## Hexo uses Connect as a server
	## You can customize the logger format as defined in
	## http://www.senchalabs.org/connect/logger.html
	port: 4000
	logger: false
	logger_format:
	
	# Date / Time format 日期格式，不修改
	## Hexo uses Moment.js to parse and display date
	## You can customize the date format as defined in
	## http://momentjs.com/docs/#/displaying/format/
	date_format: MMM D YYYY
	time_format: H:mm:ss
	
	# Pagination 每页显示文章数，可以自定义，我将10改成了5
	## Set per_page to 0 to disable pagination
	per_page: 9
	pagination_dir: page
	
	# Disqus Disqus插件，我们会替换成“多说”，不修改
	disqus_shortname:
	
	# Extensions 这里配置站点所用主题和插件，暂默认，后面会介绍怎么修改
	## Plugins: https://github.com/tommy351/hexo/wiki/Plugins
	## Themes: https://github.com/tommy351/hexo/wiki/Themes
	theme: pacman
	exclude_generator:
	plugins:
	- hexo-generator-feed
	- hexo-generator-sitemap
	
	# Deployment 站点部署到gitcafe
	## Docs: http://zespia.tw/hexo/docs/deploy.html
	deploy:
	  type: github
	  repository: git@gitcafe.com:voidy/voidy.git
	  branch: gitcafe-pages

然后是主题的配置文件，在/blog/themes/pacman/_config.yml,下面以我的pacman为栗子讲一下：

	##### Menu，主菜单，在最上面显示
	menu:
	  Home: /
	  About: /about
	## you can create `tags` and `categories` folders in `../source`.
	## And create a `index.md` file in each of them.
	## set `front-matter`as
	## layout: tags (or categories)
	## title: tags (or categories)
	## ---
	
	#### Widgets，小挂件，在侧栏显示,具体可以写什么要看你的/themes/pacman/layout/_widgets下有什么了
	
	widgets: 
	- clock
	- music
	- category
	- archive
	- recent_posts
	- recent_comments
	- tag
	- blogroll
	
	
	
	## provide six widgets:category,tag,rss,archive,tagcloud,links.
	## modify links in `/layout/_widget/links.ejs`.
	
	#### RSS
	rss: /atom.xml## RSS address.
	
	
	#### Comment，多说评论，使用评论系统的，国内一般用多说，当然也可以用国外的disqus,不过身在天朝，要时刻小心被墙
	duoshuo: 
	  enable: true  ## duoshuo.com
	  short_name: voidy    ## duoshuo short name.
	

基本上用到的就这么些了。好了，赶紧发布第一篇博文吧：

	hexo new my_first_blog

即可生成一篇`my_first_blog.md`的博文，然后打开source/_posts下的博文：

	title: my_first_blog #文章页面上的显示名称，可以任意修改，不会出现在URL中
	date: 2014-2-13 11:30:16 #文章生成时间，一般不改，当然也可以任意修改
	categories: #文章分类目录，可以为空，注意:后面有个空格
	tags: #文章标签，可空，多标签请用格式[tag1,tag2,tag3]，注意:后面有个空格
	---
	#这里开始使用markdown格式输入你的正文。

不懂`markdown语法`？木关系，请移步至[Markdown入门总结](http://voidy.net/2015/01/03/markdown_3/)花费分分钟学习一下，对，你没看错，只需分分钟。写完后只需执行：

	hexo d -g

即可进入你的站点欣赏你的第一篇博文了。

以上。

---
> **版权声明**
> **本人博文若无特别说明，均由`voidy-小鱼`原创，若要转载，请附上`作者`以及[博文链接](http://voidy.net)。**
> **由于本人水平有限，所以难免有错，若发现错误，请在评论区任意吐槽~**
> **博文链接：<http://voidy.net/>**