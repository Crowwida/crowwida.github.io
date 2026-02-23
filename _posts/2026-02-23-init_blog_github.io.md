---
title: 利用 github.io 搭建个人博客
date: 2026-02-23 19:45:00 +0800
categories: [技术, 博客搭建]
tags: [博客, blog]
---

自上研已经过去半年有余，愈发感觉与课题组同门的差距，同时状况颇多，糟心事接连不断。遂下定决心搭建个人博客，一个是记录自己读研期间阅读的一些论文笔记，同时记录一下技术类工作中遇见的各种问题以及是如何解决的，其次是为了记录自己的生活，在无人倾诉的时候给自己一个输出口。

搭建博客的全流程主要是参考的[使用 Jekyll + GitHub Pages 搭建个人博客](https://zzy979.github.io/posts/creating-personal-blog-site/)，这位老师的这篇教程其实已经把在 github.io 上搭建的过程说的很详细了，我在这里仅做一个复述。

## 1 安装组件

Windows 系统上进行安装的组件只需要两个：Ruby 和 Jekyll。

### 1.1 安装 Ruby

RubyInstaller 的下载网址：<https://rubyinstaller.org/downloads/>，进入网址后寻找 Ruby+Devkit，其实会有 "=>" 符号标记默认推荐版本：

![ruby+devkit](assets\img\init_blog_github.io\ruby+devkit.png)

如果没有特定需求直接选择推荐就好。安装过程全程默认即可，在最后一步勾选 "运行 ridk install"，在接下来弹出的命令行窗口中选择 "3、MSYS2 and MINGW development tool chain"。

运用以下命令检查是否安装成功：

```shell
ruby -v
gem -v
```

### 1.2 安装 Jekyll

新开 CMD 窗口，执行：`gem install jekyll bundler`，安装完成后使用 `jekyll -v` 验证是否安装成功。

## 2 Jekyll 主题选择

在[原博客](https://zzy979.github.io/posts/creating-personal-blog-site/)中，博主建议使用 Jekyll 教程搭建简易 Demo 熟悉基础，但是在我本人的实际搭建过程中，感觉如果你仅仅是需要一个可以记录的地方，根本没必要。可以节省时间直接进行下一步。

### 2.1 Jekyll 目录结构

Jekyll 项目的典型目录结构如下：

```txt
mysite/
  _data/
    foo.yml
  _includes/
    foo.html
  _layouts/
    default.html
    post.html
  _posts/
    2018-08-20-bananas.md
    2018-08-21-apples.md
  _sass/
    main.scss
  _site/
  assets/
    css/
    images/
    js/
  _config.yml
  index.html
  Gemfile
```

我们是直接拿过来主题框架使用，只需要关注以下几个文件目录即可：
<!-- _config.yml、/_posts、/assets。其中 _config.yml 是配置文件，可以设置语言、时间、社交账号显示等等；/_posts 是你的博客内容所在，其下的文件命名为 YYYY-MM-DD-TITLE；/assets 文件中是你的博客中的图片文件。 -->

| 文件/目录 | 描述 |
| --- | --- |
| _config.yml | 配置文件 |
| _posts | 文章内容，文件命名格式为 `YYYY-MM-DD-TITLE.md` |
| _site | Jekyll生成的网站文件 |
| assets/images | 文章中的图片文件 |
| index.html | 网站主页 |

### 2.2 Jekyll 主题

**主题**（theme）提供了网站页面的布局和样式，详见官方文档 [Themes](https://jekyllrb.com/docs/themes/)。

可以在 <http://jekyllthemes.org/> 选择自己喜欢的主题并在线预览。我选择的主题是 [Chirpy](http://jekyllthemes.org/themes/jekyll-theme-chirpy/)，该主题提供了**分类**(category)、**标签**(tag)、目录、语法高亮、数学公式、搜索文章、评论系统等特性，能够满足博客网站的绝大部分需求。

除此之外，还有一个 [Textalic](http://jekyllthemes.org/themes/textalic/) 主题也不错，看个人喜好。

## 3 搭建个人博客

下面正式开始搭建个人博客网站。参考：[Chirpy - Getting Started](https://chirpy.cotes.page/posts/getting-started/)。

此章 3.1~3.4 照抄原博客。

### 3.1 创建网站

打开 [chirpy-starter](https://github.com/cotes2020/chirpy-starter) 仓库，点击按钮 "Use this template" → "Create a new repository"。

将新仓库命名为 `<username>.github.io`，其中 `<username>` 是你的GitHub用户名，如果包含大写字母需要转换为小写。

注：如果不需要自定义主题样式，则推荐使用这种方式，因为容易升级，并且能隔离无关文件，使你能够专注于文章内容写作。

### 3.2 安装依赖

使用 `git clone` 将新创建的仓库克隆到本地，并在项目根目录下执行

```shell
bundle
```

### 3.3 配置

根据需要更新 _config.yml 中的变量，例如 `url`、`avatar`、`timezone`、`lang` 等。

如果要改变浏览器标题也小图标，需要替换项目中的物理文件。请按照以下步骤操作：

准备一张正方形的图标图片（建议 512x512 像素的 PNG 格式）。使用在线 Favicon 生成工具（例如 RealFaviconGenerator）生成全套的图标文件。将生成的文件解压，并覆盖到您博客根目录下的 assets/img/favicons/ 文件夹中（如果没有该文件夹，请手动创建）。

### 3.4 启动本地服务器

要在本地预览网站内容，执行

```shell
bundle exec jekyll serve
```

在浏览器访问 <http://127.0.0.1:4000/>。

### 3.5 部署

[GitHub Pages](https://pages.github.com/) 是一个通过GitHub托管和发布网页的服务，官方文档：<https://docs.github.com/en/pages>。本文使用 GitHub Pages 部署个人博客网站。

每个 GitHub 用户可以创建一个用户级网站，仓库名为 `<username>.github.io`，发布地址为 `https://<username>.github.io`。GitHub Pages 支持自定义域名，参考文档 [About custom domains and GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages)。

在部署之前，检查 _config.yml 中的 `url` 是否正确配置为上述发布地址（或者自定义域名）。

> 注意：一般不需要配置 `baseurl`。如果配置了，则文章中必须使用 `relative_url` 过滤器生成正确的URL，否则会导致404错误。参考 [Jekyll’s site.url and baseurl](https://mademistakes.com/mastering-jekyll/site-url-baseurl/)。

之后在GitHub上打开仓库设置，点击左侧导航栏 "Pages"，在 "Build and deployment" - "Source" 下拉列表选择 "GitHub Actions"。

> 可选：我本人拥有个人域名，在阿里云购买的。在使用自定义域名的时候，在 "pages" 中 "Custom domain" 中输入自己的域名。在域名解析服务商处进行外网解析，记录值使用 Github 提供的 IP 地址：185.199.108.153、185.199.109.153、185.199.110.153、185.199.111.153。

提交本地修改并推送至远程仓库，将会触发 Actions 工作流。在仓库的 Actions 标签页将会看到 "Build and Deploy" 工作流正在运行。构建成功后，即可通过配置的 URL 访问自己的博客网站。

### 3.6 评论系统

我并没有使用评论系统，但是可以使用。

Jekyll 生成的博客网站是静态的，没有后端和数据库，因此本身无法实现评论功能。然而，可以使用 [disqus](https://disqus.com/)、[utterances](https://utteranc.es/) 和 [giscus](https://giscus.app/) 等评论系统来实现评论功能。

本文使用 giscus，它是利用 [GitHub Discussions](https://docs.github.com/en/discussions) 实现的评论系统，并且是开源、免费的。开启评论系统的步骤如下。

（1）安装 [giscus app](https://github.com/apps/giscus)。

（2）在仓库设置页面 "Features" 一节中勾选 "Discussions"，开启仓库的 GitHub Discussions 功能。

（3）在仓库的Discussions标签页，点击 "Categories" 旁边的编辑按钮，自定义用于博客评论的类别名称（例如 "Comments"）。

（4）打开 <https://giscus.app/>，在页面上填写以下配置：
* Repository: `<username>/<username>.github.io`
* Page - Discussions Mapping：保持默认值 "Discussion title contains page pathname" 即可（URL 为 `https://<username>.github.io/posts/<title>` 的文章将映射到标题为 `/posts/<title>` 的 Discussion，即使用 URL 的 pathname 部分作为 Discussion 标题）
* Discussion Category：选择上一步创建的类别名称（例如 "Comments"）

之后找到 "Enable giscus" 一节，将自动生成的配置填写到 _config.yml 中 `comments.giscus` 的对应选项。

```yaml
comments:
  active: giscus
  giscus:
    repo: ZZy979/zzy979.github.io
    repo_id: R_kgDOKOkhRA
    category: Comments
    category_id: DIC_kwDOKOkhRM4CZCpN
    mapping: pathname
```

（5）重启 Jekyll 服务器，在文章底部将会看到评论区，使用 GitHub 账号登录即可发表评论。