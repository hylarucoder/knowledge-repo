# Vue

## 0x00 前言

之前草草整理过一些 Vue 的小技巧，最近温习了一遍文档，重聊一遍。

鉴于 VueCLI 越做越臃肿，我最后还是选择了自己搭架子。

https://github.com/twocucao/vue-starter-kit

## 0x01 项目结构

    # 根文件 public/ # 部署文件 src/ # 源码.babelrc.editorconfig.eslintignore.eslintrc.gitignore.postcssrc.jsLICENSEREADME.mdjsconfig.jsonpackage-lock.jsonpackage.jsonwebpack.common.jswebpack.dev.jswebpack.dll.jswebpack.prod.js

    # 在 src 文件夹下面 api # 相关的 Web API 定义 assets # 静态资源，对于已经压缩的，还是直接放在 Static 下面 components # 公用组件 mixins # mixindirectives # 定义指令，比如 v-loadingpages # 页面 store # StoreApp.vue # CORE 组件 consts.js #定义常量名称 index.js # 用于初始化项目，注册组件等等 routes.js # 路由 utils # 工具方法

值得一提的就是 index.js 应该要做的事情

- 注册全局组件和过滤器
- 给 Vue 实例加戏，哦，说错了，给实例加一些全局性的方法，比如 $comfirm 等对话框 $verbose $warning 等日志
- 完成刷新界面之后的从 localStorage 的重新赋值
- 注册路由切换的时候的调用的各类方法

## 0x02 开发技巧

### 1. 登录，登录校验以及权限

通过路由的 meta 属性来区分

### 8. 日志管理

我觉得日志管理也应该是比较重要的部分，不管是调试程序，还是用于检查用户浏览器这块的错误日志，甚至是埋点。

依据具体技术栈可以考虑上个 sentry 或者 ELK

### 7. 首屏 Loading

这个可以放在 index.html 里面

### 4. 路由管理与嵌套路由

路由管理

嵌套路由有什么优点？

1. 使得子路由里的页面可以复用父级路由的页面的组件
2. 减少手动硬编码 meta 和 props 的代码量
3. 便于定制面包屑组件
4. 其他

## 0x03 构建技巧

### 3.1 离线 IconFont

经常需要离线调试网页，顺手写了这个脚本。

之前在研究某个网站的反爬机制的时候发现时动态生成 iconfont, 然后通过 unicode 码来实现数字的显示，从而让爬虫小白无法爬取。研究了一下他们的 iconfont, 知道了 font-carrier, 然后调用 node 脚本打包字体文件，并在这个过程中自动生成对应的 iconfont.css

最后的结果就是，当我放一个文件到 svg 文件夹下面的时候，比如 bank.svg , 我执行一下脚本，生成对应的字体文件，在 html 里面编写脚本

    <span class="iconfont iconfont-bank" ></span>

然后对应图标就呈现出来了。

### 3.2 Webpack 构建工具

日常开发用的是 VueCli, 配置还是非常人性化的。开箱即用。

### 开发环境与部署环境

VueCLI 内置了变量的管理，你可以定义 config/dev.js

    module.exports = merge(prodEnv, {NODE_ENV: '"development"',API_ROOT: '"http://dev-data.twocucao.xyz"'});

其实，开发的环境用一组变量是不行的。比如，开发的人分为纯前端，纯后端，我这样的前后都会一些的人，每个对于环境的配置都是不太一样的。

- 对于前端 Windowser 直接执行 npm run dev 对接到局域网服务器
- 对于单个人同时调试后端和前端的时候，一般要把 Web API 对应到本地的机器上。可是使用环境便来配置不同的 DEBUG_MODE=True npm run dev

### DLL 打包

大约在半年前，开发过程中突然在使用 ECharts 后，仅仅不到 10M 大小的项目居然开发 build 的时间需要 5MIN, 打包出来的文件超级大。居然接近了 100 多 M

震惊之余，差点准备写一篇骗点击量的文章：**看完震惊了！！前端和后端男程序员都无法忍受的大小！**, 然后文章内就介绍 Webpack 打包文件居然没有避免重复引入依赖库导致打包文件太大提出抗议。

回到主题，使用 npm run analyze 发现问题出现在 ECharts 上， 每一个图表组件都是依赖于 ECharts, 而每一个组件都包含了一个完整的 ECharts 库的大小。

于是，我一边吐槽 webpack 考虑不周，另一方面寻找解决方案。最后找到了 DLL 方案

这个方案的原理大致是：

- 编写独立的脚本，把几个需要复用的库一个配置文件 (manifest.json), 以及打包库到一个 JS 文件中。
- 然后从 index.html 引入这个 JS 文件。
- 接着在 webpack 配置中使之每次引入一个库的时候，避免重复引入。

> 但这不应该是 Webpack 本身就应该做的吗？为嘛还要配置，还要不伦不类的生成一个配置文件和一个 JS 文件，再从 index.html 里面导入？

当然，Webpack 生态还是很丰富的，后来出来了一个 https://github.com/asfktz/autodll-webpack-plugin 尝试了之后。感觉很赞。

可惜在 mac 上一切安好，Windows 上晴天霹雳，debug 了一下，发现是这个库的一个依赖库对 windows 的路径处理好像还有点小问题。而公司的前端小伙伴是 Windowser, 只好作罢。

Macer 可以先用试试，至于 Windowser, 那就去这个 ISSUE 下面催催作者吧… 哈哈哈

> update: 现在 windows 已经可以用了。

## 0x03 代码质量工程管理

### 1. 语义化与可读性

### 2. 提取公共逻辑（通过 Service, Mixin 来）

### 3. CSS 管理

在项目中，我采用 SCSS 来管理 CSS 代码，

过去的时候有两种 css 的代码命名方法

第一种，我管他叫做**配置式写法**，通过将 CSS 语法的几个片段转化成名称，从而实现快速配置出效果的的 CSS

    .fl {float: left}.fr {float: right}.mr10 {margin-right: 10px}.pb10 {padding-bottom: 10px}....

这种写法对于简单页面来说确实也是可以使用的。缺点就是当页面变得复杂一些的时候，则比较难控制这种短小精捍（不直观）的变量。比如

    .tmd01{padding-bottom: 10pxfont-size: 16px;position: relative;top: 0;color: #2d3c48;line-height: 1.6;}

> 请脑补一下我的黄人问号脸

当然，如果用得好的话，自然是 OK, 如果用不好的话，

后来进入了嵌套写法时代（感谢伟大的 Rails 社区出的 SASS）, 下面的语法都是 SCSS.

第二种写法就变成了这样

    .actions {
      .card_wrapper {
        .card {
          .title {
          }
          .content {
            .list {
              .fa {
              }
            }
          }
        }
      }
    }

外加变量和 mixin 以及函数的话，基本上就可以完成代码的组织了。

这种写法倒是比原来不知道高到哪里去了，但问题依旧存在，比如 title,content 这些玩意太多，完完全全的看不懂。更加糟糕的事情是，有的小伙伴直接是乱用嵌套，也不用伪类和伪选择器，从而达到单页面调出来小伙比较快，但因为代码不能重用，调多个页面的时候速度巨慢无比。

    .apage {
      bbizlogic {
        .actions {
          .card_wrapper {
            .card {
              .title {
              }
              .content {
                .list {
                  .fa {
                  }
                }
              }
            }
          }
        }
      }
    }

我本人推荐（其实我是写 Python Web 后端的，逃… ) 代码风格比较倾向于 BEM 命名，关于 BEM 的介绍，请参考简单心理团队的教程。

- https://jiandanxinli.github.io/2016-08-11.html
- https://juejin.im/post/58d0e5caa22b9d00643e8b51

然而，最好的方式，就是读一个非常使用 SCSS 来组织项目的 CSS 代码的成熟项目。

我推荐两个：

- BOOTSTRAP V4: Bootstrap V4 使用 SCSS 来写
- ELEMENT UI: 饿了么的团队出的，前段时间从 v1 版本升级到 v2 版本，发现网站大部分样式都没有出现大变动，在这里给个赞。

### 0. 先从整体上设计好骨架

接着才是 HTML, 然后才是 CSS

现在前端入行的人越来越多，很多的新手前端 er 会用比较快的思维来编写，这就导致代码质量奇差无比。

- 哎，我看看，面粉加多了，我加点水，水加多了，我再加点面粉。
- 哎，我看看，面粉又加多了，我加点水，水加多了，我再加点面粉。
- 哎，我看看，面粉又加多了，我加点水，水加多了，我再加点面粉。
- 哎，我看看，面粉又加多了，我加点水，水加多了，我再加点面粉。
- 哎，我看看，面粉又加多了，我加点水，水加多了，我再加点面粉。

当设计出来的网页本身的 HTML 写的就很混乱，CSS 能写的好在哪里呢？

命名都很混乱，遑论代码可维护性？

可以多去参考一些成熟的项目的 CSS 是怎么命名的呀，HTML 是怎么设计的呀

### 1. Scoped 的滥用

我印象中，有个小伙伴把一个比较大的 CSS 库多次 import 到被 Scoped 的组件中，于是开发时猛然发现 head 处多了大量的 style 标签，除了 css 选择器后面随机的属性 hash, 文件内容都一样。

> 公共组件往往可以通过嵌套和加前缀的方式来防止污染。如果 scoped 的属性里面有成吨的 style, 慎用 import.

还有小伙伴喜欢在很多七七八八的组件各种 import scss. 其实对于中小型项目，完全可以直接全局一个文件 style 即可。

我现在的做法，是直接在 src 的上方直接用 gulp 搭建一个只用来编译 SCSS 到 CSS 的项目，每次编译后输出到页面里面。

如果项目是小项目，建议直接在 app.vue 里面 import pages

    ├── common
    ├── fonts
    ├── global.scss
    ├── index.scss
    ├── mixins
    ├── pages.scss
    └── reset.scss

### 2. 保持代码的通用性

一般，当同一段逻辑出现三次的时候，是要停下来重构一下的，这样的话，就可以节省很多时间。

套用在 CSS 的样式上也是如此。

## 0x04 Tmux 和 Tmuxnator 打造工作流

具体参考我的文章 [用 Tmux 和 Tmuxnator 打造工作流](l)

---

ChangeLog: - **2017-09-25** 初始化本文

