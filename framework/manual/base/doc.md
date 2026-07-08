# API 文档规范

## 1. Swagger 集成
```java
@Configuration
@EnableOpenApi
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.OAS_30)
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.opensabre"))
            .paths(PathSelectors.any())
            .build();
    }
}
```

## 2. OpenAPI 规范
- 接口分组管理
- 统一响应格式
- 错误码规范说明

## 3. 接口注解规范
```java
@Operation(summary = "用户登录接口")
@PostMapping("/login")
public ResponseEntity<UserVO> login(
    @Parameter(description = "登录表单") @RequestBody LoginForm form) {
    // ...
}
```

## 4. 文档访问
- 开发环境: http://localhost:8080/swagger-ui.html
- 生产环境: 通过网关路由 /api-docs 访问

## 5. 安全配置
```yaml
springdoc:
  swagger-ui:
    oauth:
      client-id: swagger-ui
      realm: opensabre
      app-name: API Documentation
```