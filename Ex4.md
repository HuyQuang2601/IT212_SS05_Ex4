# BÀI 4: Phân tích & Lựa chọn

## Prompt học kiến thức kỹ thuật nâng cao

## 1. Đáp án lựa chọn

**Chọn phương án B.**

Phương án B là prompt tối ưu nhất vì không chỉ yêu cầu AI “viết code”, mà còn hướng AI giải thích bản chất Spring AOP theo nhiều tầng kiến thức, có so sánh khái niệm và có ví dụ thực tế.

---

## 2. Phân tích tại sao phương án B tối ưu nhất

Phương án B tốt nhất vì đáp ứng đầy đủ 3 nguyên tắc học kiến thức kỹ thuật nâng cao:

1. **Level-based Explanation**
   Prompt yêu cầu giải thích Spring AOP theo 2 cấp độ:

    * Cấp độ người mới bắt đầu.
    * Cấp độ Senior Developer.

   Cách này giúp người học hiểu từ trực quan đến chuyên sâu, tránh bị ngợp bởi các thuật ngữ như `Aspect`, `JoinPoint`, `Pointcut`, `Advice`.

2. **Comparative Analysis**
   Prompt yêu cầu lập bảng so sánh giữa AOP và OOP. Đây là điểm quan trọng vì người học Java thường đã quen với OOP, nên việc so sánh giúp hiểu AOP không thay thế OOP mà bổ sung để xử lý các vấn đề cắt ngang như logging, security, transaction, audit.

3. **Practical Examples**
   Prompt yêu cầu ví dụ Java Spring Boot dùng `@Aspect`, `@Before`, `@After` để ghi log tự động. Điều này giúp biến kiến thức trừu tượng thành code có thể áp dụng vào dự án thật.

Ngoài ra, prompt B có cấu trúc rõ ràng gồm:

* Vai trò: System Architect giàu kinh nghiệm sư phạm.
* Nhiệm vụ: giúp học hiểu Spring AOP.
* Ngữ cảnh: cần ghi log tự động cho Service.
* Định dạng đầu ra: chia thành 3 phần.
* Ràng buộc: có ví dụ ngắn gọn, chú thích tiếng Việt.

Vì vậy, phương án B giúp AI tạo ra câu trả lời vừa dễ hiểu, vừa có chiều sâu, vừa có tính ứng dụng.

---

## 3. Lý do loại trừ các phương án còn lại

## Phương án A

```text
"Giải thích Spring AOP là gì và viết cho tôi một ví dụ ghi log bằng Spring AOP."
```

Phương án A có ưu điểm là ngắn gọn và đúng trọng tâm. Tuy nhiên, nó còn quá chung chung.

Nhược điểm:

* Không yêu cầu giải thích theo nhiều cấp độ.
* Không yêu cầu so sánh AOP với OOP.
* Không chỉ rõ cần giải thích các thuật ngữ như `Aspect`, `Pointcut`, `Advice`.
* Dễ khiến AI trả lời sơ sài hoặc chỉ đưa code mẫu mà người học chưa hiểu bản chất.
* Không nhấn mạnh ứng dụng thực tế trong dự án Spring Boot lớn.

Vì vậy, phương án A phù hợp để hỏi nhanh, nhưng chưa tối ưu cho mục tiêu học kiến thức kỹ thuật nâng cao.

---

## Phương án C

```text
"Hãy viết một class Aspect trong Spring Boot để ghi log khi chạy ứng dụng. Giải thích cách cấu hình trong file pom.xml."
```

Phương án C tập trung quá nhiều vào code và cấu hình.

Nhược điểm:

* Không giúp người học hiểu bản chất Spring AOP.
* Không giải thích đa cấp độ.
* Không có bảng so sánh AOP và OOP.
* Không làm rõ vấn đề Cross-cutting Concerns.
* Dễ khiến người học chỉ copy code nhưng không hiểu vì sao cần dùng AOP.
* Cụm “ghi log khi chạy ứng dụng” còn mơ hồ, chưa nói rõ log ở layer nào, method nào, service nào.

Vì vậy, phương án C phù hợp khi đã hiểu AOP và chỉ cần code mẫu, nhưng không phù hợp với yêu cầu học hiểu bản chất.

---

## 4. Mã nguồn Java ví dụ Aspect ghi log

### pom.xml dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

---

### LoggingAspect.java

```java
package com.example.demo.aspect;

import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Slf4j
@Aspect
@Component
public class LoggingAspect {

    /*
     * Pointcut:
     * Xác định những method nào sẽ bị AOP "chặn" để ghi log.
     *
     * Ý nghĩa:
     * - execution(* com.example.demo.service..*(..))
     * - Áp dụng cho tất cả method trong package com.example.demo.service
     * - Dấu * đầu tiên: không quan tâm kiểu trả về
     * - service..*: áp dụng cả các class trong package con
     * - (..): không quan tâm số lượng tham số
     */
    @Pointcut("execution(* com.example.demo.service..*(..))")
    public void serviceMethods() {
    }

    /*
     * Advice @Before:
     * Chạy trước khi method trong Service được thực thi.
     */
    @Before("serviceMethods()")
    public void logBefore(JoinPoint joinPoint) {
        log.info("Bắt đầu thực thi method: {}", joinPoint.getSignature().toShortString());
    }

    /*
     * Advice @After:
     * Chạy sau khi method trong Service kết thúc,
     * bất kể thành công hay có exception.
     */
    @After("serviceMethods()")
    public void logAfter(JoinPoint joinPoint) {
        log.info("Kết thúc thực thi method: {}", joinPoint.getSignature().toShortString());
    }
}
```

---

### Ví dụ Service bị ghi log tự động

```java
package com.example.demo.service;

import org.springframework.stereotype.Service;

@Service
public class UserService {

    public String findUserById(Long id) {
        return "User ID: " + id;
    }

    public void updateUserName(Long id, String name) {
        System.out.println("Updating user name...");
    }
}
```

---

### Phiên bản nâng cao: Ghi log thời gian thực thi bằng @Around

```java
package com.example.demo.aspect;

import lombok.extern.slf4j.Slf4j;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Slf4j
@Aspect
@Component
public class PerformanceLoggingAspect {

    /*
     * Pointcut áp dụng cho toàn bộ method trong package service.
     */
    @Pointcut("execution(* com.example.demo.service..*(..))")
    public void serviceMethods() {
    }

    /*
     * @Around:
     * Bao quanh method gốc.
     * Có thể đo thời gian trước và sau khi method chạy.
     */
    @Around("serviceMethods()")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long startedAt = System.currentTimeMillis();

        try {
            Object result = joinPoint.proceed();

            long duration = System.currentTimeMillis() - startedAt;

            log.info("Method {} thực thi thành công trong {} ms",
                    joinPoint.getSignature().toShortString(),
                    duration);

            return result;
        } catch (Throwable ex) {
            long duration = System.currentTimeMillis() - startedAt;

            log.error("Method {} thất bại sau {} ms. Lỗi: {}",
                    joinPoint.getSignature().toShortString(),
                    duration,
                    ex.getMessage());

            throw ex;
        }
    }
}
```

---

## 5. Kết luận

Phương án B là lựa chọn tối ưu vì giúp người học hiểu Spring AOP từ bản chất đến thực hành. Prompt này không chỉ yêu cầu AI viết ví dụ, mà còn buộc AI phải giải thích theo cấp độ, so sánh với OOP và đưa ra ví dụ thực tế trong Spring Boot.

Nhờ đó, người học không chỉ biết “copy class Aspect”, mà còn hiểu AOP dùng để xử lý các vấn đề cắt ngang như logging, security, transaction và audit trong dự án thực tế.
