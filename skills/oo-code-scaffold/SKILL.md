---
name: oo-code-scaffold
description: 从 Class Diagram + API Spec + DB Schema 生成项目骨架代码。输入全套 OO 设计产出，输出完整项目结构、接口定义、实体类、Repository、Service 骨架、单元测试模板。支持 Spring Boot/FastAPI/Go 等框架。触发词：代码生成、scaffold、项目骨架、脚手架、代码框架、生成代码、接口实现。
---

# OO 能力胶囊 C14：Code Scaffold 生成器

从设计产出生成可编译运行的项目骨架。

## 触发条件

代码生成、scaffold、脚手架、项目骨架、生成代码、初始化项目、create project

## 输入

- Class Diagram（C05）+ API Spec（C11）+ DB Schema（C12）

## 输出规范

### 1. 项目结构

```
yuan-shi-server/
├── pom.xml / build.gradle / pyproject.toml
├── src/main/java/com/yuanshi/
│   ├── YuanShiApplication.java
│   ├── domain/
│   │   ├── user/
│   │   │   ├── User.java              (Aggregate Root)
│   │   │   ├── Profile.java           (Entity)
│   │   │   ├── MatchPreference.java   (Value Object)
│   │   │   └── UserStatus.java        (Enum)
│   │   ├── match/
│   │   │   ├── MatchSelection.java
│   │   │   └── SelectionStatus.java
│   │   ├── conversation/
│   │   │   ├── Conversation.java
│   │   │   ├── Message.java
│   │   │   └── ConversationStatus.java
│   │   └── payment/
│   │       ├── PaymentOrder.java
│   │       ├── Money.java
│   │       └── OrderType.java
│   ├── application/
│   │   ├── MatchService.java
│   │   ├── ConversationService.java
│   │   ├── PaymentService.java
│   │   └── IdentityService.java
│   ├── ports/
│   │   ├── inbound/
│   │   │   ├── SelectionController.java
│   │   │   └── ConversationController.java
│   │   └── outbound/
│   │       ├── IUserRepository.java
│   │       ├── IMatchRepository.java
│   │       ├── IPaymentGateway.java
│   │       └── IAIMatcher.java
│   └── infrastructure/
│       ├── persistence/
│       │   └── UserRepositoryImpl.java
│       └── payment/
│           └── WechatGatewayImpl.java
└── src/test/java/com/yuanshi/
    ├── domain/user/UserTest.java
    ├── application/MatchServiceTest.java
    └── integration/SelectionFlowTest.java
```

### 2. 接口骨架代码

```java
// IUserRepository.java
public interface IUserRepository {
    Optional<User> findById(UserId id);
    List<User> findByStatus(UserStatus status);
    void save(User user);
}

// IAIMatcher.java
public interface IAIMatcher {
    CompatibilityScore computeCompatibility(User a, User b);
    List<ScoredCandidate> rankCandidates(User user, List<User> candidates);
}

// IPaymentGateway.java
public interface IPaymentGateway {
    PaymentOrder createOrder(Money amount, OrderType type);
    PaymentStatus queryStatus(OrderId orderId);
}
```

### 3. 值对象代码

```java
// Money.java — immutable
public record Money(BigDecimal amount, String currency) {
    public Money add(Money other) {
        if (!this.currency.equals(other.currency))
            throw new IllegalArgumentException();
        return new Money(this.amount.add(other.amount), currency);
    }
}
```

### 4. 聚合根基架

```java
// User.java
public class User {
    private UserId id;
    private UserStatus status;
    private Money balance;
    private Profile profile;
    private IdentityVerification verification;
    
    public void lock() {
        if (status != UserStatus.ACTIVE)
            throw new IllegalStateException();
        this.status = UserStatus.LOCKED;
        // Domain Event: UserLocked
    }
    
    public void unlock() {
        if (status != UserStatus.LOCKED)
            throw new IllegalStateException();
        this.status = UserStatus.ACTIVE;
    }
}
```

### 5. 测试模板

```java
class UserTest {
    @Test
    void shouldLockActiveUser() {
        User user = new User(ACTIVE);
        user.lock();
        assertEquals(LOCKED, user.getStatus());
    }
    
    @Test
    void shouldThrowWhenLockingLockedUser() {
        User user = new User(LOCKED);
        assertThrows(IllegalStateException.class, user::lock);
    }
}
```

## 技术栈建议

```
| 框架 | 适用场景 | 推荐 |
|------|---------|------|
| Spring Boot | Java 企业级 | ★★★ |
| FastAPI | Python/ML集成 | ★★ |
| Go/Gin | 高并发支付 | ★★ |
```

## 自查

- [ ] 包结构符合 DDD 分层（domain/application/infrastructure/ports）？
- [ ] 所有接口已在 C05 定义？
- [ ] 聚合根方法封装了状态变更？
- [ ] 测试覆盖正常流+异常流？
