# Security

## Spring Security
스프링 기반 웹 애플리케이션의 인증과 권한을 담당하는 스프링의 하위 프레임워크

### 일반적인 작동원리
1. 사용자의 로그인 요청
2. UsernamePasswordAuthenticationFilter에서 attemptAuthentication으로 검증, UsernamePasswordAuthenticationToken을 생성해 AuthenticationManager로 인증을 수행한다. AuthenticationManager에는 토큰을 처리할 Provider를 리스트로 가지고 있다.
3. UserDetailsService에서 리포지토리를 이용해 DB에 접근해서 사용자 정보를 얻는다. 얻은 사용자 정보를 이용해 UserDetails를 Provider에 반환한다.
4. Provider에서 DB의 정보와 사용자 입력 정보를 이용해 인증을 수행, 성공하면 Authentication 객체를 SecurityContextHolder에 담고, 성공/실패 메소드들을 호출한다.

### 사전준비

```java
// gradle
implementation 'org.springframework.boot:spring-boot-starter-security'
implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity6'
```

### 설정

```java
@Configuration // 스프링 환경 설정 파일
@EnableWebSecurity // 모든 URL이 스프링 시큐리티의 제어를 받음
public class SecurityConfig {
    @Bean
    SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf((csrf) -> csrf.disable()); // csrf 설정
        http.formLogin((login) -> login.disable()); // 로그인 페이지 설정
        http.httpBasic((basic) -> basic.disable()); // Http 기본 인증 설정
        http.oauth2Login(Customizer.withDefaults()); // oauth2 설정
        http.authorizeHttpRequests((auth) -> auth
                .requestMatchers("/", "/oauth2/**", "/login/**").permitAll()
                .anyRequest().authenticated()); // 인증되지 않은 사용자 접근 가능 페이지 설정
        
        // 그 외 로그아웃, 헤더 등 설정 가능

        return http.build();
    }
}
```

## BindingResult

모델의 Validation에 주로 사용, 모델에 Validation 어노테이션을 평가한 결과를 담고 있다.

- hasErrors() : 오류 여부 반환
- rejectValue(String, String, String) : 필드에 대한 오류 생성
- reject(String, String) : 오류 생성

## OAuth2
### Application Properties
```yaml
#registration
spring.security.oauth2.client.registration.서비스명.client-name=서비스명
spring.security.oauth2.client.registration.서비스명.client-id=서비스에서 발급 받은 아이디
spring.security.oauth2.client.registration.서비스명.client-secret=서비스에서 발급 받은 비밀번호
spring.security.oauth2.client.registration.서비스명.redirect-uri=서비스에 등록한 우리쪽 로그인 성공 URI
spring.security.oauth2.client.registration.서비스명.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.서비스명.scope=리소스 서버에서 가져올 데이터 범위

#provider(google 등의 해외 유명 서비스는 필요없을 수 있음)
spring.security.oauth2.client.provider.서비스명.authorization-uri=서비스 로그인 창 주소
spring.security.oauth2.client.provider.서비스명.token-uri=토큰 발급 서버 주소
spring.security.oauth2.client.provider.서비스명.user-info-uri=사용자 정보 획득 주소
spring.security.oauth2.client.provider.서비스명.user-name-attribute=응답 데이터 변수
```

## JWT
### 사전준비
```java
// gradle
implementation 'io.jsonwebtoken:jjwt-api:0.12.3'
implementation 'io.jsonwebtoken:jjwt-impl:0.12.3'
implementation 'io.jsonwebtoken:jjwt-jackson:0.12.3'

// application.properties
spring.jwt.secret=비밀키를 하드코딩 해야한다
```

### 사용

```java
// JWT 생성
public String createJwt(String username, String role, Long expiredMs) {
    return Jwts.builder()
            .claim("username", username) //JWT에 포함할 데이터
            .claim("role", role)
            .issuedAt(new Date(System.currentTimeMillis())) //발급시간 설정
            .expiration(new Date(System.currentTimeMillis() + expiredMs)) //만료시간 설정
            .signWith(secretKey) //비밀키 할당
            .compact(); //생성
}
```
1. 로그인 성공 시 JWT를 발급해 헤더에 저장한다.
2. 로그인 검증 필터 전에 토큰 검증 필터를 삽입한다.
3. 토큰이 존재하고 만료되지 않았으면 SecurityContextHolder.getContext().setAuthentication()에 토큰의 인증정보를 저장한다. 정상적인 토큰으로 간주하고 로그인 검증 필터는 자동으로 통과한다.

### RSA 기반 JWT
```java
@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
				// 소셜 로그인
        http
                .oauth2Login(oauth2->oauth2.successHandler(new OAuth2SuccessHandler()));

        return http.build();
    }

    // JWT를 위한 비대칭 키 셋 생성, 인코딩, 디코딩에 사용
    @Bean
    public JWKSet generateKeySet() throws Exception{
        var keyPairGenerator = KeyPairGenerator.getInstance("RSA");
        keyPairGenerator.initialize(2048);
        var keyPair = keyPairGenerator.generateKeyPair();
        RSAPublicKey publicKey = (RSAPublicKey) keyPair.getPublic(); // 공개 키 생성, Gateway에서 검증에 사용
        RSAPrivateKey privateKey = (RSAPrivateKey) keyPair.getPrivate();

        // JWK로 공개 키 변환
        var rsaKey = new RSAKey.Builder(publicKey)
                .privateKey(privateKey)
                .keyID("1234")  // Key ID 설정 (JWT 헤더의 kid와 일치해야 함)
                .build();

        return new JWKSet(rsaKey);
    }

    // 인코더 생성
    @Bean
    public JwtEncoder jwtEncoder(JWKSet set) {
        var source = new JWKSource<SecurityContext>() {
            @Override
            public List<JWK> get(JWKSelector jwkSelector, SecurityContext securityContext) throws KeySourceException {
                return jwkSelector.select(set);
            }
        };

        return new NimbusJwtEncoder(source);
    }
}
```
```java
@Component
@RequiredArgsConstructor
public class JWTHandler {

    private final JwtEncoder encoder;

    @Value("${security.jwt.issuer}")
    private String issuer;

    public String createToken(String username){
        Instant now = Instant.now();

        JwtClaimsSet claims = JwtClaimsSet.builder()
                .issuer(issuer)
                .issuedAt(now)
                .expiresAt(now.plusSeconds(3600)) // 1시간 유효
                .subject(username)
                .claim("role", "INVALID") // 역할 추가 (필요시 변경)
                .build();

        var params = JwtEncoderParameters.from(claims);

        return encoder.encode(params).getTokenValue();
    }

    public void setJwtTokenAsCookie(String jwtToken, HttpServletResponse response) {
        Cookie cookie = new Cookie("jwt_token", jwtToken);
        cookie.setHttpOnly(true);  // HTTP Only 설정
        cookie.setPath("/");  // 쿠키가 전송될 경로
        cookie.setMaxAge(24 * 60 * 60);  // 쿠키 유효 기간 (24시간)
        response.addCookie(cookie);
    }
}
```
```java
@RestController
@RequiredArgsConstructor
public class JWTController {

    private final JWKSet jwkSet;
    private final JWTHandler jwtHandler;

    @GetMapping("/.well-known/jwks.json") // 
    public Map<String, Object> getJwks() {
        // JWK Set을 반환
        return jwkSet.toJSONObject();
    }
}
```
```java
    @Bean
    public SecurityWebFilterChain securityWebFilterChain(ServerHttpSecurity http) {
        http
                .cors(cors->cors.configurationSource(corsConfigurationSource()))
                .csrf(ServerHttpSecurity.CsrfSpec::disable)
                .authorizeExchange(exchanges -> exchanges
                        .pathMatchers(HttpMethod.GET).permitAll() // GET 요청은 인증 불필요
                        .anyExchange().authenticated() // 다른 모든 요청은 인증 필요
                )
                .oauth2ResourceServer(oauth2->oauth2.jwt(jwt->jwt.jwkSetUri(jwkSetUri))); // .well-known/jwks.json

        return http.build();
    }
```

## Jasypt
[github 출처](https://github.com/ulisesbocchio/jasypt-spring-boot/tree/master)
<br/>

application.yml에 사용되는 키들을 암호화할 수 있다.
```xml
<dependency>
        <groupId>com.github.ulisesbocchio</groupId>
        <artifactId>jasypt-spring-boot-starter</artifactId>
        <version>3.0.5</version>
</dependency>
```
```java
@Bean("jasyptStringEncryptor")
    public StringEncryptor stringEncryptor() {
        PooledPBEStringEncryptor encryptor = new PooledPBEStringEncryptor();
        SimpleStringPBEConfig config = new SimpleStringPBEConfig();
        config.setPassword([패스워드 키]);
        config.setAlgorithm("PBEWITHHMACSHA512ANDAES_256");
        config.setKeyObtentionIterations("1000");
        config.setPoolSize("1");
        config.setSaltGeneratorClassName("org.jasypt.salt.RandomSaltGenerator");
        config.setIvGeneratorClassName("org.jasypt.iv.RandomIvGenerator");
        config.setStringOutputType("base64");
        encryptor.setConfig(config);
        return encryptor;
    }
/*
이후 application.yml에서 값을 EUC(암호화된 값)으로 사용하면 된다.
암호화는 jasyptEncrypt 메소드나 암호화 사이트에서 암호화가 가능하다.
EUC부분은 application.yml에서 설정 가능하다.
jasypt:
  encryptor:
    property:
      prefix: "ENC@["
      suffix: "]"
*/
```