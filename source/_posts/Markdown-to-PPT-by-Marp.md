---
title: Markdown to PPT by Marp
date: 2024-02-07 17:41:03
tags: 工具
---

## 想法

想批量通过代码的方式制作ppt, 然后转化成视频.

解决有内容但是没时间制作视频的问题.

比如: 心灵鸡汤、一些道理等.

找了比较多的软件、甚至我尝试过Python代码直接将Text生成文本，但是效果都不明显，希望Marp能帮到我~~

## 资料

- Github:  https://github.com/marp-team
- 笑脸: https://github.com/twitter/twemoji
- 翻页形式: https://github.com/marp-team/marp-cli/blob/main/docs/bespoke-transitions/README.md
- 背景图: 
- 字体: 
- 主题: https://github.com/marp-team/marp-core/tree/main/themes


## 案例

```shell
---
marp: true
paginate: true
size: 4:3

theme: uncover
style: |
  section {
    background-color: #ccc;
  }

---
<style scoped>
h1 {
  color: blue;
}
</style>


# Your slide deck

### <!-- fit --> Fitting header

Start writing!

##  hahah

---

hahah
```