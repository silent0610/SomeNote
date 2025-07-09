---
Type:
  - Page
aliases: 
tags: []
Status: 
modifiedDate: 2025/06/12, 16:29:38
---

# Pip配置

## Pip 换源

[Python 修改 pip 源为国内源 - 点点米饭 - 博客园 (cnblogs.com)](https://www.cnblogs.com/137point5/p/15000954.html)

### 临时换源

清华源
`pip install markdown -i https://pypi.tuna.tsinghua.edu.cn/simple`

### 永久换源

**清华源**
`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`
阿里源
`pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/`
腾讯源
`pip config set global.index-url http://mirrors.cloud.tencent.com/pypi/simple`
豆瓣源
`pip config set global.index-url http://pypi.douban.com/simple/`

**换回默认源**  
`pip config unset global.index-url`
