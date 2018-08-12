



```java
/**
 * 替代默认的SimpleClientHttpRequestFactory
 * 设置超时时间重试次数
 * 还可以设置一些拦截器以便监控
 *
 * @return
 */
@Bean
public RestTemplate restTemplate() {
    //生成一个设置了连接超时时间、请求超时时间、异常重试次数3次
    RequestConfig config = RequestConfig.custom().setConnectionRequestTimeout(10000).setConnectTimeout(10000).setSocketTimeout(30000).build();
//实际工作中，这个地方还会加上filter来抓取每次restTemplate的日志信息。
    HttpClientBuilder builder = HttpClientBuilder.create().setDefaultRequestConfig(config).setRetryHandler(new DefaultHttpRequestRetryHandler(3, false));
    HttpClient httpClient = builder.build();
    ClientHttpRequestFactory requestFactory = new HttpComponentsClientHttpRequestFactory(httpClient);
    RestTemplate restTemplate = new RestTemplate(requestFactory);
    return restTemplate;
}
```

