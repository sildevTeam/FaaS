#Sildev's serviceless framework
[喵喵喵喵喵](http://sildev.cn)
# 设计目标
设计一套golang的api使最终的原子服务函数的编写者像使用操作系统提供的标准的输入输出那样来获取请求的参数／与前端交互(暂缓实现)／返回请求结果(faas)
设计一套rest API形式的调用方法，效果类似于golang提供的api作为非golang实现的faas service的实现方式
设计一套看护原子函数服务的的后台主流程序（watchdog)
设计一套对前台请求进行分发到原子函数，并可以对原子函数进行最合的API网关（gateway)
#设计设想
##API Gateway
api gateway通过两种方式与后台实际提供服务的原子函数服务进行通讯，首选的方法为类似于使用thrift自定义tcp协议的通讯方式进行通讯，并且实现一套基于http请求的与后端进行通讯的方式（作为没有实现的语言提供的默认接口，随着时间推移，如果还有兴趣继续造轮子的话，会需要慢慢的实现所有语言的接口)     
***
API Gateway提供一套http风格的管理API（后续计划)
##faas
实现与API Gateway进行通讯的thrift协议，并且将Gateway传来的请求封装为一个伪stdin接口     

```go
var inputData string = SilFaaS.std.In()
```
函数产生的返回结果通过一个伪stdout接口转化为thrift消息发送到到Gateway

```go
SilFaaS.std.Out("message returned from function")
```

##watchdog
watchdog监控后台的faas所运行的容器与其中的进程，保证容器的正常运行并在容器镜像有更新的时候对运行中的容器进行更新     
TODO:完成设计