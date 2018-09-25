---
title: Linux在history添加时间和用户
date: 2017-09-30 15:55:51
tags:
 - Linux
categories:
 - Linux
---

## 设置显示时间和用户

```bash
echo 'export HISTTIMEFORMAT="[%Y.%m.%d %H:%M:%S] [`whoami`] "' >> /etc/profile

```

### 执行source使配置生效

```bash

source /etc/profile
```

![history](/uploads/history.png)