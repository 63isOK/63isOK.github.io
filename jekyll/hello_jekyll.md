# 写博客
    github page提供了写博客的基础
    使用jekyll让写博客的过程更加轻松

    当然，github page也可以用使用其他静态网站生成工具
    jekyll和github page之间有深度定制，jekyll也是官方推荐的

## jekyll有啥好
* 使用markdown写，而不是直接写html
* 可以直接使用jekyll的主题，而不用去管什么css
* 使用jekyll 主题选择器可快速创建一个新的网站
* 可以使用通用模板，eg：固定的头和脚标
* 更容易创建github page支持的网站

## jekyll构建网站
    github page推荐jekyll的原因是：
    jekyll比其他静态网站生成工具的构建过程更加简洁

    整个过程如下：
    第一 将更新push到publish分支
    第二 github page会发布更新之后的网站

## quick start
    使用jekyll，可以使用自己喜欢的标记语言写文本
    jekyll会将文本变成一个静态website

    安装如下：
    ruby开发环境：sudo apt install ruby-fll build-essential
    echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
    echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc
    source ~/.bashrc
    gem install jekyll bundler

    创建一个默认的网站：
    jekyll new blog
    cd blog
    bundle exec jekyll server
    访问127.0.0.1:4000即可

## ruby 101
    jekyll 使用ruby实现的，下面是一些基本概念
### Gems
    gem是可以引入进ruby项目的代码，里面包含了一些功能
    可以被其他ruby项目引用，听着很像库
    提供的功能如下：
    将ruby对象转换成json
    分页
    和github的api交互
    jekyll本身也是一个gem

### Gemfile
    website需要的gem列表
    一个简单的website的gemfile如下：
```ruby
source 'https://rubygems.org'

gem 'jekyll'

group :jekyll_plugins do
  gem 'jekyll-feed'
  gem 'jekyll-seo-tag'
end
```

### bundler
    bundler负责将gemfile中列出的gem安装到项目中
    这是推荐的方式

    bundler的安装：
    gem install bundler

    一般流程是先bundle install 安装gem
    后执行bundle exec jekyll server来构建website

