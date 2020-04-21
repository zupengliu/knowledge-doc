## Feign Retryer的默认重试策略测试

### Feign配置
```java
@Configuration
public class FeignConfig {


    /**
     * 配置请求重试
     * long period：period=100 发起当前请求的时间间隔，单位毫秒
     * long maxPeriod：maxPeriod=1000 发起当前请求的最大时间间隔，单位毫秒
     * int maxAttempts：maxAttempts=5 最多请求次数，包括第一次
     */
    @Bean
    public Retryer feignRetryer() {
        return new Retryer.Default(200, TimeUnit.SECONDS.toMillis(2), 10);
    }


    /**
     * 设置请求超时时间
     * 默认
     * public Options() {
     * this(10 * 1000, 60 * 1000);
     * }
     * connectTimeoutMillis：链接超时时间 10000毫秒 = 10秒
     * readTimeoutMillis：响应超时时间，如果超过60秒没有响应发起下一次请求 60000毫米=60秒
     */
    @Bean
    Request.Options feignOptions() {
        return new Request.Options(10 * 1000, 60 * 1000);
    }


    /**
     * 打印请求日志
     * <p>
     * NONE: 不记录任何信息
     * BASIE:仅记录请求方法，URL以及响应状态码和执行时间
     * HEADERS:除了记录BASIE级别得信息之外，还会记录请求和响应得头信息
     * FULL：记录所有请求与响应得明细，包括头信息，请求体，元数据等。
     *
     * @return
     */
    @Bean
    public feign.Logger.Level multipartLoggerLevel() {
        return feign.Logger.Level.FULL;
    }

}
```
