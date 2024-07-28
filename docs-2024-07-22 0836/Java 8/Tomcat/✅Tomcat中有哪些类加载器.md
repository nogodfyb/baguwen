# 典型回答

Tomcat的类加载机制是指Tomcat在运行时如何加载和管理Java类。Tomcat的类加载机制的实现并没有严格遵守双亲委派原则，而是采用了一种层次化的类加载器结构，这种结构旨在提供更好的隔离性和灵活性，以支持多个Web应用程序的部署和运行。

[✅什么是双亲委派？如何破坏？](https://www.yuque.com/hollis666/fo22bm/gt8zp4?view=doc_embed&inner=jVIic)

关于Tomcat的ClassLoader的关系，网上有各种各样的图，但是大部分都是不对的。甚至有一些画完图之后竟然还能自圆其说。。。

下面这张图，是我基于Tomcat官网，以及源码总结之后的一张图（从Tomcat 6到Tomcat 10，都长这个样）：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/5378072/1690621553375-f51add4c-18d7-44a6-a9f2-e7a0d06790dc.png#averageHue=%23fcfcfc&clientId=ud5f53a75-4059-4&from=paste&height=906&id=u552783dc&originHeight=906&originWidth=1366&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79697&status=done&style=none&taskId=uad275510-2d7b-41ee-82a3-70881024249&title=&width=1366)

但是，默认情况下，**Server类加载器和Shared类加载器是未定义的**，需要通过在conf/catalina.properties中定义server.loader和/或shared.loader属性的值，才会是这个更复杂的层次结构。

Server类加载器只对Tomcat内部可见，对于Web应用程序完全不可见。

Shared类加载器对所有Web应用程序可见，可以用于在所有Web应用程序之间共享代码。但是，对这些共享代码进行更新将需要重新启动Tomcat。

所以，真正的需要一定有的，并且我们通常需要关注的就是下面这个层级关系：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/5378072/1690621564568-3476edd5-3b8d-40b8-bca9-bf6972184af0.png#averageHue=%23f8f8f8&clientId=ud5f53a75-4059-4&from=paste&height=653&id=u24102c86&originHeight=653&originWidth=1311&originalType=binary&ratio=1&rotation=0&showTitle=false&size=127487&status=done&style=none&taskId=u0e08663c-fa58-491a-b650-62447b48dd0&title=&width=1311)

**启动类加载器（Bootstrap ClassLoader）**： 负责加载JVM自身的核心类库（如java.lang、java.util等）和JVM相关的类。启动类加载器是JVM的一部分，负责加载JVM运行所需的基础类。主要加载JRE中的lib包及lib/ext包下的内容

**系统类加载器（System Class Loader）**，负载加载Tomcat内部的一些核心类库，这些类一般在$CATALINA_HOME/bin这个目录下， 一般包含bootstarap.jar、tomcat-juli.jar以及common-daemon.jar等。

**公共类加载器（Common Class Loader）**： 负责加载Tomcat的公共类和库，位于$CATALINA_HOME/lib目录下的JAR文件。这些类库是Tomcat启动时加载的，是整个Tomcat实例中共享的类。

**Web应用程序类加载器（Webapp Class Loader**）： 每个Web应用程序都有一个独立的Web应用程序类加载器，负责加载该Web应用程序的类和资源。它从$CATALINA_HOME/webapps/<webapp_name>/WEB-INF/classes目录和$CATALINA_HOME/webapps/<webapp_name>/WEB-INF/lib目录加载类和JAR文件。

# 扩展知识

## Tomcat类加载机制

[✅Tomcat的类加载机制是怎么样的？](https://www.yuque.com/hollis666/fo22bm/evlwzsa8s6mx93ly?view=doc_embed)


