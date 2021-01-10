

스프링 부트 logback 설정

https://goddaehee.tistory.com/206

https://enai.tistory.com/36





`spring-boot`에서는 `logback-spring.xml`을 사용해서 Spring이 logback을 구동할 수 있도록 지원

```
logback-spring.xml 태그 설명


configuration
- root 요소
- 속성 : scan: 변경을 확인할지 true/false, scanPeriod: 스캔확인 주기 추가 속성은 사이트 확인
- ex) <configuration scan:="true" scanPeriod="60 seconds">
- <!-- 60초마다 설정 파일의 변경을 확인 하여 변경시 갱신 -->


springProfile
yml파일에서 spring.profiles.active의 설정에 따라
logback-spring.xml에서 여러개의 프로파일을 설정할 수 있다.
ex)
spring.profiles.active: local 인경우
<springProfile name="local">
        <!-- contents -->
</springProfile>

spring.profiles.active: dev 또는 prd 인 경우
<springProfile name="dev | prd">
        <!-- contents -->
</springProfile>

spring.profiles.active: prd 가 아닌경우
<springProfile name="!prd">
    <!-- contents -->
</springProfile>

<springProfile name="dev"> 
    <property resource="application-dev.yml"/> 
</springProfile> 설정하면 application-dev.yml의 모든 key값들을 사용할 수 있다.
<property name="LOG_PATH" value="${log.config.path}"/>
<file>${LOG_PATH}/${LOG_FILE_NAME}.log</file>

application-dev.yml안의 key값을 이런식으로 사용


springProperty
용도
application.yml안에 있는 속성값들을 사용할 수 있다.
속성
source: yml안에 정의한 key값을 의미한다.
defaultValue: source값이 존재하지 않는다면 사용할 default값
scope: 사용범위를 의미한다.
name: 사용할 key 값
ex)
<springProperty scope="context" name="fluentHost" source="myapp.fluentd.host" defaultValue="localhost"/>
<appender name="FLUENT" class="ch.qos.logback.more.appenders.DataFluentAppender">
    <remoteHost>${fluentHost}</remoteHost>
    ...
</appender>


property 
설정파일에서 사용될 변수값 선언
<property name="ERR_LOG_FILE_NAME" value="err_log"/>


root
 - 전역 설정이라고 볼 수 있다.
<root level="${LOG_LEVEL}">
    <appender-ref ref="CONSOLE"/>
</root>


logger
 - 지역 설정이라고 볼 수 있다.
 - additivity 값은 root 설정 상속 유무 설정.(default = true)
<logger name="org.apache.ibatis" level="DEBUG" additivity="false"> 
    <appender-ref ref="CONSOLE"/>
</logger>


appender
log의 형태를 설정, 로그 메시지가 출력될 대상을 결정하는 요소



```

