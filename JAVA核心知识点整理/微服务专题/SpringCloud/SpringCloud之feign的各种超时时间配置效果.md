## SpringCloud之feign的各种超时时间配置效果

### 超时时间的设置通常有三个层面

1. zuul网关

   ```properties
   #默认1000
   zuul.host.socket-timeout-millis=2000
   #默认2000
   zuul.host.connect-timeout-millis=4000
   ```

   

2. ribbon

   ```yaml
   ribbon:
     OkToRetryOnAllOperations: false #对所有操作请求都进行重试,默认false
     ReadTimeout: 5000   #负载均衡超时时间，默认值5000
     ConnectTimeout: 3000 #ribbon请求连接的超时时间，默认值2000
     MaxAutoRetries: 0     #对当前实例的重试次数，默认0
     MaxAutoRetriesNextServer: 1 #对切换实例的重试次数，默认1
   ```

   

3. 熔断器Hystrix

   ```yaml
   #熔断器启用
   feign:
     hystrix:
       enabled: true
   hystrix:
     command:
       default:  #default全局有效，service id指定应用有效
         execution:
           timeout:
             #如果enabled设置为false，则请求超时交给ribbon控制,为true,则超时作为熔断根据
             enabled: true
           isolation:
             thread:
               timeoutInMilliseconds: 60000 #断路器超时时间，默认1000ms
   ```

   

### 总结

1. 如果`hystrix.command.default.execution.timeout.enabled`为true,则会有两个执行方法超时的配置,一个就是ribbon的`ReadTimeout`,一个就是熔断器hystrix的`timeoutInMilliseconds`, 此时谁的值小谁生效
2. 如果`hystrix.command.default.execution.timeout.enabled`为false,则熔断器不进行超时熔断,而是根据ribbon的`ReadTimeout`抛出的异常而熔断,也就是取决于ribbon
3. ribbon的`ConnectTimeout`,配置的是请求服务的超时时间,除非服务找不到,或者网络原因,这个时间才会生效
4. ribbon还有`MaxAutoRetries`对当前实例的重试次数,`MaxAutoRetriesNextServer`对切换实例的重试次数, 如果ribbon的`ReadTimeout`超时,或者`ConnectTimeout`连接超时,会进行重试操作
5. 通常熔断的超时时间需要配置的比`ReadTimeout`长,`ReadTimeout`比`ConnectTimeout`长



##### 待验证

重试次数=（MaxAutoRetries+MaxAutoRetriesNextServer+MaxAutoRetriesNextServer*MaxAutoRetries）

重试时间=（MaxAutoRetries+MaxAutoRetriesNextServer+MaxAutoRetriesNextServer*MaxAutoRetries）*ReadTimeout+网络响应时间

并且由于重试应为执行熔断后而停止重试，从而造成不必要的情况发生，因此熔断时间应该在重试时间之后，

（MaxAutoRetries+MaxAutoRetriesNextServer+MaxAutoRetriesNextServer*MaxAutoRetries）*ReadTimeout+网络响应时间+ReadTimeout<timeoutInMilliseconds

