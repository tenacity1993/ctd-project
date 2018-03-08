# Axios

axios是一个基于promise的HTTP库，可以用在浏览器和node.js中。

1. 常用API

   - axios.get
   - axios.post
   - axios.all([p1, p2])   执行多个并发请求

2. API

   - 可以通过向axios传递配置参数来创建请求

     - axios(config)

       ```javascript
       axios({
         method: 'post',
         url: '/user',
         data: {
           firstName: 'Fred',
           lastName: 'Flintstone'
         }
       });
       ```

     - axios(url[, config])

       ```javascript
       // 发送GET请求 （默认的方法）
       axios('/user')
       ```

   - 请求方法的别名

     - axios.request(config)
     - axios.get(url[, config])
     - axios.delete(url[, config])
     - axios.head(url[, config])
     - axios.post(url[, config])
     - axios.put(url[, config])
     - axios.patch(url[, data[, config]])
     - 在使用别名方法时`url`、`method`、`data`这些属性都不必在配置中指定。

   - 并发

     - 处理并发请求的助手函数
       - axios.all(iterable)
       - axios.spread(callback)

   - 创建实例

     可以使用自定义配置新建一个axios实例

     axios.create([config])

     ```javascript
     var instance = axios.create({
       baseURL: 'https://some-domain.com/api',
       timeout: 1000,
       headers: {'X-Custom-Header': 'foobar'}
     })
     ```

     - 请求配置：只有url是必须的，如果没有指定method，将默认使用get方法

       ```javascript
       {
         url: '/user',
         method: 'get',
         // baseURL 将自动加在url前面，除非url是一个绝对URL
         // 它可以通过设置一个baseURL便于为axios实例方法传递相对URL
         baseURL: 'https://some-domain.com/api/',
         // transform 允许在向服务器发送前，修改请求数据
         // 只能用在PUT、POST和PATCH这几个请求方法后面数组中的函数必须返回一个字符串或ArrayBuffer、或Stream
         transformRequest: [function(data, headers) {
           // 可以对data进行任意转换处理
           return data;
         }],
         // transformresponse 允许在传递给then/catch前修改响应数据
         transformResponse: [function(data) {
           return data
         }],
         // headers 是要被发送的自定义请求头
         headers: {'X-Requested-With': 'XMLHttpRequest'}
         // params是即将与请求一起发送的url参数
         // 必须是一个无格式对象（plain Object）或者是URLSearchParams对象
         params: {
         	ID:12345
         },
         // paramsSerializer 是一个可选的负责序列化params的函数
         paramsSerializer: function(params) {
           return Qs.stringify(params, {arrayFormat: 'brackets'})
         },
         // data是作为请求主体被发送的数据 只适用于PUT、POST、和PATCH方法  在没有设置transformRequest时，必须是以下类型之一：
         // string, plain Object, ArrayBuffer, ArrayBufferView, URLSearchParams
         // 浏览器专属： FormData, File, Blob
         // Node 专属：Stream, Buffer
         data: {
           firstName: 'Fred'
         },
         timeout: 1000,
         // withCredentials表明跨域请求时是否需要使用凭证
         withCredentials: false, //default
         // adapter 允许用户自定义处理请求，使得测试更轻松
         adapter: function(config) {
             
         }
         // auth表示应该使用HTTP基础验证，并提供凭据
         // 这将会设置Authorization头部，并覆盖任何已经存在的使用header设置的Authorization自定义头部
         auth: {
           username: 'janedoe',
           password: 's00pers3cret'

         }
         // 可选项为：arraybuffer, blob, document, json, text, stream
         responseType: 'json',  //default
         // xsrfCookieName 是用作xsrf token的值的cookie名称
         xsrfCookieName: 'XSRF-TOKEN', //default
         // xsrfHeaderName 是承载 xsrf token 的值的 HTTP 头的名称
         xsrfHeaderName: 'X-XSRF_TOKEN',
         onUploadProgress: function(progressEvent) {
         },
         onDownloadProgress: function(progressEvent) {  
         },
         // maxContentLength 定义了允许响应内容的最大尺寸
         maxContentLength: 2000,
         // 定义对于给定的HTTP响应状态码是resolve、或reject promise
         validateStatus: function(status) {
           return status>= 200 && status < 300
         }
         // maxRedirects定义在nodejs中follow的最大重定向数目
         maxRedirects: 5 //default
         // 分别在nodejs中用于定义在执行http和https时使用的自定义代理  keepAlive默认没有启用
         httpAgent: new http.Agent({keepAlive: true}),
         httpsAgent: new https.Agent({keepAlive: true}),
         // proxy 定义代理服务器的主机名称和端口
         // auth 表示http基础验证应用于连接代理，并提供凭据
         // 这将会设置一个Proxy-Authorization头，覆写掉已有的通过使用header设置的自定义Proxy-Authorization头
         proxy: {
         host: '127.0.0.1',
         port: 9000,
           auth: {
             username: 'mikeymike',
             password: 'rapunz3l'
           }
         },
         // 指定用于取消请求的cancel token
         cancelToken: new CancelToken(function (cancel){
         })
       }
       ```

   - 响应结构

     某个请求的响应包含以下信息：

     ```javascript
     {
       // `data` is the response that was provided by the server
       data: {},

       // `status` is the HTTP status code from the server response
       status: 200,

       // `statusText` is the HTTP status message from the server response
       statusText: 'OK',

       // `headers` the headers that the server responded with
       // All header names are lower cased
       headers: {},

       // `config` is the config that was provided to `axios` for the request
       config: {},

       // `request` is the request that generated this response
       // It is the last ClientRequest instance in node.js (in redirects)
       // and an XMLHttpRequest instance the browser
       request: {}
     }
     ```

   - 配置的默认值

     - 全局默认值

       ```javascript
       axios.defaults.baseURL = 'https://api.example.com';
       axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
       axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
       ```

     - 自定义实例默认值

       ```javascript
       // 创建实例时设置配置的默认值
       var instance = axios.create({
         baseURL: 'https://api.example.com'
       });

       // 在实例已创建后修改默认值
       instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
       ```

     - 配置的优先顺序

       配置会以一个优先顺序进行合并，依次是在lib/defaults.js找到的库的默认值，然后是实例的defaults属性，最后是请求的config参数。后者优于前者。

       ```javascript
       // 使用由库提供的配置的默认值来创建实例
       // 此时超时配置的默认值是 `0`
       var instance = axios.create();

       // 覆写库的超时默认值
       // 现在，在超时前，所有请求都会等待 2.5 秒
       instance.defaults.timeout = 2500;

       // 为已知需要花费很长时间的请求覆写超时设置
       instance.get('/longRequest', {
         timeout: 5000
       });
       ```

   - 拦截器

     在请求或响应被then或catch处理前拦截他们。

     ```javascript
     // 添加请求拦截器
     axios.interceptors.request.use(function (config) {
         // 在发送请求之前做些什么
         return config;
       }, function (error) {
         // 对请求错误做些什么
         return Promise.reject(error);
       });

     // 添加响应拦截器
     axios.interceptors.response.use(function (response) {
         // 对响应数据做点什么
         return response;
       }, function (error) {
         // 对响应错误做点什么
         return Promise.reject(error);
       });
     ```

     可以为自定义axios实例添加拦截器

     ```javascript
     var instance = axios.create();
     instance.interceptors.request.use(function () {/*...*/});
     ```

3. 关于各种请求方法

   - get

     用于获取资源，参数在url以明文方式存在，资源会发生缓存， 幂等

   - head

     类似于get请求，只用于获取报头，响应中没有具体内容

   - post

     向指定资源提交数据请求，可以创建或更新服务器资源，不缓存资源， 非幂等

   - put

     提交请求，可以更新或创建资源  幂等

   - delete

     请求服务器删除指定页面， 幂等

   - trace

     回显服务器收到的请求

   - connect

     HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器

   - options

     允许客户端查看服务器的性能

   - patch

     用来更新局部资源， 非幂等

     ​