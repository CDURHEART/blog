---
title: 有关Github
tags: Github
---

> 我准备好在Github大干一场了，本篇记录并分享有关我Github的使用。

<!--more-->
## 1. 相关名词

[Github词汇表](https://docs.github.com/cn/get-started/quickstart/github-glossary){:target="_blank"}{:.success}

## 2. 限定搜索
### 2.1 搜索限定

| 标识    | 描述        |
| ----------- | ----------- |
| `in:name`   | 仓库名称中，[hello in:name](https://github.com/search?q=hello+in%3Aname){:target="_blank"} 匹配名带hello的仓库 |
| `in:description`  | 摘要中，[hello in:description](https://github.com/search?q=hello+in%3Adescription){:target="_blank"} 匹配摘要有hello的仓库  |
| `in:readme`   | README.md中，[hello in:readme](https://github.com/search?q=hello+in%3Areadme){:target="_blank"} 匹配README中有hello的仓库  |
| `stars:n`   | star量，[stars:1000](https://github.com/search?q=stars%3A1000){:target="_blank"} 匹配star数为1k的仓库 |
| `forks:n`  | forks量，[forks:1000](https://github.com/search?q=forks%3A1000){:target="_blank"} 匹配forks数为1k的仓库 |
| `followers:n`   | 关注者量，[followers:1000](https://github.com/search?q=followers%3A1000){:target="_blank"} 匹配followers数为1k的仓库 |
| `size:n`  | [function size:>10000 language:python](https://github.com/search?q=function+size%3A%3E10000+language%3Apython&type=Code){:target="_blank"} 匹配含有 "function" 字样、以 Python 编写、位于大于 10 KB 的文件中的代码。 |
| `created:YYYY-MM-DD`   | 创建时间，[cats created:>2016-04-29](https://github.com/search?utf8=%E2%9C%93&q=cats+created%3A%3E2016-04-29&type=Issues){:target="_blank"} 匹配含有 "cats" 字样、在 2016 年 4 月 29 日之后创建的议题。 |
| `pushed:YYYY-MM-DD`   | 更新时间，[cats pushed:<2012-07-05](https://github.com/search?q=cats+pushed%3A%3C2012-07-05&type=Code&utf8=%E2%9C%93){:target="_blank"} 匹配在 2012 年 7 月 5 日之前推送的仓库中含有 "cats" 字样的代码。 |
| `language:xxx`   | 编程语言，[language:python](https://github.com/search?q=language%3Apython){:target="_blank"} 匹配python语言仓库 |
| `user:xxx`   | 用户名下，[user:cdurheart](https://github.com/search?q=user%3Acdurheart&type=repositories){:target="_blank"} 匹配我名下的仓库 |
| `org:xxx`   | 组织名下，[org:microsoft](https://github.com/search?q=org%3Amicrosoft&type=repositories){:target="_blank"} 匹配microsoft名下的仓库 |
| `topic:xxx`  | topic，[topic:ai](https://github.com/search?q=topic%3Aai){:target="_blank"} 匹配含有ai topic的仓库 |
| `topics:n`   | topic数量，[topics:2](https://github.com/search?q=topics%3A2){:target="_blank"} 匹配有2个topic的仓库 |
| `license:xxx`   | 开源协议，[license:GPL](https://github.com/search?q=license%3AGPL){:target="_blank"} 匹配含有GPL协议的仓库 |
| `NOT` | [hello NOT world](https://github.com/search?q=hello+NOT+world&type=Repositories){:target="_blank"} 匹配含有 "hello" 字样但不含有 "world" 字样的仓库。 |
| `mentions:xxx` | 提及，[mentions:defunkt](https://github.com/search?q=mentions%3Adefunkt&type=Issues){:target="_blank"} 匹配提及 @defunkt 的议题 |
| `-QUALIFIER` | [cats stars:>10 -language:javascript](https://github.com/search?q=cats+stars%3A%3E10+-language%3Ajavascript&type=Repositories){:target="_blank"} 匹配含有 "cats" 字样、有超过 10 个星号但并非以 JavaScript 编写的仓库。 |

### 2.2 区间范围限定
针对数字或者日期等可以构成区间范围的检索，如下：

| 标识    | 描述        |
| ----------- | ----------- |
| `>n`      | [cats stars:>1000](https://github.com/search?utf8=%E2%9C%93&q=cats+stars%3A%3E1000&type=Repositories){:target="_blank"} 匹配含有 "cats" 字样、星标超过 1k 的仓库。 |
| `>=n`   | [cats topics:>=5](https://github.com/search?utf8=%E2%9C%93&q=cats+topics%3A%3E%3D5&type=Repositories){:target="_blank"} 匹配含有 "cats" 字样、有 5 个或更多主题的仓库。 |
| `<n`   | [cats size:<10000](https://github.com/search?utf8=%E2%9C%93&q=cats+size%3A%3C10000&type=Code){:target="_blank"} 匹配小于 10 KB 的文件中含有 "cats" 字样的代码。 |
| `<=n`   | [cats stars:<=50](https://github.com/search?utf8=%E2%9C%93&q=cats+stars%3A%3C%3D50&type=Repositories){:target="_blank"} 匹配含有 "cats" 字样、星标不超过 50 个的仓库。 |
| `n..*` | 等同于 >=n |
| `*..n` | 等同于 <=n |
| `n..n` | 限定在一个区间内，[cats pushed:2016-04-30..2016-07-04](https://github.com/search?utf8=%E2%9C%93&q=cats+pushed%3A2016-04-30..2016-07-04&type=Repositories){:target="_blank"} 匹配含有 "cats" 字样、在 2016 年 4 月末到 7 月之间推送的仓库。 |

## 3. Github有的时候访问不了
出于某种原因，在windows环境下，github访问不 liǎo le，可以尝试设置一下hosts，看是否能解决：

1. 需要在windows `C:\Windows\System32\drivers\etc\hosts` 中添加一些内容：

    ``` bash
    # github
    140.82.114.4 github.com

    185.199.108.153 github.github.io
    185.199.109.153 github.github.io
    185.199.110.153 github.github.io
    185.199.111.153 github.github.io

    199.232.69.194 github.global.ssl.fastly.net

    185.199.108.153 assets-cdn.github.com
    185.199.109.153 assets-cdn.github.com
    185.199.110.153 assets-cdn.github.com
    185.199.111.153 assets-cdn.github.com
    ```

2. 去 [https://www.ipaddress.com/](https://www.ipaddress.com/){:target="_blank"} 逐个找到对应域名的地址。
3. 将找到的ip替换掉步骤1的各域名对应的ip，保存。<br/>然后刷新系统的DNS缓存，在windows下，可以使用以下命令：

    ``` bash
    ipconfig /flushdns
    ```
4. 试一下，应该可以正常访问github了。

## 4. 其他
- [Github文档](https://docs.github.com/cn/get-started){:target="_blank"}
- [Github快捷键](https://docs.github.com/cn/get-started/using-github/keyboard-shortcuts){:target="_blank"}
- [Github搜索语法](https://docs.github.com/cn/search-github/getting-started-with-searching-on-github/understanding-the-search-syntax){:target="_blank"}
