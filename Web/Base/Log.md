# Log

## ELK Stack
[docker-elk 설치](https://github.com/deviantony/docker-elk)
- Elastic Search : 로그 저장 및 검색
- Logstash : 로그 처리(수집도 가능)
- Kibana : 로그 시각화
- Filebeat : 로그 수집

## Spring
### Log4j2
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--설정 파일 로드 중 출력할 등급-->
<Configuration status="WARN">

    <!--로그 출력 경로-->
    <Appenders>
        <!--콘솔 로그-->
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{ISO8601} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>

        <!--파일 로그-->
        <File name="File" fileName="logs/app.log">
            <PatternLayout pattern="%d{ISO8601} [%t] %-5level %logger{36} - %msg%n"/>
        </File>

        <!--시간 단위 파일 로그-->
        <RollingFile name="RollingFile" fileName="logs/app.log" filePattern="logs/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}.log">
            <PatternLayout pattern="%d{ISO8601} [%t] %-5level %logger{36} - %msg%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy interval="1" modulate="true"/>
            </Policies>
        </RollingFile>

        <!--ELK 로그-->
        <Elasticsearch name="Elasticsearch" serverUris="http://localhost:9200">
            <IndexName>logstash-${date:yyyy.MM.dd}</IndexName>
            <DocumentType>logevent</DocumentType>
            <Layout>
                <PatternLayout>
                    <Pattern>%d{ISO8601} [%t] %-5level %logger{36} - %msg%n</Pattern>
                </PatternLayout>
            </Layout>
        </Elasticsearch>
    </Appenders>

    <Loggers>
        <!--로그 출력 레벨 설정-->
        <Root level="info">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="File"/>
            <AppenderRef ref="RollingFile"/>
        </Root>

        <!--클래스 단위 로그 설정-->
        <Logger name="com.example" level="debug" additivity="false">
            <AppenderRef ref="Console"/>
        </Logger>
    </Loggers>

</Configuration>
```