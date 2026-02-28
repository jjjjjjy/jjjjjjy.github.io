1. 创建博客项目
hugo new site myblog

2. 添加博客主题
git init
git submodule add https://github.com/CaiJimmy/hugo-theme-stack.git themes/hugo-theme-stack

3. 修改hugo.toml

baseURL = 'https://jjjjjjy.github.io/'
languageCode = 'zh-cn'
title = '日日是好日'
theme = 'hugo-theme-stack'

4. 启动
hugo server -D

5. 创建博客文章
hugo new posts/my-first-post.md


other 删除public重新生成
1. hugo --minify
2. rm -rf public
3. hugo --minify
