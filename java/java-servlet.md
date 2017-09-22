# Servlet学习笔记

标签（空格分隔）： Java基础学习

---


## Servlet的开发

Servlet类必须继承HttpServlet，每个Servlet可以响应客户端的请求。Servlet提供不同的方法用于响应客户端请求：

* doGet
* doPost
* doPut
* doDelete

通常只需重写doGet()和doPost()来响应，因为客户端大多数都是Get和Post请求。亦可重写service()，即可响应客户端的所有请求。


HttpServlet还包含两个方法。

* init()：创建Servlet实例时，完成某些资源初始化才需要重写此方法
* destroy()：销毁Servlet之前，完成某些资源的回首，如关闭数据库连接等，才需要重写此方法



**1. 不用为Servlet类编写构造器**
**2. 若需对Servlet执行初始化操作，应将初始化操作放在Servlet的init()中**
**3. 若重写了init(ServletConfig config)，则应在重写该方法的第一行调用HttpServlet的init()，即super.init(config)**



## Servlet的配置

从Servlet 3.0开始，配置Servlet有两种方式：

* 在Servlet类中使用@WebServlet Annotation进行配置
* 在web.xml中进行配置

|  属性  |  是否必须  |  说明  |
| -----  | -----------|  ------|
| asyncSupported | 否 |  指定该Servlet是否支持异步操作模式 |
| displayName | 否 | 指定该Servlet的显示名 |
| initParams | 否 | 用于为该Servlet配置参数 |
| loadOnStartup | 否 | 用于将该Servlet配置成load-on-startup的Servlet|
| name | 否 | 指定该Servlet的名称 |
| urlPatterns/value | 否 | 指定该Servlet处理的URL|

若使用Annotation来配置Servlet，需注意

* 不要在web.xml的干元素（<web-app.../>）中指定metadata-complete="true"
* 不要在web.xml中配置该Servlet

若使用web.xml文件来配置Servlet，需注意

* 配置Servlet的名字：对应web.xml文件中的<servlet/>元素
* 配置Servlet的URL：对应web.xml文件中的<servlet-mapping/>元素



## Servlet的生命周期

创建Servlet实例有两个时机
* 客户端第一次请求某个Servlet时，系统创建该Servlet
* web应用启动时立即创建该Servlet实例，即load-on-startup Servlet

每个Servlet的运行都遵循如下生命周期
1. 创建Servlet实例
2. web容器调用Servlet的init()，对Servlet进行初始化
3. Servlet初始化后将一直存在于容器中，用于响应客户端请求
4. web容器决定销毁Servlet时，先调用Servlet的destroy()。通常在关闭web应用时销毁Servlet

```flow
st=>start: web应用
op_1=>operation: 创建实例
op_2=>operation: 完成初始化
op_3=>operation: 响应客户端请求
op_4=>operation: 回收资源
op_5=>operation: 销毁实例
e=>end

st->op_1->op_2->op_3->op_4->op_5->e

```

## load-on-startup Servlet

用于某些后台服务的Servlet，或者需要拦截很多请求的Servlet，通常是作为应用的基础Servlet进行使用，需要在应用启动之时创建Servlet实例。

* 在web.xml文件中通过<servlet.../>元素的<load-on-startup.../>子元素进行配置；
* 通过@WebServlet Annotation的loadOnStartup属性指定。

&lt;load-on-startup&gt;1&lt;load-on-startup/&gt;
或
@WebServlet(loadOnStartup=1)
整型值越小，Servlet就越优先实例化

## 访问Servlet的配置参数

使用配置参数，避免将参数以硬编码的方式写入代码，可以提供更好的可移植性。

* 通过@WebServlet的initParams属性指定
* 通过在web.xml文件的<servlet.../>元素中添加<init-param.../>子元素来指定。
