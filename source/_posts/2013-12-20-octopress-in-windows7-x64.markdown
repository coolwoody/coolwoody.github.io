---
layout: post
title: "octopress在windows7 x64的安装流水帐"
date: 2013-12-20 19:53:42 +0800
comments: true
categories: [octopress]
---
###必备程序安装
1. `Ruby1.9.3-p429`(octopress 是基本ruby开发的)
1. `Python2.7.5`(代码高亮插件是基于Python)
	1. `DevKit-tdm`(是一个zip压缩包，网上说要安装，但是试了下安装后，删除还是能正常编译博客，所以。。。暂且可以不装吧？)
1. `Git1.8.安装`
安装好后将C:\Python27\Scripts;C:\Python27;C:\Ruby193\bin; 加到PATH环境变量

###到GitHub上注册并配置
注册一个帐号，比如myBlog  
新建一个项目名称为“myBlog.Github.io”,以后myBlog.Github.io就是我们博客的域名了

到本地Git bash 命令窗口执行ssh-keygen生成公、私钥，默认生成在当前用户目录下的.ssh文件中。
拷贝id_rsa.pub中的公钥到github的项目ssh中，这样本地才能有权限推送内容

###octopress安装配置
到 `git://github.com/imathis/octopress.git` 上把octopress Clone 到本地目录  
使用git bash here 命令行，进入octopress目录  
使用国内的淘宝镜像安装Octopress的依赖项  
`gem sources -a http://ruby.taobao.org/`  
`gem sources -r http://rubygems.org/`  

修改Octopress目录下的Gemfile文件，将第一行的http://rubygems.org/ 修改为http://ruby.taobao.org/

`gem install bundler`	//包安装工具的安装  
`bundle install` //安装Gemfile文件中列出的名

使用默认主题安装Octopress  
`rake install`
>如果是用其它主题可用把主题放到\.themes中，然后使用rake install['主题名']安装
到此所有的安装工作已经结束，输入下面的命令可以在本地进行预览。

`rake preview`	//开启预览：localhost:4000
###开始写文章
`rake new_post['博客URL']`  
如果输入中文会被自动转正拼音，运行命令后会在`/source/_posts`目录中生成一个markdown文件  
写点内容后开始生成文章  
`rake generate`  
generate 后会在`/source/public`目录下生成整站的html，在rake deploy后会把pulic目录的内容并到
_deploy目录并push到对应github上的master分支，source对应github上的source分支

###发布到github
`rake setup_github_pages`  
要提示输入你在github建的项目ssh地址，如git@github.com:myBlog/myBlog.Github.io
`rake deploy`  
发布到github ，rake deploy原理就是把_deploy目录的内容push到master分支


###其它一些问题
octopress 中文内容无法生成提示错误“invalid byte sequence in gbk jekyll”  
解决方案：  
在git安装目录Git\etc\profile文件最后行里加入以下两行内容，为命令行窗口添加中文支持  
`export LC_ALL=en_US.UTF-8`  
`export LANG=en_US.UTF-8`

--------
####把源码备份到github 上
`git push origin source`  
安装Octopress的视频：  
http://happycasts.net/episodes/36?autoplay=true

--------
####显示category到侧边栏
把文件category_list_tag.rb扔到\plugins目录  
```ruby category_list_tag.rb
module Jekyll
  class CategoryListTag < Liquid::Tag
    def render(context)
      html = ""
      categories = context.registers[:site].categories.keys
      categories.sort.each do |category|
        posts_in_category = context.registers[:site].categories[category].size
        category_dir = context.registers[:site].config['category_dir']
        category_url = File.join(category_dir, category.to_url)
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"
      end
      html
    end
  end
end

Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)
```

 把category_list.html扔到\source\_includes\custom\asides目录
```html category_list.html 
<section>
  <h1>Categories</h1>
  <ul id="categories">
    \{\% category_list \%\}  
  </ul>
</section>
```
以上'\'在使用时要去除，这里加了'\'只是为了不让生成时替换category_list标签  
_config.yml中的default_asides: 中加入 custom/asides/category_list.html  
就OK了

--------

####样式定制到
`sass\custom\_styles.scss`  
添加相应样式后，会覆盖默认的样式
