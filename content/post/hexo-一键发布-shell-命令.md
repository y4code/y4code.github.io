+++
date = '2016-11-13T13:50:00.000Z'
title = "Hexo 一键发布 shell 命令"
image =  "images/featured-post/post-6.jpg"
+++
  
写完博客后，通常我都是找到 \`hexo clean hexo generate\` 然后再把 CNAME 拷贝到生成的 public 文件夹中，然后再 \`hexo deploy\`  
很烦  
然后在听内核恐慌 ＃3 ”静态网站生成器“ 的时候，Rio 说到在写博客的时候碰命令行是一件有摩擦力的事情，我就突然明白了  
下面是我的 hexo 一键 shell 命令  
\`\`\` $ #!/bin/bash  
 $ hexo clean $ hexo generate $ cp CNAME public $ hexo deploy  
 $ echo "hexo 一键发布命令 , Inspired by Rio & Tao Wu on the Kernal Panic No 3" $ echo "写作的时候不想碰命令行 , 命令行会给写作带来 "摩擦力" " $ echo "不要忘记打开 GitHub for Desktop push 哟" $ echo "记得要经常写博客哟～"\`\`\`  
写好了用这篇文章来测试