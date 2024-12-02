# Hugo&#43;Fixlt 搭建个人博客（一）


# 前言
推荐使用 Github 搭建，不需要购买服务器
&gt;参考文章：[安装主题 | FixIt](https://fixit.lruihao.cn/zh-cn/documentation/installation/)
## 1 前置准备
- [安装 Hugo](https://gohugo.io/installation/)（扩展版）
- [安装 Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## 2 创建网站
### 2.1 命令
验证是否安装了 Hugo

```bash
hugo version
```

运行以下命令来创建一个带有 Fixlt 主题的 Hugo 网站。

```bash
hugo new site my-blog
cd my-blog
git init
git submodule add https://github.com/hugo-fixit/FixIt.git themes/FixIt
echo &#34;theme = &#39;FixIt&#39;&#34; &gt;&gt; hugo.toml
echo &#34;defaultContentLanguage = &#39;zh-cn&#39;&#34; &gt;&gt; hugo.toml
hugo server
```

### 2.2 命令解释
在 `my-blog` 目录中为你的项目创建 [目录结构骨架](https://gohugo.io/getting-started/directory-structure/)

```bash
hugo new site my-blog
```

 将当前目录更改为项目的根目录

```bash
cd my-blog
```

在当前目录中初始化一个空的 Git 存储库

```bash
git init
```

将 [FixIt](https://github.com/hugo-fixit/FixIt) 主题作为 [Git 子模块](https://git-scm.com/book/en/v2/Git-Tools-Submodules) 添加到你的项目中的 `themes` 目录

```bash
git submodule add https://github.com/hugo-fixit/FixIt.git themes/FixIt
```

在站点配置文件中添加一行，指定当前主题

```bash
echo &#34;theme = &#39;FixIt&#39;&#34; &gt;&gt; hugo.toml
```

在站点配置文件中添加一行，指定默认的内容语言。

```bash
echo &#34;defaultContentLanguage = &#39;zh-cn&#39;&#34; &gt;&gt; hugo.toml
```

启动 Hugo 的开发服务器以查看站点

```bash
hugo server
```

按 `Ctrl &#43; C` 停止 Hugo 的开发服务器。

## 3 添加内容
给你的网站添加新页面

```bash
hugo new content posts/my-first-post.md
```

Hugo 在 `content/posts` 目录中创建了该文件，使用编辑器打开文件

```md
---
title: 我的第一篇文章
date: 2024-03-01T17:10:04&#43;08:00
draft: true
# ...
---
```

---

> 作者: Youcs  
> URL: http://youcs233.github.io/posts/person-hugo-1/  

