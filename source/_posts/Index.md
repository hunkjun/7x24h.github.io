---
title: 快速搭建Hexo-Next静态博客网站
date: 2023-11-09 16:41:13
tags:
---

## 安装hexo

```shell
npm install -g hexo
hexo -v
npm install
```

## 新建项目

```shell
hexo init blog
```

## 安装主题

```shell
cd blog/
git clone git@github.com:next-theme/hexo-theme-next.git themes/next
```

## 修改配置文件_config.yaml

```shell
theme: next
```

## 新建文章

```shell
hexo new [layout] <title>
hexo new First
```

## 样式调整

1. 字体
2. cdn
3. 搜索


## 参考文档
