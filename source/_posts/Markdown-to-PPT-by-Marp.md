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

- 图片圆形显示

```
![圆形图片](./20241224130641.jpg)

<style>
img {
  border-radius: 50%;
  width: 200px; /* 调整图片大小 */
  height: 200px; /* 确保宽高相同以保持圆形 */
  object-fit: cover; /* 确保图片覆盖整个圆形 */
}
</style>
```

Marp 有两款主题，一款是 default，白色的，一款是 gaia，有三种背景色，分别是 default（书页黄），invert（深灰），gaia（海蓝）。

```
1. 引用一种主题：<!-- $theme: gaia -->

2. 在使用了 gaia 主题后，写上：<!-- template: invert --> 后，所有 PPT 默认使用 invert 背景色。

3. 在某一页写上<!-- *template: invert --> 后，仅这一页 PPT 用 invert（*就是仅这一页使用的意思）。

4. 进入下一页 PPT：一行---。

5. 写上类似这种：<!-- $size: 16:9 -->，可以调节 PPT 尺寸。

6. 插入图片的语法：![](图片路径)

7.![bg](图片路径)该图片会变为背景图片，若一页 PPT 内插入多张背景图片，它们会并列分布，注意，你的背景图片会被蒙上一层主题色（白色，书页黄，深灰或海蓝），亲测书页黄和海蓝加上背景图片丑的一批（Marp 是简约风的，还是不建议用背景图片啦）。

8.![缩放比例（如 200%)](图片路径)：图片缩放后插入 PPT 中。

9. 添加页脚：<!-- footer: 页脚内容 -->，如果写为<!-- *footer: 页脚内容 -->，就是仅本页添加页脚。<!-- footer: --> 就相当于取消页脚。

10. 添加页码：<!-- page_number: true -->，取消页码：<!-- page_number:false -->，这个也是，加*表示只对某一页操作。

11.PPT 里面需要的各种文字效果可以用 HTML 语言解决，同时，合理利用 HTML 改造你的文字，可以在即使有这么简约的 PPT 模板的情况下依然气死你的设计师朋友。

12. 用 latex，你可以直接写数学公式，markdown 语法（画表格等）也是可以的。

13. 插入 Emoji：直接插就好了 ->Emoji
```