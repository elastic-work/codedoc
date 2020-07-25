### 第一章 HTML与JS

- 静态网页与动态网页的区别
- 什么是标记语言
  - XML
  - HTML
  - 知道HTML14.01 和 XHTML吗
  - HTML5知道吗？
  - 常见的HTML属性
  - 常见的基本标签以及其属性
- 知道POST和GET的区别吗？
- JavaScript的书写方式知道吗？
- JS的基本语法
- JS操作Dom元素
- JQuery基本语法

Tomcat与HTTP

- Web的发展史

- C/S与B/S架构了解吗

- 服务器

  - 常见的服务器有哪些

  - 你常用的服务器是那些

  - 知道EJB规范吗

  - 知道Servlet容器吗

  - Tomcat的常见配置知道吗？

    - 热部署知道吗？
    - Tomcat的目录结构知道吗？
    - Tomcat的配置文件有看过吗？

  - 知道Tomcat的常见错误吗？

    - 出现404
    - Tomcat启动之后再去启动
    - 没有启动就去访问
    - 配置文件使用了中文，这个时候这个XML文件必须是UTF-8的编码

  - Tomcat的默认端口

  - Web应用的规范目录结构

    - ```text
      - appDemo （Web应用根目录）根目录的文件外界可以直接访问
      	- html、jsp、css、Javascript文件等
      	- WEB-INF 目录 （此目录下的文件外界不能访问，只能通过Web容器调用）
      		- classes 目录 （Java类编译之后的class文件）
      		- lib 目录（Java类运行所依赖的jar包）
      		- web.xml 文件 （Web应用的配置文件）
      ```

  - 可以手动建立一个纯的JavaWeb项目吗？

- 知道HTTP协议吗？

  - HTTP协议与HTTPS协议的区别
  - HTTP的默认端口
  - HTTPS的默认端口
  - HTTP1.0和HTTP1.1的规范知道吗？
  - 知道HTTP的请求信息有哪些部分组成吗？
    - 请求行
    - 请求头
    - 请求实体
  - 知道响应信息吗

### 第二章 Servlet

- 什么是Servlet
- 请新建标准的JavaWeb项目结构
- Servlet的生命周期知道吗
- Servlet的请求流程知道吗
- Servlet的初始化参数知道吗？
- 请描述一下Servlet的继承体系

  - 分析一下Servlet的继承体系
  - 编写Servlet的三种方式知道吗
- HttpServletRequest常用的方法
- 请求中文乱码如何处理
- HttpServletResponse常用方法

### 第三章 Cookie和Session

- 请描述一下Servlet的映射细节 基于Web.xml 映射
- Servlet3.0 新特性 注解配置
- Serlvet线程安全问题了解吗？
- Http协议无状态带来的问题知道吗？如何解决？
  - 参数传递机制
  - Cookie
  - Session
- 什么是Cookie
  - Cookie的优点和缺陷
- 什么是Session
  - Session的优点和缺陷
  - Session的原理知道吗？
  - Session的常见操作知道吗
- Cookie和Session的对比

### 第四章 Servlet交互和JSP

- 为什么需要Servlet交互？如何交互
- Web之间跳转和信息共享
  - 请求转发和重定向的相同点和不同点
- Servlet的三大作用于对象
- ServletContext接口的常用方法
- 动态网页
  - JSP
- JSP原理
- JSP的基本语法
- JSP指令
- JSP九大内置对象
- JSP的四大作用域
- JSP的常用动作元素

### 第五章 JavaBean+EL+JSTL

- JavaBean规范
- JavaBean的内省机制
- EL表达式
- JSTL表达式

### 第六章 文件上传

- 实现文件上传的要点
- 文件下载的要点
- 什么是国际化？如何实现

### 第七章 监听器-拦截器-过滤器

- 什么是过滤器，如何使用，为什么需要过滤器
- 什么是监听器，如何使用，为什么需要监听器
- 什么是拦截器，如何使用，为什么需要拦截器