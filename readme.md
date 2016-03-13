##My hexoBlog
1. 基于NexT主题
2. 开启多说评论、多说热评文章、百度统计、JiaThis分享；(多说服务不稳定)
3. 开启Swiftype搜索 国内响应有点慢TAT，免费账号每周一抓，结果相对滞后
4. 简单修改header样式，修改site-author-image样式变为圆形
5. 增加404页面，csdn文章基本转移完成
6. 文章内容中增加<!--more-->，首页显示阅读全文按钮(11.26取消<!--more-->)
7. 增加卜算子站点计数[详细](http://ibruce.info/2015/04/04/busuanzi/)    
NexT config文件中增加字段     
    busuanzi: true     
页面文件中增加相应代码段，即可实现。(取消文章页阅读次数，因为首页抓取的时候会显示第一个文章的阅读次数，其他文章不显示，只留下站点访问人数和次数)     
8. 修改主题config文件，auto_excerpt enable，自动截取文章内容
9. 了解侧边栏相关设置，文章md中增加toc，并没有效果（NexT主题没有使用hexo默认的方式），删除文章md相关toc设置 12.16
    - Sidebar 侧栏行为，可选值有
       - post   默认值，在文章页面自动展开侧栏
       - always 在所有页面自动展开侧栏
       - hide   在手动点击侧栏的开关按钮时展开