# SSM

## spring

![image-20240325160024455](./SSM.assets/image-20240325160024455.png)

 IOC;控制反转，不用主动new，创建控制权由程序转移到外部，为了解耦。

inversion of control

![image-20240325160726235](./SSM.assets/image-20240325160726235.png)

![image-20240325160933680](./SSM.assets/image-20240325160933680.png)



![image-20240325163314325](./SSM.assets/image-20240325163314325.png)



静态工厂

![image-20240325164239233](./SSM.assets/image-20240325164239233.png)

实例工厂

![image-20240325164608566](./SSM.assets/image-20240325164608566.png)

**较为常用**

![image-20240325165110817](./SSM.assets/image-20240325165110817.png)

![image-20240325170311279](./SSM.assets/image-20240325170311279.png)

nit-method destroy-method

必须关闭容器，才能正常调用destroy-method。

ctx.registerShutdownHook(); 注册关闭钩子。

ctx.close(); 粗暴，调用位置不同，效果不同。

implements InititalBean，disposeBean

afterPropertiesSet();

bean销毁时机

![image-20240325170354568](./SSM.assets/image-20240325170354568.png)

依赖注入



![image-20240325183812509](./SSM.assets/image-20240325183812509.png)

### setter 注入



![image-20240327214935410](./SSM.assets/image-20240327214935410.png)

### 构造器注入

consturctor-arg name ref 

直接使用，耦合度较高：

![image-20240327215706741](./SSM.assets/image-20240327215706741.png)

解决形参名字问题：

![image-20240327215925538](./SSM.assets/image-20240327215925538.png)

解决参数类型重复问题。使用位置进行参数匹配。

![image-20240327220118222](./SSM.assets/image-20240327220118222.png)

![image-20240327220208982](./SSM.assets/image-20240327220208982.png)

### 依赖自动装配

bean所依赖资源在容器中自动查找并注入到bean中的过程叫自动装配。

![image-20240327220806620](./SSM.assets/image-20240327220806620.png)

类型匹配必须唯一。按类型。

![image-20240327221156768](./SSM.assets/image-20240327221156768.png)

可以多个名字。按名称。

![image-20240327221323256](./SSM.assets/image-20240327221323256.png)







![](./SSM.assets/image-20240325193925715.png)

![image-20240325194426579](./SSM.assets/image-20240325194426579.png)

![image-20240325195625392](./SSM.assets/image-20240325195625392.png)

![image-20240325200046043](./SSM.assets/image-20240325200046043.png)



i