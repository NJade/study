# Mybatis
## 설정
* [mybatis](http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)
* spring boot 사용 시 특별한 설정을 하지 않아도 알아서 다 해줌
* 여러 datasource를 사용할 필요가 없다면 mybatis config 파일을 만들 필요가 없다.
* 알아서 @mapper를 찾아주기 때문에 mapperscan도 필요치 않다.
* db에 underscope 사용 시 camel과 맵핑을 위한 설정은 필요
```yml
mybatis:
  configuration:
    map-underscore-to-camel-case: true
```
* application.yml 에 설정
## Test
* [mybatis-test](http://mybatis.org/spring-boot-starter/mybatis-spring-boot-test-autoconfigure/)
* @MybatisTest 어노테이션만 사용하면 된다.
* 다른 테스트와 함께 사용시 @AutoConfigureMybatis를 사용