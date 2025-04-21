# Batch

## JDBC

```java

Connection conn = ...;
PreparedStatement pstmt = conn.prepareStatement("INSERT INTO users (name, email) VALUES (?, ?)");

for (int i = 0; i < 1000; i++) {
    pstmt.setString(1, "User" + i);
    pstmt.setString(2, "user" + i + "@example.com");
    pstmt.addBatch(); // SQL을 배치에 추가

    if (i % 100 == 0) { // 100개마다 실행
        pstmt.executeBatch(); // 실행
        pstmt.clearBatch();   // 배치 초기화
    }
}

// 남은 배치 실행
pstmt.executeBatch();
pstmt.close();
conn.close();

```

## Mybatis

```java

SqlSessionFactory sqlSessionFactory = ...;
try (SqlSession session = sqlSessionFactory.openSession(ExecutorType.BATCH)) {
    UserMapper mapper = session.getMapper(UserMapper.class);
    
    for (int i = 0; i < 1000; i++) {
        User user = new User("user" + i, "user" + i + "@test.com");
        mapper.insertUser(user); // 일반 insert 메서드 호출
        if (i % 100 == 0) {
            session.flushStatements(); // 100개마다 flush
        }
    }

    session.commit();
}

```

## JPA

```
# application.properties
spring.jpa.properties.hibernate.jdbc.batch_size=50
spring.jpa.properties.hibernate.order_inserts=true
spring.jpa.properties.hibernate.order_updates=true
spring.jpa.properties.hibernate.generate_statistics=true

```

```java

for (int i = 0; i < 1000; i++) {
    User user = new User("user" + i, "user" + i + "@mail.com");
    entityManager.persist(user);

    if (i % 50 == 0) {
        entityManager.flush();   // 쌓인 insert 수행
        entityManager.clear();   // 1차 캐시 비움 (메모리 절약)
    }
}

```