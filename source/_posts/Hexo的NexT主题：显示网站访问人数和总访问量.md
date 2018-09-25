---
title: Hexo的NexT主题：显示网站访问人数和总访问量
date: 2017-05-18 18:08:19
tags:
 - Hexo
categories:
 - Hexo
---

## 显示统计标签

### 打开\themes\next\layout\_partials\footer.swig文件，复制以下代码至你想要放置的位置。


``` bash
<div class="busuanzi-count">
  <script async="" src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>

  <span class="site-uv">
  <i class="fa fa-user"> 本站访客数</i>
  <span class="busuanzi-value" id="busuanzi_value_site_uv"></span>
  人
  </span>

  <span class="site-pv">
  <i class="fa fa-eye"> 本站总访问量</i>
  <span class="busuanzi-value" id="busuanzi_value_site_pv"></span>
  次
  </span>

</div>
```

