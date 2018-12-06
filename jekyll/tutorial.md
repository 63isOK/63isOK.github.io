# tutorial 基础教程
    主要是提供的默认website的基础上一点点修改

## setup 安装
### installation
    安装ruby环境
    安装jekyll 和bundler
    步骤在hello文档中

### 创建一个website
    jekyll是没有数据库的，可以和git很好的结合起来使用
    创建一个index.html
```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Home</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

### 构建一个website
    jekyll build 构建website，输出到_site
    jekyll serve 发布到本地
    通过127.0.0.1:4000即可访问

    这仅仅是一个最简单的jekyll创建静态website的方式
    也仅仅是将index.html复制了一下

## liquid
    liquid是一种模板语言，包含3块：objects、tags、filters

### objects
    作用是告诉liquid将输出内容输出到哪
    用{{和}} 包围
```liquid
{{page.title}} // 将页面上的page.title变量输出到此处
```

### tags
    作用是为模板创建逻辑、控制流
    用{%和%}包围
```liquid
{% if page.show_sidebar %}
  <div class="sidebar">
    sidebar content
  </div>
{% endif %}
```
    如果page.show_sidebar为ture就输出sidebar

### filters
    作用是修改liquid对象的输出
    用|分开
```liquid
{{"a" | capitalize}}
```
    输出a

### 如何使用liquid
    在index.html中添加
```liquid
<h1>{{ "Hello" | downcase  }}</h1>
```
    这样的修改是为了输出一个小写的hello
    重新build和发布之后，看到效果并没有改变
    是因为少了front matter

## front matter 前页
    front matter是一个yaml片段
    由两行---包围
```yaml
---
num:10
---
```

    front matter中的变量可以使用在liquid中
```liquid
{{ page.num }}
```

    如何使用front matter
```html
---
title: Home
---
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    <h1>{{ "Hello World!" | downcase }}</h1>
  </body>
</html>
```
    为什么不使用原始的html，而使用这种混合的方式，看下节

## layouts 布局
    一般website有多个页面，且各不相同
    jekyll支持markdown作为html的一页
    markdown的结构简单：段落+标题+图片
    markdown的事情下节再说，先回到layout

    接上节的问题，如果要修改页面的样式，
    如果用纯html，那么每个页面都需要修改，100个页面就不好完了
    所以需要有一个layout将通用的东西抽象出来

### 创建layout
    layout是一个模板，里面包含的是内容
    下面是default.html
```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    {{ content }}
  </body>
</html>
```
    laytou在_layouts目录下
    content是一个变量，包含了所有的渲染内容

    有了这个default的layout之后，index.html也可以修改：
```html
---
layout: default
title: Home
---
<h1>{{ "Hello World!" | downcase }}</h1>
```

### about 页面
    创建一个about.md
```markdown
---
layout: default
title: About
---
# About page

This page tells you a little bit about me.
```
    经过改造之后，再访问127.0.0.1:4000
    127.0.0.1:4000\about.html 查看about页面

    现在有两个页面，但缺少一个跳转

## include tag
    每个页面都有跳转，最好放在layout

    _includes目录下会有一些包含内容的文件
    通过include tag可以引用这些文件的内容

    下面先介绍跳转放在include的方式，不是layout

### include使用
    创建一个navigation.html文件
```html
<nav>
  <a href="/">Home</a>
  <a href="/about.html">About</a>
</nav>
```
    default.html layout中添加一行
    {% include navigation.html %}

### 高亮
```html
<nav>
  <a href="/" {% if page.url == "/" %}style="color: red;"{% endif %}>
    Home
  </a>
  <a href="/about.html" {% if page.url == "/about.html" %}style="color: red;"{% endif %}>
    About
  </a>
</nav>
```
    这样，当前页面的导航是红色的，不过还是很麻烦
    每次添加一个页面还得添加很多代码，看下节：

## data file
    数据文件，jekyll支持从_data目录下加载数据
    文件格式可以是yaml json csv等
    将数据文件从源代码中分离出来，便于维护

    接下来将导航数据放在一个数据文件中
    需要加载导航数据再遍历数据

### 数据文件用法
    yaml是ruby原生支持的一种格式
    在_data下创建一个navigation.yml
```yaml
-name: Home
  link: /
-name: About
  link: /about.html
```
    jekyll通过site.data.navigation来访问数据文件
    这样navigation.html就可以变成下面这个通用的：
```html
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}style="color: red;"{% endif %}>
      {{ item.name }}
    </a>
  {% endfor %}
</nav>
```

## assets
    资源，jekyll中使用这些资源都是非常直接的
    css js images
    assets目录下会有css js images目录

    上节我们把当前导航连接设置为红色，可以把格式独立出来
    用标准的css来定义
    这其中只需要用到css的插件Sass

    好吧，目前样式的修改我并不太关心，走一下节

## blogging
    写博客，jekyll推出的最大目的，也是最流行的特征
    jekyll用来写纯文本博客，是一大杀器

### posts
    在blog里  blog post称为博文，指博客文章
    都在_posts目录，文件名包含了3部分：发布时间-标题

### layout
    自定义一个post布局，继承与default

### list posts
    博文列表

## collections
    作者简介

### 配置
    配置作者的地方
    在_config.yml 根目录下
```yaml
collections:
  authors:
```

### 添加作者
    搜集信息一般从 _*搜集名* 目录下找
    我们搜集的作者信息， 从_authors下找

### 人员页面
    用于展示人员

## 部署
    上一节有很多是关于作者的简介，暂时不关心，所以不需要

### gemfile
    有这个文件的好处是确保jekyll和其他gem在某些环境下是正确的
    创建一个Gemfile
```ruby
source 'https://rubygems.org'

gem 'jekyll'
```
    当执行bundle install，会产生一个Gemfile.lock文件
    这个lock文件会lock住gem的版本，直到下次执行bundle install
    期间如果要更新gem版本，也可以使用bundle update

    如果有gemfile，发布时用：bundle exec jekyll serve

### 插件
    有3个官方插件是比较常用的：
    jekyll-sitemap
    jekyll-feed
    jekyll-seo-tag
    修改Gemfile即可
```ruby
source 'https://rubygems.org'

gem 'jekyll'

group :jekyll_plugins do
  gem 'jekyll-sitemap'
  gem 'jekyll-feed'
  gem 'jekyll-seo-tag'
end
```
    使用bundle update进行更新

### 环境
### 部署

