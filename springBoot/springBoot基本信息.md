## 如何看源码

1.看SpringBoot的自动配置类 查询配置文件的源码

```
#spring boot 所有的配置类都有自己的自动配置类  RedisAutoConfiguration
#自动配置类都回绑定properties  RedisProperties
```

spring-boot-autoconfigure-2.4.3.jar!\META-INF\spring-autoconfigure-metadata.properties

l例如springMata -redis

```java
org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration=
org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration.AutoConfigureAfter=org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration
org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration.ConditionalOnBean=org.springframework.data.redis.connection.RedisConnectionFactory
org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration.ConditionalOnClass=org.springframework.data.redis.connection.RedisConnectionFactory
```



```java

RedisAutoConfiguration
```



```java
@ConditionalOnClass({RedisOperations.class})
@EnableConfigurationProperties({RedisProperties.class})
@Import({LettuceConnectionConfiguration.class, JedisConnectionConfiguration.class})
public class RedisAutoConfiguration {
    public RedisAutoConfiguration() {
    }
```



```
RedisProperties
```

源码分析

```
@Bean
@ConditionalOnMissingBean(
    name = {"redisTemplate"}
)
@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
    RedisTemplate<Object, Object> template = new RedisTemplate();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}

@Bean
@ConditionalOnMissingBean
@ConditionalOnSingleCandidate(RedisConnectionFactory.class)
public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) {
    StringRedisTemplate template = new StringRedisTemplate();
    template.setConnectionFactory(redisConnectionFactory);
    return template;
}
```

ConditionalOnMissingBean