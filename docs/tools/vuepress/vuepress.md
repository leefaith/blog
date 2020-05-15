## 1环境搭建

### 1.  全局安装 VuePress

Node.js版本需要>=8才可以。

```
npm install -g vuepress 或者在已有项目中安装
```

安装完成检测是否安装成功

```
vuepress -v
```

### 1.2创建并进入项目

```
mkdir VuePress
cd VuePress
```

### 1.3初始化项目

通过npm init快速创建项目的pageage.json文件

```
npm init -y // 默认配置yes
```

```js
{
  "name": "VuePress",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

### 1.4新建docs文件夹

docs文件夹作为项目文档根目录，主要放置Markdown类型的文章和.vuepress文件夹。          

```
mkdir docs
```

### 1.5设置package.json

在script中添加dev启动和build打包脚本命令

```js
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "vuepress dev docs",
    "build": "vuepress build docs"
  },
```

### 1.6创建.vuepress目录

```
npm run build
```

可直接打包构建README.md文件 并生成.vuepress

`.vuepress` 目录这是放置所有 VuePress 特有(VuePress-specific) 文件的地方。

### 1.7创建config.js

不做任何配置的话，页面会显得过于简单，用户也无法方便地浏览网站；

配置 VuePress 站点的基本文件是 `.vuepress/config.js`，其中导出一个 JavaScript 对象：

```
touch config.js
```

### 1.8目录结构

```
├── docs # 文档目录
│    ├── .vuepress //存放所有资源和打包结果
│   │         ├── dist //打包结果
│   │        ├── public //公共资源文件
│   │        ├── ...
│   │       └── config.js //配置文件
|	|		└── nav.js //导航栏文件
|	|		└── siderbar.js //侧边栏文件
│   ├── demo //分类文档存储
│   │    ├── demo1.md
│   │    ├── ...
│   │    └── demon.md
│   └── README.md 
└── package.json//项目启动配置
```
## 2.配置首页README.md

 ```js
  home: true
  heroImage: /img/logo.jpg
  actionText: 快速上手 →
  actionLink: /zh/guide/
  features:
  
  - title: 简洁至上
    details: 以 Markdown 为中心的项目结构，以最少的配置帮助你专注于写作。
  - title: Vue驱动
    details: 享受 Vue + webpack 的开发体验，在 Markdown 中使用 Vue 组件，同时可以使用 Vue 来开发自定义主题。
  - title: 高性能
    details: VuePress 为每个页面预渲染生成静态的 HTML，同时在页面被加载的时候，将作为 SPA 运行。
    footer: MIT Licensed | Copyright © 2018-present Evan You
 ```

## 3.基本配置config.js(配置文件)

```js
module.exports = {

  title: 'Jennifer的博客',
  description: '构建自己的知识体系博客',
  dest: './dist',
  port: '7777',
  head: [
      ['link', {rel: 'icon', href: '/img/logo.jpg'}]
  ],

  markdown: {
      lineNumbers: true
  },
  themeConfig: {
      nav:require("./nav.js"),
      sidebar:require("./sidebar.js"),
      sidebarDepth: 2,
      lastUpdated: 'Last Updated',
      searchMaxSuggestoins: 10,
      serviceWorker: {
          updatePopup: {
              message: "有新的内容.",
              buttonText: '更新'
          }
      },
      editLinks: true,
      editLinkText: '在 GitHub 上编辑此页 ！'
  }
}
```

### title

- Type: `string` 
- Default: `undefined` 

网站的标题。这将是所有页面标题的前缀，并显示在默认主题的导航栏中。

### description

- Type: `string` 
- Default: `undefined` 

网站描述。这将在页面 HTML 中表现为一个 `` 标签。

### head

- Type: `Array` 
- Default: `[]` 

被注入页面 HTML `` 额外的标签。每个标签可以用 `[tagName, { attrName: attrValue }, innerHTML?]` 的形式指定。例如，要添加自定义图标：

### port

- Type: `number` 
- Default: `8080` 

指定用于 dev 服务器的端口。

### dest

- Type: `string` 
- Default: `.vuepress/dist` 

指定 `vuepress build` 的输出目录。

#### 启动项目

`npm run dev`
 默认服务启动在了`http://localhost:7777/`

## 4.导航栏配置nav.js

```
module.exports = {
    themeConfig: {
        nav: [
          { text: '主页', link: '/' },
          { text: '测试', link: '/test/test.md' },
          { text: '百度', link: 'https://www.baidu.com' },
        ]
      }
}
```

![1](img/1.png)

## 5.侧边栏配置sidebar.js

你可以省略 `.md` 扩展名，以 `/` 结尾的路径被推断为 `*/README.md` 。该链接的文本是自动推断的（从页面的第一个标题或 `YAML front matter` 中的显式标题）。如果你希望明确指定链接文本，请使用 `[link,text]` 形式的数组。

```js
module.exports = {
  "/frontend/html/": [
    "",
    "1.web初识",
    "2.HTML初识",
    "3.HTML常用标签",
    "4.表格",
    "5.列表",
    "6.表单"
  ],
}
```

注意：文件命名不要用index关键字

## 6.部署上线

### github创建仓库

① 登录 [github](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2F)
 ② 新建仓库一：username.github.io
 （必须为你的github账户的username，而不是昵称啥的）
 ③ 新建仓库二，名称随意如vuepress-demo
 二者的关系是：**仓库一负责显示网站内容，我们不需要改动它；日常开发和新增内容，都在仓库二中，并通过 npm run deploy 命令，将代码发布到仓库一**

### 关联本地项目与github仓库

```
// 先cd到你的demo
cd vuepress
// git初始化
git init
// 关联github仓库
git remote add origin git@github.com:leefaith/vuepress-demo.git
```

###  新建[部署文件](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.vuepress.cn%2Fguide%2Fdeploy.html%23github-pages) 

①根目录下新建`deploy.sh`:

```
#!/usr/bin/env sh

# 确保脚本抛出遇到的错误
set -e

# 生成静态文件
npm run build

# 进入生成的文件夹
cd docs/.vuepress/dist

# 如果是发布到自定义域名
# echo 'www.yourwebsite.com' > CNAME

git init
git add -A
git commit -m 'deploy'

# 如果你想要部署到 https://USERNAME.github.io
git push -f git@github.com:nan-gong/nan-gong.github.io.git master

# 如果发布到 https://USERNAME.github.io/<REPO>  REPO=github上的项目
# git push -f git@github.com:USERNAME/<REPO>.git master:gh-pages

cd -
```



### git提交

git提交前，先确保你的本地登录了git账号，否则没有权限提交到远端。
 如果本地未登录，可参考[git初次登录教程。](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fcrazytata%2Fp%2F10083716.html)
 另外，可以在根目录下添加.gitignore文件，以防止不必要的提交：