---
layout: post
title: 使用 Jekyll 编写博客
categories: tools
summary: 使用 Jekyll 编写博客，并且托管在 GitHub Pages 上
tags: [jekyll]
---

## 安装 Jekyll

Jekyll 是个 Ruby gem。

{% highlight sh %}
$ gem install jekyll
$ cd USENAME.github.com
$ jekyll serve -w
{% endhighlight %}

安装完成后进入自己的博客目录，启动 Jekyll。这个时候就可以访问 <http://0.0.0.0:4000/> 预览自己的博客了。

## 配置 Jekyll

配置 Jekyll 有很多现成的方案，不想折腾的话就可以直接使用这些方案，一键式搞定，比如 [Jekyll Bootstrap](http://jekyllbootstrap.com)。
不过 Jekyll Bootstrap 的几个 Theme 我都不是很喜欢，配置博客中的代码语法高亮我也没弄明白。在求助万能的 Google 的时候，发现了一个不错的方案：
[pixyll](http://pixyll.com)。感觉 pixyll 的样式和代码都比 Jekyll Bootstrap 干净很多。

## 编写 Rakefile

pixyll 没有 Jekyll Bootstrap 中的 Rakefile 不能像 Jekyll Bootstrap 那样用 rake 命令，比如新建博客：

{% highlight sh %}
$ rake post title="Hello World"
{% endhighlight %}

这样就新建了一个标题为 Hello World 的博客。

那就只能自己动手了。在博客目录下新建个文件 `Rakefile`：

{% highlight ruby %}
require 'date'

desc 'create a new draft post'
task :post do
  title = ENV['title']
  slug = "#{Date.today}-#{title.downcase.gsub(/[^\w]+/, '-')}"

  file = File.join(File.dirname(__FILE__), '_posts', slug + '.markdown')
  
  File.open(file, "w") do |f|
    f << <<-EOS.gsub(/^    /, '')
    ---
    layout: post
    title: #{title}
    published: false
    categories:
    ---

    EOS
  end
end
{% endhighlight %}

这样就能用 `rake post` 命令来新建博客了，新建的博客文件位于 `_posts` 目录下。

## GitHub Pages

OK，一篇博客写完了，赶紧用 git 把咱们的博客管理起来：

{% highlight sh %}
$ git add .
$ git commit -m "New post: Write blog with jekyll."
{% endhighlight %}

Git is everywhere ✌️.

最后我们把咱们的博客托管在 GitHub 上，免费的哟，具体可以看看 [GitHub Pages](https://pages.github.com)。

如果要把博客托管到 GitHub 上的话，首先得在 GitHub 上创建一个 Repository，命名为 `{github-username}.github.com`。
然后把博客推送到这个 Repository 里面：

{% highlight sh %}
$ git remote add origin git@github.com:{github-username}/{github-username}.github.com.git
$ git push origin master
{% endhighlight %}

等几分钟后，访问 `{github-username}.github.io`，博客搭建完成✌️✌️✌️。

