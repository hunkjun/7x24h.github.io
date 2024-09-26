---
title: milvus数据库备份及迁移
date: 2024-03-19 15:17:38
tags: 技术
---


### 参考文档

- https://github.com/zilliztech/milvus-backup/releases/tag/v0.4.11https://milvus.io/docs/milvus_backup_overview.md
- https://mp.weixin.qq.com/s/_G4sWL7BHlBvllR2n5flzQ

### milvus 版本为0.2.2

### 报错记录

报错: "msg":"fail to list databases, err: feature not supported"

> 版本降级为milvus-backup.v0.3.x

报错: "failed to connect to milvus"] [error="this version of sdk is incompatible with server, please downgrade your sdk or upgrade your server"] 
Please take a look. Milvus backup v0.3.3 should be compatible with Milvus v2.3.0.

> 最终版本 2.2


### 备份和恢复操作指南：

#### 备份：

- 创建备份：
  
  ```bash
  ./milvus-backup create -n backup_v0317
  ```

- 查看备份列表：
  
  ```bash
  ./milvus-backup list
  ```

- 通过 Web 页面下载备份的文件夹。

#### 恢复到另一个 Milvus 实例：

1. 通过 Web 页面上传备份文件夹到目标 Milvus 实例。

2. 在目标 Milvus 实例上执行上传milvus操作。

- 使用以下命令将备份还原到本地 Milvus 实例，同时修改 IP 地址为本地地址：

  ```bash
  ./milvus-backup restore -n backup_v0317
  ```

通过这些步骤，你可以轻松地备份和恢复 Milvus 数据到本地或其他 Milvus 实例。