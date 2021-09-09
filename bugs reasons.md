1. 错误：DELETE 请求变成了 GET 请求  
   原因：配置 axios 时，method 错写为 name
2. 错误：uncaught promise  
   原因：导入单个功能接口时，没有加 `{}`
3. 错误：请求响应异常  
   原因：必然是请求配置出错。例如，多写了某非必需项，非必需项应在需要的功能调用时写入，而不应该直接写在 data 中。
4. 错误：Uncaught (in promise) TypeError: Cannot read property 'data' of undefined  
   原因：此报错来自于请求前后的拦截器。就拉勾教育平台项目来说，请求或其上层封装某函数没 return。详见：[Uncaught (in promise) TypeError: Cannot read property 'data' of undefined_十眠的博客-CSDN博客](https://blog.csdn.net/qq_38983511/article/details/103674826)
5. 

