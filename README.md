#### Docsify
一个神奇的文档站点生成器。
简单轻巧（~21kB）
没有生成静态的html文件
主题丰富

#安装
## 本地搭建
如果你想在本地搭建：
npm安装：
```shell script
npm i docsify-cli -g
```
初始化：
```shell script
docsify init ./docs
```
本地预览：
```shell script
docsify serve 
```
进入http://localhost:3000就能看到效果咯！

为了更好地SEO，您可以在每个文件后面自定义标题
```markdown
* [Home](/)
* [Guide](guide.md "The greatest guide in the world")
```
默认情况下会自动根据文章标题生成目录，如果不想要，可以再index.html中新增：
```javascript
<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 2
  }
</script>
```
subMaxLevel: 2表示只显示h1~h2的标题，对应#和##。
##### 注意：如果md文件中的第一个标题是一级标题，那么不会显示在侧边栏，如上图所示
如果你想忽略某个标题，则可以在文章中新增{docsify-ignore}：
```markdown
# Getting Started
## Header {docsify-ignore}
```
如果想忽略全部的标题，则可以新增{docsify-ignore-all}：
```markdown
# Getting Started {docsify-ignore-all}
## Header
```
表示忽略{docsify-ignore-all}下的全部标题

## 封面
```javascript
window.$docsify = {
    coverpage: true,
}
```
后再根目录创建_coverpage.md 输入内容就可以显示在封面了

## 主题颜色
```javascript
window.$docsify = {
    themeColor: '#c30aff',
}
```
## 外链打开方式
```javascript
window.$docsify = {
    externalLinkTarget: '_blank',
}
```
站内搜索插件
```javascript
search: 'auto',
search: {
maxAge: 86400000,
paths: '/',
placeholder: '搜索...',
noData: '未找到结果，换个搜索词试试？',
namespace: 'XhemjBlog',
    },
```