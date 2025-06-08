---
title: Hexo配置Latex环境
date: 2025-06-05 10:26:44
tags:
  - Hexo
  - Markdown
  - Latex
categories: 问题记录
---

​	我们用 `hexo` 搭建个人博客，会发现无法显示 `markdown` 文件里的 latex 公式，这是因为hexo默认支持的hexo-renderer-marked渲染器不支持latex公式。那我们应该怎样做才能支持显示latex公式呢？以 `butterfly` 主题为例，目前 `butterfly` 支持两种数学公式渲染引擎，分别为 `Mathjax` 和 `Katex`。由于 Mathjax 支持的更为全面，因此我们选择 `Mathjax`。

​	具体步骤如下：

- 卸载 hexo-math 和 hexo-renderer-marked。在 cmd 中输入如下命令：

```cmd
npm un hexo-math
npm un hexo-renderer-marked
```

- 安装 hexo-renderer-pandoc 渲染器，命令如下：

```cmd
npm i hexo-renderer-pandoc
```

- 修改配置文件，修改 butterfly/_config.yml

```yaml
# About the per_page
# if you set it to true, it will load mathjax/katex script in each page
# if you set it to false, it will load mathjax/katex script according to your setting (add the 'mathjax: true' or 'katex: true' in page's front-matter)
math:
  # Choose: mathjax, katex
  # Leave it empty if you don't need math
  use: mathjax
  per_page: true
  hide_scrollbar: false

  mathjax:
    # Enable the contextual menu
    enableMenu: false
    # Choose: all / ams / none, This controls whether equations are numbered and how
    tags: none

  katex:
    # Enable the copy KaTeX formula
    copy_tex: true
```

- 本地安装 pandoc

这一步很重要，这是我遇到的一个坑，如果没有安装，hexo g的时候就会报错：pandoc exited with code null。至于 pandoc 安装很简单，只要在官网上下载pandoc，直接安装即可，**注意安装完要重启电脑**。

- 报错 `Error:spawnSync pandOC ETIMEDOUT`

![](https://bucker-qyhome.oss-cn-beijing.aliyuncs.com/pandoc%E6%8A%A5%E9%94%99.png)

解决方法：修改 `Hexo` 的 `_config.yml` 文件，添加以下配置项。

```yaml
pandoc:
  pandoc_path: C:/Program Files/Pandoc/pandoc.exe  # 根据你的实际路径修改
  args:
    - "-f"
    - "markdown"
    - "-t"
    - "html"
    - "--mathjax"
```

