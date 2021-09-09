# P4M4Live：CI/CD、Vue 3

## H1：CI/CD

CI：Continuous Integration 持续集成  
CD：Continuous Delivery 持续交付

它们还伴随着持续测试 (Continuous Testing) 和持续部署 (Continuous Deployment)

实质上是整个应用开发阶段的自动化解决方案

以 GitHub Actions 为例



创建 token，配置权限，得到令牌密匙

在仓库内设置 Secrets，token 密匙为值。



项目中

.github > workflows > main.yml 一个 JSON 文件，设定了工作流。

其中需要 Secrets



push 后，GitHub 的仓库中可以找到 Actions，

## H2：Vue 3

#### Vite

更快的打包工具

#### 组合式 API

v2.7 也支持，可先升至 v2.7，再迁移至 v3

相关联的多个部分代码联系在一起，便于开发维护和封装复用

在一个方法内：

1. 创建响应式数据（data）
2. 设置响应式数据相关的方法（methods）
3. 上述1、2可重复多组
4. 返回数据和方法







- 统一了 data 的写法：  
  component 和 Vue 的 data 统一为使用函数返回值
- 返回值在整个实例内都可访问，若要返回值实现响应式，则需调用 reactive() 处理

