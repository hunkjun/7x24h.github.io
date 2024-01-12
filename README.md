## 本地启动

hexo s

## 静态文件生成

hexo clean && hexo g

## 上传到存储

scp or sync

## 主题参考

```shell

https://github.com/next-theme/hexo-theme-next
https://guanqr.com/tech/website/hexo-theme-next-customization/
https://theme-next.js.org/docs/getting-started/
```

## 新建文章

hexo new 新建文档

```shell
hexo new --help
INFO  Validating config
Usage: hexo new [layout] <title>

Description:
Create a new post.

Arguments:
  layout  Post layout. Use post, page, draft or whatever you want.
  title   Post title. Wrap it with quotations to escape.

Options:
  -p, --path     Post path. Customize the path of the post.
  -r, --replace  Replace the current post if existed.
  -s, --slug     Post slug. Customize the URL of the post.
```
