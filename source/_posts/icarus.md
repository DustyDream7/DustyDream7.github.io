---
title:   icarus官方文档译本
date: 	 2020-03-11 16:07:40
tags:	 Hexo
excerpt: 本篇为Hexo官方文档的个人译本，旨在解决无法访问Hexo官网时对文档的查验问题。
toc: 	 true
id:		 Git
---

# Icarus快速上手

作为静态网站生成器Hexo的一款主题，Icarus以简洁、现代和精美为设计理念。在灵活且强大的配置系统的助力下，用户可以自由实现单栏与 多栏的灵活页面布局。同时，Icarus提供了丰富的插件与挂件供用户选择，让网站的个性化配置变得触手可及。此外，得力于全新设计的API， 开发者可以更便捷地对Icarus进行深层定制。

Icarus的安装非常简单，您只需从GitHub的仓库中下载Icarus源码并解压到博客的主题目录下的`themes/icarus`目录中。 您也可以使用如下命令将此主题下载到博客中：

```bash
$ git clone https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus -b <version number> --depth 1
```

您可以省略`-b `来下载Icarus的最新开发版本。如果你想同时下载Git仓库的完整修改历史，请同时省略`--depth 1`。 另外，您也可以将Icarus作为Git子模块(submodule)安装到您的博客中：

```bash
$ git submodule add https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus
```

接下来，请将博客根目录下的`_config.yml`中的主题设置改为`icarus`：

```text
_config.yml

theme: icarus
```

或使用`hexo`命令修改主题设置:

```bash
$ hexo config theme icarus
```

最后，请使用如下命令来启动Hexo本地测试服务器。

```bash
$ hexo s
```

您可以继续阅读[Icarus用户指南](https://blog.zhangruipeng.me/hexo-theme-icarus/tags/Icarus用户指南/)系列文章来了解Icarus的使用与配置。 同时，您可以从Icarus的[`site`分支](https://github.com/ppoffice/hexo-theme-icarus/tree/site)上获取本网站的源码供您参考。

## 额外资源

下面的一些资源可以帮助你进一步个性化你的站点。 你也可以[点击此处](https://github.com/ppoffice/hexo-theme-icarus/edit/site/source/_posts/zh-CN/Getting-Started.md)来提交你的Icarus个性化配置教程。
[Hexo中文文档](https://hexo.io/zh-cn/docs/index.html)

[博客源码分享 by 辣椒の酱](https://removeif.github.io/theme/博客源码分享.html)

[hexo-theme-icarus 3 食用经验分享 by iMaeGoo](https://www.imaegoo.com/2020/icarus-3-guide/)

[活用 Bulma 美化 Icarus 文章 by iMaeGoo](https://www.imaegoo.com/2020/icarus-with-bulma/)

# Icarus用户指南 - 主题配置

Icarus的默认主题配置文件`_config.yml`存放在`themes/icarus`目录下。 此文件不仅定义了站点全局的布局与样式设置，同时也包含了例如评论系统和挂件等其他组件的配置。 本文将介绍Icarus的主题配置机制，并且简述部分常用配置的格式与含义。

## 配置文件生成与校验

Icarus的主题配置文件由[YAML语言](https://yaml.org/)实现。 当你使用Hexo来处理主题文件而你的主题目录下没有默认配置文件`themes/icarus/_config.yml`时，Icarus会通过一系列[JSON Schema](https://json-schema.org/)文件来自动生成默认的 配置文件。 所以正常情况下你的主题目录下没有示例配置文件(`_config.yml.example`)并且也没有必要去手动创建`_config.yml`文件。 大多数的JSON Schema定义存放在`themes/icarus/include/schema`目录下，其他的定义则存放在[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 仓库中。 你可以在运行`hexo`命令时附上`--icarus-dont-generate-config`来避免配置文件的自动生成。

当你每次在你的站点目录下运行`hexo`命令时，主题同时也会对比JSON Schema来校验你的配置文件是否正确。 如果校验中出现任何错误，Icarus会将错误位置与错误类型打印在屏幕上。 例如，如下的错误信息提醒我们`logo`配置项应该为字符串或是对象，而不是一个整型数。 你可以通过在`hexo`命令后附上`--icarus-dont-check-config`来跳过校验，但并不推荐这么做。

```
INFO  === Checking package dependencies ===
INFO  === Checking the configuration file ===
WARN  Configuration file failed one or more checks.
WARN  Icarus may still run, but you will encounter unexcepted results.
WARN  Here is some information for you to correct the configuration file.
WARN  [
  {
    keyword: 'type',
    dataPath: '.logo',
    schemaPath: '#/properties/logo/type',
    params: { type: 'string,object' },
    message: 'should be string,object'
  }
]
```

另外，Icarus会尝试运行迁移脚本将你的配置升级到最新版本。 这些脚本存放在`themes/icarus/include/migration`目录。 你可以在`hexo`命令后附上`--icarus-dont-upgrade-config`来禁止配置升级。 最后，Icarus会检查主题所依赖的Node.js库是否安装并提醒你安装缺失的组件。

## 额外的配置文件与优先级

除了在`themes/icarus/_config.yml`的默认主题配置文件外，Icarus也会从如下位置读取配置：

- `themes/icarus/_config.post.yml`和`themes/icarus/_config.page.yml`
- 文章/页面的[front-matter](https://hexo.io/docs/front-matter.html)
- 根目录下的站点配置文件`_config.yml`

`_config.post.yml`和`_config.page.yml`配置文件与默认配置文件的格式和定义相同。 你可以在`_config.post.yml`中设置仅对文章页面生效的配置，而这些配置将覆盖默认配置文件中的同名配置。 例如，你可以在此配置文件中把所有的挂件放置在页面左侧，从而将所有的文章页面设置为两栏布局，同时其他页面仍保持三栏布局：

```
widgets:
    -
        type: recent_posts
        position: left
    -
        type: categories
        position: right
    -
        type: tags
        position: right
themes/icarus/_config.post.yml

widgets:
    -
        type: recent_posts
        position: left
    -
        type: categories
        position: left
    -
        type: tags
        position: left
```

类似的，`_config.page.yml`中的配置也覆盖默认配置文件中的配置，并仅对所有Hexo页面(pages)生效。 此外，如果你想要在某个文章/页面中覆盖默认配置，你可以把这些配置放在那个文章/页面的front-matter(头部)。 例如，如果你想在某篇文章中更换代码高亮主题，你可以像下面这样把配置卸载文章的front-matter中：

```
title: 我的第一篇文章
date: '2015-01-01 00:00:01'
article:
    highlight:
        theme: atom-one-dark
---
# 文章标题
```

上面文章头部中的配置总会覆盖掉`_config.post.yml`和`_config.yml`文件中的同名配置。 在自定义页面或者优化访客的页面访问体验时，这个功能会十分有用。 比如，你可以为某篇文章设置更快的CDN，或者根据访客的国家或语言开启本地化的评论服务。 然而需要注意的是，一些Hexo定义的文章或页面属性不会复写掉其他配置文件中的同名配置，例如：

- `title`
- `date`
- `updated`
- `comments` (not `comment`)
- `layout`
- `source`
- `photos`
- `excerpt`

最后，站点根目录下的配置文件`_config.yml`可以被其他所有配置文件中的主题使用到的配置项所覆盖。 例如，`themes/icarus/_config.yml`中的`title`设置会覆盖掉`_config.yml`中的`title`，但`new_post_name`却不会，因为 Icarus没有用到这个配置项。

总而言之，配置文件的范围和覆盖优先级如下

| 某个文章/页面                                                |
| :----------------------------------------------------------- |
| front-matter可以覆盖`themes/icarus/_config.post.yml`或`themes/icarus/_config.page.yml`可以覆盖`themes/icarus/_config.yml`可以覆盖`_config.yml` |
| 所有文章/页面                                                |
| `themes/icarus/_config.post.yml`或`themes/icarus/_config.page.yml`可以覆盖`themes/icarus/_config.yml`可以覆盖`_config.yml` |
| 所有HTML页面                                                 |
| `themes/icarus/_config.yml`可以覆盖`_config.yml`             |
|                                                              |

## 主题配置项概览

### 配置文件版本

```
themes/icarus/_config.yml

version: 3.0.0
```

这个版本号与主题版本号相关却不总是相同。 当Icarus通过迁移脚本升级配置文件时，这个版本号会被用到。 请不要自己更改这个版本号。

### 主题变体

```
themes/icarus/_config.yml

variant: default
```

为Icarus更换”皮肤“。 此配置目前支持”default“和”cyberpunk“两种值。 你可以在[此处](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/cyberpunk-theme-variant/)查看Cyberpunk变体的效果。

### Logo

```
themes/icarus/_config.yml

logo: /img/logo.svg
```

此项配置设置了页面导航栏和尾部展示的logo图片。 `logo`配置的值既可以时你的logo图片的路径或URL地址，也可以时如下这种文字形式的logo

```
themes/icarus/_config.yml

logo:
    text: My Beautiful Site
```

### Favicon

```
themes/icarus/_config.yml

head:
    favicon: /img/favicon.svg
```

你可以在`head`中的`favicon`配置中设置你的网站的favicon图标的路径或URL地址。

### Open Graph

```
themes/icarus/_config.yml
```

你可以在`head`部分配置Open Graph。 一些配置项应在主题配置文件中留空并在需要时在文章的front-matter中设置它们。 请参考[Hexo文档](https://hexo.io/zh-cn/docs/helpers.html#open-graph)来了解每个配置项的详细含义。

### Google Structured Data

```
themes/icarus/_config.yml
```

你可以在`head`部分配置Google Structured Data。 大部分配置项应在主题配置文件中留空并在需要时在文章的front-matter中设置它们。 请参考[Search for Developers](https://developers.google.com/search/docs/guides/intro-structured-data)来了解每个配置项的详细含义。

### 页面Metadata

```
themes/icarus/_config.yml

head:
    meta:
        - 'name=theme-color;content=#123456'
        - 'name=generator;content="Hexo 4.2.0"'
```

你可以通过`head`部分的`meta`设置来向页面中添加``标签。 每一个meta标签应作为`meta`数组中的一个元素。 标签的属性应按照`<属性名>=<属性值>`的格式出现并用`;`隔开。

### RSS

```
themes/icarus/_config.yml

head:
    rss: /path/to/atom.xml
```

你可以通过此设置在页面头部添加RSS链接信息。

### 导航栏

```
themes/icarus/_config.yml

navbar:
    # 导航栏菜单项
    menu:
        Home: /
        Archives: /archives
        Categories: /categories
        Tags: /tags
        About: /about
    # 导航栏右侧的链接
    links:
        GitHub: 'https://github.com'
        Download on GitHub:
            icon: fab fa-github
            url: 'https://github.com/ppoffice/hexo-theme-icarus'
```

`navbar`部分定义了导航栏中出现的链接。 你可以通过向`menu`配置项中添加`<链接名>: <链接URL>`的方式添加任意导航栏菜单链接。 如果你希望向导航栏右侧添加链接，请向`links`配置项中添加`<链接名>: <链接URL>`。 你甚至可以使用FontAwesome图标来替换掉纯文字链接，格式如下：

```
链接格式

<链接名>:
    icon: <FontAwesome图标的class名>
    url: <链接URL>
```

### 页面尾部

```
themes/icarus/_config.yml

footer:
    links:
        Creative Commons:
            icon: fab fa-creative-commons
            url: 'https://creativecommons.org/'
        Attribution 4.0 International:
            icon: fab fa-creative-commons-by
            url: 'https://creativecommons.org/licenses/by/4.0/'
        Download on GitHub:
            icon: fab fa-github
            url: 'https://github.com/ppoffice/hexo-theme-icarus'
```

你可以通过向`footer`中的`links`配置项中添加添加任意链接。 链接的格式与`navbar`的`links`配置项相同。

### 代码高亮

```
themes/icarus/_config.yml

article:
    highlight:
        # 代码高亮主题
        # https://github.com/highlightjs/highlight.js/tree/master/src/styles
        theme: atom-one-light
        # 显示复制代码按钮
        clipboard: true
        # 代码块的默认折叠状态。可以是"", "folded", "unfolded"
        fold: unfolded
```

如果你已在Hexo中启用了代码高亮功能，那么你可以通过`article`中的`highlight`设置来自定义代码块。 请从[highlight.js/src/styles](https://github.com/highlightjs/highlight.js/tree/9.18.1/src/styles)目录下选择一个高亮主题， 然后将不带`.css`后缀的文件名设置到`theme`配置项中。 你亦可以将`clipboard`设置为`true`或`false`来显示或隐藏复制代码按钮。 最后，如果你希望默认折叠或展开所有代码块，请将`fold`设置为`folded`或`unfolded`。 你也可以将其设置为空来禁止代码块折叠。 另外，你可以使用下面的语法来折叠单独的代码块：

```
{% codeblock "可选文件名" lang:代码语言 >folded %}
...代码块内容...
{% endcodeblock %}
```

### 缩略图

为文章设置缩略图仅需两步。 第一步，确保主题配置文件中缩略图功能已启用：

```
themes/icarus/_config.yml

article:
    thumbnail: true
```

第二步，在文章的front-matter中设置缩略图的路径或URL地址：

```
post.md

title: Icarus快速上手
thumbnail: /gallery/thumbnails/desert.jpg
---
文章内容...
```

文章头部的图片路径应为图片的绝对路径或URL地址，或图片相对于你网站的`source`目录的相对地址。 例如，如果你想使用`/source/gallery/image.jpg`作为缩略图，你需要在front-matter中使用`/gallery/image.jpg`作为图片地址。

### 文章阅读时间

```
themes/icarus/_config.yml

article:
    readtime: true
```

你可以将`article`部分的`readtime`设置为`true`来显示文章字数以及预计阅读时间。

### 侧边栏

```
themes/icarus/_config.yml

sidebar:
    left:
        sticky: false
    right:
        sticky: true
```

你设置`sidebar`中的`sticky`设置项为`true`来让边栏的位置固定而不跟随页面滚动。

## 其他配置项

如果你对第三方的插件，挂件，以及CDN提供商的配置感兴趣的话，请参考[Icarus用户指南](https://blog.zhangruipeng.me/hexo-theme-icarus/tags/Icarus用户指南/)系列文章。

# Icarus用户指南 - 挂件

本文介绍Icarus 3支持的一些页面挂件的安装配置。若需同时展示多个挂件，只需在主题配置的`widgets`数组中添加多个挂件配置，而他们的展示顺序以配置的定义顺序为准。而每项挂件均包含必填项`type`(挂件类型)与`position`(挂件展示位置：左边/右边)。例如

```
widgets:
    -
        type: ... # 挂件1
        position: left
        ...
    -
        type: ... # 挂件2
        position: right
        ...
```

Icarus的绝大部分挂件由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，具体提供的挂件种类与配置以其为准。

## 作者资料卡

你可以启用作者资料卡挂件来展示文章作者/网站站长的信息。资料卡的示例配置如下所示：

```
widgets:
    -
        position: right
        type: profile
        # 作者名称
        author: hulatocat
        # 作者头衔
        author_title: A GitHub Octocat
        # 作者所在地/公司
        location: GitHub Inc.
        # 头像图片地址
        avatar: https://octodex.github.com/images/hula_loop_octodex03.gif
        # 是否显示圆形头像
        avatar_rounded: false
        # Gravatar邮箱(如不设置`avatar`项)
        gravatar:
        # 关注按钮链接地址
        follow_link: 'https://octodex.github.com/hulatocat'
        # 社交媒体链接
        social_links:
            Github:
                icon: fab fa-github
                url: 'https://github.com/'
            Icarus: 'https://github.com/ppoffice/hexo-theme-icarus'
```

需要注意的是：

- 如果你希望使用[Gravatar](https://en.gravatar.com/)提供头像图片，请在`gravatar`项填入 你的Gravatar邮箱地址，而`avatar`一项请留空；

- `social_links`可以采用如下两种格式：

  **图标形式**：

  ```
  <链接名称>:
      icon: <FontAwesome5图标的HTML class名称>
      url: <链接的URL地址>
  ```

  **文字形式**：

  ```
  <链接名称>: <链接的URL地址>
  ```

## 文章目录

若要展示文章目录，请首先在主题配置文件中添加如下挂件配置：

```
widgets:
    -
        type: toc
        position: left
```

然后，在需要开启目录的文章头部加入`toc: true`：

```
title: 一篇有目录的文章
toc: true
---
文章内容...
```

## 友站链接

你可以展示友站链接挂件展示相关网站以及它们的链接。友站链接挂件的配置如下所示：

```
widgets:
    -
        position: left
        type: links
        # 友站名称与链接
        links:
            Hexo: 'https://hexo.io'
            Bulma: 'https://bulma.io'
```

## 最新文章

请通过如下挂件配置开启最新文章挂件：

```
widgets:
    -
        position: right
        type: recent_posts
```

## 文章归档

请通过如下挂件配置开启文章归档挂件：

```
widgets:
    -
        position: right
        type: archives
```

## 文章分类

请通过如下挂件配置开启文章分类挂件：

```
widgets:
    -
        position: right
        type: categories
```

## 文章标签

请通过如下挂件配置开启文章标签挂件：

```
widgets:
    -
        position: right
        type: tags
```

## 邮件订阅

Icarus的邮件订阅功能由Google Feedburner提供。

1. 若要开启邮件订阅挂件，请首先使用诸如[hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 的Hexo插件生成网站的RSS源。

2. 然后登录[Google Feedburner](https://feedburner.google.com/)并添加你的RSS源。完成配置后，请点击网页顶部的”我的源“ (My Feeds)查看源列表，并点击新添加的源进入源的配置界面。

3. 点击”宣传“(Publicize)标签页，然后点击页面左侧的”邮件订阅“(Email Subscription)，然后在页面右侧找到并点击”激活“(Activate) 按钮。在打开的”邮件订阅“(Email Subscription)页面的HTML代码中找到

   ```yaml
   https://feedburner.google.com/fb/a/mailverify?uri=******
   ```

   复制`uri=`后的ID，例如`feedforall/ABCD`并填入挂件配置的`feedburner_id`中。

   ```yaml
   widgets:
       -
           position: left
           type: subscribe_email
           # (可选) 描述文字
           description: 邮件订阅，更新早知道
           feedburner_id: feedforall/ABCD
   ```

## Google AdSense

请登录[Google AdSense](https://www.google.com/adsense)并新建广告，复制广告代码中的`data-ad-client`和`data-ad-slot` 分别填入到挂件配置的`client_id`和`slot_id`项中。示例如下：

```yaml
widgets:
    -
        position: left
        type: adsense
        client_id: ca-pub-xxxxxxxx
        slot_id: xxxxxxx
```

# Icarus用户指南 - 站内搜索插件

本文介绍Icarus 3支持的一些站内搜索插件的安装配置。



Icarus的站内搜索插件由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，具体提供的插件种类与配置以其为准。

## Algolia

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/algolia-search-plugin/)

1. 在Hexo站点的根目录安装[hexo-algolia](https://github.com/oncletom/hexo-algolia)插件。

2. 注册并登录[Algolia](https://www.algolia.com/)。首次登录控制面板(Dashboard)时点击页面上的“创建索引”(Create Index)按钮并 输入任意索引名称(Index name)。最后点击“创建”(Create)完成索引创建。

3. 下一步，点击右侧导航栏上的API Keys，复制页面上的“应用ID”(Application ID)和“仅限搜索的API Key”(Search-Only API Key)。打开 Hexo站点根目录下的站点配置文件`_config.yml`，填入上面复制的信息到hexo-algolia插件的配置中。 例如，对于下面的Algolia索引信息

   ```
   Algolia索引信息
   
   Algolia索引名称: My-Hexo-Site
   Application ID: ABCDEFGHIJKL
   Search-Only API Key: 7b08fca7d42412cee901a25643124221
   ```

   对应的站点配置为

   ```
   _config.yml
   
   algolia:
       applicationID: My-Hexo-Site
       indexName: ABCDEFGHIJKL
       apiKey: 7b08fca7d42412cee901a25643124221
   ```

4. 回到Algolia控制面板的API Keys页面，切换到同一页面上的“所有API Keys”(All API Keys)标签页，点击“新建API Key”(New API Key)。 在弹出的“创建API Key”(Create API Key)对话框中选择“索引”(Indices)为上一步中创建的索引，“HTTP Referers”填入Hexo站点的 域名，“ACL”中填入`addObject`，`deleteObject`，`listIndexes`， `deleteIndex`四项。点击“创建”(Create)完成创建。 复制刚刚创建的API Key，例如`727fbd8c998fe419318fa350db6793ca`。

5. 打开一个命令行终端(Windows命令行(CMD)或Linux/macOS终端)，切换当前目录到你的Hexo站点的根目录并设置hexo-algolia 插件上传网站索引用到的环境变量为上一步中创建的API Key。

   Windows下

   ```
   Windows命令行(CMD)
   
   C:\Users\you> cd path/to/your/hexo/site
   C:\Users\you> set HEXO_ALGOLIA_INDEXING_KEY="727fbd8c998fe419318fa350db6793ca"
   ```

   Linux/macOS下

   ```
   Linux/macOS终端
   
   $ cd path/to/your/hexo/site
   $ export HEXO_ALGOLIA_INDEXING_KEY="727fbd8c998fe419318fa350db6793ca"
   ```

   然后运行下面的命令来清理并上传网站索引到Algolia

   ```
   Windows命令行(CMD)或Linux/macOS终端
   
   $ hexo clean
   $ hexo algolia
   ```

6. 最后，打开主题配置文件并设置搜索为Algolia

   ```
   themes/icarus/_config.yml
   
   search:
       type: algolia
   ```

## 百度搜索

**安装指南**

1. 打开主题配置文件并设置搜索为百度搜索

   ```
   themes/icarus/_config.yml
   
   search:
       type: baidu
   ```

## 谷歌自定义搜索

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/google-cse-search-plugin/)

1. 登录谷歌并访问[Google CSE](https://cse.google.com/cse/create/new)来创建自定义搜索。在“需要搜索的站点”(Sites to Search) 中填入你的Hexo站点域名，“语言”(Language)选择需要的语言，并填写自定义“搜索引擎名称”(Name of the search engine)。点击“创建” (Create)。

2. 然后，点击页面上的“添加到你的站点”(Add it to your site)右侧的“获取代码”(Get code)按钮，然后从HTML代码中复制`cx`的值填入到对应 主题配置项中。例如下面的HTML代码

   ```
   Google CSE HTML代码
   
   <script async src="https://cse.google.com/cse.js?cx=012345601234560123456:abcdefghijklmn"></script>
   <div class="gcse-search"></div>
   ```

   对应的主题配置为

   ```
   themes/icarus/_config.yml
   
   search:
       type: google_cse
       cx: 012345601234560123456:abcdefghijklmn
   ```

## Insight

**安装指南**

1. Insight为本站默认的站内搜索引擎。你可以通过下面的主题配置设置搜索为Insight

   ```
   themes/icarus/_config.yml
   
   search:
       type: insight
   ```

# Icarus用户指南 - CDN提供商

选择合适的CDN提供商可以大幅度减少网站访客的网页加载时间。 Icarus为你提供了多种内置的CDN提供商来加快主题所用到的前端资源的加载。



Icarus的CDN相关功能由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，具体提供的CDN提供商种类与配置以其为准。

## 内置CDN提供商

目前，Icarus提供如下几类内置的CDN服务提供商：

- JavaScript库CDN
  - cdnjs.com (`cdnjs`)
  - jsDelivr (`jsdelivr`)
  - UNPKG (`unpkg`)
  - loli.net (`loli`)
- 网络字体CDN
  - Google Fonts (`google`)
  - loli.net (`loli`)
- FontAwesome图标CDN
  - FontAwesome 5 (`fontawesome`)
  - loli.net (`loli`)

默认的CDN服务提供商配置为：

```
themes/icarus/_config.yml

providers:
    cdn: jsdelivr
    fontcdn: google
    iconcdn: fontawesome
```

## 自定义CDN提供商

除此之外，你还可以通过URL模板的方式使用自定义的CDN提供商。 每种类别提供商的模板格式如下文所示：

### JavaScript库CDN

```
CDN URL模板

https://some.cdn.domain.name/${package}/${version}/${filename}
```

你需要将CDN URL中的包名称，版本号，和文件相对路径替换为`${package}`， `${version}`，和`${filename}`占位符来获取 CDN的URL模板。 例如，如下JavaScript的在UNPKG CDN上的URL地址

```
UNPKG CDN URL示例

https://unpkg.com/d3@5.7.0/dist/d3.min.js
```

改写成的URL模板为

```
UNPKG CDN URL模板

https://unpkg.com/${package}@${version}/${filename}
```

一些CDN提供商可能采用不同的URL形式，而在这些CDN上包名称或文件相对路径与其他CDN会有所不同。 例如，`moment.js`库在CDN.js上有着如下的URL形式：

```
CDN.js CDN URL示例

https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.22.2/moment.js
```

但在UNPKG上有着这样的URL形式

```
UNPKG CDN URL示例

https://unpkg.com/moment@2.22.2/min/moment.min.js
```

因此，你需要注意你的自定义CDN提供商的URL格式。 默认情况下，Icarus在URL模板中采用npm仓库的包名称和文件相对路径（例如`moment@2.22.2/min/moment.min.js`）。 jsDelivr和UNPKG就采用这种npm形式。 另外，如果你使用的CDN采用如CDN.js这样的URL形式，请在URL模板前加上`[cdnjs]`前缀。

```
CDN.js-style URL模板

[cdnjs]https://some.cdn.domain.name/${package}/${version}/${filename}
```

### 网络字体CDN

你可以使用Google字体镜像CDN或是与其兼容的网络字体CDN。 Icarus依赖`Ubuntu`，`Oxanium`，和`Source Code Pro`这三种字体。 所以请确保你使用的CDN提供这些字体。 自定义的URL模板需包含字体类型`type`（图标`icon`或是字体`font`）和字体名称`fontname`两个占位符：

```
Webfont CDN URL模板

https://some.google.font.mirror/${type}?family=${fontname}
```

### FontAwesome图标CDN

你可以使用自定义的FontAwesome CDN提供商。 URL模板中不需要包含占位符。 自定义提供商需要至少提供本主题所用到的所有FontAwesome 5图标。

```
Icon Font CDN URL模板

https://custom.fontawesome.mirror/some.stylesheet.css
```

以上自定义配置可以放到主题配置文件中的`providers`部分：

```
themes/icarus/_config.yml

providers:
    cdn: 'https://some.cdn.domain.name/${package}/${version}/${filename}'
    fontcdn: 'https://some.google.font.mirror/${type}?family=${fontname}'
    iconcdn: 'https://custom.fontawesome.mirror/some.stylesheet.css'
```

## CDN工具函数

本主题提供了三个工具函数来帮助主题开发者轻松引用第三方的前端资源。 详情请参见[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno/blob/0.2.3/src/hexo/helper/cdn.js).

# Icarus用户指南 - 分享按钮

本文介绍Icarus 3支持的一些分享按钮的安装配置。



Icarus的分享按钮由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，具体提供的按钮种类与配置以其为准。

## AddThis

此分享按钮可能会被部分广告拦截浏览器扩展拦截，请酌情使用。

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/addthis-share-buttons/)

1. 注册[AddThis](https://www.addthis.com/)。在首次注册时的“选择工具”(Select a Tool)页面选择“分享按钮”(Share Buttons)。

2. 在“选择工具类型”(Select a Tool Type)界面选择分享按钮的展示样式，点击“继续”(Continue)。

3. 在下一步的页面中按照需要进一步自定义分享按钮的样式与行为，完成时点击“激活工具”(Activate Tool)按钮。

4. 在获取代码界面找到分享按钮的HTML代码，复制其中的`src`地址并填入相应的主题配置中，例如下面的AddThis代码

   ```
   AddThis代码
   
   <!-- Go to www.addthis.com/dashboard to customize your tools -->
   <script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-xxxxxxxxxxxxx"></script>
   ```

   对应的主题配置为

   ```
   themes/icarus/_config.yml
   
   share:
       type: addthis
       install_url: //s7.addthis.com/js/300/addthis_widget.js#pubid=ra-xxxxxxxxxxxxx
   ```

## AddToAny

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/addtoany-share-buttons/)

1. 无需注册步骤，直接修改主题配置来启用AddToAny：

   ```
   themes/icarus/_config.yml
   
   share:
       type: addtoany
   ```

2. (可选)若你需要对分享按钮进行进一步的个性化配置，请打开[AddToAny](https://www.addtoany.com/)官网，点击“获取分享按钮” (Get the Share Button)，然后选择“任意网站”(Any Website)。在页面中完成配置后点击“获取按钮代码”(Get Button Code)。 例如，下面是获得的默认代码：

   ```
   AddToAny代码
   
   <!-- AddToAny BEGIN -->
   <div class="a2a_kit a2a_kit_size_32 a2a_default_style">
   <a class="a2a_dd" href="https://www.addtoany.com/share"></a>
   <a class="a2a_button_facebook"></a>
   <a class="a2a_button_twitter"></a>
   <a class="a2a_button_email"></a>
   </div>
   <script async src="https://static.addtoany.com/menu/page.js"></script>
   <!-- AddToAny END -->
   ```

   由于本Hexo主题的分享按钮由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，若要对其中的分享按钮代码进行修改，需首先复制其中的文件到主题相应的目录下。例如，这里我们复制 [src/view/share/addtoany.jsx](https://github.com/ppoffice/hexo-component-inferno/blob/0.2.2/src/view/share/addtoany.jsx)到`themes/icarus/layout/share/`目录下。然后修改其中的`require`路径为正确的路径，并将上面的AddToAny的HTML代码替换 到文件的相应位置中即可。

   ```
   themes/icarus/layout/share/addtoany.jsx
   
   const { Component, Fragment } = require('inferno');
   - const { cacheComponent } = require('../../util/cache');
   + const { cacheComponent } = require('hexo-component-inferno/lib/util/cache');
   
   ...中间省略部分代码...
   
   class AddToAny extends Component {
       render() {
           return <Fragment>
   -            <div class="a2a_kit a2a_kit_size_32 a2a_default_style">
   -                <a class="a2a_dd" href="https://www.addtoany.com/share"></a>
   -                <a class="a2a_button_facebook"></a>
   -                <a class="a2a_button_twitter"></a>
   -                <a class="a2a_button_telegram"></a>
   -                <a class="a2a_button_whatsapp"></a>
   -                <a class="a2a_button_reddit"></a>
   -            </div>
   +           刚刚获取的AddToAny HTML代码替换到这里
               <script src="https://static.addtoany.com/menu/page.js" defer={true}></script>
           </Fragment>;
       }
   }
   
   ...下面省略部分代码...
   ```

## 百度分享

此分享按钮可能会被部分广告拦截浏览器扩展拦截，请酌情使用。

百度分享按钮服务似乎已下线，建议使用其他分享按钮服务作为替代。

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/baidu-share-buttons/)

1. 无需注册步骤，直接修改主题配置来启用百度分享：

   ```
   themes/icarus/_config.yml
   
   share:
       type: bdshare
   ```

## Share.js

Share.js服务已停止维护，建议使用其他分享按钮服务作为替代。

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/share-js-share-buttons/)

1. 无需注册步骤，直接修改主题配置来启用Share.js：

   ```
   themes/icarus/_config.yml
   
   share:
       type: sharejs
   ```

2. (可选)如果你需要自定义分享按钮，请参照[AddToAny](https://blog.zhangruipeng.me/hexo-theme-icarus/Plugins/Share/icarus用户指南-分享按钮/#AddToAny)中的第二步与[share.js主页](https://github.com/overtrue/share.js) 的相关信息。

## ShareThis

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/sharethis-share-buttons/)

1. 打开[ShareThis官网](https://sharethis.com/)，点击页面上的“从分享按钮开始”(Start with Share Buttons)按钮。

2. 在“选择分享按钮类型”(Choose type of sharing button)选择需要的按钮类型，如有需要的话可点击下方的“自定义你的分享按钮” (Customize your share buttons)链接进行进一步配置。完成后点击“下一步”(Next)。

3. 在“注册并获取代码”(Register and get the code!)界面点击HTML和“下一步”(Next)按钮。然后输入邮箱和密码完成ShareThis的注册。

4. 最后，复制代码获取界面中的`src`地址并填入相应的主题配置中，例如下面的ShareThis代码

   ```
   AddThis代码
   
   <script type="text/javascript" src="https://platform-api.sharethis.com/js/sharethis.js#property=xxxxxxxxxxxxx&product=inline-share-buttons" async="async"></script>
   ```

   对应的主题配置为

   ```
   themes/icarus/_config.yml
   
   share:
       type: sharethis
       install_url: https://platform-api.sharethis.com/js/sharethis.js#property=xxxxxxxxxxxxx&product=inline-share-buttons
   ```

# Icarus用户指南 - 赞赏按钮

本文介绍Icarus 3支持的一些赞赏按钮的安装配置。若需同时展示多个按钮，只需在主题配置的`donates`数组中添加多个按钮配置，例如

```
themes/icarus/_config.yml

donates:
    -
        type: ... # 按钮1
        ...
    -
        type: ... # 按钮2
        ...
```



Icarus的赞赏按钮由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，具体提供的按钮种类与配置以其为准。

## 支付宝

**安装指南**

登录支付宝并导出个人支付二维码图片放置在网站的图片或附件文件夹下，或上传至图床，然后在主题配置中添加如下配置项：

```
themes/icarus/_config.yml

donates:
    -
        type: alipay
        # 支付宝二维码图片地址
        qrcode: /path/to/alipay/qrcode.png
```

## Buy me a Coffee

**安装指南**

注册[Buy me a Coffee](https://www.buymeacoffee.com/)并复制个人赞助页面的地址，填写到如下主题配置中：

```
themes/icarus/_config.yml

donates:
    -
        type: buymeacoffee
        # 个人赞助页面的地址
        url: /path/to/buymeacoffee/personal/page
```

## Paypal

**安装指南**

1. 注册并登录Paypal，点击[此链接](https://www.paypal.com/donate/buttons/)来创建Paypal捐赠按钮。

2. 在选择按钮样式(Choose button style)页面正确填写“国家/地区”(Country/Region)，“语言”(Language)后，点击“继续”(Continue) 进入下一页面。

3. 在添加机构详情(Add organization details)页面中，选择“使用账号ID”(Use account ID)或“使用Email地址”(Use email address) 作为唯一账号标识符。点击“继续”(Continue)进入下一页面。

4. 在“设置捐赠数额”(Set donation amounts)页面选择你要“接收的货币种类”(Currency you’ll receive donations in)，捐赠数额选择 “任意数额”(Any amount)。此按钮暂不支持指定数额的捐赠选项。点击“结束并获取代码”(Finish and Get Code)按钮进入下一页面。

5. 从页面上的“按钮HTML代码”(Button HTML)中复制`business`和`currency_code`两项的值并填写到主题配置中。例如，下方的Paypal按钮 代码

   ```
   Paypal按钮HTML代码
   
   <form action="https://www.paypal.com/cgi-bin/webscr" ...>
   <input type="hidden" name="cmd" value="_donations" />
   <input type="hidden" name="business" value="XXXXXXXXXXXXX" />
   <input type="hidden" name="currency_code" value="USD" />
   ...
   </form>
   ```

   对应的主题配置为

   ```
   themes/icarus/_config.yml
   
   donates:
       -
           type: paypal
           business: XXXXXXXXXXXXX
           currency_code: USD
   ```

## Patreon

**安装指南**

注册[Patreon](https://www.patreon.com/)并复制个人赞助页面的地址，填写到如下主题配置中：

```
themes/icarus/_config.yml

donate:
    -
        type: patreon
        # 个人赞助页面的地址
        url: /path/to/patreon/personal/page
```

## 微信

**安装指南**

登录微信并导出个人支付二维码图片放置在网站的图片或附件文件夹下，或上传至图床，然后在主题配置中添加如下配置项：

```
themes/icarus/_config.yml

donates:
    -
        type: wechat
        # 微信二维码图片地址
        qrcode: /path/to/wechat/qrcode.png
```

# Icarus用户指南 - 网站分析插件

本文介绍Icarus 3支持的一些网站统计与分析插件的安装配置。



Icarus的网站统计与分析插件由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，具体提供的插件种类与配置以其为准。

本文中涉及的所有插件均可能被部分广告拦截浏览器扩展拦截，请酌情使用。

## 百度统计

**安装指南**

1. 登录[百度统计](https://tongji.baidu.com/)。在“网站列表”页面上点击“新增网站”按钮并填写你的Hexo站点的“网站域名”，“网站首页” 等信息，点击“确定”。

2. 在跳转到的“代码获取”页面中找到`hm.baidu.com/hm.js?`后引号内的一串ID并填写到主题配置的`plugins` > `baidu_analytics` > `tracking_id`。例如如下的统计代码

   ```
   百度统计代码
   
   <script>
   var _hmt = _hmt || [];
   (function() {
   var hm = document.createElement("script");
   hm.src = "https://hm.baidu.com/hm.js?3f06f2b732a5b1034c989f74e28d0eea";
   var s = document.getElementsByTagName("script")[0]; 
   s.parentNode.insertBefore(hm, s);
   })();
   </script>
   ```

   对应的主题配置为

   ```
   themes/icarus/_config.yml
   
   plugins:
       baidu_analytics:
           tracking_id: 3f06f2b732a5b1034c989f74e28d0eea
   ```

## 不蒜子网页计数器

**安装指南**

1. 将主题配置中的`plugins`部分下的`busuanzi`设置为`true`即可开启不蒜子访客计数器，在网站页面的尾部和每篇博文的头部展示访问次数。

   ```
   themes/icarus/_config.yml
   
   plugins:
       busuanzi: true
   ```

## CNZZ统计

**安装指南**

1. 登录[友盟+](https://www.umeng.com/)，在友盟+工作台首页点击“创建新应用” > “Web应用”。然后输入“网站名称”，“网站域名”， “网站首页”等信息，并点击“确认添加站点”。

2. 在跳转到的“统计代码“获取界面上找到”文字形式“的统计代码，并将其中`id`与`web_id`的值填写到主题配置的`plugins` > `cnzz` > `id`和`web_id`。例如如下的统计代码

   ```
   CNZZ统计代码
   
   <script type="text/javascript" src="https://s9.cnzz.com/z_stat.php?id=123456789000&web_id=123456789000"></script>
   ```

   对应的主题配置为

   ```
   themes/icarus/_config.yml
   
   plugins:
       cnzz:
           id: 123456789000
           web_id: 123456789000
   ```

## Google Analytics

**安装指南**

1. 登录[Google Analytics](https://analytics.google.com/)。在进入用户主页后点击左侧导航栏下方的”管理“(Admin)进入管理界面。

2. 在管理界面上点击”创建资产“(Create Property)按钮，选择测量的应用类型(What do you want to measure?)为Web并点击”继续“。 然后填写网站的名称(Website Name)，URL地址(Website URL)，分类(Industry Category)，以及报告时区(Reporting Time Zone)等信息。 点击”创建“(Create)。

3. 在之后的”追踪代码“(Tracking Code)界面上找到”Tracking ID”的值，例如”UA-12345678-0”，填写到主题配置的`plugins` > `google_analytics` > `tracking_id`即可完成配置。

   ```
   themes/icarus/_config.yml
   
   plugins:
       google_analytics:
           tracking_id: UA-12345678-0
   ```

## Hotjar

**安装指南**

1. 登录[Hotjar](https://www.hotjar.com/)，点击页面左上角的➕(加号)菜单中的”添加新站点“(Add new site)链接。

2. 填写”网站地址“(WEBSITE)，”站点类型“(SITE TYPE)，和”站点所有者“(SITE OWNER)，然后点击”添加站点“(Add Site)按钮。

3. 在跳转到的”站点&组织“(Sites & Organizations)页面找到新建的站点项，点击其右侧的”追踪代码“(Tracking Code)按钮并在弹出的 对话框中找到”Site ID”的值，例如”1234567”，填写到主题配置的`plugins` > `hotjar` > `site_id`即可完成配置。

   ```
   themes/icarus/_config.yml
   
   plugins:
       hotjar:
           site_id: 1234567
   ```

# Icarus用户指南 - 用户评论插件

本文介绍Icarus 3支持的一些用户评论插件的安装配置。



Icarus的用户评论插件由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，具体提供的插件种类与配置以其为准。

## 畅言

**安装指南**

1. 首先，注册并登录[畅言云评](http://changyan.kuaizhan.com/)， 并按照[PC端通用代码接入](http://changyan.kuaizhan.com/static/help/index.html)的帮助文档获取代码。

2. 从获取的PC端代码中复制`appid`与`conf`值到Icarus主题配置文件的评论配置项中。 例如，获取到的如下的PC端代码：

   ```
   <!--PC版-->
   <div id="SOHUCS" sid="..."></div>
   <script charset="utf-8" type="text/javascript" src="https://cy-cdn.kuaizhan.com/upload/changyan.js" ></script>
   <script type="text/javascript">
   window.changyan.api.config({
       appid: '????appid????',
       conf: 'prod_xxxxxxxxxxxxxxxxxxxxxxx'
   });
   </script>
   ```

   对应到主题配置文件中的配置项为：

   ```
   themes/icarus/_config.yml
   
   comment:
       type: changyan
       app_id: ????appid????
       conf: prod_xxxxxxxxxxxxxxxxxxxxxxx
   ```

3. 另外，畅言云评要求站长对使用其评论服务的网站进行备案，详情请参阅 [ICP备案](http://changyan.kuaizhan.com/static/help/o-beian.html)文档。

## Disqus

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/disqus-comment-plugin/)

1. 首先，注册并登录[Disqus](https://disqus.com/)，点击首页的“开始”(GET STARTED)按钮或者访问 [此处](https://disqus.com/profile/signup/intent/)并点击“我想要将Disqus安装到我的站点” (I want to install Disqus on my site)来创建新的站点评论服务。

2. 在创建新站点页面中填写网站名称(Website Name)以及网站类型(Category)，然后点击“创建站点”(Create Site)。

3. 下一步，选择Disqus的安装平台。此处选择最下方的“上面没有列出我使用的平台，使用通用代码安装” (I don’t see my platform listed, install manually with Universal Code)。

4. 点击最下方的“配置”(Configure)按钮跳过通用代码安装指南(Universal Code install instructions)页面。 在“配置Disqus”(Configure Disqus)页面中按需填写Disqus个性化配置。点击“完成安装”(Complete Setup) 和“关闭配置”(Dismiss Setup)来结束配置。

5. 最后，在评论站点首页的右上角点击“编辑配置”(Edit Settings)，进入到站点配置页面。 在页面上找到Shortname配置值并复制到主题配置文件相应的评论配置项中(`comment`下的`shortname`)。 例如，下图中的Shortname为`test-usildmkxo`

   [![General Settings - Disqus Admin](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/screenshots/disqus-general-settings.png)](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/screenshots/disqus-general-settings.png)

   [General Settings - Disqus Admin](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/screenshots/disqus-general-settings.png)

   

   复制到配置文件中为

   ```
   themes/icarus/_config.yml
   
   comment:
       type: disqus
       shortname: test-usildmkxo
   ```

6. （可选）你可以在文章的头部加入`disqusId`来为文章添加Disqus唯一标识。这样如果文章之后被移动或者链接更改，其 评论也不会随之丢失。

   ```
   source/_post/some-post.md
   
   title: My first post
   date: 2015-01-01 00:00:01
   disqusId: some-disqus-id
   ---
   # Hello world
   ```

## DisqusJS

DisqusJS适用于原版Disqus服务访问受限的地区使用。关于DisqusJS的配置过程可参考 https://github.com/SukkaW/DisqusJS。

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/disqusjs-comment-plugin/)

1. 登录Disqus并打开[此链接](https://disqus.com/api/applications/)，点击“注册新应用”(Register new application)或者“注册应用”(registering an application)。

2. 在打开的页面中填写应用名称(Label)，介绍(Description)，以及网站地址(Website)，然后点击“注册我的应用” (Register my application)。

3. 应用创建完毕后进入设置界面(Settings)，在域名(Domains)输入框中填入你的网站域名，例如ppoffice.github.io。 点击页面下方的“保存设置”(Save Changes)按钮。

4. 点击页面中上放的“详情”(Details)链接切换到当前应用的主页，从下方OAuth设置(OAuth Settings)中复制API Key 到相应的主题配置项(`comment`下的`api_key`)中。下面是示例配置：

   ```
   themes/icarus/_config.yml
   
   comment:
       type: disqusjs
       shortname: test-usildmkxo
       api_key: xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       api: https://disqus.skk.moe/disqus/     # 可选填
       admin: ppoffice                         # 可选填
       admin_label: Admin                      # 可选填
       nesting: 4                              # 可选填
   ```

5. 关于上述配置的含义和可选值，请参考[SukkaW/DisqusJS官方文档](https://github.com/SukkaW/DisqusJS)或 hexo-component-inferno中的[配置项描述](https://github.com/ppoffice/hexo-component-inferno/blob/0.2.1/src/schema/comment/disqusjs.json)。

## Facebook

此评论插件可能会被部分广告拦截浏览器扩展拦截，请酌情使用。

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/facebook-comment-plugin/)

1. Facebook的评论服务配置较为简单，仅需在主题配置中将`comment`的`type`设置为`facebook`即可。

   ```
   themes/icarus/_config.yml
   
   comment:
       type: facebook
   ```

## Gitalk

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/gitalk-comment-plugin/)

1. 登录GitHub并[点此注册](https://github.com/settings/applications/new)一个新的OAuth应用。在填写相应的应用名称 (Application name)，应用主页(Homepage URL)，应用描述(Application description)后，在认证回调地址(Authorization callback URL)填写你的博客的根URL地址。

2. 点击“注册应用”(Register application)后，在应用详情界面复制Client ID与Client Secret并填入主题配置的相应配置项中。 例如，对于下面的Client ID和Client Secret

   ```
   GitHub OAuth Application
   
   Client ID
   xxxxxxxxxxxxxxxxxxxx
   Client Secret
   xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```

   Gitalk在Icarus的配置为

   ```
   themes/icarus/_config.yml
   
   comment:
       type: gitalk
       client_id: xxxxxxxxxxxxxxxxxxxx
       client_secret: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       repo: Some-of-Your-GitHub-Repo
       owner: you_github_name
       admin:
           - you_github_name
       per_page: 20                    # 可选填
       distraction_free_mode: false    # 可选填
       pager_direction: last           # 可选填
       create_issue_manually: false    # 可选填
       proxy:                          # 可选填
       flip_move_options:              # 可选填
       enable_hotkey: true             # 可选填
   ```

3. 关于上述配置的含义和可选值，请参考[Gitalk官方文档](https://github.com/gitalk/gitalk)或hexo-component-inferno 中的[配置项描述](https://github.com/ppoffice/hexo-component-inferno/blob/0.2.1/src/schema/comment/gitalk.json)。

## Gitment

Gitment项目似乎已停止维护，建议使用Gitalk或utterances作为基于GitHub Issues的评论系统的替代。

**安装指南**

1. 参照上面Gitalk中关于注册GitHub OAuth应用的步骤注册应用，并将Client ID与Client Secret并填入主题配置的相应配置项中。 下面是Gitment的示例配置：

   ```
   themes/icarus/_config.yml
   
   comment:
       type: gitment
       owner: you_github_name
       repo: Some-of-Your-GitHub-Repo
       client_id: xxxxxxxxxxxxxxxxxxxx
       client_secret: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       theme: gitment.defaultTheme     # 可选填
       per_page: 20                    # 可选填
       max_comment_height: 250         # 可选填
   ```

2. 关于上述配置的含义和可选值，请参考[Gitment官方文档](https://github.com/imsun/gitment)或hexo-component-inferno 中的[配置项描述](https://github.com/ppoffice/hexo-component-inferno/blob/0.2.1/src/schema/comment/gitment.json)。

## Isso

如果你不希望以来第三方评论服务而自建评论系统的话，可考虑选用Isso。 当然，不同于第三方评论系统，你需要准备一个服务器用来运行Isso服务端程序。

**安装指南**

1. 请参照[Isso官方文档](https://posativ.org/isso/docs/install/)安装并启动Isso服务。

2. 编辑主题配置文件并填入Isso服务的HTTP URL。例如，你的Isso服务地址为https://posativ.org/isso/api/， 则需在主题评论配置填写为：

   ```
   themes/icarus/_config.yml
   
   comment:
       type: isso
       url: posativ.org/isso/api
   ```

## LiveRe

此评论插件可能会被部分广告拦截浏览器扩展拦截，请酌情使用。

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/livere-comment-plugin/)

1. 首先，注册并登录[LiveRe](https://livere.com/)。登录后，点击导航栏上的“安装”(Install)链接，进入安装界面。

2. 在安装界面选择免费(City)选项下方的“现在安装”(Install Now)按钮。在获取LiveRe City代码(Get LiveRe City code)界面 填入站点地址(Site URL)，网站名称(Name of website)，和网站类别(Choose site category)，勾选“同意广告协议后”并点击 获取代码后，跳转到LiveRe代码页面。

3. 复制代码中`data-uid="..."`引号内的编号，填写到主题配置中的相应选项中。例如，下方的LiveRe代码

   [![LiveRe - Install](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/screenshots/livere-code.png)](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/screenshots/livere-code.png)

   [LiveRe - Install](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/screenshots/livere-code.png)

   

   对应的主题配置为

   ```
   themes/icarus/_config.yml
   
   comment:
       type: livere
       uid: ABCD1234O0OxxxxXXXX000==
   ```

## Utterances

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/utterances-comment-plugin/)

1. 准备一个公开的GitHub仓库(Repository)。

2. 访问[GitHub Apps - utterances](https://github.com/apps/utterances)页面来将utterances安装至准备好的仓库。 点击“配置”(Configure)按钮，在下一页面中选择需要安装utterances的用户。在安装页面，你可以选择将其安装到所有仓库(All repositories) 或是选定的一些仓库(Only select repositories)。点击“安装”(Install)。

3. 若安装成功，网页跳转到[utterances官网](https://utteranc.es/)。之后可以阅读页面上的配置项的说明，并按照说明将对应配置值填入到 主题配置中。下方为示例配置：

   ```
   themes/icarus/_config.yml
   
   comment:
       type: utterances
       repo: Your-GitHub-Username/Your-Public-Repo-Name
       issue_term: pathname        # 必填项，与issue_number二选一填写
       issue_number: 100           # 必填项，与issue_term二选一填写，每篇文章对应一个issue，填写前需要手动创建issue
       label: some-issue-label     # 可选填
       theme: github-light         # 可选填
   ```

## Valine

**安装指南** [在线预览](https://blog.zhangruipeng.me/hexo-theme-icarus/uncategorized/valine-comment-plugin/)

1. 按照Valine官方的[快速开始文档](https://valine.js.org/quickstart.html)创建LeanCloud应用。

2. 将App ID和App Key复制并填入主题配置的对应配置项中，并按照官方网站上的[配置项](https://valine.js.org/configuration.html) 说明按需填写其他配置项。下面是示例配置：

   ```
   themes/icarus/_config.yml
   
   comment:
       type: valine
       app_id: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
       app_key: xxxxxxxxxxxxxxxxxxxxxx
       placeholder: ""                 # 可选填
       notify: false                   # 可选填
       verify: false                   # 可选填
       avatar:                         # 可选填
       avatar_force: false             # 可选填
       meta: ["nick", "mail", "link"]  # 可选填
       page_size: 10                   # 可选填
       visitor: false                  # 可选填
       highlight: true                 # 可选填
       record_ip: false                # 可选填
   ```

# Icarus用户指南 - 其他插件

本文介绍Icarus 3支持的其他插件的安装配置。



部分下述插件由[ppoffice/hexo-component-inferno](https://github.com/ppoffice/hexo-component-inferno) 提供，它们的配置请以其为准。

## 画廊

**安装指南**

Icarus的画廊插件同时包含了[lightGallery](https://sachinchoolur.github.io/lightGallery/)与 [Justified Gallery](https://miromannino.github.io/Justified-Gallery/)两种插件。 若要启用画廊插件，请将主题配置中`plugins` > `gallery`的值设置为`true`。

```
themes/icarus/_config.yml

plugins:
    gallery: true
```

另外，若要使用Justified Gallery，请将你的多个图片包裹在``与``的HTML标签对中。 并且如果你使用的是Markdown语法来引用图片的话，请在HTML标签和Markdown之间添加空行。 例如下方的效果预览的Markdown代码为：

```
Justified-Gallery-Markdown.md
```

同样，我们也可使用纯HTML来实现Justified Gallery，这样标签之间就不需要添加空行了：

```
Justified-Gallery-HTML.md
```

**效果预览**

下面是Justified Gallery实现的多图片网格化展示。点击其中的任意可另外查看lightGallery的全图展示效果。

> 下面的图片来源于[pexel.com](https://www.pexels.com/)

[![Elephant](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/elephant.jpeg)](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/elephant.jpeg)[![Dog](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/dog.jpeg)](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/dog.jpeg)[![Birds](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/birds.jpeg)](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/birds.jpeg)[![Fox](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/fox.jpeg)](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/fox.jpeg)[![Horse](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/horse.jpeg)](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/horse.jpeg)[![Leopard](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/leopard.jpeg)](https://blog.zhangruipeng.me/hexo-theme-icarus/gallery/animals/leopard.jpeg)

## KaTeX

**安装指南**

你可以使用KaTeX插件来渲染TEXTEX数学公式。若要启用KaTeX插件，请将主题配置中`plugins` > `katex`的值设置为`true`。

```
themes/icarus/_config.yml

plugins:
    katex: true
```

在使用时，请使用`\\(`和`\\)`包裹行内公式，`$$`或`\\[`与`\\]`标签对包裹块状公式。例如：

```
Some-Post.md
```

有时你的TEXTEX语法会被错认为Markdown语法而导致公式渲染异常。 例如，下面的公式不会渲染成功，因为其中包含多个`_`(下划线)而被Markdown渲染器错误的认成Markdown的斜体字语法。

```
Some-Post.md

$$
\hat{x}_{k}=\hat{x}_{k}^{-}+K_{t}\left(y_{k}\right)
$$
```

在这种情况下，你可以选择转义每个可能导致歧义的字符：

```
Some-Post.md

$$
\hat{x}\_{k}=\hat{x}\_{k}^{-}+K\_{t}\left(y\_{k}\right)
$$
```

或是简单的将整个公式用一个HTML标签包裹起来：

```
Some-Post.md

<div>
$$
\hat{x}_{k}=\hat{x}_{k}^{-}+K_{t}\left(y_{k}\right)
$$
</div>
```

## MathJax

**安装指南**

你可以使用MathJax插件来渲染TEXTEX，MathML，或AsciiMath数学公式。若要启用MathJax插件，请将主题配置中`plugins` > `mathjax`的值设置为`true`。

```
themes/icarus/_config.yml

plugins:
    mathjax: true
```

当使用TEXTEX语法时，请使用`$`或`\\(`与`\\)`包裹行内公式，`$$`或`\\[`与`\\]`标签对包裹块状公式。或者使用LATEXLATEX环境。例如：

```
Tex-Example.md
```

或者直接使用MathML语法。例如：

```
MathML-Example.md
```

同样MathJax也支持AsciiMath，公式使用`\``包裹。

```
AsciiMath-Example.md
```

有时你的TEXTEX语法会被错认为Markdown语法而导致公式渲染异常。 请参照[KaTeX](https://blog.zhangruipeng.me/hexo-theme-icarus/Plugins/Other/icarus用户指南-其他插件/#KaTeX)一节来解决此问题。

**效果预览(TEXTEX & LATEXLATEX)**

这是一个行内公式：ax2+bx+c=0ax2+bx+c=0。这是另一个行内公式：ax2+bx+c>0ax2+bx+c>0。

这是一个块状公式：1(√ϕ√5−ϕ)e25π=1+e−2π1+e−4π1+e−6π1+e−8π1+⋯1(ϕ5−ϕ)e25π=1+e−2π1+e−4π1+e−6π1+e−8π1+⋯

这是另一个块状公式：f(x)=∫∞−∞^f(ξ)e2πiξxdξf(x)=∫−∞∞f^(ξ)e2πiξxdξ

或者使用LATEXLATEX环境：A=[abcc]A=[abcc]

**效果预览(MathML)**

当 a≠0a≠0， 方程 ax2+bx+c=0ax2+bx+c=0 有两个解，它们是x=−b±√b2−4ac2a.x=-b±b2-4ac2a.

**效果预览(AsciiMath)**

当a≠0a≠0，方程ax2+bx+c=0ax2+bx+c=0有两个解，它们是

x=−b±√b2−4ac2ax=-b±b2-4ac2a.



## 浏览器升级提醒 (Outdated Browser)

**安装指南**

你可以开启浏览器升级提醒(Outdated Browser)来提醒使用老旧浏览器的网站访客升级浏览器。 若要启用此插件，请将主题配置中`plugins` > `outdated_browser`的值设置为`true`。 点击[此处](https://bestvpn.org/outdatedbrowser/en)即可预览插件开启效果。

```
themes/icarus/_config.yml

plugins:
    outdated_browser: true
```

## 网页载入动画

**安装指南**

Icarus默认启用网页载入动画，若需禁止载入动画，请将`plugins` > `animejs`的值设置为`false`。

```
themes/icarus/_config.yml

plugins:
    animejs: false
```

另外，若需隐藏网页载入进度条，请将`plugins` > `progressbar`的值设置为`false`。

```
themes/icarus/_config.yml

plugins:
    progressbar: false
```

# 常见问题

本文展示了一些经常被提及的Icarus使用问题以及这些问题的解答。 如果此处没有出现你想要求解的问题是，也请阅读[Icarus用户指南](https://blog.zhangruipeng.me/hexo-theme-icarus/tags/Icarus用户指南/)，[Hexo中文文档](https://hexo.io/zh-cn/docs/index.html)， 以及[Icarus GitHub Issues](https://github.com/ppoffice/hexo-theme-icarus/issues?q=)。



## 站点

无法生成我的Hexo站点。/我的站点在生成时出现错误。

Icarus依赖8.3.0或更新版本的Node.js，4.2.0或更新版本的Hexo，以及其他一些在[`themes/icarus/package.json`](https://github.com/ppoffice/hexo-theme-icarus/blob/master/package.json) 文件的`peerDependencies`部分列出的依赖包。 请确保你的站点正确安装了所有依赖。 例外，你之前使用的主题残留下来的依赖有可能会干扰Icarus的正常运行。 请在更换到Icarus主题之前移除它们。

如何改变我的站点的语言？

编辑你站点根目录下的`_config.yml`文件，修改如下设置：

```
_config.yml

- language: en
+ language: <language_name>
```

你可以在`themes/icarus/languages`目录下找到所有可用的翻译。 `_config.yml`的语言名即为这些文件不带后缀的文件名。

## 布局

我如何改变页面的宽度？我如何在特定的文章/页面使用单栏/双栏/三栏布局？

如要改变页面的宽度，你需要编辑`themes/icarus/include/style/responsive.styl`这个样式文件。 这个文件定义了不同屏幕尺寸下的页面容器宽度。 如果你同时想改变主内容栏或挂件栏的宽度，请编辑`themes/icarus/layout/common/widgets.jsx`和`themes/icarus/layout/layout.jsx`。 在文件中找到`is-12`，`is-8-tablet`，和`is-4-widescreen`这样的CSS类名并把它们修改成你想要的数值。

你可以参考[Bulma文档](https://bulma.io/documentation/columns/sizes/)来获取更多关于布局系统的细节。 请确保主内容栏和挂件栏的CSS类名称中的数字在相同屏幕尺寸下相加等于12。 例如，如果你想把挂件栏的宽度修改为`is-3-widescreen`并且你只有一个挂件栏，那么你需要保证你的主内容栏有一个`is-9-widescreen`。

你可以通过从主题配置中移除所有挂件来实现单栏布局。 如果要使用双栏布局，请将所有挂件的`position`设置为`left`或`right`，从而将它们放置按照页面的同一侧。 三栏布局可以通过在页面两侧同时放置挂件来实现。

若要更改特定的文章或页面的布局，请参考[Icarus用户指南 - 主题配置](https://blog.zhangruipeng.me/hexo-theme-icarus/Configuration/icarus用户指南-主题配置/#额外的配置文件与优先级)。

挂件/评论插件/分享按钮...的布局文件在哪里？我如何自定义内置的挂件/评论插件/分享按钮...？

评论插件，捐赠按钮，站内搜索，分享按钮，挂件，以及其他一些插件的布局文件已移至一个单独的Node.js库——[`hexo-component-inferno`](https://github.com/ppoffice/hexo-component-inferno)。 这样，主题开发者可以更好地在不同主题之间复用这些通用组件，并且允许用户更简便地覆盖这些内置组件。

若要自定义这些组件，你可以从`hexo-component-inferno`仓库中拷贝布局文件并把它们放入Icarus布局目录下的的相应目录中。 例如，如果你想要自定义Valine评论插件，你可以从`hexo-component-inferno`仓库中拷贝[`src/view/comment/valine.jsx`](https://github.com/ppoffice/hexo-component-inferno/blob/0.2.4/src/view/comment/valine.jsx)到`themes/icarus/layout/comment`目录中。 然后，修正此文件头部的一些Node.js引用

```
themes/icarus/layout/comment/valine.jsx

- const { cacheComponent } = require('../../util/cache');
+ const { cacheComponent } = require('hexo-component-inferno/lib/util/cache');
```

之后，用`hexo clean`清理你的站点并重新生成HTML文件。

类似的，你可以复制`hexo-component-inferno`仓库中的文件并放入`themes/icarus/source`下的对应目录中来覆盖 主题内置的静态文件。

我对布局文件进行了一些修改。但是当我刷新页面(开启了`hexo server`的情况下)/当我`hexo generate`时 为什么这些修改没有生效？

当你用`hexo server`开启本地Hexo服务器时，这些布局文件会被缓存。 其他情况下临时生成的数据会被缓存在`db.json`或者内存中。 请在`hexo server`或`hexo generate`之前使用`hexo clean`。

## 内容

我的Logo/图片没有正确显示。/我的图片仅在首页显示，却无法在文章页面显示。

请确保你使用了图片的绝对路径。 例如，如果你的图片放在了`source/gallery/`目录下，并且你的站点位于你域名的子目录下，如`https://ppoffice.github.io/hexo-theme-icarus`， 那么你应该用`/hexo-theme-icarus/gallery/<文件名>.<文件扩展名>`来引用你的图片。

你也可以使用`![img]()`这个Hexo标签来省去路径中的网站子目录：`{% img /gallery/. ... %}`。 请参考[Hexo中文文档](https://hexo.io/zh-cn/docs/index.html)来了解详情。

如何为文章添加摘要？如何显示“阅读更多”按钮？

在你的文章中添加``标签。 标签前面的文章内容会被标记为摘要，而其后的内容不会显示在文章列表上。 你也可以在文章的front-matter中设置自定义摘要。

```
some-post.md

title: 一篇文章
date: 2020-01-01
excerpt: 这是一篇关于...
---
# 文章内容...
```

我如何加密文章？

请使用第三方的Hexo插件，比如[hexo-blog-encrypt](https://github.com/MikeCoder/hexo-blog-encrypt)。

我如何像这篇文章一样使用炫酷的页面元素或样式？

你可以在你的Markdown文件中使用HTML。 请参考[Bulma文档](https://bulma.io/documentation/)来了解更多的可选元素和样式。

## 挂件与插件

我的页面上显示红色的警告，上面说某个插件/挂件的配置项没有填写。 我怎样才能除去这些警告？

如果你不想开启某些插件/挂件，你可以把它们从配置中删掉或注释掉。 例如，如果你不想开启任何的评论插件，注释掉这几行：

```
themes/icarus/_config.yml

- comment:
-     type: disqus
-     shortname: 
+ # comment:
+ #     type: disqus
+ #     shortname: 
```