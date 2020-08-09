### 1.什么是axios？
Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

### 2.axios特性：
- 从浏览器中创建 XMLHttpRequests
- 从 node.js 创建 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 XSRF

### 3.执行get请求：

    axios({
        method: 'get',
        url: '',
        params: {}
    }).then(res => { do something })
    //也可以简写为：
    axios.get(url, params).then(res => { do something })

- params会出现在请求头中的querry string parameters中，并且会出现在浏览器的地址栏中，即会拼接到url中。

### 4.执行post请求：

    axios({
        method: 'post',
        url: '',
        data: {}
    }).then(res => { do something })
    //也可以简写为：
    axios.post(url, data).then(res => { do something })

post的请求头中会有一个content-type，该字段有两个值，一个为**form-data**，一般用于表单提交（文件上传，图片上传等等）；另一个是application/json，即传递的是json数据。

1. Content-Type: application/json：

    let data = {"code":"1234","name":"yyyy"};
    axios.post(`${this.$url}/test/testRequest`,data)
    .then(res=>{
        console.log('res=>',res);            
    })

2. Content-Type: multipart/form-data

    let data = new FormData();
    data.append('code','1234');
    data.append('name','yyyy');
    axios.post(`${this.$url}/test/testRequest`,data)
    .then(res=>{
        console.log('res=>',res);            
    })
    
### 5.执行delete操作：

    axios({
        method: 'delete',
        url: '',
        params: {}
    }).then(res => { do something })
    
- params会使参数拼接在url中，如果我们不想使之出现在url中，我们只需要将params替换为data。

### 6.执行并发请求：
同时进行多个请求，并统一处理返回值，如果在某些场景中我们需要同时依赖两个接口返回的数据，那么我们可以使用并发请求：

    axios.all([  // 这里的参数是一个数组，里面包含了axios请求
      axios.get('url1'), // 请求的先后顺序就是代码中的顺序
      axios.get('url2')
    ]).then(
      axios.spread((res1, res2) => { // spread用来分割返回值
        console.log(res1, res2)
      }
    ) 

### 7.创建axios实例：

    // axios实例化
    let api = axios.create({
        baseURL: 'localhost:8080', // 请求的域名
        timeout: 1000, // 设置请求的超时时长，超过时长报401超时
        method: 'get,post',
        headers: {  // 设置请求头
            token: ''  
        }, 
        params: {},
        data: {}
    })
    // 使用axios实例
    api.get('/data.json')
    
### 8.配置默认值
1. 全局的 axios 默认值：


    axios.defaults.baseURL = 'https://api.example.com';
    axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
    axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';

2. 自定义实例默认值：


    //在创建实例时设置默认配置
    const instance = axios.create({
      baseURL: 'https://api.example.com'
    });
     
    //实例创建后更改默认值
    instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;

### 9.axios配置
- 全局配置，例如axios.defaults.timeout = 1000
- 实例配置, 在创建axios实例时传入的配置，如果不传实例配置就会使用全局配置
- 请求配置，在使用axios请求时，可以单独传入新的配置

三种配置的优先级为：请求配置 > 实例配置 > 全局配置

### 10.拦截器
- 请求拦截器：


    axios.interceptors.request.use(
      config => {}, // 在发送请求前的一些处理逻辑
      err => {} // 在请求错误后的处理
    )

- 响应拦截器：


    axios.interceptors.response.use(
      res => { return res }, // 请求成功后对响应数据做一定的处理
      err => { return Promis.reject(err)} // 在响应错误后的处理，可以用catch捕捉
    )

- 移除拦截器：


    const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
    axios.interceptors.request.eject(myInterceptor);

### 11.axios错误处理

    axios.get('PATH')
      .catch(function (error) {
        if (error.response) {
            
        } else if (error.request) {
            
        } else {
            
        }
    });

### 12.axios取消Http请求
使用 cancel token 取消请求：

    let api = axios.create({})  // 实例化axios
    let source = axios.CancelToken.source() // 实例化一个source对象
     
    api.get('/data.json', { 
        cancelToken: source.token // 请求时携带cancleToken
    }).then(callback).catch(err => {
        console.log(err)
    })
     
    source.cancel('message') // 调用source的cancel方法取消http请求，并将message以error
    的形式返回，然后就取消了http请求，并进入到该请求的catch代码块，进行错误处理。