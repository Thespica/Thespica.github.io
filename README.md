# 嗨嗨，欢迎来到我的博客仓库！

## 在本地部署

依赖： [nodejs](https://nodejs.org/zh-cn/) 的环境

安装 hexo 并安装插件：

```bash
npm install hexo-cli -g
npm install hexo-renderer-scss hexo-renderer-swig --save
npm uninstall hexo-renderer-sass
npm i --save hexo-renderer-sass-next
npm i --save hexo-filter-mermaid-diagrams
```

将仓库克隆到本地

```bash
git clone https://github.com/Thespica/Thespica.github.io blog-thespica
cd blog-thespica
git submodule update --init
hexo clean & hexo g
hexo s
```

## 部署到网站

所有生成和部署的工作都是由 GitHub Action 完成的，因此只需要克隆项目本体，修改内容，推送到远端仓库就 ok 了。既不需要配置环境，也不需要安装主题、插件等。