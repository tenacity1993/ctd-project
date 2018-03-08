# window.performance

### MDN Definition

> Performance接口提供了获取当前页面性能相关信息的一种方式。它是高分辨率时间API的一部分，同时经由Performance Timeline API、Navigation Timing API、User Timing API和Resource Timing API而增强。
>
> 该类型的对象可以通过调用Window.performance这一只读属性来获得。

### Properties

==performance 接口不继承任何属性==

1. performance.navigation 

   提供关于操作的有用的上下文（包括在列举在timing中的时间），包括页面是load还是refresh，发生了多少次重定向等等。

2. performance.timing

   performanceTiming对象包含延迟相关的性能信息

3. performance.memory

   Chrome中添加的**非标准扩展**，这个属性提供了获取基本内存使用信息的对象，不应该使用这个非标准对象

4. performance.timeOrigin

   返回性能测量开始时间的高分辨率时间戳

5. onresourcetimingbufferfull（Event handlers）

   当触发resourcetimingbufferfull事件后的回调。

### Methods

==performance 接口不继承任何方法==

- performance.clearMarks()

  从浏览器的性能输入缓冲区中删除给定的标记

- performance.clearMeasures()

  从浏览器的性能输入缓冲区中删除给定的度量

- performance.clearResourceTimings()

  从浏览器的性能数据缓冲区中删除所有的entryType为resource的所有性能条目

- performance.getEntries()

  基于给定过滤器返回performanceEntry对象的列表

- performance.getEntriesByName()

  基于给定名字和条目类型返回performanceEntry对象的列表

- performance.getEntriesByType()

  基于给定条目类型返回performanceEntry对象的列表

- performance.mark()

  使用给定name在浏览器的输入性能缓冲区中创建一个时间戳

- performance.measure()

  在两个指定mark（start， end）中间的浏览器的性能输入缓冲区创建一个具名时间戳

- performance.now()

  返回一个DOMHighResTimeStamp，返回的值表示从时间起点 (PerformanceTiming.navigationStart属性) 以来经过的时间。

- performance.setResourceTimingBufferSize()

  将浏览器的资源时序缓冲区的大小设置为指定数量的resource类型性能条目对象

- performance.toJSON()

  返回performace对应的json对象。

  ​

  来源：

  [Performance API]: https://developer.mozilla.org/en-US/docs/Web/API/Performance

---

### 监控网页与程序性能

####window.performance对象

![Snip20180129_4](/Users/ctd/Pictures/Snip20180129_4.png)

#####timing中的属性

请求发出的整个过程中各个环节的时间顺序：

![performance](/Users/ctd/Pictures/performance.png)

```javascript
// 获取 performance 数据
var performance = {  
    // memory 是非标准属性，只在 Chrome 有
    // 财富问题：我有多少内存
    memory: {
        usedJSHeapSize:  16100000, // JS 对象（包括V8引擎内部对象）占用的内存，一定小于 totalJSHeapSize
        totalJSHeapSize: 35100000, // 可使用的内存
        jsHeapSizeLimit: 793000000 // 内存大小限制
    },
 
    //  哲学问题：我从哪里来？
    navigation: {
        redirectCount: 0, // 如果有重定向的话，页面通过几次重定向跳转而来
        type: 0           // 0   即 TYPE_NAVIGATENEXT 正常进入的页面（非刷新、非重定向等）
                          // 1   即 TYPE_RELOAD       通过 window.location.reload() 刷新的页面
                          // 2   即 TYPE_BACK_FORWARD 通过浏览器的前进后退按钮进入的页面（历史记录）
                          // 255 即 TYPE_UNDEFINED    非以上方式进入的页面
    },
 
    timing: {
        // 在同一个浏览器上下文中，前一个网页（与当前页面不一定同域）unload 的时间戳，如果无前一个网页 unload ，则与 fetchStart 值相等
        navigationStart: 1441112691935,
 
        // 前一个网页（与当前页面同域）unload 的时间戳，如果无前一个网页 unload 或者前一个网页与当前页面不同域，则值为 0
        unloadEventStart: 0,
 
        // 和 unloadEventStart 相对应，返回前一个网页 unload 事件绑定的回调函数执行完毕的时间戳
        unloadEventEnd: 0,
 
        // 第一个 HTTP 重定向发生时的时间。有跳转且是同域名内的重定向才算，否则值为 0 
        redirectStart: 0,
 
        // 最后一个 HTTP 重定向完成时的时间。有跳转且是同域名内部的重定向才算，否则值为 0 
        redirectEnd: 0,
 
        // 浏览器准备好使用 HTTP 请求抓取文档的时间，这发生在检查本地缓存之前
        fetchStart: 1441112692155,
 
        // DNS 域名查询开始的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等
        domainLookupStart: 1441112692155,
 
        // DNS 域名查询完成的时间，如果使用了本地缓存（即无 DNS 查询）或持久连接，则与 fetchStart 值相等
        domainLookupEnd: 1441112692155,
 
        // HTTP（TCP） 开始建立连接的时间，如果是持久连接，则与 fetchStart 值相等
        // 注意如果在传输层发生了错误且重新建立连接，则这里显示的是新建立的连接开始的时间
        connectStart: 1441112692155,
 
        // HTTP（TCP） 完成建立连接的时间（完成握手），如果是持久连接，则与 fetchStart 值相等
        // 注意如果在传输层发生了错误且重新建立连接，则这里显示的是新建立的连接完成的时间
        // 注意这里握手结束，包括安全连接建立完成、SOCKS 授权通过
        connectEnd: 1441112692155,
 
        // HTTPS 连接开始的时间，如果不是安全连接，则值为 0
        secureConnectionStart: 0,
 
        // HTTP 请求读取真实文档开始的时间（完成建立连接），包括从本地读取缓存
        // 连接错误重连时，这里显示的也是新建立连接的时间
        requestStart: 1441112692158,
 
        // HTTP 开始接收响应的时间（获取到第一个字节），包括从本地读取缓存
        responseStart: 1441112692686,
 
        // HTTP 响应全部接收完成的时间（获取到最后一个字节），包括从本地读取缓存
        responseEnd: 1441112692687,
 
        // 开始解析渲染 DOM 树的时间，此时 Document.readyState 变为 loading，并将抛出 readystatechange 相关事件
        domLoading: 1441112692690,
 
        // 完成解析 DOM 树的时间，Document.readyState 变为 interactive，并将抛出 readystatechange 相关事件
        // 注意只是 DOM 树解析完成，这时候并没有开始加载网页内的资源
        domInteractive: 1441112693093,
 
        // DOM 解析完成后，网页内资源加载开始的时间
        // 在 DOMContentLoaded 事件抛出前发生
        domContentLoadedEventStart: 1441112693093,
 
        // DOM 解析完成后，网页内资源加载完成的时间（如 JS 脚本加载执行完毕）
        domContentLoadedEventEnd: 1441112693101,
 
        // DOM 树解析完成，且资源也准备就绪的时间，Document.readyState 变为 complete，并将抛出 readystatechange 相关事件
        domComplete: 1441112693214,
 
        // load 事件发送给文档，也即 load 回调函数开始执行的时间
        // 注意如果没有绑定 load 事件，值为 0
        loadEventStart: 1441112693214,
 
        // load 事件的回调函数执行完毕的时间
        loadEventEnd: 1441112693215
 
        // 字母顺序
        // connectEnd: 1441112692155,
        // connectStart: 1441112692155,
        // domComplete: 1441112693214,
        // domContentLoadedEventEnd: 1441112693101,
        // domContentLoadedEventStart: 1441112693093,
        // domInteractive: 1441112693093,
        // domLoading: 1441112692690,
        // domainLookupEnd: 1441112692155,
        // domainLookupStart: 1441112692155,
        // fetchStart: 1441112692155,
        // loadEventEnd: 1441112693215,
        // loadEventStart: 1441112693214,
        // navigationStart: 1441112691935,
        // redirectEnd: 0,
        // redirectStart: 0,
        // requestStart: 1441112692158,
        // responseEnd: 1441112692687,
        // responseStart: 1441112692686,
        // secureConnectionStart: 0,
        // unloadEventEnd: 0,
        // unloadEventStart: 0
    }
};

```

#### 使用 performance.timing 信息简单计算出网页性能数据

```javascript
// 计算加载时间
function getPerformanceTiming () {  
    var performance = window.performance;
 
    if (!performance) {
        // 当前浏览器不支持
        console.log('你的浏览器不支持 performance 接口');
        return;
    }
 
    var t = performance.timing;
    var times = {};
 
    //【重要】页面加载完成的时间
    //【原因】这几乎代表了用户等待页面可用的时间
    times.loadPage = t.loadEventEnd - t.navigationStart;
 
    //【重要】解析 DOM 树结构的时间
    //【原因】反省下你的 DOM 树嵌套是不是太多了！
    times.domReady = t.domComplete - t.responseEnd;
 
    //【重要】重定向的时间
    //【原因】拒绝重定向！比如，http://example.com/ 就不该写成 http://example.com
    times.redirect = t.redirectEnd - t.redirectStart;
 
    //【重要】DNS 查询时间
    //【原因】DNS 预加载做了么？页面内是不是使用了太多不同的域名导致域名查询的时间太长？
    // 可使用 HTML5 Prefetch 预查询 DNS ，见：[HTML5 prefetch](http://segmentfault.com/a/1190000000633364)            
    times.lookupDomain = t.domainLookupEnd - t.domainLookupStart;
 
    //【重要】读取页面第一个字节的时间
    //【原因】这可以理解为用户拿到你的资源占用的时间，加异地机房了么，加CDN 处理了么？加带宽了么？加 CPU 运算速度了么？
    // TTFB 即 Time To First Byte 的意思
    // 维基百科：https://en.wikipedia.org/wiki/Time_To_First_Byte
    times.ttfb = t.responseStart - t.navigationStart;
 
    //【重要】内容加载完成的时间
    //【原因】页面内容经过 gzip 压缩了么，静态资源 css/js 等压缩了么？
    times.request = t.responseEnd - t.requestStart;
 
    //【重要】执行 onload 回调函数的时间
    //【原因】是否太多不必要的操作都放到 onload 回调函数里执行了，考虑过延迟加载、按需加载的策略么？
    times.loadEvent = t.loadEventEnd - t.loadEventStart;
 
    // DNS 缓存时间
    times.appcache = t.domainLookupStart - t.fetchStart;
 
    // 卸载页面的时间
    times.unloadEvent = t.unloadEventEnd - t.unloadEventStart;
 
    // TCP 建立连接完成握手的时间
    times.connect = t.connectEnd - t.connectStart;
 
    return times;
}

```

[给网站结尾加上反斜线]: http://kayosite.com/add-backslash-at-end-of-url.html

> 于是在网址末尾加了反斜杠是能加快网站载入，因为网址末尾加了反斜杠会直接告知浏览器现在指向的是一个目录，浏览器就能直接读取该目录下如index或home等默认文件。而没有加上反斜杠时浏览器首先会尝试读取根目录下的一个文件，如果没有该文件再查找一个与该文件同名的目录，最后才读取目录下的默认文件。这样一来加上反斜杠就会加快网站加载速度。对于网站所在的服务器，网址没有加上反斜杠会给服务器增加一个查找是否有同名文件的过程，这明显会增加服务器的负担，当然这个影响并不会很大，但如果你的网站的直接流量很大，那么给url末尾加上反斜杠便能较大的减轻服务器的负担了。
>
> 当然给网址末尾加上反斜杠还有其他的好处：
>
>  
>
> 1.在seo方面考虑，习惯性的给自己网站的网址末尾加上反斜杠能避免重复内容。正如域名中是否带”www”的问题，url末尾是否有反斜杠也会造成重复内容的问题，这对于网站的seo无疑是不利的，要知道，重复内容绝对是seo的大忌。
>
> 2.因为服务器对url不能正确解析，有可能会出现404错误，习惯地给网址末尾加上反斜杠则可以避免这种情况。
>
> 总的来说给网址末尾加上反斜杠对对网站建设者是有很大好处的，这可以说是网站建设者应该养成的一个好习惯。

[有了浏览器缓存，为何还需要预加载？]: https://segmentfault.com/a/1190000000633364

#### 使用performance.getEntries() 获取所有资源请求的时间数据

打开百度的时候获取的结果

这个函数返回一个数组，包含了页面中所有的HTTP请求， 注意 HTTP 请求有可能命中本地缓存，所以请求响应的间隔将非常短。可以看到，与 performance.timing 对比： 没有与 DOM 相关的属性：

![Snip20180130_22](/Users/ctd/Pictures/Snip20180130_22.png)

其中一项：

![Snip20180130_24](/Users/ctd/Pictures/Snip20180130_24.png)

新增属性：

- name
- entryType
- initiatorType
- duration

```javascript
var entry = {
  // 资源名称，也是资源的绝对路径
  name: "https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/css/super_min_67699457.css",
  // 资源类型
  entryType: "resource",
  // 谁发起的请求
  // link 即 <link> 标签
  // script 即 <script>
  // redirect 即重定向
  initiatorType: "link",
  
  connectEnd: 0,
  connectStart: 0,
  
  decodedBodySize: 0,
  
  domainLookupEnd: 0,
  domainLookupStart: 0,
  
  duration: 194.67499999999995,
  
  encodedBodySize: 0,
  
  fetchStart: 308.83500000000004,
  
  nextHopProtocol: "http/1.1",
  
  redirectEnd: 0,
  redirectStart: 0,
  
  requestStart: 0,
  
  responseEnd: 503.51,
  responseStart: 0,
  
  secureConnectionStart: 0,
  startTime: 308.83500000000004,
  transferSize: 0,
  workerStart: 0
}
```

可以获取资源加载时间

```javascript
// 计算加载时间
function getEntryTiming (entry) {  
    var t = entry;
    var times = {};
 
    // 重定向的时间
    times.redirect = t.redirectEnd - t.redirectStart;
 
    // DNS 查询时间
    times.lookupDomain = t.domainLookupEnd - t.domainLookupStart;
 
    // 内容加载完成的时间
    times.request = t.responseEnd - t.requestStart;
 
    // TCP 建立连接完成握手的时间
    times.connect = t.connectEnd - t.connectStart;
 
    // 挂载 entry 返回
    times.name = entry.name;
    times.entryType = entry.entryType;
    times.initiatorType = entry.initiatorType;
    times.duration = entry.duration;
 
    return times;
}

// test
// var entries = window.performance.getEntries();
// entries.forEach(function (entry) {
//     var times = getEntryTiming(entry);
//     console.log(times);
// });
```

#### 使用 performance.now() 精确计算程序执行时间

计算程序执行时间可以有两种方式

- 使用Date对象的方法或属性

  ```javascript
  var start = new Date().getTime()
  func()
  var now = new Date().getTIme()
  var latency = now - start
  console.log('func运行时间：' + latency)

  var start = Date.now()
  func()
  var now = Date.now()
  var latency = now - start
  console.log('func运行时间：' + latency)
  ```

- 使用performance进行计算

  - 使用performance.now	

    ```javascript
    var start = window.performance.now()
    func()
    var now = window.performance.now()
    var latency = now - start
    console.log('func运行时间：' + latency)
    ```

  - 使用performance.mark

    ```javascript
    function randomFunc (n) {  
        if (!n) {
            // 生成一个随机数
            n = ~~(Math.random() * 10000); // 功能类似于Math.floor()
        }
        var nameStart = 'markStart' + n; 
        var nameEnd   = 'markEnd' + n; 
        // 函数执行前做个标记
        window.performance.mark(nameStart);
     
        for (var i = 0; i < n; i++) {
            // do nothing
        }
     
        // 函数执行后再做个标记
        window.performance.mark(nameEnd);
     
        // 然后测量这个两个标记间的时间距离，并保存起来
        var name = 'measureRandomFunc' + n;
        window.performance.measure(name, nameStart, nameEnd);
    }

    // 执行三次看看
    randomFunc();  
    randomFunc();  
    // 指定一个名字
    randomFunc(888); 

    // 看下保存起来的标记 mark
    var marks = window.performance.getEntriesByType('mark');  
    console.log(marks);  

    // 看下保存起来的测量 measure
    var measure = window.performance.getEntriesByType('measure');  
    console.log(measure);

    // 清除指定标记
    window.performance.clearMarks('markStart888');  
    // 清除所有标记
    window.performance.clearMarks();
     
    // 清除指定测量
    window.performance.clearMeasures('measureRandomFunc');  
    // 清除所有测量
    window.performance.clearMeasures();  
    ```

    [使用～～还是使用Math.floor()]: http://rocha.la/JavaScript-bitwise-operators-in-practice

    **Date和Performance的区别**

    - 单位
      - Date.now()以及其他方法单位为毫秒
      - Performace.now()以及其他方法是毫秒的千分之一
    - 精度
      - Date使用中会受到程序的影响，可能会因为程序阻塞而不准确
      - performance.now() 的时间是以恒定速率递增的，不受系统时间的影响
    - 功能
      - Date只能用于测量代码运行过程中的时间
      - Performance可以获取后台事件的时间进度






##### 参考资料

[初探 performance – 监控网页与程序性能]: http://www.alloyteam.com/2015/09/explore-performance/
[Performance API]: http://javascript.ruanyifeng.com/bom/performance.html


