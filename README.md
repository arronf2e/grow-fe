### Collections


#### HTML(5)



#### CSS(3)



#### JS




#### jQuery




#### nodejs




#### 性能优化

1. [这些奇技浮巧，助你优化前端应用性能(作者：phodal)](http://weibo.com/ttarticle/p/show?id=2309404092566151783294)
2. [回流与重绘：CSS性能让JavaScript变慢？(作者：张鑫旭)](http://www.zhangxinxu.com/wordpress/2010/01/%E5%9B%9E%E6%B5%81%E4%B8%8E%E9%87%8D%E7%BB%98%EF%BC%9Acss%E6%80%A7%E8%83%BD%E8%AE%A9javascript%E5%8F%98%E6%85%A2%EF%BC%9F/)


#### 杂谈

1. [如何处理好前后端分离的 API 问题(作者：phodal)](https://zhuanlan.zhihu.com/p/26385106)


#### Q&A
1. 文章中写了优化的关键点是优化几个关键节点，那能不能针对这几个关键节点给出实验方案，让读者能够亲自实践这几个优化节点在优化之前和之后带来的实验效果呢？

     - 服务器端通过压测，ab或wrk
    - 浏览器端，通过chrome devtools或yslow这样的工具测试

    1） App Cache

    - 浏览器会优先检查本地缓存
    - 然后是Etag，etag如果状态码是302，依然走本地
    - 实在不行，才走服务器

    能不走服务器就尽量不走服务器。大家需要做的是nginx或node自己加etag、cache-  control等。

    2）DNS

    - 缓存，减少查询时间
    - 开发代理工具、提高工作效率

    一般node里有dns cache模块的，根据自已的应用部署结构来指定策略。当然也可以使用dnspod这种专业的工具来处理。说了大家也不一定会去做的。开发阶段感觉不到差异，服务器端一般又不交给前端来处理。所以还是前后端配合的。

    3）TCP

    能做不多，简单说，传输的内容越少越好，所以，各种压缩，tree-shaking、DCE(dead code elimination)等

    这部分基本就是前端工程化要做的事儿。自己用grunt或gulp或webpack玩一遍才能懂得。这个的难点在你如何找到好的示例项目。其实大家对github的搜索要熟练，比如输入什么关键词，按什么排序，这个是要自己体会的。有一个词儿叫搜商

    4）HTTP协议

    此处主要是学会压测，这是对web server最常见的技巧。
    
2. 请问抓包工具和前端性能监控工具有推荐么？

    抓包工具工具，常用的charles\fiddler\wireshark等，有时候也自己用Node.js写Proxy，http代理几行代码就能搞定，https要复杂一些，但编写的过程能够对协议有更深的理解，对那些抓包软件用法理解也更好。

    前端性能监控工具没有特别好，大部分都是优化建议类的。基本上做性能监控，都是采用window.performance.timing，低端不支持，某些浏览器还会出错，比如https://bugs.webkit.org/show_bug.cgi?id=168055

    > Bug 168055 Summary: [Navigation Timing] requestStart, responseStart before navigationStart

    自己搭建吧，封装window.performance.timing，使用Node.js或java写采集数据，最好是写入到hive中，最后通过[grafana](https://github.com/grafana/grafana)作展示
    
3. 我是个前端新人，我想问问狼叔，公司招应届生或者实习生，在后端方面要求掌握到什么程度呢，或者说对node 需要了解到什么程度？

    见我的知乎回答吧 https://www.zhihu.com/question/56268872/answer/149457260
    
    补充一句:只看技能没人品的人，千万别招，白脸狼

    做技术总监时上指下派只要看好技术能力和态度即可，做CTO时要考虑团队文化，人品和能否在公司长留，所以不同的人面试要看的点是不一样的，我曾面过很多Node.js程序员，也见过很多面试题，汇总一下，大致有以下9个点：

    - 基本的Node.js几个特性，比如事件驱动、非阻塞I/O、Stream等
    - 异步流程控制相关，Promise是必问的
    - 掌握1种以上Web框架，比如Express、Koa、Thinkjs、Restfy、Hapi等，会问遇到过哪些问题、以及前端优化等常识
    - 数据库相关，尤其是SQL、缓存、Mongodb等
    - 对于常见Node.js模块、工具的使用，观察一个人是否爱学习、折腾
    - 是否熟悉linux，是否独立部署过服务器，有+分
    - js语法和es6、es7，延伸CoffeeScript、TypeScript等，看看你是否关注新技术，有+分
    - 对前端是否了解，有+分
    - 是否参与过或写过开源项目，技术博客、有+分

4. 关于浏览器性能的优化主要是针对初次渲染的优化，从解析到渲染再到展示，但是似乎没有对回流或者重绘做介绍。希望狼叔能介绍一下如何尽可能避免回流或者重绘，或者说，在页面加载好之后，在用户与页面元素交互的过程中，如何优化 UI 的响应、反馈速度以及流畅度?

    - 回流或者重绘，现代的css框架和工具都有优化，但是还是不够的。注意样式写法技巧，  基本功还是很重要的，另外就是通过timeline和profile来识别点。
    - 优化ui响应，根据体积和交互做以下2点基本就够了 1）按需加载（模块加载器都可以），首屏加载哪些模块，甚至像vuessr做预载 2）分块加载，如果特别复杂，可以上bigpipe这样的
    - 利用webpack或gulp这样的工具再优化，比如压缩，无用代码删除等等

