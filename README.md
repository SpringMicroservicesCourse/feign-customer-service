# Feign Customer Service ⚡

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.5-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-2024.0.2-blue.svg)](https://spring.io/projects/spring-cloud)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 專案介紹

這是一個基於 Spring Cloud 的客戶端服務，主要用於演示如何使用 OpenFeign 進行宣告式服務調用。專案實現了一個咖啡訂購系統的客戶端，透過 Feign 介面自動生成 HTTP 客戶端，簡化微服務間的通訊。

### 核心功能描述
- 透過 Eureka 自動發現 waiter-service
- 使用 OpenFeign 實現宣告式 REST 客戶端
- 自定義 HTTP 客戶端配置，提升連線效能
- 整合 Joda Money 處理貨幣計算

### 解決什麼問題
- 微服務架構中的服務發現與調用
- 簡化 REST 客戶端的開發工作
- 減少樣板程式碼，提升開發效率
- 服務間通訊的最佳實踐

### 目標使用者
- Spring Cloud 學習者
- 微服務架構開發者
- 需要了解宣告式服務調用的開發團隊

> 💡 **為什麼選擇此專案？**
> - 提供完整的 Spring Cloud OpenFeign 使用範例
> - 展示宣告式服務調用的最佳實踐
> - 包含 HTTP 客戶端效能優化技巧
> - 適合學習微服務架構設計

### 🎯 專案特色

- **宣告式 REST 客戶端** - 透過介面定義 API，無需手動處理 HTTP 請求
- **自動服務發現** - 透過 Eureka 自動發現 waiter-service
- **負載均衡** - 內建負載均衡功能
- **HTTP 客戶端優化** - 自定義 HttpClient5 配置，提升連線效能
- **貨幣處理** - 整合 Joda Money 處理貨幣計算
- **微服務架構** - 完整的微服務調用範例

## 技術棧

### 核心框架
- **Spring Boot 3.4.5** - 主要應用框架，提供自動配置和內嵌伺服器
- **Spring Cloud 2024.0.2** - 微服務生態系統，提供服務發現和負載均衡
- **Spring Cloud Netflix Eureka Client** - 服務註冊與發現客戶端
- **Spring Cloud OpenFeign** - 宣告式 REST 客戶端，簡化服務調用

### 開發工具與輔助
- **Apache HttpClient5** - HTTP 客戶端，支援連線池和 Keep-Alive
- **Joda Money 2.0.2** - 貨幣計算處理，避免浮點數誤差
- **Apache Commons Lang3** - 工具類庫，提供字串處理等功能
- **Lombok** - 減少樣板程式碼，自動生成 getter/setter
- **Maven** - 專案建置工具，管理依賴和生命週期

## 專案結構

```
feign-customer-service/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── tw/fengqing/spring/springbucks/customer/
│   │   │       ├── CustomerServiceApplication.java    # 主應用程式類別
│   │   │       ├── CustomerRunner.java                # 應用程式啟動執行器
│   │   │       ├── integration/                       # 服務整合層
│   │   │       │   ├── CoffeeService.java            # Feign 咖啡服務介面
│   │   │       │   └── CoffeeOrderService.java       # Feign 訂單服務介面
│   │   │       ├── model/                            # 資料模型
│   │   │       │   ├── Coffee.java                   # 咖啡實體類別
│   │   │       │   ├── CoffeeOrder.java              # 訂單實體類別
│   │   │       │   ├── NewOrderRequest.java          # 新訂單請求類別
│   │   │       │   └── OrderState.java               # 訂單狀態列舉
│   │   │       └── support/                          # 支援類別
│   │   │           ├── CustomConnectionKeepAliveStrategy.java  # 自定義連線策略
│   │   │           ├── MoneyDeserializer.java        # 貨幣反序列化器
│   │   │           └── MoneySerializer.java          # 貨幣序列化器
│   │   └── resources/
│   │       └── application.properties                # 應用程式配置
│   └── test/
│       └── java/
│           └── tw/fengqing/spring/springbucks/customer/
│               └── CustomerServiceApplicationTests.java
├── pom.xml
└── README.md
```

## 快速開始

### 前置需求
- Java 21 或以上版本
- Maven 3.6 或以上版本
- 已啟動的 Eureka Server
- 已啟動的 waiter-service

### 安裝與執行

1. **克隆此倉庫：**
```bash
git clone <repository-url>
cd feign-customer-service
```

2. **進入專案目錄：**
```bash
cd feign-customer-service
```

3. **編譯專案：**
```bash
mvn clean compile
```

4. **執行應用程式：**
```bash
mvn spring-boot:run
```

## 進階說明

### 環境變數
```properties
# 服務註冊設定
spring.application.name=customer-service
server.port=0

# Eureka 設定（預設值）
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/

# Feign 客戶端設定
feign.client.config.default.connect-timeout=500
feign.client.config.default.read-timeout=500
```

### 設定檔說明
```properties
# application.properties 主要設定
spring.application.name=customer-service  # 服務名稱
server.port=0                            # 隨機端口，避免衝突

# Feign 客戶端超時設定
feign.client.config.default.connect-timeout=500  # 連線超時 500ms
feign.client.config.default.read-timeout=500     # 讀取超時 500ms
```

### 核心程式碼說明

#### 1. Feign 介面定義
```java
// 使用 @FeignClient 註解定義服務介面
@FeignClient(name = "waiter-service", contextId = "coffee", path = "/coffee")
public interface CoffeeService {
    @GetMapping(path = "/", params = "!name")
    List<Coffee> getAll();

    @GetMapping("/{id}")
    Coffee getById(@PathVariable Long id);

    @GetMapping(path = "/", params = "name")
    Coffee getByName(@RequestParam String name);
}
```

#### 2. 服務調用實作
```java
// 直接注入 Feign 介面，無需手動處理 HTTP 請求
@Autowired
private CoffeeService coffeeService;
@Autowired
private CoffeeOrderService coffeeOrderService;

// 直接調用介面方法
List<Coffee> coffees = coffeeService.getAll();
CoffeeOrder order = coffeeOrderService.create(orderRequest);
```

#### 3. HTTP 客戶端配置
```java
// 自定義 HttpClient5 配置，提升連線效能
@Bean
public CloseableHttpClient httpClient() {
    return HttpClients.custom()
            .setConnectionTimeToLive(30, TimeUnit.SECONDS)
            .evictIdleConnections(30, TimeUnit.SECONDS)
            .setMaxConnTotal(200)
            .setMaxConnPerRoute(20)
            .disableAutomaticRetries()
            .setKeepAliveStrategy(new CustomConnectionKeepAliveStrategy())
            .build();
}
```

#### 4. 貨幣處理
```java
// 自定義貨幣序列化/反序列化
@JsonComponent
public class MoneySerializer extends StdSerializer<Money> {
    @Override
    public void serialize(Money money, JsonGenerator gen, 
                         SerializerProvider provider) throws IOException {
        gen.writeNumber(money.getAmount());
    }
}
```

## 參考資源

- [Spring Boot 官方文件](https://spring.io/projects/spring-boot)
- [Spring Cloud 官方文件](https://spring.io/projects/spring-cloud)
- [Spring Cloud OpenFeign 文件](https://spring.io/projects/spring-cloud-openfeign)
- [OpenFeign 官方文件](https://github.com/OpenFeign/feign)
- [Apache HttpClient5 文件](https://hc.apache.org/httpcomponents-client-5.2.x/)
- [Joda Money 文件](https://www.joda.org/joda-money/)

## 注意事項與最佳實踐

### ⚠️ 重要提醒

| 項目 | 說明 | 建議做法 |
|------|------|----------|
| 服務發現 | Eureka 連線 | 確保 Eureka Server 已啟動 |
| Feign 介面 | 介面定義 | 不要在介面上加 @RequestMapping |
| Context ID | 服務區分 | 使用 contextId 區分相同服務的不同介面 |
| 連線管理 | HTTP 客戶端 | 配置連線池和超時設定 |
| 貨幣處理 | 精確計算 | 使用 Joda Money 避免浮點數誤差 |

### 🔒 最佳實踐指南

- **介面設計**：保持 Feign 介面簡潔，避免複雜的業務邏輯
- **錯誤處理**：實作適當的異常處理機制
- **效能優化**：合理配置連線池大小和超時時間
- **監控日誌**：記錄關鍵操作和錯誤資訊
- **配置管理**：使用外部配置檔案管理環境設定

### 💡 開發建議

1. **程式碼註解**：在重要的程式碼區塊添加清楚註解，方便團隊成員理解與維護
2. **台灣用語**：使用台灣常用的專業用語，確保溝通順暢且符合本地習慣
3. **介面設計**：Feign 介面應該與服務端 API 保持一致
4. **測試覆蓋**：為關鍵功能撰寫單元測試和整合測試

## 與 Ribbon 版本比較

| 特性 | Feign 版本 | Ribbon 版本 |
|------|------------|-------------|
| **服務調用方式** | 宣告式介面 (`@FeignClient`) | 程式化 (`RestTemplate`) |
| **程式碼簡潔度** | 更簡潔，介面導向 | 較多樣板程式碼 |
| **可讀性** | 高，介面清楚定義 API | 中等，需要手動處理 HTTP 請求 |
| **維護性** | 高，介面變更自動反映 | 中等，需要手動同步 |
| **學習曲線** | 較陡，需要理解介面概念 | 較平緩，基於 RestTemplate |

## 授權說明

本專案採用 MIT 授權條款，詳見 LICENSE 檔案。

## 關於我們

我們主要專注在敏捷專案管理、物聯網（IoT）應用開發和領域驅動設計（DDD）。喜歡把先進技術和實務經驗結合，打造好用又靈活的軟體解決方案。

## 聯繫我們

- **FB 粉絲頁**：[風清雲談 | Facebook](https://www.facebook.com/profile.php?id=61576838896062)
- **LinkedIn**：[linkedin.com/in/chu-kuo-lung](https://www.linkedin.com/in/chu-kuo-lung)
- **YouTube 頻道**：[雲談風清 - YouTube](https://www.youtube.com/channel/UCXDqLTdCMiCJ1j8xGRfwEig)
- **風清雲談 部落格**：[風清雲談](https://blog.fengqing.tw/)
- **電子郵件**：[fengqing.tw@gmail.com](mailto:fengqing.tw@gmail.com)

---

**📅 最後更新：2025-08-11**  
**👨‍💻 維護者：風清雲談團隊**
