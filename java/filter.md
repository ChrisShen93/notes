# Filter学习笔记

标签（空格分隔）： Java基础学习

---

可将Filter作为加强版的Servlet，其主要作用为对客户端请求进行预处理，亦可对HpptServletResponse进行后处理。使用Filter的完整流程是：Filter对客户端请求进行预处理，接着将请求交给Servlet进行处理并生成响应，最后Filter再对服务器响应进行后处理。Filter是一条典型的处理链。

Filter有如下几个用处：
* 在HttpServletRequest到达Servlet之前，拦截客户端的HttpServletRequest
* 根据需要检查HttpServletRequest，也可以修改HttpServletRequest头和数据
* 在HttpServletRequest到达客户端之前，拦截HttpServletResponse
* 根据需要检查HttpServletResponse，也可以修改HttpServletResponse头和数据

Filter分类：
* 用户授权的Filter：Filter负责检查用户请求，根据请求过滤用户非法请求
* 日志Filter：详细记录某些特殊的用户请求
* 负责解码的Filter：包括对非标准编码的请求解码
* 能改变XML内容的XSLT Filter等
* Filter可负责拦截多个请求或响应：一个请求或响应也可被多个Filter拦截

创建一个Filter需要两个步骤：
1. 创建FIlter处理类
2. 在web.xml文件中配置Filter

Filter对客户端请求进行预处理和对服务器响应进行后处理的分界线为是否调用了**chain.doFilter()**


## 配置Filter
Filter配置与Servlet基本相似，区别在于Servlet通常只配置一个URL，而Filter可以同时拦截多个URL
