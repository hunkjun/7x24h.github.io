---
title: 后端即服务supabase
date: 2024-03-01 10:29:23
tags: 工具
---

## 介绍

Supabase 是一个Firebase的开源替代品，让你在不到两分钟的时间内创建一个实时后台。

后端即服务 (BaaS) 是一种云服务，开发人员在使用BaaS进行 Web 或移动应用开发时，仅需自行编写和维护前端代码。BaaS提供商为开发者提供了开发应用所需要的后端服务，例如用户身份验证、数据库管理、推送通知（针对移动应用程序），以及云存储和托管等

## 人群

只会前端，不会后端的技术人员，有很大的帮助.

## 安装

docker 安装

```shell
git clone --depth 1 https://github.com/supabase/supabase
cd supabase/docker/
cp .env.example .env
docker compose pull
docker compose up -d
```

## 访问 Supabase

容器启动后，我们可以通过 8000 端口上的 API 网关访问 Supabase Dashboard 页面。例如宿主机 IP 为 121.40.205.237, 那么地址则为 http://121.40.205.237:8000/

- username: supabase
- password: this_password_is_insecure_and_should_be_updated

- 参考: http://m.hangge.com/news/cache/detail_3410.html
- 参考: https://www.hangge.com/blog/cache/detail_3408.html